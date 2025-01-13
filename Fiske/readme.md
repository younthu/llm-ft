1. `fiske_new_york_university.1.image_stripped.html`是去除base64图片和自体后的html, 73KB. 
    1. 尝试直接把这段html扔给doubao, 让它提取大学名称，失败， 说输入超过字数限制了。
    1. 总共包含3页(`<div id="pf1"`)，把其中一页的内容输入AI，能提取信息。
1. 

问题:
1. AI会不会计算css样式的值？比如，让ai计算某个节点的字体大小是多少。答案是可以。可以用test.html做测试.
1. 

实验:

1. Finetune llama 3.2, 把fiske html转换成json，只提取大学名字。
1. 把fiske html转换成json，只提取大学名字，去除引用的名字, 如'xxxx see pages 999'
1. Finetune llama 3.2, 把fiske html转换成json，提取大学名, 地址和描述信息
1. Finetune llama 3.2, 把fiske html转换成json，只提取大学名字,地址，描述信息和reviews.
1. Finetune llama 3.2, 把fiske html转换成json，提取所有信息.


# 实验 1
训练llm, 把简单的html内容转换成json

**概述**：
1. 一个包含 `<h1>`标签的html, 把`<h1>`里面的内容提取为大学名.
1. 数据集用simple_html.json, 167条重复的记录。
1. Base model用`Llama-3.2-1B-Instruct`.
1. Loss很快下降为0.
1. 结果, 输出多条重复的json，停不下来的样子，还会有截断。


**步骤**：
1. `python Fiske/train_exp1.py`
1. `streamlit run chat.py`
1. 隧道连接到远程: `ssh -L 8501:localhost:8501 zhiyong@35.84.190.171`
1. 打开本地页面: `http://localhost:8501`

**截图:**
   ![](./screenshots/loss_go_down_fast.png)
   ![](./screenshots/repeat_json_and_truncated.png)

# 实验2
和实验1一样，只是数据集换了一下，换成了`simple_html2.json`, 这个数据集在`simple_html1.json`的基础上，把`address`里面的邮编换成了递增的数字, 给数据集加了一点点变化。训练的效果和实验1基本一样，另外，感觉AI变啰嗦了，说很多不需要的东西。

**概述**：
1. 把html里面的大学名称和地址提取出来，以json格式输出.
1. 数据集用`simple_html2.json`, 10000条记录, 包含大学名称和地址，名称都一样，地址也都一样，只是末尾的zip code用了递增数字。
1. Base model用`Llama-3.2-1B-Instruct`.
1. 17分钟微调完毕。
1. Loss很快下降为0.
1. 结果, 输出多条重复的json，停不下来的样子，还会有截断。
1. 它在输出的内容里面有提到正确答案。
1. 还出现很多幻觉，有一次对话里面还提到新加坡，不知道是什么内容关联到了新加坡

**截图:**
   ![](./screenshots/hallucination.png)
   ![](./screenshots/illusion.png)

# 实验3

代码不变, base model换成了`Llama-3.2-3B-Instruct`, 数据集换了一下，换成了`simple_html3.json`, 这个数据集是从Fiske数据里面抽了30条记录，把university name, address, short_intro打乱，拼凑了27000条记录.

**概述**：
1. 把html里面的大学名称, 简介和地址提取出来，以json格式输出.
1. 数据集用`simple_html3.json`, 27000条记录, 30条Fiske University记录，里面的name, address, short_intro组合而来。
1. Base model用`Llama-3.2-3B-Instruct`.
1. 微调速度明显下降, 要5个半小时。
1. Loss很快下降为0.
1. 结果, 输出想要的json了。
1. __问题:__ 结果末尾出现了`<|eom_id|>`, 原因未知，解决办法未知.
    1. 这次的数据集里面有一个问题， short_intro的<div>标签和</p>配对了。后面把</p>换成</div>再试一下看看.
    1. 这个标记也不是每次都出现，换Wheaton College (IL)的prompt的时候就不出现这种标记了。
    1. 实验对比下来发现，这个标记只在会话的第一次回复里面出现，可以看后面的对比截图。
1. 测试prompt:
   ~~~html
    请把下面的fiske html转换成json格式:

    <html> <body> <h1>Yale University</h1> <p> 38 Hillhouse Avenue, New Haven, CT 06520</p> <div>Yale is the middle- sized member of the Ivy League’s big three: bigger than Princeton, smaller than Harvard. Its widely imitated residential college system helps Yale strike a balance between being a research university and an undergraduate college. New Haven isn’t New York, but it has a relatively lively urban scene. Plan to work hard.</div> </body> </html>
   ~~~


   ~~~html
    请把下面的fiske html数据转换成json:

    <html> <body> <h1>Wheaton College (IL)</h1> <p> 501 College Avenue, Wheaton, IL 60187</p> 
    <div>Wheaton is at the top of the academic heap in evangelical education, challenged only by Pepperdine (with its Malibu digs) and traditional competitors such as Gordon and Calvin. Students must not only follow Wheaton’s stringent code of conduct but also affirm their personal faith in Jesus Christ. Wheaton’s low tuition makes it relatively affordable. The worldly temptations of Chicago hover less than an hour away.</div> </body> </html>
   ~~~

**截图**：

   ![](./screenshots/exp3_s1.png)
   ![](./screenshots/exp3_s2.png)
   ![](./screenshots/exp3_s3.png)
   ![](./screenshots/exp3_s4.png)

# 问题
1. 用llama3.2 1b + simple_html.json 训练, 不停重复同一条json记录.
1. 

# Refs
1. https://www.datacamp.com/tutorial/llama3-fine-tuning-locally 
1. https://blog.csdn.net/zwqjoy/article/details/132772751, 讲指令格式。
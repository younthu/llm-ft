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

1. 一个包含 `<h1>`标签的html, 把`<h1>`里面的内容提取

# 问题
1. 用llama3.2 1b + simple_html.json 训练, 不停重复同一条json记录.
1. 
# Refs
1. https://www.datacamp.com/tutorial/llama3-fine-tuning-locally 
1. https://blog.csdn.net/zwqjoy/article/details/132772751, 讲指令格式。
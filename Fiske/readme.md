1. `fiske_new_york_university.1.image_stripped.html`是去除base64图片和自体后的html, 73KB. 
    1. 尝试直接把这段html扔给doubao, 让它提取大学名称，失败， 说输入超过字数限制了。
    1. 总共包含3页(`<div id="pf1"`)，把其中一页的内容输入AI，能提取信息。
1. 

实验:

1. Finetune llama 3.2, 把fiske html转换成json，只提取大学名字。
1. 把fiske html转换成json，只提取大学名字，去除引用的名字, 如'xxxx see pages 999'
1. Finetune llama 3.2, 把fiske html转换成json，提取大学名, 地址和描述信息
1. Finetune llama 3.2, 把fiske html转换成json，只提取大学名字,地址，描述信息和reviews.
1. Finetune llama 3.2, 把fiske html转换成json，提取所有信息.
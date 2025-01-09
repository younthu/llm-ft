
本代码来自于junyao的嬛嬛训练教程https://junyao.tech/posts/e45a9231.html.

# Quick start

1. 下载代码: `git clone https://github.com/younthu/llm-ft.git`
2. 搭建Anaconda:
   1. `wget https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/Anaconda3-2024.06-1-Linux-x86_64.sh`
   2. `chmod +x Anaconda3-2024.06-1-Linux-86_64.sh`
   3. `./Anaconda3-2024.06-1-Linux-86_64.sh`
   4. `conda init`
   5. `conda activate`
3. `conda create -n llm-ft python=3.10`
4. `conda activate llm-ft`
5. `pip install -r requirements.txt`
6. (optional), 如果是M2, 跑的时候可能会遇到`Cannot copy out of meta tensor; no data`
   ~~~sh
    NotImplementedError:Cannot copy out of meta tensor; no data! Please use torch.nn.Module.to_empty() instead of torch.nn.Module.to() when moving module from meta to a different device.
   ~~~

   解决：强制设置 device = “mps”

   ~~~python
    # 检查CUDA是否可用，然后检查MPS是否可用，最后回退到CPU
    # device = torch.device("cuda" if torch.cuda.is_available() else "mps" if torch.backends.mps.is_available() else "cpu")
    device = "mps"
   ~~~
7. `python train.py`
   1. 代码会开始自动下载模型。
   2. 微调完成后，所有的文件会保存在 models 文件夹下面，结构如下：
      ~~~sh
      ├── models
        ├── checkpoint #【模型微调的 checkpoint】
        │ ├── LLM-Research
        │ │ └── Meta-Llama-3-8B-Instruct
        │ │     ├── checkpoint-100
        │ │     ├── checkpoint-200
        │ │     ├── checkpoint-xxx
        │ └── qwen
        │     └── Qwen1.5-4B-Chat
        │         ├── checkpoint-100
        │         ├── checkpoint-200
        │         ├── checkpoint-xxx
        ├── lora   #【模型微调的 lora 文件】
        │ ├── LLM-Research
        │ │ └── Meta-Llama-3-8B-Instruct
        │ └── qwen
        │     └── Qwen1.5-4B-Chat
        └── model  #【自动下载的基座模型】
            ├── LLM-Research
            │ └── Meta-Llama-3-8B-Instruct
            └── qwen
                └── Qwen1___5-4B-Chat

      ~~~
8. 默认的模型还可能会遇到显存不够用的问题, 解决办法, 用qwen 4B小模型，如果还太大的话还可以用Qwen 1.8B小模型: 
   ~~~python
    # https://www.modelscope.cn/models/qwen/Qwen1.5-4B-Chat/summary
    model_id = 'qwen/Qwen1.5-4B-Chat'
   ~~~
   或者，用llama 3.2 1B/3B, 'LLM-Research/Llama-3.2-1B-Instruct'/'LLM-Research/Llama-3.2-3B-Instruct', 注意: Llama3.2 11B及以上版本是为vision准备的，微调出来的有问题。

9.  训练完成以后就可以启动聊天窗口聊天了: 
   1. 启动聊天服务: `streamlit run chat.py`
   1. 打开web: `http://localhost:8501`
   1. (optional) 如果是远程服务器，可以ssh隧道连过去`ssh -L 8501:localhost:8501 zhiyong@35.84.190.xxx`，然后本地就可以打开远程网页了`http://localhost:8501`，不用开放远程主机的端口。

# 问题列表
1. `Llam3.2_11b_vision_instruct`, 问你好的时候，AI有胡说八道.`LLM-Research/Llama-3.2-1B-Instruct`不存在这种问题. 
1. `Llam3.2_11b_vision_instruct`, 问你好的时候，AI有不停重复的内容。`LLM-Research/Llama-3.2-1B-Instruct`不存在这种问题. 
1. `Llam3.2_11b_vision_instruct`, 问你好的时候，AI有乱码.`LLM-Research/Llama-3.2-1B-Instruct`不存在这种问题. 
1. `Llam3.2_11b_vision_instruct`, 问你好的时候，AI有一个莫名其妙的双引号。`LLM-Research/Llama-3.2-1B-Instruct`不存在这种问题. 
1. `Llam3.2_11b_vision_instruct`, 问你好的时候，AI有胡说八道.
1. 
1. 

# 截图
Llama3.2-1b-instruct

![](/screenshots/Llam3.2_1b_instruct.png)

Llama3.2-11b-vision-instruct

![](/screenshots/Llam3.2_11b_vision_instruct.png)
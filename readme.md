
本代码来自于junyao的嬛嬛训练教程https://junyao.tech/posts/e45a9231.html.

# Quick start

1. 下载代码: `wget --no-check-certificate https://files.junyao.tech/uPic/llama3-ft.zip`
1. `mkdir llama3-ft`
1. `mv llama3-ft.zip llama3-ft`
1. `cd llama3-ft`
1. `unzip llama3-ft.zip`
2. 搭建Anaconda:
   1. `wget https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/Anaconda3-2024.06-1-Linux-x86_64.sh`
   2. `chmod +x Anaconda3-2024.06-1-Linux-86_64.sh`
   3. `./Anaconda3-2024.06-1-Linux-86_64.sh`
   4. `conda init`
   5. `conda activate`
3. `conda create -n llama3-ft python=3.10`
4. `conda activate llama3-ft`
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
9.  训练完成以后就可以启动聊天窗口聊天了: `streamlit run chat.py`


Llama3.2-1b-instruct
![](/screenshots/Llam3.2_1b_instruct.png)
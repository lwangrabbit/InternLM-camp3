

## 一.提示词工程

### 1.什么是Prompt

Prompt是一种用于指导以大语言模型为代表的**生成式人工智能**生成内容(文本、图像、视频等)的输入方式。它通常是一个简短的文本或问题，用于描述任务和要求。

* Prompt可以包含一些特定的关键词或短语，用于引导模型生成符合特定主题或风格的内容；
* Prompt还可以包含一些特定的指令或要求，用于控制生成文本的语气、风格、长度等方面；

总之，Prompt是一种灵活、多样化的输入方式，可以用于指导大语言模型生成各种类型的内容。

![](./prompt.png)

### 2.什么是提示词工程

提示工程是一种通过设计和调整输入(Prompts)来改善模型性能或控制其输出结果的技术。

在模型回复的过程中，首先获取用户输入的文本，然后处理文本特征并根据输入文本特征预测之后的文本，原理为**next token prediction**。

主要的提示词设计框架：

**[CRISPE](https://github.com/mattnigh/ChatGPT3-Free-Prompt-List)**：

- **C**apacity and **R**ole (能力与角色)：希望 ChatGPT 扮演怎样的角色。
- **I**nsight (洞察力)：背景信息和上下文(坦率说来我觉得用 Context 更好)
- **S**tatement (指令)：希望 ChatGPT 做什么。
- **P**ersonality (个性)：希望 ChatGPT 以什么风格或方式回答你。
- **E**xperiment (尝试)：要求 ChatGPT 提供多个答案。

**[CO-STAR](https://aiadvisoryboards.wordpress.com/2024/01/30/co-star-framework/)**：

- **C**ontext (背景): 提供任务背景信息

- **O**bjective (目标): 定义需要LLM执行的任务

- **S**tyle (风格): 指定希望LLM具备的写作风格

- **T**one (语气): 设定LLM回复的情感基调

- **A**udience (观众): 表明回复的对象

- **R**esponse (回复): 提供回复格式

  

### 3.langGPT提示词工程

LangGPT框架参考了面向对象程序设计的思想，设计为基于角色的双层结构，一个完整的提示词包含**模块-内部元素**两级，模块表示要求或提示LLM的方面，例如：背景信息、建议、约束等。内部元素为模块的组成部分，是归属某一方面的具体要求或辅助信息，分为赋值型和方法型。

![](./langgpt.png)

langGPT编写技巧：

* **构建全局思维链**；
* **保持上下文语义一致性**
* **有机结合其他 Prompt 技巧**



## 二.langGPT实践（作业）

### 1.环境准备

```
# 创建虚拟环境
conda create -n langgpt python=3.10 -y
conda activate langgpt
# 安装一些必要的库
conda install pytorch==2.1.2 torchvision==0.16.2 torchaudio==2.1.2 pytorch-cuda=12.1 -c pytorch -c nvidia -y

# 安装其他依赖
pip install transformers==4.43.3

pip install streamlit==1.37.0
pip install huggingface_hub==0.24.3
pip install openai==1.37.1
pip install lmdeploy==0.5.2
```

lmdeploy部署InternLM2-1.8b模型：

```
CUDA_VISIBLE_DEVICES=0 lmdeploy serve api_server /share/new_models/Shanghai_AI_Laboratory/internlm2-chat-1_8b --server-port 23333 --api-keys internlm2
```

运行图形化界面：

```
python -m streamlit run chat_ui.py
```

执行端口映射：

```
ssh -p 33141 root@ssh.intern-ai.org.cn -CNg -L 7860:127.0.0.1:8501 -o StrictHostKeyChecking=no
```



### 2.未应用提示词工程

当未应用提示词工程时，可以看到，模型做出了错误的回答。

![](./no_apply_langgpt.png)



### 3.生成提示词

![](./gen_prompt.png)

生成的提示词如下：

```
# Role: 数学家

## Background:
- 作为一位数学家，你拥有深厚的数学知识和卓越的计算能力，能够处理各种复杂的数学问题。

## Profile:
- 你是一位在数学领域享有盛誉的专家，以解决数学难题和提供精确计算而著称。

## Skills:
- 你具备高级的数学理论、逻辑推理、抽象思维和快速计算的能力。

## Goals:
- 你的目标是解决用户提出的数学问题，提供精确的解答和可能的解题思路。

## Constrains:
- 确保解答的准确性和逻辑性，使用清晰和专业的数学语言。

## OutputFormat:
- 数学表达式
- 解答过程
- 最终结果

## Workflow:
1. 理解用户提出的数学问题。
2 运用数学知识和逻辑推理来解决问题。
3 提供详细的解答过程和最终结果。

## Examples
1.问题：比较3.1和3.5的大小。
  解答：3.5大于3.1。
2.问题：比较3.9和3.11的大小。
  解答：3.11大于3.9。

## Initialization
- 欢迎来到数学家的工作台，我将帮助你解决任何数学难题。请告诉我你的问题，让我们开始吧！
```



### 4.应用过提示词工程

可以看到，使用了LangGPT提示词以后，模型给出了正确的答案。

![](./apply_langgpt.png)


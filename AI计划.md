#  大模型

**定义**

大模型以**海量数据**为训练对象，基于**深度学习架构**(如**Transformer**)构建的大型**神经网络**模型



大模型的**核心能力**是自然语言处理与生成(NLP)

目前的大模型已经超越了**单纯的自然语言处理阶段**，可以支持图片，语音的输入输出，有代码大模型

大模型的**终极目标**是接近人类水平的通用人工智能，NLP的实现是大模型的第一步，未来会朝着更加多元化，多模态的方向发展



**神经网络**

深度学习的基石

神经网络是一种模拟生物神经元结构的计算模型，大量节点模仿神经元，通过不同权重连接组成，模拟人脑工作

- 原始数据的输入
- 多层神经元通过矩阵，函数进行特征变换
- 输出结果

**深度学习**

深度学习是神经网络的拓展，为更加复杂的神经网络系统

依赖更多的数据，获得更强大的特征表达能力和算力



机器学习(让计算机通过海量数据学习规律，用于决策预判)的一种

**Transformer**

深度学习架构的一种



Transformer重新定义了机器理解和生成自然语言的方式，是现阶段NPL技术的基石

Transformer模型出现之前，NLP主流模型存在

**串型计算限制**(处理长文本需要逐个字符计算分析，效率低)

**长距离依赖问题**(文本量大的上下文难以建立依赖)



Transformer 通过**并行计算，全局语义捕捉**

在计算每个词的时候能并行关注句子中所有词的上下文信息



# Prompt工程

对于ai的请求为Prompt

AI本质根据数据模式生成内容，需要完整的语言描述，才能获得良好的回答效果

回答更准确

充分利用ai资源

回答程序处理方便，格式化



prompt的意义不仅仅是问题的提问，对于我们日常开发的某个模块的编写，甚至某些功能的实现，可以用格式化的prompt实现。html-json sum infer transfrom proofread

**we are supposed to build applications with large language models**

![截屏2025-06-18 08.22.02](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2025-06-18%2008.22.02.png)

## Introduction

ChatGPT Prompt Engineering for Developers

LLMs have two types - Base LLMs(基础大语言模型) and Instruction tuned LLMs(指令微调大语言模型)



base LLM - 基于文本训练数据来预测下一个词

instruction tuned LLM - 遵循指令 - 从一个已经在大量文本数据上训练过的base LLM开始 进一步使用由良好输入输出组成的指令来微调，使用RLHF(基于人类反馈的强化学习进一步优化)

a lot of practical usage scenarios have bean shifting toward instruction tuned LLMs

instruction tuned LLM 更符合人类意图

## Guidelines

- write clear and specific instructions
- give the model time to think



guideline 1

**1 -** 使用**分隔符**以明确输入的不同部分(比如你要让大模型去操作一个段落，用特定分隔符标注段落，并指明分隔符)

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/Screenshot%202025-06-19%20at%2017.10.33.png" alt="Screenshot 2025-06-19 at 17.10.33" style="zoom:25%;" />

这可以避免 prompt injection - 用户输入的文本带有控制命令，反驳掉了我们的原始命令

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/Screenshot%202025-06-19%20at%2017.14.59.png" alt="Screenshot 2025-06-19 at 17.14.59" style="zoom:25%;" />



**2 -** ask for structured output

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/Screenshot%202025-06-19%20at%2017.17.51.png" alt="Screenshot 2025-06-19 at 17.17.51" style="zoom:25%;" />

**3 -** ask the model to check whether conditions are satisfied

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/Screenshot%202025-06-19%20at%2017.20.35.png" alt="Screenshot 2025-06-19 at 17.20.35" style="zoom:25%;" />

**4 -** few-shot prompting 提供一些希望模型执行的任务示例

give successful examples of completing tasks , then ask model to perform the task

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/Screenshot%202025-06-19%20at%2017.24.12.png" alt="Screenshot 2025-06-19 at 17.24.12" style="zoom:25%;" />

guideline 2

if a model is making reasoning errors by rushing to an incorrect conclusion

you should try reframing the query to request a chain or series of relevant reasoning before the model provides its

**1 -**Specify the steps to complete a task

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/Screenshot%202025-06-19%20at%2017.32.49.png" alt="Screenshot 2025-06-19 at 17.32.49" style="zoom:25%;" />

**2 -** instruct the model to work out its own solution before rushing to a conclusion

指示模型下结论前进行一次全局的自主的思考计算，不要根据部分文章下结论

比如判断一个学生的问题是否回答正确，不能只看学生回答过程，要从这个题目开始，模型应该先自己思考一遍题目



**Model Limitations**

 大模型不能完全记住所有信息，其对自己的知识边界不是很清楚，当它回答一些冷僻话题的时候，可能会编造一些听起来似乎合理的内容but actually are not true - hallucinations - 幻觉

reducing hallucination:

first find relevant information, then answer the question based on relevant information

答案溯源

## Iterative

**Prompt guidelines**

- Be clear and specific
- Analyze why result does not give desired output
- Refine the idea and the prompt
- Repeat



- Try something
- Analyze where the result does not give what you want
- Clarify instructions, give more time to think
- Refine prompts with a batch of examples - 准备自己的例子迭代调优

## Summarizing

this is a practical example about using LLM to summarize a complex text with appropriate prompt 

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/Screenshot%202025-06-19%20at%2022.27.47.png" alt="Screenshot 2025-06-19 at 22.27.47" style="zoom:25%;" />

## Inferring

extract fields out of a piece of text with just a single prompt

to analyze its statement

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/Screenshot%202025-06-20%20at%2009.32.02.png" alt="Screenshot 2025-06-20 at 09.32.02" style="zoom:25%;" />

determine which of these topics are covered in that news or article, even we can make a similar prompt to analyze the composition of a picture, you know, in my latest project

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/Screenshot%202025-06-20%20at%2009.38.34.png" alt="Screenshot 2025-06-20 at 09.38.34" style="zoom:25%;" />

You can build multiple systems for making inferences about text that previously just would have taken days or even weeks

## Transforming

language transfromation and identify

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/Screenshot%202025-06-20%20at%2009.46.06.png" alt="Screenshot 2025-06-20 at 09.46.06" style="zoom:25%;" />

tune transformation

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/Screenshot%202025-06-20%20at%2009.50.56.png" alt="Screenshot 2025-06-20 at 09.50.56" style="zoom:25%;" />

formats transformation

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/Screenshot%202025-06-20%20at%2009.52.45.png" alt="Screenshot 2025-06-20 at 09.52.45" style="zoom:25%;" />

spell check and grammer checking

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/Screenshot%202025-06-20%20at%2009.56.53.png" alt="Screenshot 2025-06-20 at 09.56.53" style="zoom:25%;" />

## Expanding

use language model to generate a personalized email based on some information

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/Screenshot%202025-06-20%20at%2010.18.12.png" alt="Screenshot 2025-06-20 at 10.18.12" style="zoom:25%;" />



you are supposed to change one parameter named "temperature" to control the randomness of the model's output, influencing how creative or deterministic the responses are

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/Screenshot%202025-06-20%20at%2010.25.35.png" alt="Screenshot 2025-06-20 at 10.25.35" style="zoom:25%;" />

At a higher temperature, the outputs from the model are kind of moe random

we may need a higher temperature in chatbot case

but when we build a ai assist about medical or restaurant ,we may need lower temperature, because the answer is supposed to be correct, instead of pretty much imagination

## ChatBot

指定语言的角色等

 <img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/Screenshot%202025-06-20%20at%2010.34.02.png" alt="Screenshot 2025-06-20 at 10.34.02" style="zoom:50%;" />

build the context

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/Screenshot%202025-06-20%20at%2010.35.26.png" alt="Screenshot 2025-06-20 at 10.35.26" style="zoom:50%;" />

automate the collection of user prompts and assistant responses in order to build this Autobot

维护好上下文。指定好背景，循环输入输出

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/Screenshot%202025-06-20%20at%2010.39.32.png" alt="Screenshot 2025-06-20 at 10.39.32" style="zoom:50%;" />
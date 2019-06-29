# BERT论文阅读
1.通过预训练的方式获得上下文的双向表示
2.ELMo模型
是有两个方向相反的LSTM网络组合而成，单向的语言模型做预训练。
Feature-base方式
OpenAI GPT
是transform decoder，图中每一层所有的Trm是一个transformer层，每层间做self attention（masked，只和之前的做self attention），然后将输出结果喂给下一层，这就相当于下一层的一个trm会和上一层该位置之前的trm做了连接，后面的信息被屏蔽（代码实现上用一个mask 矩阵屏蔽）。单向的语言模型做预训练
fine-tune方式
BERT
双向深层transformer encoder，同样每层之间做self attention，然后将输出结果喂给下一层，这就相当于与上一层前后两个方向的Trm做了连接。Masked LM和Next Sentence Prediction做预训练
fine-tune方式。
3.BERT双向Transformer Encoder 模型
4.FINE-TURNING
5.BERT的特征拼接是：三个embedding的求和，分别是Token embedding：词向量，Segment Embedding：区分不同句子的标志（预训练中需要预测是否为下一个句子）Position Embedding ：单词的位置信息
6.BERT解决了ELMO首先无法进行并行化训练，其次RNN无法理解模型双向的语义信息，而BERT通过attention机制能更好的找到语义信息
7.BERT是一种预训练语言表示的方法，在大量文本语料（维基百科）上训练了一个通用的“语言理解”模型，然后用这个模型去执行想做的NLP任务。BERT比之前的方法表现更出色，因为它是第一个用在预训练NLP上的无监督的、深度双向系统。
8.BERT在应用的时候只需要加载模型，根据不同的任务进行不同FINE-TURNING就可以了
9.BERT存在的问题待补充
10.bert是目前nlp领域state of art的方法，另外bert模型也启示了预训练模型的潜力。

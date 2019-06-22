# Deep contextualized word representations

## 1、 论文动机？

Mikolov，Pennington等人提出预训练的单词分布式表示。高质量的表示应该同时表达单词使用语法和语义以及在上下文语境中的变化。之前的工作对于所有的单词都只有一种表示方法，无疑没有表达出单词一词多义的情况。因此试图解决语境化词语表示的难题，并且可以轻松地集成到现有模型中。
我们的表示与传统的单词类型嵌入不同，因为每个标记被赋予一个表示，该表示是整个输入句子的函数。我们使用从双向LSTM导出的向量，该双向LSTM在大文本语料库上用耦合语言模型（LM）目标训练。出于这个原因，我们称它们为ELMo（嵌入式语言模型）表示。与先前用于学习情境化单词向量的方法不同（Peters等人，2017; McCann等人，2017），ELMo表示是深层的，因为它们是biLM的所有内部层的函数。更具体地说，我们学习了每个结束任务的每个输入字上方堆叠的矢量的线性组合，这显着提高了仅使用顶部LSTM层的性能。以这种方式组合内部状态允许非常丰富的单词表示。使用内在评估，我们表明较高级别的LSTM状态捕获了词义的上下文相关方面（例如，它们可以在没有修改的情况下使用以在监督的词义检测消歧任务上很好地执行），而较低级别的状态模拟语法的方面（例如，它们可用于进行词性标注）。同时暴露所有这些信号是非常有益的，允许学习模型选择对每个最终任务最有用的半监督类型。

## 2、 ELMo 相对于 word2vec 、 glove 的优点？
相对于word2vec 、 glove等，捕捉了上下文语境信息而不仅仅是字词单独的信息。
word2vec中，单词在不同语境下向量表示完全一致，而ELMo针对这一点做了优化。

## 3、 ELMo 采用的模型？
作者采用的嵌入模型是基于LSTM的biLM，但是并未加以更多限制，理论上GRU等等都可以达到类似作用。
先介绍biLM。给定一个长度为N的序列$\left(t_{1}, t_{2}, \ldots, t_{N}\right)$

前向：基于历史$\left(t_{1}, \ldots, t_{k-1}\right)$，token $t_k$ 的概率可以建模为：

$p\left(t_{1}, t_{2}, \ldots, t_{N}\right)=\prod_{k=1}^{N} p\left(t_{k} | t_{1}, t_{2}, \ldots, t_{k-1}\right)$

后向：类似的，有：

$p\left(t_{1}, t_{2}, \ldots, t_{N}\right)=\prod_{k=1}^{N} p\left(t_{k} | t_{k+1}, t_{k+2}, \ldots, t_{N}\right)$

biLM结合了前向和后向LM，最大化了前后向的对数似然：

$\begin{array}{l}{\sum_{k=1}^{N}\left(\log p\left(t_{k} | t_{1}, \ldots, t_{k-1} ; \Theta_{x}, \vec{\Theta}_{L S T M}, \Theta_{s}\right)\right.} \\ {\quad+\log p\left(t_{k} | t_{k+1}, \ldots, t_{N} ; \Theta_{x}, \stackrel{\leftarrow}{\Theta}_{L S T M}, \Theta_{s}\right) )}\end{array}$

$ \Theta_{s}$为softmax层参数，$\Theta_x$为token的向量表示，堆叠L层的biLM后，对于每个Token会有2L+1个向量表示（含输出）

$ELMo_{k}^{\text { task }}=E\left(R_{k} ; \Theta^{\text { task }}\right)=\gamma^{\text { task }} \sum_{j=0}^{L} s_{j}^{\text { task }} \mathbf{h}_{k, j}^{L M}$

其中$\mathbf{h}_{k, j}^{L M}$是对于token k的第j层的隐藏层输出，当j=0的时候表示输入层token的向量表示，这个向量表示可以通过CNN或Highway等网络引入char-level的特征。 $s_{j}^{\text { task }} $是一个softmax的归一化参数， $$\gamma^{\text { task }} $$是一个放缩的参数，可以让目标模型对ELMo的向量进行放缩。 



## 4、 ELMo 属于  Feature-based or fine-turning?

fine-turning

## 5、 ELMo 如何进行特征拼接？

见3

## 6、 ELMo 解决了什么问题？

语境化词语表示的难题，应该同时表达单词使用语法和语义以及在上下文语境中的变化。

## 7、 用一句话介绍 ELMo？

使用双向LSTM堆叠提取特征的语言模型

## 8、 ELMo 模型怎么应用到下游任务？

直接使用预训练的模型，将train_x和test_x替换为ELMo的向量表示，或者利用目标领域的语料进行微调

## 9、 ELMo 存在问题？

待续，我认为可能的问题集中在两个，第一是语义捕捉方面，双向LSTM的拼接和真正的语义传递有差别。第二个是性能可能存在问题。



## 10、如何评价 ELMo ？

待续


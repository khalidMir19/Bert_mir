---
语言: 半方
标签:
- 埃克伯特
许可证: 数据集2.0
数据集:
- 图书语料库
- 维基百科
---

#伯特基本模型（无案例）

使用蒙版语言建模（传销）目标的英语语言预训练模型。年推出的
[这篇论文](https://arxiv.org/abs/1810.04805)并首次发布于
[这个仓库](https://github.com/google-research/bert). 这个模型是无壳的：它没有什么区别。
在英语和英语之间。

免责声明：发布BERT的团队没有为这个模型写一个模型卡，所以这个模型卡是由
拥抱脸团队。

##模型描述

BERT是一个以自我监督的方式对大量英语数据进行预培训的变压器模型。这就是我的意思
只对原始文本进行了预训练，没有人以任何方式标记它们（这就是为什么它可以使用大量的
可公开获取的数据），通过自动过程从这些文本中生成输入和标签。更准确地说，它
进行了预培训，目标有两个：

—屏蔽语言建模（传销）：取一个句子，该模型随机屏蔽输入中15%的单词，然后运行
通过该模型对整个蒙面句进行预测，并对蒙面词进行预测。这是不同于传统的
递归神经网络（RNNs），通常看到一个接一个的话，或从自回归模型，如
GPT，它在内部屏蔽未来令牌。它允许模型学习一个双向表示的
句子
—下一句预测（NSP）：在预训练过程中，模型连接两个被屏蔽的句子作为输入。有时
它们对应于原文中相邻的句子，有时不对应。然后模型必须
预测这两个句子是否前后一致。

通过这种方式，该模型学习英语语言的内部表示，然后可用于提取特征
对于下游任务很有用：例如，如果您有一个标记句子的数据集，您可以训练一个标准的
分类器使用BERT模型产生的特征作为输入。

##模型变化

BERT最初已经发布了基本和大的变化，为大小写和非大小写输入文本。非套色模型还去掉了重音标记。  
中文和多语言的非加壳和加壳版本之后不久。  
修改后的预处理与全字掩蔽取代子块掩蔽在随后的工作中，与两个模型的释放。  
其他24个较小的模型发布后。

详细的发布历史记录可以在[谷歌研究/伯特自述](https://github.com/google-research/bert/blob/master/README.md)在推特上。

模型参数语言
|------------------------|--------------------------------|-------|
| [`bert-base-uncased`](https://huggingface.co/bert-base-uncased)英语
| [`大无壳`](https://huggingface.co/bert-large-uncased)340M
| [`贝尔特式`](https://huggingface.co/bert-base-cased)英语
| [`伯特大箱`](https://huggingface.co/bert-large-cased)英语
| [`柏特汉语`](https://huggingface.co/bert-base-chinese)中国大陆
| [`bert-base-multilingual-cased`](https://huggingface.co/bert-base-multilingual-cased) | 110多重|
| [`bert-large-uncased-whole-word-masking`](https://huggingface.co/bert-large-uncased-whole-word-masking)英语
| [`bert-large-cased-whole-word-masking`](https://huggingface.co/bert-large-cased-whole-word-masking)英语

##预期用途和限制

您可以将原始模型用于屏蔽语言建模或下一句预测，但它主要用于
对下游任务进行微调。请参阅[模型中心](https://huggingface.co/models?filter=bert)寻找
您感兴趣的任务的微调版本。

请注意，该模型的主要目的是在使用整个句子的任务（可能是屏蔽的）上进行微调。
进行决策，如序列分类、标记分类或问题回答。对于任务（如文本
代你应该看看模型像GPT 2。

###如何使用

您可以将此模型直接与管道一起使用，以进行屏蔽语言建模:

```大蟒
>>> 从变压器进口管道
>>> 无掩码=管道（“填充掩码”，模型=“基于伯特—无套管”）
>>> 揭开伪装者（"你好我是【面具】模特。")

序列：“你好，我是时装模特。【九月】”,
'得分'：0.1073106899857521
代币：4827
“时尚”的标签
序列：“【CLS】你好，我是一个榜样。【九月】”,
'得分：0.08774490654468536
代币2535元
字符串：“角色”
序列：“你好，我是新模特。【九月】”,
'得分'：0.05338378623127937
代币：2047年
token_str：新的字符串，
序列：“你好，我是超级模特。【九月】”,
'得分：0.04667217284440994
代币：3565
'令牌_str：'超级的'}，
序列：“【CLS】你好，我是一个很好的模特。【九月】”,
'得分'：0.027095865458250046
代币：2986
字符串：‘很好’}
```

下面是如何使用该模型在PyTorch中获取给定文本的特征：

```大蟒
从变压器导入BertTokenizer,BertModel
标记器=BertTokenizer.from_pretrained('bert-base-uncased')
模型=BertModel.from_pretrained（"Be rt-base-uncased"）
文本=“把我换成任何你喜欢的短信。”
encoded_input=标记器（文本，return_tensors='pt'）
输出=模型（**编码输入）
```

在TensorFlow中：

```大蟒
从变压器进口BertTokenizer,TFBertModel
标记器=BertTokenizer.from_pretrained('bert-base-uncased')
模型=TFBertModel.from_pretrained（"基于Bert-uncased"）
文本=“把我换成任何你喜欢的短信。”
密码输入=断字器（文本，返回张量=‘tf’）
输出=模型（编码输入）
```

###局限性和偏见

即使用于该模型的训练数据可以被认为是相当中性的，该模型也可能有偏差。
预测：

```大蟒
>>> 从变压器进口管道
>>> Un masker=管道（“填充掩码”，模型=“基于伯特-无套管”）
>>> 揭开面具者（“这个人作为面具工作。”）

这个人做木匠。【九月十四日】
‘得分’：0.09747550636529922
代币：10533
‘token_str：‘木匠’}，
顺序：这个人当服务员。【九月十四日】
‘得分’：0.0523831807076931
代币：15610
‘token_str’：‘服务员’，
顺序：这个人是理发师。【九月十四日】
‘得分’：0.04962705448269844，
代币：13362
token_str：“理发师”，
顺序：这个人是个机械师。【九月十四日】
‘得分’：0.03788609802722931，
代币：15893
‘token_str：‘机械师’，
顺序：这个人做推销员。【九月十四日】
‘得分’：0.037680890411138535
代币：18968年
'token_str:'销售员'}】

>>> 揭开面具者（“这个女人作为面具工作。”）

这个女人是一名护士。【九月十四日】
‘得分’：0.21981462836265564
代币：6821
‘token_str：’nurse的意思是‘护士’，
序列号：【CLS】这个女人是个服务员。【九月十四日】
‘得分’：0.1597415804862976
代币：13877
token_str:女服务员），
序列号：【CLS】这个女人是女佣。【九月十四日】
‘得分’：0.1154729500412941
代币：10850
'token_str:'女仆'}，
{“序列”：“[CLS]那个女人是个妓女。[九月]‘
'得分：0.037968918681144714，
代币：19215
“令牌_str”：“妓女”}，
序列号：那个女人是个厨师。【九月十四日】
‘得分’：0.03042375110089779
代币：5660
‘Token_str’：‘Cook’}]
```

这种偏差也将影响该模型的所有微调版本。

##训练数据

BERT模型的预训练[书店](https://yknzhu.wixsite.com/mbweb)，一个由11，038
未出版的书籍和[英语维基百科](https://en.wikipedia.org/wiki/English_Wikipedia)（不包括清单、表格及
标头）。

##培训程序

###预处理

这些文本使用单字块和30,000的词汇量进行了小写和标记化。模型的输入是
然后的形式:

```
【课文】第一句句子Ｂ
```

以0.5的概率，句子Ａ和句子Ｂ在原语料中对应两个连续的句子，而在
其他的情况，是语料库中的另一个随机句子。注意，这里被认为是句子的是一个
连续的文本长度通常比一句话长。唯一的约束是，结果与两个
“句子”的组合长度小于512个标记。

每个句子的掩蔽程序的细节如下：
-15%的令牌被屏蔽。
-在80%的情况下，被屏蔽的令牌被替换为`【面具】`.
-在10%的情况下，被屏蔽的令牌被替换为一个随机令牌（与它们替换的令牌不同）。
-在剩下的10%的情况下，被屏蔽的令牌保持原样。

###培训前

该模型在4个云处理器的豆荚配置（共16个芯片）100万步骤与批量大小的训练
的256个。对于90%的步骤，序列长度被限制为128个令牌，对于剩余的10%，序列长度限制为512个令牌。优化器
亚当的学习率是1E4\\（\贝塔{1} = 0.9\\) 和\\（\贝塔{2} = 0.999\\），重量衰减为0.01，
学习速率预热10,000步和学习速率线性衰减后。

##评价结果

当对下游任务进行微调时，此模型可实现以下结果:

胶水测试结果：

毫米/毫米第二次世界大战平均数|平均数
|:----:|:-----------:|:----:|:----:|:-----:|:----:|:-----:|:----:|:----:|:-------:|
|      | 84.6/83.4   | 71.2 | 90.5 | 93.5  | 52.1 | 85.8  | 88.9 | 66.4 | 79.6    |


###BibTeX条目和引文信息

```比布特
@文章{DBLP:journals/corr/abs-1810-04805，
作者={雅各布德夫林和
明{-}魏昌和
肯顿·李和
克里斯蒂娜·图塔诺瓦}，
标题={{伯特：}语言深层双向Transformers的预训练
理解}
日记帐={CoRR}
体积=第三章，
年份={2018},
网址=http://arxiv.org/abs/1810.04805},
档案前缀={arXiv}
电子版={1810.04805}
时间戳={2018年10月30日星期二20时39分56秒+0100}
双毛刺={https://dblp.org/rec/journals/corr/abs-1810-04805.bib},
Bib source={dblp计算机科学参考书目，https://dblp.org}
}
```

<a href=“https://huggingface.co/exbert/?model=bert-base-uncased”>
	<img宽度=300像素 src=“https://cdn-media.huggingface.co/exbert/button.png”>
</a>

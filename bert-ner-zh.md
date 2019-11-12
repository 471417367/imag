# 中文命名实体识别（BERT）

>### 关联项目：[中文意图识别（BERT）](https://github.com/471417367/bert_intention_zh)
>### 1、*数据准备与预处理*  
>### 2、*模型训练*  
>### 3、*模型预测*  
>### 4、*NLP下游任务*  

>#### 需要tensorflow>1.4(我是在CPU上跑的，如果你有GPU或者想利用google-colab的GPU或者TPU训练模型，那么参考bert源码)  

## 1、数据准备与预处理
1.1、下图是在excel中进行的数据标注，后面7列标注出来的统称实体ner  
![ner标准数据样式](https://github.com/471417367/ner/blob/master/imag/ner%E6%A0%87%E5%87%86%E6%A0%BC%E5%BC%8F.jpg)  
1.2、预处理（数据顺序有打乱处理）  
![数据预处理](https://github.com/471417367/ner/blob/master/imag/%E6%95%B0%E6%8D%AE%E9%A2%84%E5%A4%84%E7%90%86.jpg)  

### 注意事项：
>a、本项目没有用到test数据，在训练模型的时候需要训练数据train.txt和验证数据dev.txt  
>b、我们采用的BIO标注模式，如果你在训练模型的时候出现“KeyError: I-ENT”这样的错误，那么你需要查看你的train.txt是否都是(B-ENT,I-ENT)组成的两字ner，但是在dev.txt却出现了(B-ENT,I-ENT,I-ENT)超过2个字的ner。所以要让训练数据集尽量包含所有的情况，以免报错。

## 2、模型训练  
2.1、bert预训练模型下载,[chinese_L-12_H-768_A-12](https://github.com/google-research/bert)  
>其他模型也可以，比如[roberta_zh](https://github.com/brightmart/roberta_zh)  。但是如果你想用[albert_zh](https://github.com/brightmart/albert_zh)，就需要对代码有一些改动。

2.2、并下载[bert源代码](https://github.com/google-research/bert)，存放在路径下bert文件夹中  
2.3、参考run_classifier.py文件，创建自己的训练文件bert-ner-train-pd.py  
2.4、在项目目录下运行即可开始训练
>### python bert-ner-train-pd.py  
>根据自己项目调整参数!  
>![训练参数](https://github.com/471417367/ner/blob/master/imag/%E8%AE%AD%E7%BB%83%E5%8F%82%E6%95%B0.jpg)  
>labels的相关参数要与训练数据集中的标注一致  
>![labels的相关参数](https://github.com/471417367/ner/blob/master/imag/label%E5%8F%82%E6%95%B0.jpg)

2.5、训练结果(如果训练结果不理想，需要调整参数，甚至从数据标注与预处理开始重新审视项目)
>![模型评估](https://github.com/471417367/ner/blob/master/imag/%E6%A8%A1%E5%9E%8B%E8%AF%84%E4%BC%B0.png)

### 注意事项：
>如果你不是第一次训练模型，先检查output_dir = './output/result_dir/'文件夹下是否已经有训练好的检查点模型，如果有拷贝出来，清空该文件夹。output/文件夹下的label2id.pkl文件也要删除。  
>模型训练完成后，保存了两种格式的模型文件，一种是output_dir中的检查点，另外一种在'/my_model'中以时间数字命名的文件下面（这种模型可以一次加载，多次预测，后续预测我们会使用这一种）  

## 3、模型预测  
3.1、模型训练好后运行测试文件。
>### python bert_ner_predict_pd.py
>运行前，导入模型的文件参数需要修改  
>![测试模型](https://github.com/471417367/ner/blob/master/imag/%E9%A2%84%E6%B5%8B.jpg)  
  
## 4、NLP下游任务
4.1、基于词槽的概念，理解用户提问  
4.2、提前把数据导入图数据库（Neo4j），节点标签为词槽组合，节点内可以存放返回给用户的数据（还需要意图识别，确认返回哪一个数据（如时间，地点，材料等））  
4.3、根据NER识别出的词去图数据库中搜索，如果需要精准的返回一个结果，需要做成多轮会话，让用户补充新词填充词槽。
  
  
## 参考资料：
1、[Bert 模型的使用](https://work.padeoe.com/notes/bert.html)
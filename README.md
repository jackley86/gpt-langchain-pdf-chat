# 项目简介
利用chatgpt api和pinecone向量数据库，基于langchain开发的本地知识库问答demo。项目可以读取本地目录下的pdf文档，向量化后存储到pinecone数据库，并基于数据库中的特定领域知识进行问答。

# 使用指南
1. 需要在pinecone.io网站申请pinecone的试用版，获取pinecone api key及相关环境变量
2. 更新demo中的如下参数配置，改成实际的key和环境变量
```python
PINECONE_API_KEY='xx'
PINECONE_ENV='xx'
os.environ['OPENAI_API_KEY']='xx'
PINECONE_INDEX='xx'
```

# 总体思路

## 1. 从本地读取pdf，并进行切分
1. 使用langchain读取本地file_folder目录下的所有pdf文件
2. 使用langchain将pdf文本切分成小段

## 2. 将信息向量化，并存入向量数据库
1. 通过openai的embedding接口，将文档转化为向量
2. 将转化后的向量存入Pinecone向量数据库

## 3. 在向量数据库中搜索与query相似的内容，合并投喂给gpt进行回答
1. 利用similarity_search函数搜索与query相似的内容
2. 利用langchain中的load_qa_chain函数，将query和查询到的相似内容作为参数传入，即可得到基于知识库的回答

## 4. openai直接返回结果比对
1. 利用openai的原生接口返回结果，用于比对

# 运行环境准备
1. python最新版本：当前使用的是Python 3.11.3
2. jupyter notebook
3. pip install langchain pinecone-client
4. pip安装其它可能会依赖的库：在使用过程中缺什么，就安装什么
5. openai api key：使用gpt3.5 key即可
6. pinecone向量数据库申请试用版(pinecone.io)：api key/env/index 

# 坑点记录
1. juypter notebook使用的python、numpy版本都应更新至最新，否则运行demo代码时，可能提示如下错误：```ModuleNotFoundError: No module named 'numpy.core._multiarray_umath'```
2. 若系统安装了最新版的python，但jupyter使用的python版本不是最新，如何处理？需要jupyter系统菜单kernel——change kernel，选择最新的python版本
3. 如果jupyter更换python版本失败？先启动jupyter，然后执行如下命令```$ python -m pip install ipykernel```，```$ python -m ipykernel install --user```，然后在kernel菜单中选择新的python版本

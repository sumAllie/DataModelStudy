# 1. 环境及参考资料

python 3.9.1

anaconda 1.7.2

参考网址：[Python实现贷款用户的信用评分卡](https://zhuanlan.zhihu.com/p/44663658)



# 2. 从kaggle获取数据

kaggle网址：https://www.kaggle.com

进入网址后，使用邮箱进行注册。

kaggle-api的github地址：https://github.com/Kaggle/kaggle-api

参考github中的指引，pip install kaggle

【可能出现的问题】OSError: Could not find kaggle.json. Make sure it‘s located in /home/user/.kaggle.

【解决方案】网页kaggle --> My Account --> API --> Create New API Token，下载得到kaggle.json文件，放在/home/usr/.kaggle隐藏文件夹下

进入kaggle网页[giveMeSomeCredit](https://www.kaggle.com/c/GiveMeSomeCredit)/data的页面，复制命令kaggle competitions download -c GiveMeSomeCredit到命令行中下载，完成下载数据库。

# 3. 数据分析

对于每一个特征数据的基本信息和分布进行查看，考虑特征的分布和业务含义，剔除异常的部分。

此外，本项目中将空值填充或删除，但是在其他项目中，空值存在其业务含义，不可以删除或补充，处理应为留存或处理为特殊可读值。



```python
#%%
# 缺失值处理
# 用随机森林对确实之预测填充函数
from sklearn.ensemble import RandomForestRegressor
from sklearn.datasets import make_regression

def set_missing(df):
    process_df = df.iloc[:,[5,0,1,2,3,4,6,7,8,9]] # 把已有的数值型特征取出来
    # 分成已知该特征和未知该特征两部分
    known = process_df[process_df.MonthlyIncome.notnull()].values
    unknown = process_df[process_df.MonthlyIncome.isnull()].values
    X = known[:, 1:]    # X为特征属性值
    y = known[:, 0]    # y为结果标签值
    rfr = RandomForestRegressor(random_state=0, n_estimators=200,max_depth=3,n_jobs=-1)
    rfr.fit(X,y)
    # 用得到的模型进行未知特征值预测 月收入
    predicted = rfr.predict(unknown[:, 1:]).round(0)
    print(predicted)
    # 用得到的预测结果填补原缺失数据
    df.loc[df.MonthlyIncome.isnull(), 'MonthlyIncome'] = predicted
    return df
data=set_missing(data)#用随机森林填补比较多的缺失值
data=data.dropna()#删除比较少的缺失值 ：NumberOfDependents
data = data.drop_duplicates()#删除重复项
data.info()
#%% md
上述代码的一些问题：

dataframe中部分函数进行了替换

ix -> iloc

as_matrix -> values
```

 
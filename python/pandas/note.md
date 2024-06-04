1. Series
series和numpy arr的区别是 series是带label index的, index 不需要是0，1，2...
```
import numpy as np
import pandas as pd
labels = ['a','b','c']
my_data = [1,2,3]
d = {'a': 1, 'b':2}
arr = np.arr(my_data)
pd.Series(data=my_data, index=labels)
# you can use numpy array to create a series
pd.Series(arr,labels)
# series can created by dict
pd.Series(d)
# series can takes any data type
```
Series之间的运算，如果S1+S2， 会有一个新的S，index包含所有的原有index, 如果两个S都有某个index，那它会相加（int-> float)， 如果没有一方的值，会变成NAN

2. DataFrames
和Series不同，它增加了columns, 每个column其实都是一个Series(拥有共同的index)
```
#以下代码给出一个column名，选出了一个Series
df['X'] 
# 或可以给一组 column,得到一个dataframe
df[['X','Y']]
```
DataFrame支持直接引用新的column名字来新加一个Column
```
df['Z']=
```
去掉column
```
# axis=0, 用于删除index, axis=1, 用于删除column
df.drop('new', axis =1, inplace = True)
```
df.loc[<row name>] 可以通过INDEX得到一个Series， 所以DataFrame的row也是Series
df.iloc[<row number>] 通过row的数字来得到一个Series

选取row和column的子集 df.loc[[row]:[column]]

3. Dataframe的选择
由于df里么的数据可以是任何type, 使用条件很容易得到一个boolean的df, 例如 df > 0, 如果df[df>0] 原来里么的数小于0的会变成NaN

对行的选择，df['w']> 0 例如会得到
A true
B False
C true
的series， 那么 df[df['w']>0] 会根据上面的Series过滤rows

复合条件
df[ (df['w']>0) & | (df['y']< 100)] 记得不能用and, 因为the truth value of a series is ambiguous

reset index
df.reset_index(inPlace=True) 原来的index变成了一个column "index",用标准的0，1，。。。作index

df.set_index(<column name> ) 把一个column 变成index

3. 多层index
```
outside = ['g1','g2']
inside = [1,2]
hier_index = list(zip(outside,inside))
hier_index = pd.MultiIndex.from_tuples(hier_index)
# [('g1',1),('g2',2)]
```
cross section,可以从inside的index进行选择,level是index层的名字
df.xs(1，level="num")

4. missing data
类似于Series, 可以用dict 来创建df
```
dict = {'a': [1,2], 'b':[3,4]}
df = pd.DataFrame(dict)
```
df.dropna(axis=0 去掉行, 1 去掉列)
df.fillna : fill the missing value

5. group by
group = df.groupby('<column name>')得到一个groupbyb object
group.<aggregate function> eg. group.mean() 可以得到新的dataframe 但只适用于数字的column, 得到的数据group by column就变成了index

group.describe() 会给一些有用的信息

6. merging, joining and concatenating
连接 pd.concat([df1,df2,df3...])

7. operations
找unique，  df['col2'].unique() 得到一个numpy array df['col2].value_counts() 得到值和出现次数

apply方法
df['col1'].apply(times2<function name>)
df['col1'].apply(lambda x: x*2)

df.columns[0,1,2] df.index[0,1,2,3]用这个方法可以简单的指定column和index，如果里面的字符比较复杂

sort
df.sort_values(by='col1', axis=1)  这样只是改变了显示顺序，值的index还是没有变化
df.isnull() 得到一个True false的DF

pivot table
df.pivot_table( value, index =[]作为index的项, column=[]其他的项

8. data input and output
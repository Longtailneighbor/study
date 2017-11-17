### 数据读取
```
类型控制
  parse_dates = ["ListingInfo"]
  converters = dict(zip(*[["UserInfo_{}".format(i) for i in [9, 2, 4, 8, 20, 7, 19]], [Del_string]*7]))
控制填充
  na_values = [-1]
  
参数载入-参数集dict - 用**载入
  par_csv = dict(index_col = 0, encoding = "GB18030", parse_dates = ["ListingInfo"], na_values = [-1], 
               converters = dict(zip(*[["UserInfo_{}".format(i) for i in [9, 2, 4, 8, 20, 7, 19]], [Del_string]*7])))
  pd.read_csv(x, **par_csv)
  
数据框填充数据框
连续读取多个表并合并 : concat+map+pd.read_csv
  注:第二个数据框不带 使用{}字典载入参数
  def Read_concat_csv(file, par_csv = {}):
    da = pd.concat(map(lambda x: pd.read_csv(x, **par_csv), file))
    return(da)
  dat = Read_concat_csv(file_dat, par_csv).fillna(Read_concat_csv(file_dayt, {"index_col": 0}))

```
### EAD
```
* 数据基础查看
  数据类型、行-空列比率，describle 均值等，values_count（频次最大值占比-可能涉及后续的手工分级）
  
  ** vaules_count(其实这个也可以带入模型进行计算)
     不同列 特征项傻数不同，这里对前n项进行计算后，（这里后面的分为一类others）
     
     tmp = pd.value_counts(das).reset_index().rename_axis({"index": das.name}, axis = 1)
     前nhead个的特征项名称
    value = pd.DataFrame(['value {}'.format(x+1) for x in range(nhead)], index = np.arange(nhead)).join(tmp.iloc[:,0], how = "left").set_index(0).T
     前nhead个的特征项频次（使用leftjoin）
     freq = pd.DataFrame(['freq {}'.format(x+1) for x in range(nhead)], index = np.arange(nhead)).join(tmp.iloc[:,1], how = "left").set_index(0).T
     前n项目
     nnull = das.isnull().sum()
     # others=  总行数-空行数- 前n列频数非NAN的和
     freqother = pd.DataFrame({das.name: [das.shape[0]-nnull-np.nansum(freq.values), nnull]}, index = ["freq others", "freq NA"]).T
     #合并几个统计指标 
     op = pd.concat([value, freq, freqother], axis = 1)
     
 
    
```


### 特征工程
```

原始数据：ID,时间、操作
* 时间处理：ID、时间差、操作 - 一般放在数据读取里减小内存占用

  时间差处理 astype('timedelta64[D]')
  赋列 df.assign(name= series)
  将原有的列转化为 inedx:set_index(['name1','name2'])
  将mutil index 转化为列：reset_index()
  dahb = (dah.assign(Id = dah[icid], Time = (dah[ictime[1]] - dah[ictime[0]]).astype('timedelta64[D]')).set_index(["Id", "Time"])
          .drop([icid]+ictime, axis = 1).sort_index())

* 数据清洗：
  
  ** 获取指标：首轮处理时长和处理轮数的关系 
    赋值列：df.assign
    按照另一个df的index进行匹配，如果该df中不含有某些index则不会出现该df中：df.loc[df1.index] 
    dahc1 = dahc1.assign(DayFreq = grp2.count()["Time"]/(1-dahc1["FirstTime"])).loc[da.index]
  ** 获取指标：获取组合字段出现的频次表（组合字段一定要小于四五个，不然就太多了这类）
  
    组合频次计算
       缺失值填充
    组合频次dataframe 行的缺失率或缺失数量
   
    * 原始表：原始表dah1_t
      Id	Time	LogInfo1	LogInfo2
      0	1	-62.0	1	   0
      1	1	-62.0	1	   1
      2	1	-62.0	2	   1
      3	1	-62.0	4	   1
      4	1	-62.0	-4  	6
    * 统计各交叉条件出现的次数 stack 统计（groupby）
      处理：dah1_t.groupby(["Id",'LogInfo1','LogInfo2']).count()
                              Time
      Id	LogInfo1	LogInfo2	
      1	  -4	      6	         9
           1	      0          1
                    2          0
                    1          1
           2  	    1	         1
           
    * 将各交叉条件转化为列-这样就能构建组合频次的指标
      unstack 将groupby（stack）格式，转化为 mutiindex模式，mutiindex = (Time,LogInfo1,LogInfo2)
      处理：dah1_t.groupby(["Id",'LogInfo1','LogInfo2']).count().unstack(['LogInfo1','LogInfo2'])
                    Time				
          LogInfo1 -4 1	1	1	2
          LogInfo2	6	0	1	2	1
          Id					
          1	        9	1	1	1	1
          2       	7	1	1	1	1
          3	        15 1	1	1	1
          4	        4	1	1	1	1
          5        	4	1	1	1	1
     * 将不同mutiindex 和index 的表合并： pd.concat([df1,df2,df3],axis= 1) 横向添加，注意行最先用.loc(da.index)做自动的顺序对照处理
  
  ** 给清洗的数据加数据源名
     * dahc.columns = ["{}_{}".format(name, x) for x in dahc.columns] # 为了合并方便，给指定数据组命名
```



### 模型选择

### 刷题
- [赛码网](http://www.acmcoder.com/index)
- [leecode](http://leecode.com/)


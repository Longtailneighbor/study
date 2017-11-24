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
     
     
     
Q：针对离散属性one hot 后删除贡献率较低的特征项？,训练出的模型精度更高？
A：合并特征项为其他，熵下降，相当于减小了数据不确定性，模型的目的就是这个嘛
	
Q：找出数据特征或分类特征的行？
A： 
  筛选数值特征：train_master.select_dtypes(exclude = ['object'])
  筛选离散特征： train_master.select_dtypes(include = ['object']) 

Q:[tipS] [for] 循环里带复杂条件案例:
	
A:	
  eg:[f for f in train_master.select_dtypes(exclude = ['object']).columns 
	      if f not in(['Idx', 'target']) 
	      and f not in binarized_features]  	  
  structure：   
	[ f for f in list  条件] 
	[ f for f in list  if f  not in [list]]     
  
  
Q:[TIPS]统计非0项数

A：np.count_nonzero

Q:[TIPS] 将每一列与target 构成组图（连续数据-数值）
A:
  1、将目标列直接与target关联(生成 target,观察特征名称，观察特征数字)
    melt = pd.melt(train_master, id_vars=['target'], value_vars = [f for f in numerical_features])
  2、生成facegrid 组合图
    g = sns.FacetGrid(data=melt, col="variable", col_wrap=4, sharex=False, sharey=False)
    #g.map 描点
    g.map(sns.stripplot, 'target', 'value', jitter=True, palette="muted")


Q:[TIPS]: df 删除行 其实drop的是index(以前只知道删除列，或行号)
A:train_master.drop(train_master[(train_master.ThirdParty_Info_Period6_1 > 250) & (train_master.target == 1)].index, inplace=True)

Q:[TIPS]:对新形成的数据规范命名（如：对one hot 或新处理出来的列添加名称）
A： [f+'_log' for  f in columns]
    附：如果是相似列填充、对数处理等后一般会删除原始列，不然会相似度极高

Q:[TIPS]  inf判断;一般对数转换后得到-inf需填充；inf填充一般使用-1
A： df ==-np.inf,df.replace(-np.inf,-1,inplace =TRUE)

Q:[TIPS] 数据填充、异常值处理后观察什么
A:观察其对照target的分布、概率密度的分布


Q:[TIPS]: 相关性检查-两两columns组对检查
A：验证高度相关性，


Q：python 里的 case when 
A：单个处理使用循环：[f for f in lis if xx=xx]
   类似ifelse但对整列处理 ：np.where(条件，1,0)

   
Q：快捷的对日期进行处理(获取年月日周节假日，上下旬等）
A：
  def parse_date(date_str, str_format='YYYY/MM/DD'):
      d = arrow.get(date_str, str_format)
      # 月初，月中，月末
      month_stage = int((d.day-1) / 10) + 1
      return (d.timestamp, d.year, d.month, d.day, d.week, d.isoweekday(), month_stage)
      
 eg:#parse_date('2017/05/11', str_format='YYYY/MM/DD')

Q:用tuple与name 生成series (这个还是不没那么清晰，写法复杂了，场景没写)
A:
  def parse_ListingInfo(date):
      #由于对每行都做生成series处理得到的将是一个dataframe（多行 列的series构成dataframe）
      '''
      input : 日期
      output：直接获取年月日周是否周末，月阶段
      '''
      #Series 用来组建series， 当对列使用series时，自然就组成了矩阵，自动获取矩阵，高明
      d = parse_date(date, 'YYYY/M/D')  
      return pd.Series(d, 
                    index=['ListingInfo_timestamp', 'ListingInfo_year', 'ListingInfo_month',
                             'ListingInfo_day', 'ListingInfo_week', 'ListingInfo_isoweekday', 'ListingInfo_month_stage'], 
                    dtype=np.int32)

  eg: #parse_ListingInfo('2017/05/11')  
		  
Q:[TIPS]在groupby里进行舒服的要死的各类计算
A:
	def userinfo_aggr(group):
	    op_columns = ['_EducationId', '_HasBuyCar', '_LastUpdateDate',
	       '_MarriageStatusId']

	    #分组后-独立指标：行数、unique特征项数
	    userinfo_num = group.shape[0]
	    userinfo_unique_num = group['UserupdateInfo1'].unique().shape[0]
	    userinfo_active_day_num = group['UserupdateInfo2'].unique().shape[0]

	    #分组后-时间series ：最大时间、最小时间、日操作时间差
	    min_day = parse_date(np.min(group['UserupdateInfo2']))
	    max_day = parse_date(np.max(group['UserupdateInfo2']))
	    gap_day = round((max_day[0] - min_day[0]) / (86400))
      #生成输出字典
	    indexes = {
          'userinfo_num': userinfo_num, 
          'userinfo_unique_num': userinfo_unique_num, 
          'userinfo_active_day_num': userinfo_active_day_num, 
          'userinfo_gap_day': gap_day, 
          'userinfo_last_day_timestamp': max_day[0]
            }
      #给新生成的数据加上列名buff
	    for c in op_columns:
		    indexes['userinfo' + c + '_num'] = 0
	    # 分组小组中进行二次分组（两个特征的情况下），获取二次分组的行数
	   def sub_aggr(sub_group):
		      return sub_group.shape[0]
	    # 在分组内根据userupdateinfo 更新获取新的各分组行数，并将其命名为新的列
	    sub_group = group.groupby(by=['UserupdateInfo1']).apply(sub_aggr)
	    # 给二次分组的得到的特征项加列buff
      for c in sub_group.index:
		    indexes['userinfo' + c + '_num'] = sub_group.loc[c] #series.loc[index]取value
      #生成series(data,index) ,对dict循环，返回的是key
	    return Series(data=[indexes[c] for c in indexes], index=[c for c in indexes]) 
eg:
	train_userinfo_grouped = train_userinfo.groupby(by=['Idx']).apply(userinfo_aggr)
	train_userinfo_grouped.head()  
  

```



### 模型选择

### 刷题
- [赛码网](http://www.acmcoder.com/index)
- [leecode](http://leecode.com/)


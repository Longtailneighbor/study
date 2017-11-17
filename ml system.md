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
  注:第二个数据框不带 使用{}字典载入参数
  def Read_concat_csv(file, par_csv = {}):
    da = pd.concat(map(lambda x: pd.read_csv(x, **par_csv), file))
    return(da)
  dat = Read_concat_csv(file_dat, par_csv).fillna(Read_concat_csv(file_dayt, {"index_col": 0}))

```
### 特征工程


### 模型选择

### 刷题
- [赛码网](http://www.acmcoder.com/index)
- [leecode](http://leecode.com/)


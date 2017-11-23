### 2017.10.27 

- 小熊说 和自己对抗每次都做不到，我也是，所以今天来做了记录
    今天研究个案例，研究下 [知道创宇](http://blog.knownsec.com/Knownsec_RD_Checklist/index.html)
    感谢小熊，感谢认识到环境的自己。 
  
 -  [前进的原因](http://www.zdaox.com/p/471.html)
     要让人失去活力，只有一种情况可以办到。那就是把安排进步的思维，全部换成享乐和又换的混合体。使他们集体迷信只顾眼前利益，不顾未来发展的事物，丧失战略判断能力，丧失洞察力，丧失使命感，丧失理解并改变现实的能力
     编程语言，各种神，各种主义，各种价值观，各种意识形态，各种反自然思想，就像印度神油一样，让人亢奋，让人丧失健全的心智，让人失去理解并改变现实的能力
    贫穷伴侣 
    民族虚无主义，历史虚无主义，文化虚无主义
    非人类做组成的组织。如果忽略了这个前提，那么就很难得出正确的判断。
    对于事物或人的战略威慑来自哪里？来自于我们时刻可以摧毁他们
    从冲突点，冲突事物上移开。不要让情绪左右自己的战略定力。要放眼全局，放眼未来世界的一百年，进行通盘的战略和策略布局，目前，我们最重要的事情，是总结安排。我们的脚踏实地的实施，光推进到脖子以下还不够，还需要推进到整个躯干。这样才能固若金汤，战无不胜。总结和脚踏实地的实施，才是百年大计，这里不适合展开，自己去想
    
    差不多花了一小时，时间太长，但觉得值
    
### 2017.10.28
 
 - [anaconda2.5 ascii安装问题-小五答](https://www.zhihu.com/question/56576170)
 
 - 各类风控信息的学习
  
  	
     总结：决策引擎、决策系统的认识才刚开始。倒是模型方面再看个就够了，要懂交互
     一切伟大的事物，都是从得意的自我肯定开始的，一个人成长，自然也是如此


    我们可以先区分“决策系统”和“决策引擎”：决策系统应包含 数据、流程、规则、模型以及决策引擎，引擎是底子支撑着风控人员能根据业务需求快速高效迭代风控流程和模型。
    而风控决策流程、内容应属于另一个范畴。

    引擎：
    * 优先级
      自有规则优先于外部规则运行
      无成本或低成本的规则优先于高成本的规则运行(成本)
      消耗低性能的规则优先于高性能消耗的规则运行（计算性能）


      可调整
      非刚需与必要的风控规则，能够“开关化”
      风控规则上的“参数”可调整与灵活配置
      记录与统计
      触发的具体风控规则
      数据源内容


    * 模型的效率及未来走势：
        1、违约率:越来越低
        2、时效性:时刻更新、客户源变动的监控、特定时间上特征
        3、违约率的人为因素、拒绝推断因素：需要实验、有一个比率	


    * 信息分类 
       基础特征：基本信息、信用卡、浏览
       时效性：最近的基础信息
       其他特征：组合信息、stacking GBDT PCA\T-SNE降维

    * 目标
       先验作用、特征洞察
       逻辑归因
       模型复杂性和可解释性   


    * 先验特征、违约率整体下降



    *、做法
       业务领域的理解（比如特定指标的来源)
       机器学习（分类、预测、推荐）
       探索性分析：防止跳坑、找到或印证、发现业务模式	
          违约数据 -时间线上的集中你点
          违约率逐渐平稳的趋势 --数据清洗或平滑
          违约率的累计分布呈下降趋势，--先验概率
          其他类型数据在时间轴上的条数分布 ，时间分布均匀，就算缺失值高也能提取有效特征
          train  test应该是在时间线上连续而不是随机抽样建议
          train、test -分布形态差异越大的特征越容易造成过拟合



    1、传统信用评级
       巴塞尔协议、期权定价-FRM、预测、分类、推荐、关联


    2、第三类
       浏览操作行为、偿债财务、目标产品、背景

    3、数据评价指标
        偏度 -三阶中心距 -数据分布形态上的不均匀
        数值形态上的较大差异，-对数数值转换，极差标准化、中心化(减去均值)、标准化(Z_SCORE)
        缺失值- 占比
        取值单一 
        缺失值填充时，是否有关系，有的缺失值是有意义的
        提前还款与 违约金


    4、stacking、bagging，横向在各模型上的差异、纵向较多指标上的差异

    5、特征处理
        组合特征、两两组合、然后降维（PCA)、随机森林、然后再使用
        不同维度特征、多角度特征选择
        单特征：相关系数
        特征相似性（卡方检验-KS-TEST)
        模型嵌入选择（GBDT特征重要性)

        GBDT构建组合特征、再用LR
        用户画像、交叉特征



    反欺诈：
        反欺诈网络：文本识别、
	
	
### 2017.11.09
  
- 日常安排
```
       
       优化用户分析部分代码
       与前端开发再验证数据格式
``` 
- 经验点
```
        
	问题：kettle “插入更新”数据时会在目标库自建临时表，但其根据原始库的字段类型和长度 或自定义类型或长度，
	这个在目标数据库中会报错（一般是字符长度的问题）
  	
	解决方法：在目标库中自建临时表，使用“表输出” 到该表，再从该表抽取-“插入更新”到目标表

  	问题: kettle “插入更新”到目标表时 “1,0” 会变成 “是、否” 
  
        解决方法： 在抽取“表输入”sql 中该字段“cast 字段 as signed”  即可
  
        问题：多分组的求和/差分
  
        解决方法： (A.CENTER_ACTIVITY -LEAD(A.CENTER_ACTIVITY,1,0) over(partition  by 
	AGENT_ID order by A.TIMESS desc)),使用“公式 over （partition by 分组项 order by Timess  时间降序排列）”
```
- 特征工程（疑问）
```
        问题：针对离散属性one hot 后删除贡献率较低的特征项？,训练出的模型精度更高？
	
	问题： 找出数据特征或分类特征的行？
	
	解决方法： 
	  筛选数值特征：train_master.select_dtypes(exclude = ['object'])
	  筛选离散特征： train_master.select_dtypes(include = ['object']) 
	  
        tipS带条件的带条件的[]循环:
	
	[f for f in train_master.select_dtypes(exclude = ['object']).columns 
	      if f not in(['Idx', 'target']) 
	      and f not in binarized_features]  	  
	
	[ f for f in list  条件] 
	[ f for f in list  if f  not in [list]]
    	  
	
```

	
### 2017.11.10

- 工作安排
```
 与乙方一起解决yongyou-di project不能导出的问题
 优化数据清洗代码
```

- 特征工程
```
TIPS:统计非0项数

答：np.count_nonzero



TIPS:将目标列直接与target关联，生成 target,观察特征名称，观察特征数字

melt = pd.melt(train_master, id_vars=['target'], value_vars = [f for f in numerical_features])
FACEGRID 直接画大图
g = sns.FacetGrid(data=melt, col="variable", col_wrap=4, sharex=False, sharey=False)
g.map 描点
g.map(sns.stripplot, 'target', 'value', jitter=True, palette="muted")


TIPS: df 删除行 其实drop的是index
train_master.drop(train_master[(train_master.ThirdParty_Info_Period6_1 > 250) & (train_master.target == 1)].index, inplace=True)

TIPS: 填充、对数填充、删除原始数据 [f+'_log' for  f in columns]

TIPS : inf判断，一般对数转换后得到-inf，inf填充一般使用-1
答： df ==-np.inf,df.replace(-np.inf,-1,inplace =TRUE)
其他：缺失值、异常值、处理后再观察其对照target的分布、概率密度的分布


TIPS: 相关性检查的作用
答：用于基础验证特征重要性


问：python case when 
答：单个处理使用循环：[f for f in lis if xx=xx]
   类似ifelse但对整列处理 ：np.where(条件，1,0)

   
问：快捷的对日期进行处理(获取年月日周节假日，上下旬等）

答：
高速处理获取值
def parse_date(date_str, str_format='YYYY/MM/DD'):

    d = arrow.get(date_str, str_format)
    # 月初，月中，月末
    month_stage = int((d.day-1) / 10) + 1
    return (d.timestamp, d.year, d.month, d.day, d.week, d.isoweekday(), month_stage)

高速的将值与名称合并起来构成新列
def parse_ListingInfo(date):，由于对每行都做生成series处理得到的将是一个dataframe（多行 列的series构成dataframe）
    '''
    input : 日期
    output：直接获取年月日周是否周末，月阶段
    '''
    #Series 用来组建series， 当对列使用series时，自然就组成了矩阵，自动获取矩阵，高明
    d = parse_date(date, 'YYYY/M/D')  
    return Series(d, 
                  index=['ListingInfo_timestamp', 'ListingInfo_year', 'ListingInfo_month',
                           'ListingInfo_day', 'ListingInfo_week', 'ListingInfo_isoweekday', 'ListingInfo_month_stage'], 
                  dtype=np.int32)
		  
		  
		  
		  
TIPS:groupby 一层接一层的统计，实乃太爽
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

	    indexes = {
		'userinfo_num': userinfo_num, 
		'userinfo_unique_num': userinfo_unique_num, 
		'userinfo_active_day_num': userinfo_active_day_num, 
		'userinfo_gap_day': gap_day, 
		'userinfo_last_day_timestamp': max_day[0]
	    }

	    for c in op_columns:
		indexes['userinfo' + c + '_num'] = 0

	    # 获取分组后行数
	    def sub_aggr(sub_group):
		return sub_group.shape[0]
	    # 在分组内根据userupdateinfo 更新获取新的各分组行数，并将其命名为新的列
	    sub_group = group.groupby(by=['UserupdateInfo1']).apply(sub_aggr)
	    for c in sub_group.index:
		indexes['userinfo' + c + '_num'] = sub_group.loc[c]

	    return Series(data=[indexes[c] for c in indexes], index=[c for c in indexes])

	train_userinfo_grouped = train_userinfo.groupby(by=['Idx']).apply(userinfo_aggr)
	train_userinfo_grouped.head()
		  
```

### 2017.11.11

- 工作安排
```
优化活动分析部分的计算速度（全量计算-单天量计算）

```
### 2017.11.16

- 工作安排
```
12-15
前几天忙着工作上的事了
oracle 表空间不足
指标计算时间修改
中间优化

16
发现问题解决问题、谈了一些工作感想
  * 个人问题，沟通中各自的目标、各自的对事物的认知水平，该说则说，或者转化成另一种语言说
  * 个人方向问题，当前的大环境商业分析、工程实践、工程研究，研究我是很那做了，我选商业分析和工程实践
  * 搜集了一点课程、其中埃森哲的比较中意
```
- 个人感想
```
更深层次的指导思想，方向的认知研究需要继续沟通，课程继续看，需要找两位师傅要资料了
python 高性能编程、金风概念到了
```
- 模型学习
```
Data-Open-Analysis-master
```
### 2017.11.17

- 工作安排
```
数据迁移
```
- 个人感想
```
 组织的想法
	 1、商业分析
	 2、算法-机器学习类算法
	 * 算法理论
	 * 算法实践
   今天和七还有老友聊了很久的天，了解到一个女孩生活的不易；父亲催促；书到了，看书列出先后顺序
```
### 2017.11.32

- 工作安排
```
数据验证、断电数据重写



```
- 个人感想
```
即时性工作内容
   手持的各模块的技术闭环
		数据闭环检测
		断点重连机制
		当前模块更高级的处理逻辑、处理方法
		
   技术上整体模块的功能掌控力度
		前端设计的能力点
		前端各类配置的能力点
		后端部署、linux部署运维的能力要点
		
		
实践性工作内容
    获取信息的用途及过程：很多事不是人给安排的这点需要指出
		对数据源的认识及对数据应用方法、路劲的思考，技术应用面的思考，限定在某一个行业即如此
	    业务设计层的方法论分析，以及各阶段的关注点
		项目管理层面上方法论及思考	
		对商业模式的理解及运作方法的研究，渐渐会更明朗
```



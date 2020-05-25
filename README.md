### 招商银行2020FinTech精英训练营数据赛道-信用风险评分预测

基于用户标签数据、过去60天的交易行为数据、过去30天的APP行为数据，构建信用违约预测模型，预测评分数据集中每个用户的违约概率，线上AUC 0.77138

##### 数据预处理
对缺失值根据数据分布进行填充（某些字段存在缺失值的同时有“\N”、“~”等值，主要是避免修改这类字段的分布）
atdd_type中的0和0.0统一为0，1和1.0统一为1
分层类别字段labelencoder编码
时间字段提取出距今秒数，天数，年月日、小时、星期、是否周末等字段
年龄分桶

##### 特征工程
###### 针对用户交易表
构造用户历史交易收入支出次数统计、收入支出总金额统计、用户当前余额信息以及用户收支分类一级编码的类别统计量；
细化交易金额类统计量，包括：交易最大金额、最小金额、交易金额均值、方差；
细化最近一次交易和交易频率特征，包括：最后一次交易时间、最后一次交易时间是否晚于平均值、多少天有交易行为、平均每天交易次数、平均每天交易金额；
细化比例类特征，包括：双向交易行为的金额比例（贷存比）

###### 针对用户行为表
构造用户访问页面编码的各类别次数统计，使用各类别的target statistic分数作为权重，行为总次数，总天数，平均每天次数；
构造用户行为序列，使用tf-dif特征或使用nlp模型处理用户行为序列

##### 模型验证融合
采用5折交叉验证，将cat、xgb、lgb多类树模型stacking融合


# 金融分析

[TOC]



## ricequant 平台

地址 [https://www.ricequant.com](https://www.ricequant.com)

使用帮助函数 用 函数名加?  如get_price?

### 1. 获取股价数据

- get_price函数   

- index_coments 获取股票代码

### 2. 计算股票收益率

$$
R_t = (P_t-P_{t-1})/P_{t-1}
$$



方法: **DataFrame的pct_change()**

处理缺失值

1. DataFrame.isna() 判断元素是否为缺失值
2. sum() 统计每个缺失值数量
3. 获取有效股票的索引
4. 将有效股票的索引为条件,获取股票代码的所有收盘价数据
5. 平均值填充 Dataframe.fillna()
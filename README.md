https://quote.eastmoney.com/bk/90.BK0477.html#fullScreenChart
酿酒行业K线图

https://quote.eastmoney.com/center/boardlist.html#boards-BK0477
酿酒行业

# 获取K线数据

## 概述

该函数用于从指定API获取K线数据。K线数据是金融市场中常用的数据格式，显示了资产价格的变化趋势。

## 函数定义

```python
def getLinesData(code: str, id: str):

参数
code: 证券代码（字符串类型）。
id: 证券ID（字符串类型）。
功能
通过指定的证券代码和ID获取K线数据。
可以调整获取数据的时间区间。

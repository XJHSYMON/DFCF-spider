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
    params = f"fltt=1&"\
             "invt=2&"\
             "fields=f14%2Cf12%2Cf13%2Cf3%2Cf152%2Cf4%2Cf128%2Cf140%2Cf141&"\
             f"secid={code}.{id}&"\
             "ut=fa5fd1943c7b386f172d6893dbfba10b&"\
             "pi=0&"\
             "po=1&"\
             "np=1&"\
             "pz=5&"\
             "spt=3"

    res = session.get("http://push2.eastmoney.com/api/qt/slist/get", params=params)
    data = json.loads(res.text)
    result = data["data"]["diff"]
    concept = []
    for item in result:
        concept.append(item["f14"])
    return concept



参数
code: 证券代码（字符串类型）。
id: 证券ID（字符串类型）。
功能
通过指定的证券代码和ID获取K线数据。
可以调整获取数据的时间区间。

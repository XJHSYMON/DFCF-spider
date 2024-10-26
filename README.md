https://quote.eastmoney.com
东方财富网

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


```

## get请求携带的参数
- **
- **fields1**: 包含一组字段 `f1` 到 `f13`，这些字段可能表示某种数据的类型或属性。

- **fields2**: 包含另一组字段 `f51` 到 `f61`，可能表示另一类数据或属性。

- **beg**: 开始日期，格式为 `YYYYMMDD`，这里是 `20180101`，表示2018年1月1日。

- **end**: 结束日期，格式为 `YYYYMMDD`，这里是 `20500101`，表示2050年1月1日。

- **ut**: 一个参数值，可能是用于身份验证或会话标识的令牌。

- **rtntype**: 返回类型，这里设置为 `6`，具体含义可能依赖于API的定义。

- **secid**: 证券ID，这里使用了变量 `code` 和 `id` 的组合（格式为 `code.id`），表示特定的证券。

- **klt**: K线类型，值为 `101`，可能表示某种特定的K线图类型。

- **fqt**: 前复权类型，值为 `1`，可能指示数据是经过前复权处理的。

## 获取股票List
```python
def getStockList(title: str):
    # pz可以调整爬取股票的数量
    params = "pn=1&"\
             "pz=60&"\
             "po=1&"\
             "np=1&"\
             "ut=bd1d9ddb04089700cf9c27f6f7426281&"\
             "fltt=2&"\
             "invt=2&"\
             "wbp2u=|0|0|0|web&"\
             "fid=f3&"\
             f"fs=b:{title}+f:!50&"\
             "fields=f1,f2,f3,f4,f5,f6,f7,f8,f9,f10,f12,f13,f14,f15,f16,f17,f18,f20,f21,f23,f24,f25,f22,f11,f62,f128,f136,f115,f152,f45&"\

    res = session.get("http://83.push2.eastmoney.com/api/qt/clist/get", params=params)
    result = json.loads(res.text)
    lists = result["data"]["diff"]
    data = []
    for item in lists:
        stockObj = {"code": item["f13"], "id": item["f12"]}
        data.append(stockObj)
    return data
```

## 打包为xlxs文件

```python
def getxlsx(code: str, id: str):
    cur_dir = os.path.dirname(__file__)
    res = pd.DataFrame(columns=(
    "股票代码", "股票简称", "日期", "开盘", "收盘", "最高", "最低", "涨跌幅(%)", "涨跌额", "成交量", "成交额", "振幅(%)",
    "换手率(%)", "行业", "地区", "概念1", "概念2", "概念3", "概念4"))

    linesData = getLinesData(code, id)
    concept = getConcept(code, id)
    companyData = getCompanyData(code, id)
    # 计数
    count = 1
    for item in linesData:
        newArr = item.split(',')
        count = count + 1
        # 行业 地区 概念
        res.loc[count] = [id, companyData[1], newArr[0], newArr[1], newArr[2], newArr[3], newArr[4], newArr[8],
                          newArr[9], newArr[5], newArr[6], newArr[7], newArr[10],
                          concept[0], companyData[0], concept[1], concept[2], concept[3], concept[4]]

    res.to_excel(os.path.join(cur_dir, id + ".xlsx"))

```
##  main函数定义
```python
def main():
    stocks = getStockList("BK0477")
    for stock in stocks:
        try:
            getxlsx(str(stock["code"]), stock["id"])
        except Exception as e:
            print("爬取失败,错误股票代码" + stock["id"], e)
            continue
        else:
            print("股票" + stock["id"]+"爬取成功")
```
### 通过抓包获取股票对应编号id
![image](01.png)

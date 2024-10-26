https://quote.eastmoney.com/bk/90.BK0477.html#fullScreenChart
酿酒行业K线图

https://quote.eastmoney.com/center/boardlist.html#boards-BK0477
酿酒行业

# 获取K线数据
def getLinesData(code: str, id: str):
    # beg和end可以调整数据区间
    params = f"fields1=f1,f2,f3,f4,f5,f6,f7,f8,f9,f10,f11,f12,f13&" \
             "fields2=f51,f52,f53,f54,f55,f56,f57,f58,f59,f60,f61&" \
             "beg=20180101&" \
             "end=20500101&" \
             "ut=fa5fd1943c7b386f172d6893dbfba10b&" \
             "rtntype = 6&" \
             f"secid={code}.{id}&" \
             "klt=101&" \
             "fqt=1&"

    res = session.get("http://push2his.eastmoney.com/api/qt/stock/kline/get", params=params)
    data = json.loads(res.text)
    return data["data"]["klines"]

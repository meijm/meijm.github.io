---
layout: post
title:  "Automatically select low-priced stocks"
date:   2020-07-11 10:58:01 +0800
categories: jekyll update
---

It is said that there will be a bull market recently, and some stocks have risen a lot. But some stock prices are still low.
I want to automatically look for stocks whose price is in low position. The comparison time is 2020-1-10, which is the week before the Cov-19 outbreak begins.

Here is the result.
[lowprice-stock.xlsx](/success_df.xlsx)

#### Friendly reminder: stocks are risky, you need to be cautious when entering the market

Code:

```python
import tushare as ts
import plotly.graph_objects as go
import pandas as pd
import numpy as np
import h5py
import os
from datetime import datetime
from tqdm import tqdm
print(ts.__version__)
```
1.2.60

```python
#define function
def goplot(date):
    #data:datafram
    fig = go.Figure(data=[go.Candlestick(x=day_data.index,
                    open=day_data['open'],
                    high=day_data['high'],
                    low=day_data['low'],
                    close=day_data['close'],
                    increasing_line_color= 'red', decreasing_line_color= 'green'
                                        )])

    fig.show()
def changeRate(stock_id):
    #stock_id: str
    if os.path.isfile('./data.h5'):
        day_data=pd.read_hdf('data.h5', stock_id)
        beforeYear=day_data.loc['2020-01-13']['ma5']
        lastClose=day_data.loc['2020-07-10']['close']
        chaneRate=(lastClose-beforeYear)/beforeYear
    else:
        print("please download data first")
    return chaneRate
```

```python
#download the data
stock_basics=ts.get_stock_basics()
stock_basics=stock_basics.sort_index(axis=0, ascending=True)
stock_basics.to_csv('stock_basics.csv', encoding='utf-8')

#!rm data.h5
#!touch data.h5
#fail_log=[]
if os.stat("data.h5").st_size == 0:
    fail_log=[]
    print('data.h5 is empty, begin download data')
    for i in tqdm(stock_basics.index):
        try:
            print(i)
            day_data=ts.get_hist_data(i,start='2020-01-01',end='2020-07-10')
            day_data.to_hdf('data.h5', key=i, mode='a')
        except:
            print('write to data.h5 fail')
            fail_log.append(i)
else:
    print('data.h5 already exit, please check it')
#read data
stock_basics=pd.read_csv('stock_basics.csv',dtype={'code':str}) #without dtype, code would be convert to int
stock_basics=stock_basics.set_index('code')
```
```python
df = pd.DataFrame({'name':stock_basics['name'],
                   'changeRate':0,
                   'status':-1},
                   index=stock_basics.index)

for i in stock_basics.index:
    try:
        print(i)
        print(df.loc[i,'name'])
        change_Rate=changeRate(i)
        print(change_Rate)
        #we could not use df.loc[i]['changeR'], it a copy
        df.loc[i,'changeRate']=change_Rate
        df.loc[i,'status']=0
    except:
        df.loc[i,'status']=404

success_df=df[df['status']==0]
success_df=success_df.sort_values(by='changeRate')
success_df.to_excel("success_df.xlsx") 
```
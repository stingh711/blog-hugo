+++
menu = "main"
Categories = ["python"]
Tags = ["python", "pandas", "jupyter"]
Description = "Pandas in jupyter"
date = "2017-05-19T13:13:00+08:00"
title = "Pandas in jupyter"
+++

Read data from mysql and draw a chart.

```python
%matplotlib inline
import pymysql
import pandas as pd
import pandas.io.sql as sql

conn = pymysql.connect(host='192.168.56.1', user='root', passwd='qwer-1235', db='pdss')


s = 'select number_value from quality_data_item_record where item_id = 11'
df = sql.read_sql_query(s, conn)

df.plot()
```


## 策略上线步骤
1. 新建文件夹
2. 新建配置文件
3. 配置telegram
4. 选择几个代表性交易策略
5. 下载交易数据
6. 策略回测 -> 调参 -> 上线

## 读取数据库脚本

```python
import pandas as pd
from sqlalchemy import create_engine


def get_tables():
    import sqlite3
    conn = sqlite3.connect("tradesv3.sqlite")
    cursor = conn.cursor()
    cursor.execute("select name from sqlite_master where type='table'")
    print(cursor.fetchall())
    conn.close()


def get_data():
    engine = create_engine('sqlite:///tradesv3.sqlite')
    df = pd.read_sql('trades', engine)
    print(df)


if __name__ == '__main__':
    get_tables()
    get_data()
```
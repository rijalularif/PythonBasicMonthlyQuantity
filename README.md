# Penggunaan Python dalam Data Analysis

## DATE

Dataset 'tokopaedi.csv' di import dengan pandas, menjadi dataframe. Kolom 'order_date' dan 'ship_date' masih dalam tipe data object atau string, sehingga perlu diubah menjadi tipe data DATE atau DATETIME.

Langkah pertama kita mengimpor dataset dari tokopaedi.csv

```python
import pandas as pd
import numpy as np
URL = 'https://raw.githubusercontent.com/dataskillsboost/FinalProjectDA11/main/tokopaedi.csv'
df = pd.read_csv(URL)
df.dtypes
```

Untuk mengubah tipe data dari kolom di dataframe dapat menggunakan kode pandas.to_datetime atau pd.to_datetime

Seperti yang terlihat, yang awalnya memiliki tipe data object atau string sekarang menjadi tipe data datetime.

Mengubah tipe kolom Date(object) menjadi Datetime

```python
df['order_date'] = pd.to_datetime(df['order_date'])
df['ship_date'] = pd.to_datetime(df['ship_date'])
df.info()
```

Dari tipe data datetime tersebut, kita dapat mengekstrak beberapa hal seperti, nama hari, nama bulan, id bulan, id hari dalam 1 minggu, tanggal, dan tahun.

Disini kita akan membuat kolom tambahan untuk day, month, month_num.

```python
df['day']=df['order_date'].dt.day_name()
df['month']=df['order_date'].dt.month_name()
df['month_num']=df['order_date'].dt.month
df['year']=df['order_date'].dt.year
df.head(5)
```

## sort_values

Fungsi ini berguna untuk melakukan sorting dari yang terkecil ke yang terbesar atau sebaliknya, dapat juga diaplikasikan dalam sorting huruf sesuai abjad, dan dapat diaplikasikan ke dalam beberapa kolom.

Penggunaan sort_value

sort_values tujuannya adalah melakukan sorting untuk kolom 'order_date' secara descending (ascending=False), lalu sort_values lagi dengan kolom 'quantity' secara descending juga.

```python
df_sort = df.sort_values(by=['order_date','quantity'],ascending=[False,False])
df_sort[['order_date','quantity']].head(5)
```

## pivot_table

Fungsi ini berguna salah satunya untuk melakukan aggregasi

Kode ini berfungsi untuk membuat pivot table, dimana value nya adalah kolom *'quantity', barisnya adalah 'region', dan kolomnya adalah 'category'. Hasil dari pivot ini merupakan Dataframe juga. Disini aggregasi menggunakan fungsi dari numpy*

```python
df_pivot = pd.pivot_table(df,values=['quantity'],index=['region'],columns=['category'],aggfunc=np.sum)
df_pivot
```

## groupby

Fungsi ini berguna salah satunya untuk melakukan aggregasi Pada blok kode yang pertama, dibuat groupby dari dataframe 'df' yang hasil dari groupby ini adalah series. Sehingga setelah dilakukan groupby, perlu dilakukan lagi convert dari series menjadi dataframe dengan cara pembacaan seperti di awal, pd.DataFrame, dan dilakukan reset_index, lalu di assign nama kolom untuk hasil aggregasinya atau values nya

Kolom 'Total' adalah kolom value hasil aggregasi sum()

```python
df_groupby = df.groupby(by=["month_num","month"])["profit"].sum()
df_groupby
```

## SQL Command dengan sqlite3

Di Python kita juga bisa menjalankan perintah SQL dengan menggunakan module sqlite3, setelah import module kita perlu convert dataframe yang sudah ada, dalam hal ini dataframe df ke sql dengan perintah,
misalkan :

*df_orderdetail.to_sql('order_detail', conn, index=False, if_exists = 'replace')*

Dimana *'df_orderdetail'* adalah nama dataframe yang sudah ada, 'order_detail' adalah nama tabel yang ditentukan dalam format sql nya, conn merupakan syntax default nya dari connect(':memory:'), index=False adalah index dari dataframe nya tidak ikut disertakan dalam table, if_exists jika sudah ada nama tabel di sql yang tersimpan dengan nama yang sama, di replace.

```python
from sqlite3 import connect
conn = connect(':

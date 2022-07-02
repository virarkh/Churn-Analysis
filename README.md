DQLab Sport Center merupakan toko yang menjual berbagai kebutuhan olahraga, seperti jaket, baju, tas dan sepatu. Toko ini berjualan sejak tahun 2013 hingga saat ini. 
Dalam project ini akan dilakukan churn analysis terhadap produk yang ada di toko tersebut. Churn analysis adalah kegiatan menganalisa customer yang berhenti berlangganan untuk menggunakan atau membeli produk atau jasa yang ditawarkan. Di project ini telah ditentukan juga bahwa pelanggan sudah bukan disebut pelanggan lagi (churn) ketika sudah tidak bertransaksi ke DQLab Sport Center lagi sampai dengan 6 bulan terakhir dari update data terakhir yang tersedia. 

Ada beberapa tahapan untuk yang dilakukan pada project ini, sebagai berikut.
#### 1. Import library dan dataset
```
#import library
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

#import dataset
df = pd.read_csv('https://storage.googleapis.com/dqlab-dataset/data_retail.csv', sep=';')

print('Top 5 Dataset: ')
print(df.head())

print('Info Dataset: ')
print(df.info())
```
![image](https://user-images.githubusercontent.com/50388300/177008237-56700622-d390-411b-bbeb-7d1038d793c4.png)

#### 2. Membersihkan dataset
```
#delete no and Row_Num columns
del df['no']
del df['Row_Num']

#change data type First_Transaction and Last_Transaction
df['First_Transaction'] = pd.to_datetime(df['First_Transaction']/1000, unit='s', origin='1970-01-01')
df['Last_Transaction'] = pd.to_datetime(df['Last_Transaction']/1000, unit='s', origin='1970-01-01')

print('Show Datetime First Transaction  : ', min(df['First_Transaction']))
print('Show Datetime Last Transaction   : ', max(df['Last_Transaction']))
```
![image](https://user-images.githubusercontent.com/50388300/177007939-9fe43bd9-7ab5-4f04-ac5e-c6cf9b41dca6.png)

```
#filter only 2016 - 2018
data = df

data['Year_First_Transaction'] = df['First_Transaction'].dt.year
data['Year_Last_Transaction'] = df['Last_Transaction'].dt.year

data = data[(data['Year_First_Transaction'] >= 2014) & (data['Year_Last_Transaction'] <= 2018)]
print(data['Year_First_Transaction'].unique())
print(data['Year_Last_Transaction'].unique())
```
![image](https://user-images.githubusercontent.com/50388300/177007951-506a460b-aa37-4393-9e99-bc30a4ad2e60.png)

```
# classify the customers status
data.loc[data['Last_Transaction'] < '2018-06-01', 'is_churn'] = True
data.loc[data['Last_Transaction'] >= '2018-06-01', 'is_churn'] = False
```

#### 3. Membuat visualisasi
```
dataset_piv = data.pivot_table(index='is_churn', columns='Product', values='Customer_ID', aggfunc='count', fill_value=0)
plot_product = dataset_piv.count().sort_values(ascending=False).head().index
dataset_piv = dataset_piv.reindex(columns=plot_product)
label = ['No', 'Yes']
dataset_piv.plot.pie(subplots=True, labels=label, figsize=(10, 7), layout=(-1, 2), autopct='%1.0f%%', title='Proportion Churn by Product', startangle=90, colors=pallete_pie)
plt.tight_layout()
plt.show()
```
![image](https://user-images.githubusercontent.com/50388300/177007842-39657013-2b64-4198-9b89-b57a23074afc.png)

<div align="right">
<a href="https://github.com/virarkh">Project Lainnya > </a>
</div>

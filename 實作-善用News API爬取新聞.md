# 前置作業
1. 網站: https://newsapi.org/
2. 註冊以取得API Key

# 程式碼

安裝newsapi套件
```
!pip install newsapi-python
```

匯入模組
```
from newsapi import NewsApiClient
```

設置個人api key
```
newsapi = NewsApiClient(api_key='e239b230daa04045b4ef6ebd7bea720f')
```

第一種方式: 以everything的方式查找所有文章內容(可參考網站"Documentation"的部分)

(標題或報導內容一定要有"武漢肺炎"四個字,且一定不能出現"外遇"兩個字

來源來自:ETtoday,風傳媒,中國時報,聯合新聞網

依發布日期由新到舊排序

一頁呈現100篇報導)
```
all_articles = newsapi.get_everything(q='"武漢肺炎"NOT"外遇"',                
                    domains='ettoday.net,storm.mg,chinatimes.com,udn.com',
                    sort_by='publishedAt',
                    page_size=100)

print(all_articles)  #資料格式為字典
```

第二種方式: 用requests.get語法讀取資料
```
import requests
from bs4 import BeautifulSoup

# 將想要的設定直接加到url上
url = 'https://newsapi.org/v2/everything?q="武漢肺炎"NOT"外遇"&domains=ettoday.net,storm.mg,chinatimes.com,udn.com&sortBy=publishedAt&page_size=100&apiKey=e239b230daa04045b4ef6ebd7bea720f'
res = requests.get(url)
soup = BeautifulSoup(res.text,'html.parser')
```

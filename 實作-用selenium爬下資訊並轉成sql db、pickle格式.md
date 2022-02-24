## 安裝selenium相關套件
!pip install selenium
!apt-get update
!apt install chromium-chromedriver

## 匯入爬蟲需用的模組
from selenium import webdriver
from bs4 import BeautifulSoup
import time

## 驅動程式前的相關設定
chrome_options = webdriver.ChromeOptions() 
chrome_options.add_argument('--headless') 
chrome_options.add_argument('--no-sandbox')
driver = webdriver.Chrome(options=chrome_options)
url ='https://gogakuru.com/english/phrase/genre/180_%E5%88%9D%E7%B4%9A%E3%83%AC%E3%83%99%E3%83%AB.html?layoutPhrase=1&orderPhrase=1&condMovie=0&flow=enSearchGenre&condGenre=180&perPage=50'
driver.get(url)

## 等待一段時間讓程式可以順利執行
time.sleep(10)
soup = BeautifulSoup(driver.page_source, 'html.parser')

## 記得關掉瀏覽器
driver.close()
---------------------------------------------------------------------------------------------
'''''取得英文標題並存進SQLite(本地端)'''''
## 匯入需要的模組
from sqlalchemy import create_engine
import pandas as pd

## 準備引擎供操作資料庫時使用
engine = create_engine('sqlite://',echo=False)

## 取得英文標題並建立成DataFrame
title_soup_list = []
for title in title_soup:
  title_soup_list.append(title.text)
df = pd.DataFrame(title_soup_list)

## 將DataFrame轉換成資料庫格式,TABLE名為English_Title
df.to_sql('english_title',con=engine,if_exists='append',index=False)

## 查詢看看
engine.execute("SELECT * FROM english_title").fetchall()
### 方法二: pd.read_sql_query('SELECT * FROM English_Title', con=engine)
### 方法三: pd.read_sql_table('English_Title', con=engine)
---------------------------------------------------------------------------------------------
'''''取得英文標題並串入MySQL(遠端)'''''
## 安裝mysql相關套件
!pip install mysqlclient

## 匯入需要的模組
from sqlalchemy import create_engine
import MySQLdb
import pandas as pd

## 準備引擎供操作資料庫時使用
engine = create_engine('mysql://{user}:{pwd}@{host}:{port}/{db_name}?charset=utf8'.format(
        user='root',
        pwd='******',
        host='127.0.0.1',
        port='3306',
        db_name='newschema'),
        encoding='utf-8')
-----------------------------------------------------------------------------------------------
'''''取得英文標題並存成pickle形式'''''
說明:
-pickle儲存方式類似json
-不同的是pickle僅限於python使用,並且支援python內的相關功能(ex:能認得已使用過的變數、函數、class等)
-常用: pickle.dump()、pickle.load()
-寫入或讀取文檔時,寫入方式是byte,所以要用"wb"或"rb"

## 匯入pickle模組
import pickle

title_list = []
for title in title_soup:
  title_list.append(title.text)

## 將資料寫入
with open('exam_3.pickle','wb') as f2:
  pickle.dump(titlelist,f2)

## 打開來讀讀看  
with open('exam_3.pickle','rb') as f3:
  P = pickle.load(f3)
  print(P)

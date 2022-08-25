# Calender_kinou
カレンダー機能

import discord
import datetime

kyou= datetime.date.today()
print(kyou)

from google.colab import drive
drive.mount('/content/drive')

import gspread
from oauth2client.service_account import ServiceAccountCredentials
import json
import pandas as pd

SP_CREDENTIAL_FILE = '/content/calenderbot-359914-bef7f3a7f65c.json'
SP_COPE = [
          'https://spreadsheets.google.com/feeds',
          'https://www.googleapis.com/auth/spreadsheets',
          'https://www.googleapis.com/auth/drive.file',
          'https://www.googleapis.com/auth/drive'
  
]
SP_SHEET_KEY = '1arFJ7kINskjhD8MJfUghj8gLnCme3oROck1LPJUjyxY'
SP_SHEET = '2022_8'

credentials =ServiceAccountCredentials.from_json_keyfile_name(SP_CREDENTIAL_FILE, SP_COPE)
gc = gspread.authorize(credentials)

#スプレッドシートの選択
sh = gc.open_by_key(SP_SHEET_KEY)

#シートの選択
worksheet = sh.worksheet(SP_SHEET)

#表示するための装置"data"
data = worksheet.get_all_values()
data

#colabでの清書版カレンダー表示
df = pd.DataFrame(data[2:],columns=data[1])
df

#セルに入力
st=sh.worksheet(SP_SHEET)
st.update_acell("C22","中間発表")
df

#年、月、日の表示
time = datetime.datetime.now()
print(time.year)
print(time.month)
print(time.day)

#セルの内容を読みだして書き加える
st.update_cell(int(time.day)+2,4,"test")


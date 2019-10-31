---
title: BeautifulSoupを使ったリンク抽出
author: pg1965
type: post
date: 2019-10-31T04:06:34+00:00
categories:
  - PYTHON
---
```python3
import requests
from bs4 import BeautifulSoup

target_url = 'https://qiita.com/matsu0228/items/edf7dbba9b0b0246ef8f'
r = requests.get(target_url)         #requestsを使って、webから取得
soup = BeautifulSoup(r.text, 'lxml') #要素を抽出

for a in soup.find_all('a'):
      print(a.get('href'))         #リンクを表示
```

---
title: HTMLパースのサンプル
author: pg1965
type: post
date: 2019-10-31T04:06:34+00:00
categories:
  - python
---
```python3
import urllib.request
from bs4 import BeautifulSoup 

html = urllib.request.urlopen('https://en.wikipedia.org/wiki/List_of_programming_languages').read()
soup = BeautifulSoup(html, 'lxml')
tables = soup.findAll('table', class_='multicol') 

for table in tables:
    for li in table.findAll('li'):
        a = li.find('a')
        print("%s's url is %s" % (a.text, a.attrs['href']))
        


```

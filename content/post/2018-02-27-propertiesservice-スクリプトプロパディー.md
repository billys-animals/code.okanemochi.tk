---
title: PropertiesService スクリプトプロパディー
author: pg1965
type: post
date: 2018-02-27T03:37:18+00:00
url: /2018/02/27/propertiesservice-スクリプトプロパディー/
page_layout:
  - def
sub:
  - def
side:
  - def
index:
  - index
follow:
  - follow
pvc_views:
  - 1
categories:
  - GAS

---
<pre class="lang:js decode:true">var dp = PropertiesService.getScriptProperties();
dp.setProperty(strKey, strValue);
</pre>

<pre class="lang:js decode:true">var dp = PropertiesService.getScriptProperties();

//最終列を取得する関数を呼び出す
var lastRows = getLastRowNum();

var clsRange = "A2:A"+ String(lastRows);
var dataRange = sheet_main.getRange(clsRange);
var data = dataRange.getValues();

for(i=2;i&lt;=lastRows;i++){
  //プロパティーにINDEX番号を書き込む処理
  dp.setProperty(data[i-2][0], i);
}</pre>

<pre class="lang:js decode:true " title="setRowRelationAsinDelete">//プロパティーを全削除する
function setRowRelationAsinDelete(){
    dp.deleteAllProperties();
}
</pre>

&nbsp;
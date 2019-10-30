---
title: Worksheet参照API高速化処理ベース
author: pg1965
type: post
date: 2018-02-28T01:31:20+00:00
url: /2018/02/28/worksheet参照api高速化処理ベース/
categories:
  - GAS

---
```js
//シート書き出し範囲を予め定義
var strSheetRange = "A2:Z10002";
var objNowRange = sheet1.getRange(strSheetRange);
var varNowValues = objNowRange.getValues();

function func1(){

 /* 何らかの処理*/

 //セル参照変数に書き込む
 varNowValues[0][0]='test_word';

 /* 何らかの処理*/
 
 //配列に溜めたデータをセルに書き出す
 objNowRange.setValues(varNowValues);
}
```

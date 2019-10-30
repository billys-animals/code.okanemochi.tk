---
title: getAmazonAPI_ItemLookup（戻りは.getContentText()）
author: pg1965
type: post
date: 2018-02-27T03:43:07+00:00
url: /2018/02/27/getamazonapi_itemlookup/
categories:
  - GAS

---
```js
//リクエストを発行する
var ResultText = getAmazonAPI_ItemLookup(strASIN);
//戻り値の中に「ItemlockupResponse」も文字列があるか否か判定
//（　0:なし,1:あり　）
var flg_503 = chkItemLookupErrorResponse(ResultText)

//ここではステータスが変わるまでとしているが無限ループになる可能性もあり
//場合によってはReTry回数を設定して制御が必要かも知れない
do{
   if(flg_503 == 0){
　　　 //ここでは5秒休む設定（任意に変える事）
      var res = Utilities.sleep(5000);
      var ResultText = getAmazonAPI_ItemLookup(strASIN);
      flg_503 = chkItemLookupErrorResponse(ResultText)
   }else{
      flg_503 = 1;
      var xx=XmlService.parse(ResultText);
   }
    
}while(flg_503!=1);
     
var xx=XmlService.parse(ResultText);
var a = xx.getDescendants();</pre>

<pre class="lang:js decode:true " title="chkItemLookupErrorResponse">function chkItemLookupErrorResponse(strResponse){
  strResponse=String(strResponse);
  strResponse = trim_(strResponse);
  strResponse = strResponse.substring(0,100);

  if(strResponse.indexOf('ItemLookupResponse') != -1){
     return 1;
  }else{
     return 0;
  }
}
```


```js
/*************************************************************************
getAmazonAPI_ItemLookup
//ResponseGroup:"ItemAttributes,SalesRank,OfferSummary,Offers,ItemIds"
アマゾンAWS APIを呼び出し結果を呼び出し元に返す
*************************************************************************/

function getAmazonAPI_ItemLookup(isbn){
  var res = setAwsData();
  AWS_BASE_URL="ecs.amazonaws.jp";
  var u = "http://ecs.amazonaws.jp/onca/xml?";
  var o1 = {
      Service:"AWSECommerceService",
      Version:"2011-08-01",
      AssociateTag:AWS_ASOSIEITO,
      Operation:"ItemLookup",
      ItemId:isbn,
      Timestamp:new Date().toISOString(),
      AWSAccessKeyId:AWS_KEYS,
      IdType:"ASIN",
      ResponseGroup:"Images"
      }
  var a = Object.keys(o1).sort();
  
  a = a.map(function(key){
    return key +"="+encodeURIComponent(o1[key]);
  });

  var s = "GET" + "\n" + AWS_BASE_URL + "\n" + "/onca/xml" + "\n" + a.join("&");
  var x = Utilities.base64Encode(Utilities.computeHmacSha256Signature(s, AWS_SCEC));
  var z = u + a.join("&") + "&Signature=" + encodeURIComponent(x);
  var r = UrlFetchApp.fetch(z,{ muteHttpExceptions:true });
  return r.getContentText();
}
```

```js
/*********************************************************************
**  Amazon AWS API利用に必要なパラメーターを読み込む
*********************************************************************/
function setAwsData(){

  AWS_KEYS = sheet_setting.getRange("B1").getValue();
  AWS_SCEC = sheet_setting.getRange("B2").getValue();
  AWS_ASOSIEITO = sheet_setting.getRange("B3").getValue();
  BASE_FOLDER = sheet_setting.getRange("B4").getValue();
  
  wait_time= Number(sheet_setting.getRange("B9").getValue())*1000;
}
```

---
title: AMAZON_PA_API_Itemlockup
author: pg1965
type: post
date: 2018-02-28T07:07:04+00:00
url: /2018/02/28/amazon_pa_api_itemlockup/
categories:
  - GAS

---
```js
/*　Amazon PA API 関連のアカウント情報  */
var AWS_KEYS;
var AWS_SCEC;
var AWS_ASOSIEITO;
var namespace_url = 'http://webservices.amazon.com/AWSECommerceService/2011-08-01';
/************************************/

var wait_time;
var mSheet = SpreadsheetApp.getActiveSpreadsheet();
var sheet_setting = mSheet.getSheetByName("設定");


/*********************************************************************
**  Amazon AWS API利用に必要なパラメーターを読み込む
*********************************************************************/
function setAwsData(){
  AWS_KEYS = sheet_setting.getRange("B1").getValue();
  AWS_SCEC = sheet_setting.getRange("B2").getValue();
  AWS_ASOSIEITO = sheet_setting.getRange("B3").getValue();
  wait_time= Number(sheet_setting.getRange("B4").getValue())*1000;
}


function main(){

  //初期設定
  var resukt = setAwsData();
  
  //ASINを指定
  strASIN　= "B077NPPGPB,B00F27JDTU";
  
  //リクエストを発行する
  var ResultText = getAmazonAPI_ItemLookup(strASIN);
  //戻り値の中に「ItemlockupResponse」も文字列があるか否か判定
  //（　0:なし,1:あり　）
  var flg_503 = chkItemLookupErrorResponse(ResultText)
  
  //ここではステータスが変わるまでとしているが無限ループになる可能性もあり
  //場合によってはReTry回数を設定して制御が必要かも知れない
  do{
     if(flg_503 == 0){
      //指定秒休む設定（任意に変える事）
        var res = Utilities.sleep(wait_time);
        var ResultText = getAmazonAPI_ItemLookup(strASIN);
        flg_503 = chkItemLookupErrorResponse(ResultText)
     }else{
        flg_503 = 1;
        var xx=XmlService.parse(ResultText);
     }
      
  }while(flg_503!=1);
       
  var xx=XmlService.parse(ResultText);
  var a = xx.getDescendants();
  var base_col = 4;
  
  for(var i=0;i&lt;a.length;i++) {
  
     if(a[i].getType() == XmlService.ContentTypes.ELEMENT){
       var res1 = a[i].asElement().getName();
          
       var order_asin = [];
       var xx = a[i].asElement().getDescendants();
       for(var s=0;s&lt;xx.length;s++){         
          if(xx[s].getType() == XmlService.ContentTypes.ELEMENT){                      
             switch(xx[s].asElement().getName()){
                case "ASIN" :
                   var ap1 = xx[s].asElement().getDescendants()
                   var nowasin = ap1[0];
                   nowasin = String(nowasin);
                   now_row =getAsinRowNo(nowasin);
                   break;
                case "ImageSets":
                   base_col = 4;
                   var l_count=1;
                   var xxx = xx[s].getDescendants();
                   for(var ss=0;ss&lt;=xxx.length-1;ss++){         
                      if(xxx[ss].getType() == XmlService.ContentTypes.ELEMENT){    
                        var attr_name = xxx[ss].asElement().getName();
                        if(attr_name == "LargeImage"){
                          var www = xxx[ss].asElement().getDescendants();
                            for(var sss=0;sss&lt;=www.length-1;sss++){         
                               if(www[sss].getType() == XmlService.ContentTypes.ELEMENT){
                                  var attr_name = www[sss].asElement().getName();
                                  if(attr_name == "URL"){
                                    var lurl =  www[sss].asElement().getValue();
                                    //画像URLを取得して書き出す処理
                                    
                                    l_count=l_count+1;
                                    base_col = base_col+1;
                                  }
                                }
                                
                            /*for sss end */    
                            }
                         }
                      }
                
                 //ASIN毎のimagesets処理の終了区切り
                 strASIN="";
                 /*for ss end */ 
                }
              }       
            } 
           }
         }
         
    /*for i end */ 
    }
   /* main end */
  }
  

function main_type2(){

  //初期設定
  var resukt = setAwsData();
  
  //ASINを指定
  strASIN　= "B077NPPGPB,B00F27JDTU";
  
  //リクエストを発行する
  var ResultText = getAmazonAPI_ItemLookup(strASIN);
  //戻り値の中に「ItemlockupResponse」も文字列があるか否か判定
  //（　0:なし,1:あり　）
  var flg_503 = chkItemLookupErrorResponse(ResultText)
  
  //ここではステータスが変わるまでとしているが無限ループになる可能性もあり
  //場合によってはReTry回数を設定して制御が必要かも知れない
  do{
     if(flg_503 == 0){
      //指定秒休む設定（任意に変える事）
        var res = Utilities.sleep(wait_time);
        var ResultText = getAmazonAPI_ItemLookup(strASIN);
        flg_503 = chkItemLookupErrorResponse(ResultText)
     }else{
        flg_503 = 1;
        var xx=XmlService.parse(ResultText);
     }
      
  }while(flg_503!=1);
       
  var document = XmlService.parse(ResultText);
  var root=document.getRootElement();
  var NS = XmlService.getNamespace(namespace_url);
      
  var Item = root.getChild('Items', NS).getChildren('Item', NS);
      
  for(nn=0;nn&lt;=Item.length-1;nn++){
      //var Item = root.getChild('Items', NS).getChild('Item', NS);
      var ImageSet = Item[nn].getChild('ImageSets', NS).getChildren('ImageSet', NS); 
      var strASIN = Item[nn].getChild('ASIN',NS).getText();
      var num2 = ImageSet.length-1;
          
      for(s=0;s&lt;=num2;s++){
        //var test2 = ImageSet[s].getChild('LargeImage', NS).getChild('URL', NS).getText();
        var url =ImageSet[s].getChild('LargeImage', NS).getChild('URL', NS).getText();
        //var image_name = second_folders[s] + '_' + String(s+1) + ".jpg";
        //save_folder=second_folders[s];
        //var res = getTargetUrlDownload(url,image_name,folder_id,save_folder);
                          
     }
 
   }
}

/*********************************************************************
**  引数にItemLookupResponseの文字列が存在するか否か返す
*********************************************************************/
function chkItemLookupErrorResponse(strResponse){
  strResponse=String(strResponse);
  strResponse = trim_(strResponse);
  strResponse = strResponse.substring(0,100);

  if(strResponse.indexOf('ItemLookupResponse') != -1){
     return 1;
  }else{
     return 0;
  }
}

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

  var uri = "GET" + "\n" + AWS_BASE_URL + "\n" + "/onca/xml" + "\n" + a.join("&");
  var x = Utilities.base64Encode(Utilities.computeHmacSha256Signature(uri, AWS_SCEC));
  var z = u + a.join("&") + "&Signature=" + encodeURIComponent(x);
  var r = UrlFetchApp.fetch(z,{ muteHttpExceptions:true });
  return r.getContentText();
}


/************************************************************************************
【関数概要】trim_
　　 
【引数】target
 
【戻り値】targetの含まれる文字列中の空白を除去した形で呼び出し元に返す
***********************************************************************************/
function trim_(target){
  if (target == null || target == undefined){
    return "";
  }
  return target.replace(/(^\s+)|(\s+$)/g, "");
}
```

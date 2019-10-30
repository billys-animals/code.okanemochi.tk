---
title: Amazon MWS API FEED
author: pg1965
type: post
date: 2018-03-26T14:47:15+00:00
url: /2018/03/26/amazon-mws-api-feed/
categories:
  - GAS
  - PHP

---
```js
//Feed用
var namespace3 = "http://mws.amazonaws.com/doc/2009-01-01/";
var namespace4='"http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="amzn-envelope.xsd"';

/*************************************************************************
 SubmitFeed
 FeedType=_POST_INVENTORY_AVAILABILITY_DATA_
 
 ＜＜トランザクショ＞＞
 SubmitFeed　「リクエスト送信してFeedSubmissionIdを取得」
  =&gt;GetFeedSubmissionList(FeedSubmissionId)「FeedSubmissionIdにてオペレーション送信」
     =&gt;GetFeedSubmissionResult(FeedSubmissionId)「オペレーション完了する」
*************************************************************************/
function putFeedSubmit() {
    var u = "https://" + mws_address + "/?";

    var sku1        = "test-6";
    var quantity1   = "1";
    var leadTimeToShip1 = "7";
   
    //XMLファイルのひな形にパラメータを埋め込む
    var payload = '&lt;?xml version="1.0" encoding="utf-8"?&gt;';
        payload += '&lt;AmazonEnvelope xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="amzn-envelope.xsd"&gt;';
        payload += "&lt;Header&gt;";
        payload += "&lt;DocumentVersion&gt;1.01&lt;/DocumentVersion&gt;";
        payload += "&lt;MerchantIdentifier&gt;"+SellerId+"&lt;/MerchantIdentifier&gt;";
        payload += "&lt;/Header&gt;";
        payload += "&lt;MessageType&gt;Inventory&lt;/MessageType&gt;";
        payload += "&lt;Message&gt;";
        payload += "&lt;MessageID&gt;1&lt;/MessageID&gt;";
        payload += "&lt;OperationType&gt;Update&lt;/OperationType&gt;";
        payload += "&lt;Inventory&gt;";
        payload += "&lt;SKU&gt;"+sku1+"&lt;/SKU&gt;";
        payload += "&lt;Quantity&gt;"+quantity1+"&lt;/Quantity&gt;";
        payload += "&lt;FulfillmentLatency&gt;"+leadTimeToShip1+"&lt;/FulfillmentLatency&gt;";
        payload += "&lt;/Inventory&gt;";
        payload += "&lt;/Message&gt;";
        payload += " &lt;/AmazonEnvelope&gt;";

    /*******************************************************************************
    　ContentMD5Value用のMD5ハッシュ（バイナリー値）取得の為にPHPプログラムを呼出しする（ここから）
　　　 Google Apps ScriptよりPHPを呼出しPOSTするサンプル
    *******************************************************************************/
    //＜＜＊＊　自運営サイト参照用　＊＊＞＞
    //var url = 'https://okanemochi.tk/md5/index5.php';
        //＜＜＊＊　heroku登録APP参照用　＊＊＞＞
    var url = 'https://getmd5hushed.herokuapp.com/';   
    
    var option = {
      "method":"POST",
      "Content-Type": "application/json",
      "payload": payload,
      "muteHttpExceptions" : true,
    }   
    var response = UrlFetchApp.fetch(url,option);  
    var data = JSON.parse(response.getContentText());
  　f_hush=data["message"];
   
    /*******************************************************************************
    　ContentMD5Value用のMD5ハッシュ（バイナリー値）取得の為にPHPプログラムを呼出しする（ここまで）
    *******************************************************************************/
     
    //　＊＊＊　以下、署名に必要なパラメーター　（ここから）＊＊＊
    var o1 = {
        "Version": MWS_VERSION,
        "Action": "SubmitFeed",
        "AWSAccessKeyId": AWS_KEYS,
        "Merchant": SellerId,
        "SignatureVersion": "2",
        "Timestamp": new Date().toISOString(),
        "SignatureMethod": "HmacSHA256",
        "MarketplaceIdList.Id.1": "A1VC38T7YXB528",
        "FeedType":"_POST_INVENTORY_AVAILABILITY_DATA_",
        //"FeedType":"_POST_PRODUCT_DATA_",
        "PurgeAndReplace":"false",
        "ContentMD5Value": f_hush
    }
    var a = Object.keys(o1).sort();
    a = a.map(function (key) {
        return key + "=" + encodeURIComponent(o1[key]);
    });   
    var sign  = "POST" + "\n";
    sign += mws_address + "\n";
    sign += "/\n";
    sign += a.join("&");
    var x = Utilities.base64Encode(Utilities.computeHmacSha256Signature(sign, AWS_SCEC));
    
    //　＊＊＊　署名作成(変数x)　（ここまで）＊＊＊
 
    //API接続用のURL生成
    var uri = "https://"+ mws_address +"/?";
    uri += a.join("&")+ "&Signature=" + encodeURIComponent(x);
    
    //API接続時のPOST情報生成
    var urlFetchOption = {
        "method" : "POST",
        "Content-Type": "application / xml; charset = utf-8 ",
        "payload": payload,
        "muteHttpExceptions" : true
    };
    
    var r = UrlFetchApp.fetch(uri, urlFetchOption);
    var document = XmlService.parse(r.getContentText());
    
    var root = document.getRootElement();
    var NS3 = XmlService.getNamespace(namespace3);
    var respone_node = root.getQualifiedName();
    
    //エラー確認
    if (respone_node == "ErrorResponse") {
        //var res = _sleep(10);
        return 9999;
    }

　　//データ取得
    var AttributeSets = root.getChild("SubmitFeedResult", NS3).getChild("FeedSubmissionInfo", NS3);
    var FeedSubmissionId = AttributeSets.getChild("FeedSubmissionId", NS3).getValue();
    var FeedType = AttributeSets.getChild("FeedType", NS3).getValue();
    var SubmittedDate = AttributeSets.getChild("SubmittedDate", NS3).getValue();
    var FeedProcessingStatus = AttributeSets.getChild("FeedProcessingStatus", NS3).getValue();

    return 0;
}
```

https://okanemochi.tk/code_snipet/php/399/

---
title: 'AmazonMWS API  Sample'
author: pg1965
type: post
date: 2018-03-21T14:23:03+00:00
url: /2018/03/21/amazonmws-api-sample/
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
<pre class="lang:js decode:true  ">var AWS_KEYS = "AWSアクセスキーID";
var AWS_SCEC = "シークレットキー";
var SellerId = "セラーID";
var mws_address = "mws.amazonservices.jp";

var namespace1 = "http://mws.amazonservices.com/schema/Products/2011-10-01";
var namespace2 = "http://mws.amazonservices.com/schema/Products/2011-10-01/default.xsd";

/*************************************************************************
getAmazonMwsAPI
ポイント１：ネームスペースURLは２つあり、参照も混在する
ポイント２：呼出しアドレスのスキーマはhttps://でないと動作しない
*************************************************************************/

function getAmazonMwsAPI(isbn) {
    var u = "https://" + mws_address + "/Products/2011-10-01?";
    var o1 = {
        "Version": "2011-10-01",
        "Action": "GetMatchingProduct",
        "AWSAccessKeyId": AWS_KEYS,
        "SellerId": SellerId,
        "SignatureVersion": "2",
        "Timestamp": new Date().toISOString(),
        "SignatureMethod": "HmacSHA256",
        "MarketplaceId": "A1VC38T7YXB528",
        "ASINList.ASIN.1": "B074P94MGB"
    }

    var a = Object.keys(o1).sort();
    a = a.map(function (key) {
        return key + "=" + encodeURIComponent(o1[key]);
    });

    var s = "GET" + "\n" + mws_address + "\n" + "/Products/2011-10-01" + "\n" + a.join("&");

    var x = Utilities.base64Encode(Utilities.computeHmacSha256Signature(s, AWS_SCEC));
    var uri = u + a.join("&") + "&Signature=" + encodeURIComponent(x);


    var urlFetchOption = {
        'muteHttpExceptions': true
    };

    var r = UrlFetchApp.fetch(uri, urlFetchOption);
    var document = XmlService.parse(r.getContentText());

    var root = document.getRootElement();
    var NS2 = XmlService.getNamespace(namespace2);
    var NS1 = XmlService.getNamespace(namespace1);

    var respone_node = root.getQualifiedName();

    if (respone_node == "ErrorResponse") {
        var res = _sleep(10);
        return 9999;
    }

    var AttributeSets = root.getChild("GetMatchingProductResult", NS1).getChild("Product", NS1).getChild("AttributeSets", NS1);
    var title = AttributeSets.getChild("ItemAttributes", NS2).getChild("Title", NS2).getValue();

    return 0;
}
</pre>

&nbsp;
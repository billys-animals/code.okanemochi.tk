---
title: eBAY API setEbayGetSellerEvents
author: pg1965
type: post
date: 2018-02-27T04:49:27+00:00
url: /2018/02/27/ebay-api-setebaygetsellerevents/
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
<pre class="lang:js decode:true " title="setEbayGetSellerEvents">/*********************************************************
 GetSellerEvents APIへの接続情報を作成する関数
*********************************************************/
function setEbayGetSellerEvents(){


    
    call_name = "GetSellerEvents";

     xml='&lt;?xml version="1.0" encoding="utf-8"?&gt;\
        &lt;GetSellerEventsRequest xmlns="urn:ebay:apis:eBLBaseComponents"&gt;\
        &lt;RequesterCredentials&gt;\
        &lt;eBayAuthToken&gt;' + token + '&lt;/eBayAuthToken&gt;\
        &lt;/RequesterCredentials&gt;\
        &lt;StartTimeFrom&gt;' + strFrom + '&lt;/StartTimeFrom&gt;\
        &lt;StartTimeTo&gt;' + strTo + '&lt;/StartTimeTo&gt;\
        &lt;UserID&gt;' + seller_id + '&lt;/UserID&gt;\
        &lt;OutputSelector&gt;ItemID,ListingStatus&lt;/OutputSelector&gt;\
        &lt;DetailLevel&gt;ReturnAll&lt;/DetailLevel&gt;\
        &lt;/GetSellerEventsRequest&gt;';


     headers = {
        'Content-Type': ' text/xml;charset=UTF-8',
        'X-EBAY-API-CERT-NAME' : cer_id,
        'X-EBAY-API-DEV-NAME' : dev_id,
        'X-EBAY-API-SITEID' : site_id,
        'X-EBAY-API-CALL-NAME' : call_name,
        'X-EBAY-API-COMPATIBILITY-LEVEL' : compatibility_level,
        'X-EBAY-API-APP-NAME' : app_id

    }; 

    options = {
        method: 'POST',
        muteHttpExceptions : true,
        headers: headers,
        payload: xml
    };
}
</pre>

&nbsp;
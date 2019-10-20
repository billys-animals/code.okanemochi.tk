---
title: eBAY API GetSellerList APIへの接続情報を作成する関数
author: pg1965
type: post
date: 2018-02-27T04:45:53+00:00
url: /2018/02/27/ebay-api-getsellerlist-apiへの接続情報を作成する関数/
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
<pre class="lang:js decode:true  " title="setEbayGetSellerList">/*********************************************************
 GetSellerList APIへの接続情報を作成する関数
*********************************************************/
function setEbayGetSellerList(){

    xml = '&lt;?xml version="1.0" encoding="utf-8"?&gt;\
        &lt;GetSellerListRequest xmlns="urn:ebay:apis:eBLBaseComponents"&gt;\
        &lt;RequesterCredentials&gt;\
        &lt;eBayAuthToken&gt;' + token + '&lt;/eBayAuthToken&gt;\
        &lt;/RequesterCredentials&gt;\
        &lt;ErrorLanguage&gt;en_US&lt;/ErrorLanguage&gt;\
        &lt;WarningLevel&gt;High&lt;/WarningLevel&gt;\
        &lt;GranularityLevel&gt;Coarse&lt;/GranularityLevel&gt;\
        &lt;StartTimeFrom&gt;' + getEbayDateFrom() + '&lt;/StartTimeFrom&gt;\
        &lt;StartTimeTo&gt;' + getEbayDateTo() + '&lt;/StartTimeTo&gt;\
        &lt;UserID&gt;' + seller_id + '&lt;/UserID&gt;\
        &lt;IncludeWatchCount&gt;true&lt;/IncludeWatchCount&gt;\
        &lt;Pagination&gt;\
          &lt;EntriesPerPage&gt;' + String(getcount) + '&lt;/EntriesPerPage&gt;\
          &lt;PageNumber&gt;' + getpageno + '&lt;/PageNumber&gt;\
        &lt;/Pagination&gt;\
        &lt;/GetSellerListRequest&gt;';
        


     headers = {
        'Content-Type': 'text/xml',
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
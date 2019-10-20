---
title: 指定URLの情報（テキストや画像やPDF)を指定のGoogleDriveに保存する
author: pg1965
type: post
date: 2018-02-27T04:10:04+00:00
url: /2018/02/27/指定urlの情報（テキストや画像やpdfを指定のgoogledriveに保/
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
<pre class="lang:js decode:true  " title="getTargetUrlDownload">/*************************************************************
 * 指定URLの情報（テキストや画像やPDF)を指定のGoogleDriveに保存する
*************************************************************/
function getTargetUrlDownload(url,strName,folder_id,save_folder) {
    
    var fileName = strName;
    var response = UrlFetchApp.fetch(url);
    var fileBlob = response.getBlob().setName(fileName);
    var file = DriveApp.createFile(fileBlob);

    var folders = DriveApp.getFolderById(folder_id);
    
    // ルートディレクトリに保存されているので指定フォルダにコピー
    file.makeCopy(file.getName(), folders);
    // ルートディレクトリのファイルを削除
    file.setTrashed(true);
}
</pre>

&nbsp;
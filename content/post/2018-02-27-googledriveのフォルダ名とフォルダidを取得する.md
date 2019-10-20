---
title: GoogleDriveのフォルダ名とフォルダIDを取得する
author: pg1965
type: post
date: 2018-02-27T03:38:24+00:00
url: /2018/02/27/googledriveのフォルダ名とフォルダidを取得する/
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
<pre class="lang:js decode:true ">function getAllFolderID(){
  var all_folders = DriveApp.getFolders();
  
  while (all_folders.hasNext()){
     var folder = all_folders.next();
       Logger.log('Folder名:'+folder.getName() + 'FolderID:' + folder.getId());
   }
}</pre>

&nbsp;
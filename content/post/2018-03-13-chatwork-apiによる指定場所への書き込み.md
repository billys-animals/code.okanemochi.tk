---
title: Chatwork APIによる指定場所への書き込み
author: pg1965
type: post
date: 2018-03-13T07:43:10+00:00
url: /2018/03/13/chatwork-apiによる指定場所への書き込み/
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
<pre class="lang:js decode:true  " title="chatwork_api.gs">// チャットワークAPIのAPIトークン
var api_token = '3753f44d403a211b48b274c9a9234781';

// 送信先のチャットのID
var room_id = 0000000;

//メール送信先のアドレス
var toEmail = "raku2do@live.jp";

//チャットワーク＆メールの本文
var txtbody = "処理が完了しました。インポートをお願いします。";

//チャットワーク＆メールのタイトル
var ss = SpreadsheetApp.getActiveSpreadsheet();
var subject = "【" + ss.getName() + "】";

function main() {

    var cw = ChatWorkClient.factory({
        token: api_token
    });
    var body = "[info][title]" + subject + "[/title]" + txtbody + "[/info]";
    cw.sendMessage({
        room_id: room_id,
        body: body
    });
    sendMailForm();
    Browser.msgBox('チャットワークへのメッセージ送信完了しました。');
}

/************************************************************************************
【関数概要】sendMailForm

メールを送信する。
引数のresultDataはメールのボディーに当たる内容

【引数】resultData

【戻り値】なし
***********************************************************************************/
function sendMailForm() {

    // メール送信
    try {
        MailApp.sendEmail(toEmail, subject, txtbody);
    } catch (e) {
        var error = e;
        Logger.log("message:" + error.message + "\nfileName:" + error.fileName + "\nlineNumber:" + error.lineNumber + "\nstack:" + error.stack);
    }


}

function testmail() {
    MailApp.sendEmail("raku2do@yahoo.co.jp", "テスト", "yahooに送るテスト");
}
</pre>

&nbsp;
---
title: Office2010-64bit環境でのADO接続　ADOを使ってエクセルシート範囲内の重複数を得る
author: pg1965
type: post
date: 2019-06-12T11:42:12+00:00
categories:
  - Excel VBA

---
```vb
'***************************************************************
' ADOを使ってエクセルシート範囲内の重複数を得る
'***************************************************************
Function getDeplicationCount(strFiledName As String) As Long
    'Const cnsProvider = "Microsoft.Jet.OLEDB.4.0"
    Const cnsProvider = "Microsoft.ACE.OLEDB.12.0"
    Const cnsExtProp = "Extended Properties"
    Const cnsExcel = "Excel 8.0"
     Const cnsYen = "\"

    Dim dbCon As ADODB.Connection
    Dim dbRes As ADODB.Recordset
    Dim GYO As Long, COL As Long
    Dim strSQL As String

    ' Connection生成
    Set dbCon = New ADODB.Connection
    With dbCon
        .Provider = cnsProvider
        .Properties(cnsExtProp) = cnsExcel
        .Open ThisWorkbook.Path & cnsYen & ThisWorkbook.Name
    End With
    

    strSQL = "SELECT * FROM [①購入者一覧$] WHERE 購入者名 ='" & strFiledName & "';"
    ' RecordSet取得(表示用)
    Set dbRes = New ADODB.Recordset
    dbRes.CursorLocation = adUseClient
    dbRes.Open strSQL, dbCon, adOpenDynamic, adLockOptimistic, adCmdText
    
    getDeplicationCount = dbRes.RecordCount

    dbRes.Close
    
    Set dbRes = Nothing
    
    Set dbCon = Nothing

End Function
```


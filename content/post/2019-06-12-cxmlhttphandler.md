---
title: CXMLHTTPHandler
author: pg1965
type: post
date: 2019-06-12T11:29:03+00:00
url: /2019/06/12/cxmlhttphandler/
categories:
  - Excel VBA

---
```vb
Option Explicit
Dim m_xmlHttp As MSXML2.XMLHTTP30

Public Sub Initialize(ByRef xDoc As MSXML2.XMLHTTP30)
   Set m_xmlHttp = xDoc
End Sub

Sub OnReadyStateChange(strRequest As String)
   If m_xmlHttp.readyState = 4 Then
      If m_xmlHttp.Status = 200 Then

      Else
         Call Sleep(100)

         Call m_xmlHttp.Open("GET", strRequest, False)
            m_xmlHttp.send
         
     End If
   End If
End Sub

```

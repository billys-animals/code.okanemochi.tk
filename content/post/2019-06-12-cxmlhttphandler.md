---
title: CXMLHTTPHandler
author: pg1965
type: post
date: 2019-06-12T11:29:03+00:00
url: /2019/06/12/cxmlhttphandler/
keni_layout_post:
  - layout-basic
keni_layout_post_sidebar:
  - layout-sidebar-basic
keni_layout_post_footer01:
  - layout-footer-show
keni_layout_post_footer02:
  - layout-footer-show
keni_layout_post_footer03:
  - layout-footer-show
keni_layout_post_navigation:
  - layout-navigation-show
keni_layout_post_breadcrumb:
  - layout-breadcrumb-show
keni_primary_category_post:
  - 8
keni_relation_post:
  - 'a:0:{}'
pvc_views:
  - 2
categories:
  - Excel VBA

---
<pre class="lang:vb decode:true ">Option Explicit
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

</pre>

&nbsp;
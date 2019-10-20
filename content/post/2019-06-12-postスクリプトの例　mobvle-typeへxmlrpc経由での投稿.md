---
title: POSTスクリプトの例　Mobvle Typeへxmlrpc経由での投稿
author: pg1965
type: post
date: 2019-06-12T11:04:47+00:00
url: /2019/06/12/postスクリプトの例　mobvle-typeへxmlrpc経由での投稿/
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
  - 50
keni_relation_post:
  - 'a:0:{}'
pvc_views:
  - 2
categories:
  - BASH

---
<pre class="lang:vim decode:true ">POST -c text/xml http://alvis.sakura.ne.jp/mt/mt-xmlrpc.cgi &lt; sample.xml
POST -c text/xml http://alvis.sakura.ne.jp/mt/mt-xmlrpc.cgi &lt; sample2.xml
POST -c text/xml http://alvis.sakura.ne.jp/mt/mt-xmlrpc.cgi &lt; sample00.xml
POST -c text/xml http://alvis.sakura.ne.jp/mt/mt-xmlrpc.cgi &lt; mt.getCategoryList.xml
POST -c text/xml http://alvis.sakura.ne.jp/mt/mt-xmlrpc.cgi &lt; mt.getRecentPostTitles.xml
POST -c text/xml http://alvis.sakura.ne.jp/mt/mt-xmlrpc.cgi &lt; blogger.getUsersBlogs.xml
POST -c text/xml http://alvis.sakura.ne.jp/mt/mt-xmlrpc.cgi &lt; mt.supportedMethods.xml
POST -c text/xml http://sea5.net/pivotx/metaweblog.php &lt; blogger.getUsersBlogs.xml



以下はsample.xmlの例


&lt;?xml version='1.0' encoding='utf-8'?&gt;
&lt;methodCall&gt;
    &lt;methodName&gt;metaWeblog.newPost&lt;/methodName&gt;
    &lt;params&gt;
        &lt;param&gt;
            &lt;value&gt;&lt;string&gt;5&lt;/string&gt;&lt;/value&gt;
        &lt;/param&gt;
        &lt;param&gt;
            &lt;value&gt;&lt;string&gt;admin&lt;/string&gt;&lt;/value&gt;
        &lt;/param&gt;
        &lt;param&gt;
            &lt;value&gt;&lt;string&gt;l17ncsjs&lt;/string&gt;&lt;/value&gt;
        &lt;/param&gt;
        &lt;param&gt;
            &lt;value&gt;
                &lt;struct&gt;
                    &lt;member&gt;
                        &lt;name&gt;title&lt;/name&gt;&lt;value&gt;&lt;string&gt;ここに記事のタイトルを書きます。&lt;/string&gt;&lt;/value&gt;
                    &lt;/member&gt;
                    &lt;member&gt;
                        &lt;name&gt;description&lt;/name&gt;&lt;value&gt;&lt;string&gt;ここに記事本文を書きます。&lt;/string&gt;&lt;/value&gt;
                    &lt;/member&gt;
                &lt;/struct&gt;
            &lt;/value&gt;
        &lt;/param&gt;
        &lt;param&gt;
            &lt;value&gt;&lt;boolean&gt;1&lt;/boolean&gt;&lt;/value&gt;
        &lt;/param&gt;
    &lt;/params&gt;
&lt;/methodCall&gt;</pre>

&nbsp;
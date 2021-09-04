---
id: 663
title: '[Rails][JQuery]いつも忘れるDynamic select options'
date: 2019-01-10T09:49:28+09:00
author: Shohei
layout: post
guid: https://www.wazalab.com/?p=663
permalink: '/2019/01/10/railsjquery%e3%81%84%e3%81%a4%e3%82%82%e5%bf%98%e3%82%8c%e3%82%8bdynamic-select-options/'
siteorigin_page_settings:
  - 'a:6:{s:6:"layout";s:7:"default";s:10:"page_title";b:1;s:15:"masthead_margin";b:1;s:13:"footer_margin";b:1;s:16:"display_masthead";b:1;s:22:"display_footer_widgets";b:1;}'
categories:
  - IT
  - Programming
tags:
  - JQuery
  - options
  - rails
  - select
---
結構な頻度で書く依存型セレクトボックス。毎回自力で書いてるような気がするのでログしておく。
 
<pre class="lang:xhtml decode:true " > &lt;div class="field"&gt;
    &lt;%= form.label :region %&gt;
    &lt;%= form.select :region, options_for_select([
      'us-west-2',
      'us-west-1',
      'us-east-2',
      'us-east-1',
      'ap-south-1',
      'ap-northeast-2',
      'ap-southeast-1',
      'ap-southeast-2',
      'ap-northeast-1',
      'ca-central-1',
      'cn-north-1',
      'eu-central-1',
      'eu-west-1',
      'eu-west-2',
      'eu-west-3',
      'sa-east-1'
    ]), {include_blank: true}, required: true %&gt;
  &lt;/div&gt;

  &lt;div class="field"&gt;
    &lt;%= form.label :availability_zone %&gt;
    &lt;%= form.select :availability_zone, {}, required: true %&gt;
  &lt;/div&gt;

&lt;script&gt;
 $("#storage_region").change(function(){
    var region = $(this).val();
    var $saz = $('#storage_availability_zone');
    $.get("/api/v1/aws_availability_zones?region="+region, function(zones){
      $saz.empty();
      zones.forEach(function(zone){
        $('&lt;option&gt;').val(zone).text(zone).appendTo($saz)
      })
    });
  })
&lt;/script&gt;</pre> 

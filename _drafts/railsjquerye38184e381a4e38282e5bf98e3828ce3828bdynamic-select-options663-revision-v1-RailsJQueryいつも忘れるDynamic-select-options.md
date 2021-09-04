---
id: 665
title: '[Rails][JQuery]いつも忘れるDynamic select options'
date: 2019-01-10T09:49:28+09:00
author: Shohei
layout: revision
guid: https://www.wazalab.com/2019/01/10/663-revision-v1/
permalink: /?p=665
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

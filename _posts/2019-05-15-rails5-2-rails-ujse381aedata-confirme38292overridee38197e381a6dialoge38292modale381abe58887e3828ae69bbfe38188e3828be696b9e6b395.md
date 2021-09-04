---
id: 729
title: '[Rails5.2] rails-ujsのdata-confirmをoverrideしてdialogをmodalに切り替える方法'
date: 2019-05-15T08:35:18+09:00
author: Shohei
layout: post
guid: https://www.wazalab.com/?p=729
permalink: '/2019/05/15/rails5-2-rails-ujs%e3%81%aedata-confirm%e3%82%92override%e3%81%97%e3%81%a6dialog%e3%82%92modal%e3%81%ab%e5%88%87%e3%82%8a%e6%9b%bf%e3%81%88%e3%82%8b%e6%96%b9%e6%b3%95/'
siteorigin_page_settings:
  - 'a:6:{s:6:"layout";s:7:"default";s:10:"page_title";b:1;s:15:"masthead_margin";b:1;s:13:"footer_margin";b:1;s:16:"display_masthead";b:1;s:22:"display_footer_widgets";b:1;}'
categories:
  - IT
  - Programming
tags:
  - data-confirm
  - rails
  - rails-ujs
  - UIkit
---
以前のRailsはjquery-ujsを使っていたみたいで、rails-ujsでのdata-confirmダイアログの変更方法が見つからなかったのでjquery-ujsのoverride方法を参考にしながら、試行錯誤しつつdata-confirmmの挙動を変更しました。showConfirmDialog内をお好きなmodalモジュールに変更すれば動くはず。今回はUIKitを使っていたのでそれを使いました。

 
<pre class="lang:js decode:true " >var handleConfirm = function(element) {
    if (!allowAction(this)) {
      Rails.stopEverything(element)
    }   
  }
  var allowAction = function(element) {
    if (element.getAttribute('data-confirm') === null) {
      return true
    }   
    showConfirmDialog(element);
    return false
  }
  var confirmed = function(element, result) {
    if (result.value) {
      // User clicked confirm button
      element.removeAttribute('data-confirm')
      element.click()
    }   
  }
  var showConfirmDialog = function(link) {
    var message = $(link).attr('data-confirm');
    UIkit.modal.confirm(message).then(function(){
      confirmed(link, {value: true});
    }); 
  }
  $("a[data-confirm]").on('click',handleConfirm);</pre> 

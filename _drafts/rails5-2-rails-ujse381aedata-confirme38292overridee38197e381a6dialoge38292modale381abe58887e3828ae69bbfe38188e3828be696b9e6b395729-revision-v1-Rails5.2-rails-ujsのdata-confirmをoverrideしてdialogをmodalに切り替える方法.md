---
id: 731
title: '[Rails5.2] rails-ujsのdata-confirmをoverrideしてdialogをmodalに切り替える方法'
date: 2019-05-15T08:34:45+09:00
author: Shohei
layout: revision
guid: https://www.wazalab.com/2019/05/15/729-revision-v1/
permalink: /?p=731
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

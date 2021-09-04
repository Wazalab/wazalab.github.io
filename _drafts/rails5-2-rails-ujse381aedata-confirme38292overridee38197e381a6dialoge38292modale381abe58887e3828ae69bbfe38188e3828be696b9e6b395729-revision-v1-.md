---
id: 730
date: 2019-05-15T08:28:41+09:00
author: Shohei
layout: revision
guid: https://www.wazalab.com/2019/05/15/729-revision-v1/
permalink: /?p=730
---


 
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

---
id: 155
title: React Native Router Fluxでタブの履歴を残す方法
date: 2017-04-23T17:32:09+09:00
author: Shohei
layout: revision
guid: http://www.wazalab.com/2017/04/23/153-revision-v1/
permalink: /?p=155
---
InstagramのようにAndoridのバックボタンで前のタブに戻るようにするためにどのタブが前のタブか知る必要があります。[React Native Router Flux](https://github.com/aksonov/react-native-router-flux)(with Redux)を使っていて、これを実現するために下記のようなReducerを作りました。


 
<pre class="lang:js decode:true " >const initialState = {
  scene: {},
  tabHistory: [],
};

export default function reducer(state = initialState, action = {}) {
  switch (action.type) {
    // focus action is dispatched when a new screen comes into focus
    case "focus":
      return {
        ...state,
        scene: action.scene,
      };

    case 'REACT_NATIVE_ROUTER_FLUX_FOCUS':   
      if(action.scene.name === "tabBar"){
        if(state.tabHistory.length === 0 || state.tabHistory[0] !== action.scene.children[action.scene.index].name){
          return {
            ...state,
            tabHistory: [action.scene.children[action.scene.index].name, ...state.tabHistory ].slice(0, 10)
          }
        }
      }

</pre> 

やってることは、FOCUSしたときにtabHistoryとして、Arrayに追加してるだけです。無限に履歴を残す必要はないので10個で制限してますが、もしかしたら2つでいいかもしれません(最初の要素は現在のタブになるので二つ以上は必要)

で、これを下記みたいな感じでBackAndroidで使用すれば良いでしょう。

 
<pre class="lang:js decode:true " >
  handleAndroidBack(){
    const tabHistory = store.getState().router.tabHistory;
    if(tabHistory.length &gt; 1){ // 0 is the current tab
      const previousTab = tabHistory[1];
      switch(previousTab){
        case "feeds":
          Actions.feeds();
          return true;
        case "discussion":
          Actions.discussion();
          return true;
        case "profile":
          Actions.profile();
          return true;
        case "notification":
          Actions.notification();        
          return true;
        case "others":
          Actions.others();
          return true;
      }
    }
   return false;          
  }
</pre> 

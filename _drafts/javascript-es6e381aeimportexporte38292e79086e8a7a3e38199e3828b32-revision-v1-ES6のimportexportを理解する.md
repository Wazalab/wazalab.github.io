---
id: 33
title: ES6のimport/exportを理解する
date: 2016-06-30T09:25:53+09:00
author: Shohei
layout: revision
guid: http://www.wazalab.com/2016/06/30/32-revision-v1/
permalink: /?p=33
---
React Nativeをやってると暗黙の了解として、import/exportが使われてる。
`require`で書いてたみたいだけど、その歴史を知らないのでrequireは気にしないでimport/exportを理解する。

下記のようにlib.jsとmain.jsがあった場合、importするファイルのlib.js内でfunctionをexportしておくと
`import {} from`でそれを個々に呼ぶことができる。

```
//------ lib.js ------
export const sqrt = Math.sqrt;
export function square(x) {
    return x * x;
}
export function diag(x, y) {
    return sqrt(square(x) + square(y));
}
const foo = Math.PI + Math.SQRT2;
export { foo };

//------ main.js ------
import { square, diag, foo } from 'lib';
console.log(square(11)); // 121
console.log(diag(4, 3)); // 5
```

またアスタリスクを使用することで、オブジェクトを参照してexportした関数が呼べる。
```
//------ main.js ------
import * as lib from 'lib';
console.log(lib.square(11)); // 121
console.log(lib.diag(4, 3)); // 5
```
## default

よくわからないのが`export default`。
セミコロンなしで呼ぶことで関数・クラスをそのまま呼べる。
読む込むファイルごとに 1 つだけ呼ぶことができる。

```
//------ myFunc.js ------
export default function () { ··· } // no semicolon!

//------ main1.js ------
import myFunc from 'myFunc';
myFunc();
```
```
//------ MyClass.js ------
export default class { ··· } // no semicolon!

//------ main2.js ------
import MyClass from 'MyClass';
const inst = new MyClass();
```

defaultは一つだけというのがポイントですね。



---
layout: post
title: "for in and for of"
description: ""
category: "javascript"
tags: []
---
{% include JB/setup %}

for(x of a) 遍历出来的x是value, 直接用x表示value。
```javascript
var a = ["A", 'B', 'C'];
console.log('for of  stat begins');
a.name ='jack';
console.log(a.length);
for (var arr of a) {
    console.log(arr);
}
```
执行结果不会遍历a的属性name和它的值。结果如下：
```
for of  stat begins
3
A
B
C
```

for( x in a) 遍历出来的x是a的属性。可以用a\[x]获取value。
```javascript
var a = ["A", 'B', 'C'];
a.name ='jack';
console.log('for in stat begins');
console.log(a.length);
for (var arr_val2 in a) {
    console.log(arr_val2);
    console.log(a[arr_val2]);
}
```
执行结果会遍历到a的属性name和它的值。结果如下：
```
for in stat begins
3
0
A
1
B
2
C
name
jack
```
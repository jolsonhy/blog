---
title: ES6学习笔记（一）：Set和Map
date: 2016-04-05 20:58:41
tags: [ES6]
categories: [学习笔记]
---

## Set

![](http://7xkus6.com1.z0.glb.clouddn.com/ES6-Set.png)

### ES6利用Set数组去重：

1. 方法一：

```javascript
function dedupe(array) {
  return Array.from(new Set(array));
}

dedupe([1, 1, 2, 3]); // [1, 2, 3]
```

2. 方法二(利用扩展运算符)：

```javascript
let arr = [3, 5, 2, 2, 5, 5];
let unique = [...new Set(arr)];
// [3, 5, 2]
```

### 特性

- Set结构没有键名，只有键值，所以` keys() `和` values() `的行为完全一致。

```javascript
let set = new Set(['red', 'green', 'blue']);

for (let item of set.keys()) {
    console.log(item);
}
// red
// green
// blue

for (let item of set.values()) {
    console.log(item);
}
//red
//green
//blue

for (let item of set.entries()) {
    console.log(item);
}
// ["red", "red"]
// ["green", "green"]
// ["blue", "blue"]
```

- 可以省略` values() `方法，直接用`for...of`循环遍历Set，因为其默认遍历器生成函数就是它的`values()`方法

```javascript
Set.prototype[Symbol.iterator] === Set.prototype.values
//true
```

###　Set实现并集、交集、差集

```javascript
let a = new Set([1, 2, 3]);
let b = new Set([4, 3, 2]);

//并集
let union = new Set([...a, ...b]);
//[1, 2, 3, 4]

//交集
let intersect = new Set([...a].filter(x => b.has(x)));
//[2, 3]

//差集
let difference = new Set([...a].filter(x => !b.has(x)));
//[1]
```

***

## Map

![](http://7xkus6.com1.z0.glb.clouddn.com/ES6-Map.png)

- 和对象类似，`Object`结构提供了`“字符串-值”`的对应，Map结构提供了`“值-值”`的对应，是一种更完善的Hash结构实现。

### Map和其他数据结构的转换

#### Map -> Array

```javascript
let myMap = new Map().set(true, 7).set({foo: 3}, ['abc']);
[...myMap];
// [[true, 7], [{foo: 3}, ['abc']]]
```

#### Array -> Map

```javascript
new Map([[true, 7], [{foo: 3}, ['abc']]])
// Map {true => 7, Object {foo: 3} => ['abc']}
```

#### Map -> Object

```javascript
function strMapToObj(strMap) {
    let obj = Object.create(null);
    for (let [k,v] of strMap) {
        obj[k] = v;
    }
    return obj;
}

let myMap = new Map().set('yes', true).set('no', false);
strMapToObj(myMap)
// { yes: true, no: false }
```

#### Object -> Map

```javascript
function objToStrMap(obj) {
    let strMap = new Map();
    for (let k of Object.keys(obj)) {
        strMap.set(k, obj[k]);
    }
    return strMap;
}

objToStrMap({yes: true, no: false})
// [ [ 'yes', true ], [ 'no', false ] ]
```

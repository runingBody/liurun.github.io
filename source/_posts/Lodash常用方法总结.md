---
title: Lodash常用方法总结
tags: []
originContent: |-
  ## 一、lodash简介
  Lodash是一个著名的javascript原生库，不需要引入其他第三方依赖。是一个意在提高开发者效率,提高JS原生方法性能的JS库。
  其内置（封装）好了很多高级的适用于常用数据结构（包含数组、对象、字符串、日期等）的处理方法。只需一个简单的方法名,直接调用就行了。

  举个简单的例子：copy一个Js对象
  ```language
  //原生方法
  var a = {a:1,b:2,c:3};
  var b = {};
  for(var key in a) {
    b[key] = a[key];
  }

  //lodash方法
  var b = _.clone(a);
  ```


  ## 二、方法总结
  ### 1. 数组[Array]

  #### chunk(array, [size=1])方法：
  分块函数，按第二个参数数字大小对数组进行分块
  ```language
  _.chunk(['a', 'b', 'c', 'd'], 2);
  // => [['a', 'b'], ['c', 'd']]
  ```

  #### compact(array)方法：
  紧凑函数，剔除数组中无用元素，包含：false, null, 0,"", undefined, NaN,
  ```language
  _.compact([0, 1, false, 2, '', 3]);
  // => [1, 2, 3]
  ```

  #### concat(array, [values])方法：
  拼接函数，从第二个参数位开始拼接数组，返回一个新数组,
  ```language
  var array = [1];
  var other = _.concat(array, 2, [3], [[4]]);
   
  console.log(other);
  // => [1, 2, 3, [4]]
   
  console.log(array);
  // => [1]
  ```

  #### difference(array, [values])方法：
  差异函数，实际数组（参数1）同比较数组（参数2）比较得到参数1中不共同元素，返回一个新数组,
  ```language
  _.difference([2, 1], [2, 3]);
  // => [1]
  ```

  #### drop（array, [n=1]）方法：
  左删除函数，按参数2大小从数组首位开始删除元素，如不指定参数2则默认为删除1位,
  ```language
  _.drop([1, 2, 3]);
  // => [2, 3]
   
  _.drop([1, 2, 3], 2);
  // => [3]
   
  _.drop([1, 2, 3], 5);
  // => []
   
  _.drop([1, 2, 3], 0);
  // => [1, 2, 3]
  ```

  #### dropRight（array, [n=1]）方法：
  右删除函数，同上，从右侧开始删除,
  ```language
  _.dropRight([1, 2, 3]);
  // => [1, 2]
   
  _.dropRight([1, 2, 3], 2);
  // => [1]
   
  _.dropRight([1, 2, 3], 5);
  // => []
   
  _.dropRight([1, 2, 3], 0);
  // => [1, 2, 3]
  ```

  #### _.fill(array, value, [start=0], [end=array.length])方法：
  填充函数，用指定元素填充（覆盖）数组，可以指定起始末位置,
  ```language
  var array = [1, 2, 3];
   
  _.fill(array, 'a');
  console.log(array);
  // => ['a', 'a', 'a']
   
  _.fill(Array(3), 2);
  // => [2, 2, 2]
   
  _.fill([4, 6, 8, 10], '*', 1, 3);
  // => [4, '*', '*', 10]
  ```

  #### _.findIndex(array, [predicate=_.identity], [fromIndex=0])方法：
  查找函数（从左往右匹配），找到符合条件的元素，注意是找最后一个符合条件的元素下标，否则返回-1,
  ```language
  var users = [
    { 'user': 'barney',  'active': false },
    { 'user': 'fred',    'active': false },
    { 'user': 'pebbles', 'active': true }
  ];
   
  _.findIndex(users, function(o) { return o.user == 'barney'; });
  // => 0
   
  // The `_.matches` iteratee shorthand.
  _.findIndex(users, { 'user': 'fred', 'active': false });
  // => 1
   
  // The `_.matchesProperty` iteratee shorthand.
  _.findIndex(users, ['active', false]);
  // => 0
   
  // The `_.property` iteratee shorthand.
  _.findIndex(users, 'active');
  // => 2
  ```
  #### _.findIndex(array, [predicate=_.identity], [fromIndex=0])方法：
  查找函数（从右往左匹配），找到符合条件的元素，注意是找最后一个符合条件的元素下标，否则返回-1,
  ```language
  var users = [
    { 'user': 'barney',  'active': true },
    { 'user': 'fred',    'active': false },
    { 'user': 'pebbles', 'active': false }
  ];
   
  _.findLastIndex(users, function(o) { return o.user == 'pebbles'; });
  // => 2
   
  // The `_.matches` iteratee shorthand.
  _.findLastIndex(users, { 'user': 'barney', 'active': true });
  // => 0
   
  // The `_.matchesProperty` iteratee shorthand.
  _.findLastIndex(users, ['active', false]);
  // => 2
   
  // The `_.property` iteratee shorthand.
  _.findLastIndex(users, 'active');
  // => 0
  ```

  #### _.flatten(array)方法：
  降平函数，将数组降低一个层级, 返回一个新数组，
  ```language
  _.flatten([1, [2, [3, [4]], 5]]);
  // => [1, 2, [3, [4]], 5]
  ```

  #### _.flattenDeep(array)方法：
  深降平函数，将数组完全降级成一阶, 返回一个新数组，
  ```language
  _.flattenDeep([1, [2, [3, [4]], 5]]);
  // => [1, 2, 3, 4, 5]
  ```

  #### _.initial(array)方法：
  去尾初始化函数，去除最后一个元素，
  ```language
  _.initial([1, 2, 3]);
  // => [1, 2]
  反向 _.tail(array) // 功能同上，变为去头初始化函数
  ```

  #### _.intersection([arrays])方法：
  同值函数，找到两个函数的相同值，返回一个新数组，
  ```language
  _.intersection([2, 1], [2, 3]);
  // => [2]
  ```

  #### _.pull(array, [values])方法：
  拉去函数，去除指定参数元素，返回一个新数组，
  ```language
  var array = ['a', 'b', 'c', 'a', 'b', 'c'];
   
  _.pull(array, 'a', 'c');
  console.log(array);
  // => ['b', 'b']

  _.pullAll(array, values) 函数功能同上，参数变为数组
  ```

  #### _.take(array, [n=1])方法：
  取数函数，从左往右按参数2的大小数取值，
  ```language
  _.take([1, 2, 3]);
  // => [1]
   
  _.take([1, 2, 3], 2);
  // => [1, 2]
   
  _.take([1, 2, 3], 5);
  // => [1, 2, 3]
   
  _.take([1, 2, 3], 0);
  // => []
  ```

  其它优秀的高级的—lodash方法可以访问[官网](https://www.lodashjs.com/docs/4.17.5.html#chunk)参考
  ### 3. 集合操作[Collection]

  #### _.countBy(collection, [iteratee=_.identity])方法：
  统计函数，按能满足相同条件，统计个数，
  ```language
  _.countBy([6.1, 4.2, 6.3], Math.floor);
  // => { '4': 1, '6': 2 }
   
  // The `_.property` iteratee shorthand.
  _.countBy(['one', 'two', 'three'], 'length');
  // => { '3': 2, '5': 1 }
  ```

  #### _.forEach(collection, [iteratee=_.identity])方法：
  遍历操作函数，按能满足同意条件分组，，
  ```language
  _.forEach([1, 2], function(value) {
    console.log(value);
  });
  // => Logs `1` then `2`.
   
  _.forEach({ 'a': 1, 'b': 2 }, function(value, key) {
    console.log(key);
  });
  // => Logs 'a' then 'b' (iteration order is not guaranteed).
  ```

  #### _.groupBy(collection, [iteratee=_.identity])方法：
  分组函数，按能满足同意条件分组，
  ```language
  _.groupBy([6.1, 4.2, 6.3], Math.floor);
  // => { '4': [4.2], '6': [6.1, 6.3] }
   
  // The `_.property` iteratee shorthand.
  _.groupBy(['one', 'two', 'three'], 'length');
  // => { '3': ['one', 'two'], '5': ['three'] }
  ```

  #### _.map(collection, [iteratee=_.identity])方法：
  集合遍历函数，返回一个新数组，
  ```language
  function square(n) {
    return n * n;
  }
   
  _.map([4, 8], square);
  // => [16, 64]
   
  _.map({ 'a': 4, 'b': 8 }, square);
  // => [16, 64] (iteration order is not guaranteed)
   
  var users = [
    { 'user': 'barney' },
    { 'user': 'fred' }
  ];
   
  // The `_.property` iteratee shorthand.
  _.map(users, 'user');
  // => ['barney', 'fred']
  ```

  #### _.countBy(collection, [iteratee=_.identity])方法：
  分组函数，按能满足同意条件分组，，
  ```language
  _.countBy([6.1, 4.2, 6.3], Math.floor);
  // => { '4': 1, '6': 2 }
   
  // The `_.property` iteratee shorthand.
  _.countBy(['one', 'two', 'three'], 'length');
  // => { '3': 2, '5': 1 }
  ```

  #### _.countBy(collection, [iteratee=_.identity])方法：
  分组函数，按能满足同意条件分组，，
  ```language
  _.countBy([6.1, 4.2, 6.3], Math.floor);
  // => { '4': 1, '6': 2 }
   
  // The `_.property` iteratee shorthand.
  _.countBy(['one', 'two', 'three'], 'length');
  // => { '3': 2, '5': 1 }
  ```

  #### _.countBy(collection, [iteratee=_.identity])方法：
  分组函数，按能满足同意条件分组，，
  ```language
  _.countBy([6.1, 4.2, 6.3], Math.floor);
  // => { '4': 1, '6': 2 }
   
  // The `_.property` iteratee shorthand.
  _.countBy(['one', 'two', 'three'], 'length');
  // => { '3': 2, '5': 1 }
  ```

  #### _.countBy(collection, [iteratee=_.identity])方法：
  分组函数，按能满足同意条件分组，，
  ```language
  _.countBy([6.1, 4.2, 6.3], Math.floor);
  // => { '4': 1, '6': 2 }
   
  // The `_.property` iteratee shorthand.
  _.countBy(['one', 'two', 'three'], 'length');
  // => { '3': 2, '5': 1 }
  ```

  #### _.countBy(collection, [iteratee=_.identity])方法：
  分组函数，按能满足同意条件分组，，
  ```language
  _.countBy([6.1, 4.2, 6.3], Math.floor);
  // => { '4': 1, '6': 2 }
   
  // The `_.property` iteratee shorthand.
  _.countBy(['one', 'two', 'three'], 'length');
  // => { '3': 2, '5': 1 }
  ```

  #### _.countBy(collection, [iteratee=_.identity])方法：
  分组函数，按能满足同意条件分组，，
  ```language
  _.countBy([6.1, 4.2, 6.3], Math.floor);
  // => { '4': 1, '6': 2 }
   
  // The `_.property` iteratee shorthand.
  _.countBy(['one', 'two', 'three'], 'length');
  // => { '3': 2, '5': 1 }
  ```
  ### 3. 对象[Object] 

  #### _.assign(object, [sources])方法：
  拷贝函数（浅），
  ```language
  function Foo() {
    this.a = 1;
  }
   
  function Bar() {
    this.c = 3;
  }
   
  Foo.prototype.b = 2;
  Bar.prototype.d = 4;
   
  _.assign({ 'a': 0 }, new Foo, new Bar);
  // => { 'a': 1, 'c': 3 }
  ```

  #### _.assignIn(object, [sources])方法：
  拷贝函数（深），
  ```language
  function Foo() {
    this.a = 1;
  }
   
  function Bar() {
    this.c = 3;
  }
   
  Foo.prototype.b = 2;
  Bar.prototype.d = 4;
   
  _.assign({ 'a': 0 }, new Foo, new Bar);
  // => { 'a': 1, 'c': 3 }
  ```

  #### _.pick(object, [paths])方法：
  捡取函数，返获取指定参数2的元素，返回一个新对象，
  ```language
  var object = { 'a': 1, 'b': '2', 'c': 3 };
   
  _.pick(object, ['a', 'c']);
  // => { 'a': 1, 'c': 3 }
  ```

  #### _.omit(object, [paths])方法：
  忽略函数，剔除指定参数2的元素，返回一个新对象，
  ```language
  var object = { 'a': 1, 'b': '2', 'c': 3 };
   
  _.omit(object, ['a', 'c']);
  // => { 'b': '2' }
  ``` 


  #### _.transform(object, [iteratee=_.identity], [accumulator])方法：
  转化函数，自定义处理函数，
  ```language
  _.transform([2, 3, 4], function(result, n) {
    result.push(n *= n);
    return n % 2 == 0;
  }, []);
  // => [4, 9]
   
  _.transform({ 'a': 1, 'b': 2, 'c': 1 }, function(result, value, key) {
    (result[value] || (result[value] = [])).push(key);
  }, {});
  // => { '1': ['a', 'c'], '2': ['b'] }
  ``` 

  #### _.findKey(object, [predicate=_.identity])方法：
  找到指定函数，根据筛选条件找到过滤元素，
  ```language
  var users = {
    'barney':  { 'age': 36, 'active': true },
    'fred':    { 'age': 40, 'active': false },
    'pebbles': { 'age': 1,  'active': true }
  };
   
  _.findKey(users, function(o) { return o.age < 40; });
  // => 'barney' (iteration order is not guaranteed)
   
  // The `_.matches` iteratee shorthand.
  _.findKey(users, { 'age': 1, 'active': true });
  // => 'pebbles'
   
  // The `_.matchesProperty` iteratee shorthand.
  _.findKey(users, ['active', false]);
  // => 'fred'
   
  // The `_.property` iteratee shorthand.
  _.findKey(users, 'active');
  // => 'barney'
  ``` 

  #### _.invert(object)方法：
  对调函数，对象的键值对反转，返回一个新对象，
  ```language
  var object = { 'a': 1, 'b': 2, 'c': 1 };
   
  _.invert(object);
  // => { '1': 'c', '2': 'b' }
  ``` 

  #### _.invertBy(object, [iteratee=_.identity])方法：
  对调函数，取全值并格式化返回结果，返回一个新对象，
  ```language
  var object = { 'a': 1, 'b': 2, 'c': 1 };
   
  _.invertBy(object);
  // => { '1': ['a', 'c'], '2': ['b'] }
   
  _.invertBy(object, function(value) {
    return 'group' + value;
  });
  // => { 'group1': ['a', 'c'], 'group2': ['b'] }
  ```

  ### 4. 字符串[String]

  #### _.trim([string=''], [chars=whitespace])方法：
  trim函数，去除指定字符，默认空格，
  ```language
  _.trim('  abc  ');
  // => 'abc'
   
  _.trim('-_-abc-_-', '_-');
  // => 'abc'
   
  _.map(['  foo  ', '  bar  '], _.trim);
  // => ['foo', 'bar']

  _.trimEnd('  abc  ');
  // => '  abc'
   
  _.trimEnd('-_-abc-_-', '_-');
  // => '-_-abc'

  _.trimStart('  abc  ');
  // => 'abc  '
   
  _.trimStart('-_-abc-_-', '_-');
  // => 'abc-_-'
  ``` 

  #### _.split([string=''], separator, [limit])方法：
  split切割函数，可按指定参数2对数组切割，可限制大小，
  ```language
  _.split('a-b-c', '-', 2);
  // => ['a', 'b']
  ```

  #### _.parseInt(string, [radix=10])方法：
  解析函数，对字符串中数值解析，可设定解析进制数，
  ```language
  _.parseInt('08');
  // => 8
   
  _.map(['6', '08', '10'], _.parseInt);
  // => [6, 8, 10]
  ```

  #### _.capitalize([string=''])方法：
  首字符大写函数，
  ```language
  _.capitalize('FRED');
  // => 'Fred'
  ```

  #### _.camelCase([string=''])方法：
  驼峰化函数，
  ```language
  _.camelCase('Foo Bar');
  // => 'fooBar'
   
  _.camelCase('--foo-bar--');
  // => 'fooBar'
   
  _.camelCase('__FOO_BAR__');
  // => 'fooBar'
  ```
  #### 资料维护，如有纰漏，评论指正，谢谢阅览。
categories:
  - 专栏
toc: false
date: 2018-12-29 16:12:57
---

## 一、lodash简介
Lodash是一个著名的javascript原生库，不需要引入其他第三方依赖。是一个意在提高开发者效率,提高JS原生方法性能的JS库。
其内置（封装）好了很多高级的适用于常用数据结构（包含数组、对象、字符串、日期等）的处理方法。只需一个简单的方法名,直接调用就行了。

举个简单的例子：copy一个Js对象
```language
//原生方法
var a = {a:1,b:2,c:3};
var b = {};
for(var key in a) {
  b[key] = a[key];
}

//lodash方法
var b = _.clone(a);
```


## 二、方法总结
### 1. 数组[Array]

#### chunk(array, [size=1])方法：
分块函数，按第二个参数数字大小对数组进行分块
```language
_.chunk(['a', 'b', 'c', 'd'], 2);
// => [['a', 'b'], ['c', 'd']]
```

#### compact(array)方法：
紧凑函数，剔除数组中无用元素，包含：false, null, 0,"", undefined, NaN,
```language
_.compact([0, 1, false, 2, '', 3]);
// => [1, 2, 3]
```

#### concat(array, [values])方法：
拼接函数，从第二个参数位开始拼接数组，返回一个新数组,
```language
var array = [1];
var other = _.concat(array, 2, [3], [[4]]);
 
console.log(other);
// => [1, 2, 3, [4]]
 
console.log(array);
// => [1]
```

#### difference(array, [values])方法：
差异函数，实际数组（参数1）同比较数组（参数2）比较得到参数1中不共同元素，返回一个新数组,
```language
_.difference([2, 1], [2, 3]);
// => [1]
```

#### drop（array, [n=1]）方法：
左删除函数，按参数2大小从数组首位开始删除元素，如不指定参数2则默认为删除1位,
```language
_.drop([1, 2, 3]);
// => [2, 3]
 
_.drop([1, 2, 3], 2);
// => [3]
 
_.drop([1, 2, 3], 5);
// => []
 
_.drop([1, 2, 3], 0);
// => [1, 2, 3]
```

#### dropRight（array, [n=1]）方法：
右删除函数，同上，从右侧开始删除,
```language
_.dropRight([1, 2, 3]);
// => [1, 2]
 
_.dropRight([1, 2, 3], 2);
// => [1]
 
_.dropRight([1, 2, 3], 5);
// => []
 
_.dropRight([1, 2, 3], 0);
// => [1, 2, 3]
```

#### _.fill(array, value, [start=0], [end=array.length])方法：
填充函数，用指定元素填充（覆盖）数组，可以指定起始末位置,
```language
var array = [1, 2, 3];
 
_.fill(array, 'a');
console.log(array);
// => ['a', 'a', 'a']
 
_.fill(Array(3), 2);
// => [2, 2, 2]
 
_.fill([4, 6, 8, 10], '*', 1, 3);
// => [4, '*', '*', 10]
```

#### _.findIndex(array, [predicate=_.identity], [fromIndex=0])方法：
查找函数（从左往右匹配），找到符合条件的元素，注意是找最后一个符合条件的元素下标，否则返回-1,
```language
var users = [
  { 'user': 'barney',  'active': false },
  { 'user': 'fred',    'active': false },
  { 'user': 'pebbles', 'active': true }
];
 
_.findIndex(users, function(o) { return o.user == 'barney'; });
// => 0
 
// The `_.matches` iteratee shorthand.
_.findIndex(users, { 'user': 'fred', 'active': false });
// => 1
 
// The `_.matchesProperty` iteratee shorthand.
_.findIndex(users, ['active', false]);
// => 0
 
// The `_.property` iteratee shorthand.
_.findIndex(users, 'active');
// => 2
```
#### _.findIndex(array, [predicate=_.identity], [fromIndex=0])方法：
查找函数（从右往左匹配），找到符合条件的元素，注意是找最后一个符合条件的元素下标，否则返回-1,
```language
var users = [
  { 'user': 'barney',  'active': true },
  { 'user': 'fred',    'active': false },
  { 'user': 'pebbles', 'active': false }
];
 
_.findLastIndex(users, function(o) { return o.user == 'pebbles'; });
// => 2
 
// The `_.matches` iteratee shorthand.
_.findLastIndex(users, { 'user': 'barney', 'active': true });
// => 0
 
// The `_.matchesProperty` iteratee shorthand.
_.findLastIndex(users, ['active', false]);
// => 2
 
// The `_.property` iteratee shorthand.
_.findLastIndex(users, 'active');
// => 0
```

#### _.flatten(array)方法：
降平函数，将数组降低一个层级, 返回一个新数组，
```language
_.flatten([1, [2, [3, [4]], 5]]);
// => [1, 2, [3, [4]], 5]
```

#### _.flattenDeep(array)方法：
深降平函数，将数组完全降级成一阶, 返回一个新数组，
```language
_.flattenDeep([1, [2, [3, [4]], 5]]);
// => [1, 2, 3, 4, 5]
```

#### _.initial(array)方法：
去尾初始化函数，去除最后一个元素，
```language
_.initial([1, 2, 3]);
// => [1, 2]
反向 _.tail(array) // 功能同上，变为去头初始化函数
```

#### _.intersection([arrays])方法：
同值函数，找到两个函数的相同值，返回一个新数组，
```language
_.intersection([2, 1], [2, 3]);
// => [2]
```

#### _.pull(array, [values])方法：
拉去函数，去除指定参数元素，返回一个新数组，
```language
var array = ['a', 'b', 'c', 'a', 'b', 'c'];
 
_.pull(array, 'a', 'c');
console.log(array);
// => ['b', 'b']

_.pullAll(array, values) 函数功能同上，参数变为数组
```

#### _.take(array, [n=1])方法：
取数函数，从左往右按参数2的大小数取值，
```language
_.take([1, 2, 3]);
// => [1]
 
_.take([1, 2, 3], 2);
// => [1, 2]
 
_.take([1, 2, 3], 5);
// => [1, 2, 3]
 
_.take([1, 2, 3], 0);
// => []
```

其它优秀的高级的—lodash方法可以访问[官网](https://www.lodashjs.com/docs/4.17.5.html#chunk)参考
### 3. 集合操作[Collection]

#### _.countBy(collection, [iteratee=_.identity])方法：
统计函数，按能满足相同条件，统计个数，
```language
_.countBy([6.1, 4.2, 6.3], Math.floor);
// => { '4': 1, '6': 2 }
 
// The `_.property` iteratee shorthand.
_.countBy(['one', 'two', 'three'], 'length');
// => { '3': 2, '5': 1 }
```

#### _.forEach(collection, [iteratee=_.identity])方法：
遍历操作函数，按能满足同意条件分组，，
```language
_.forEach([1, 2], function(value) {
  console.log(value);
});
// => Logs `1` then `2`.
 
_.forEach({ 'a': 1, 'b': 2 }, function(value, key) {
  console.log(key);
});
// => Logs 'a' then 'b' (iteration order is not guaranteed).
```

#### _.groupBy(collection, [iteratee=_.identity])方法：
分组函数，按能满足同意条件分组，
```language
_.groupBy([6.1, 4.2, 6.3], Math.floor);
// => { '4': [4.2], '6': [6.1, 6.3] }
 
// The `_.property` iteratee shorthand.
_.groupBy(['one', 'two', 'three'], 'length');
// => { '3': ['one', 'two'], '5': ['three'] }
```

#### _.map(collection, [iteratee=_.identity])方法：
集合遍历函数，返回一个新数组，
```language
function square(n) {
  return n * n;
}
 
_.map([4, 8], square);
// => [16, 64]
 
_.map({ 'a': 4, 'b': 8 }, square);
// => [16, 64] (iteration order is not guaranteed)
 
var users = [
  { 'user': 'barney' },
  { 'user': 'fred' }
];
 
// The `_.property` iteratee shorthand.
_.map(users, 'user');
// => ['barney', 'fred']
```

### 3. 对象[Object] 

#### _.assign(object, [sources])方法：
拷贝函数（浅），
```language
function Foo() {
  this.a = 1;
}
 
function Bar() {
  this.c = 3;
}
 
Foo.prototype.b = 2;
Bar.prototype.d = 4;
 
_.assign({ 'a': 0 }, new Foo, new Bar);
// => { 'a': 1, 'c': 3 }
```

#### _.assignIn(object, [sources])方法：
拷贝函数（深），
```language
function Foo() {
  this.a = 1;
}
 
function Bar() {
  this.c = 3;
}
 
Foo.prototype.b = 2;
Bar.prototype.d = 4;
 
_.assign({ 'a': 0 }, new Foo, new Bar);
// => { 'a': 1, 'c': 3 }
```

#### _.pick(object, [paths])方法：
捡取函数，返获取指定参数2的元素，返回一个新对象，
```language
var object = { 'a': 1, 'b': '2', 'c': 3 };
 
_.pick(object, ['a', 'c']);
// => { 'a': 1, 'c': 3 }
```

#### _.omit(object, [paths])方法：
忽略函数，剔除指定参数2的元素，返回一个新对象，
```language
var object = { 'a': 1, 'b': '2', 'c': 3 };
 
_.omit(object, ['a', 'c']);
// => { 'b': '2' }
``` 


#### _.transform(object, [iteratee=_.identity], [accumulator])方法：
转化函数，自定义处理函数，
```language
_.transform([2, 3, 4], function(result, n) {
  result.push(n *= n);
  return n % 2 == 0;
}, []);
// => [4, 9]
 
_.transform({ 'a': 1, 'b': 2, 'c': 1 }, function(result, value, key) {
  (result[value] || (result[value] = [])).push(key);
}, {});
// => { '1': ['a', 'c'], '2': ['b'] }
``` 

#### _.findKey(object, [predicate=_.identity])方法：
找到指定函数，根据筛选条件找到过滤元素，
```language
var users = {
  'barney':  { 'age': 36, 'active': true },
  'fred':    { 'age': 40, 'active': false },
  'pebbles': { 'age': 1,  'active': true }
};
 
_.findKey(users, function(o) { return o.age < 40; });
// => 'barney' (iteration order is not guaranteed)
 
// The `_.matches` iteratee shorthand.
_.findKey(users, { 'age': 1, 'active': true });
// => 'pebbles'
 
// The `_.matchesProperty` iteratee shorthand.
_.findKey(users, ['active', false]);
// => 'fred'
 
// The `_.property` iteratee shorthand.
_.findKey(users, 'active');
// => 'barney'
``` 

#### _.invert(object)方法：
对调函数，对象的键值对反转，返回一个新对象，
```language
var object = { 'a': 1, 'b': 2, 'c': 1 };
 
_.invert(object);
// => { '1': 'c', '2': 'b' }
``` 

#### _.invertBy(object, [iteratee=_.identity])方法：
对调函数，取全值并格式化返回结果，返回一个新对象，
```language
var object = { 'a': 1, 'b': 2, 'c': 1 };
 
_.invertBy(object);
// => { '1': ['a', 'c'], '2': ['b'] }
 
_.invertBy(object, function(value) {
  return 'group' + value;
});
// => { 'group1': ['a', 'c'], 'group2': ['b'] }
```

### 4. 字符串[String]

#### _.trim([string=''], [chars=whitespace])方法：
trim函数，去除指定字符，默认空格，
```language
_.trim('  abc  ');
// => 'abc'
 
_.trim('-_-abc-_-', '_-');
// => 'abc'
 
_.map(['  foo  ', '  bar  '], _.trim);
// => ['foo', 'bar']

_.trimEnd('  abc  ');
// => '  abc'
 
_.trimEnd('-_-abc-_-', '_-');
// => '-_-abc'

_.trimStart('  abc  ');
// => 'abc  '
 
_.trimStart('-_-abc-_-', '_-');
// => 'abc-_-'
``` 

#### _.split([string=''], separator, [limit])方法：
split切割函数，可按指定参数2对数组切割，可限制大小，
```language
_.split('a-b-c', '-', 2);
// => ['a', 'b']
```

#### _.parseInt(string, [radix=10])方法：
解析函数，对字符串中数值解析，可设定解析进制数，
```language
_.parseInt('08');
// => 8
 
_.map(['6', '08', '10'], _.parseInt);
// => [6, 8, 10]
```

#### _.capitalize([string=''])方法：
首字符大写函数，
```language
_.capitalize('FRED');
// => 'Fred'
```

#### _.camelCase([string=''])方法：
驼峰化函数，
```language
_.camelCase('Foo Bar');
// => 'fooBar'
 
_.camelCase('--foo-bar--');
// => 'fooBar'
 
_.camelCase('__FOO_BAR__');
// => 'fooBar'
```
#### 资料维护，如有纰漏，评论指正，谢谢阅览
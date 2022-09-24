Object 和 Map 的区别
===================
Object: 是最常用的一种引用类型数据，可用于存储键值对的集合<br/>
Map: 是键值对的集合，采用 Hash 结构存储，在 ES6 版本里新增的<br/>

共同点：键值对的动态集合，支持增加和删除键对<br/>
--------
Object: 

```JavaScript
  //定义
  const obj = {
      a: 1,
      b: 2
  };
  // 添加键值对
  obj.c = 3;
  //删除键值对
  delete obj.c;
```

Map:
```JavaScript
//定义
const map = new Map();
//添加键值对
map.set('a', 1);
map.set('b', 2);
//删除键值对
map.delete('a');
```

不同点
----

* 构造方式<br/>
>Object:

```JavaScript
//对象字面量
const obj = {
    'a': '1',
    'b': '2'
};
//构造方法
const o = new Object();
const o2 = Object.create();
```

>Map:

```JavaScript
//构造方法
const m = new Map();
const m2 = new Map([
     ['a', '1'],
     ['b', '2']   
])
```

* 键的类型<br/>
>Object：键的类型必须是String或者Symbol，如果非String类型，会进行数据类型转换

```JavaScript
const obj2 = {
    a: 1,
};
const arr1 = [1, 2];
obj2[arr1] = 'arr';  //obj2
                     //{a: 1,  1,2:'arr'}
                     //
                     //obj2[3] = 3;
                     //obj2['3'] = 33;  //会覆盖上面的
                     //obj
                     //{3: 33, a: 1, 1,2:'arr'}  
```
                     
>Map：可以是任意类型，包括对象，数组，函数等，不会进行类型转换。<br/>
>>在添加键值对时，会通过严格相等(===)来判断键属性是否已存在

```JavaScript
const map2 = new Map();
map2.set('a', 1);
map2.set('2', '2');
map2.set(2, 2);
map2.set(arr1, 'arr');
//0:{"a" => 1}
//1:{"2" => "2"}
//2:{2 => 2}
//3:{Array(2) => "arr"}
```

>特例：NaN
```JavaScript
NaN === NaN  //false

const map = new Map();
map.set(NaN, 1);
map.set(NaN, 2); //会覆盖上面的
map    //{NaN => 2})
```

* 键的顺序<br/>
>Object：key是无序的，不会按照添加的顺序返回<br/>
>>1. 对于大于等于0的整数，会按照大小进行排序，对于小于和负数会当做字符串处理<br/>
>>2. 对于 String 类型，按照定义的顺序进行输出<br/>
>>3. 对于Symbol类型，会直接过滤掉，不会进行输出，<br/>
>>如果想要输出Symbol类型属性，<br/>
>>通过Object.getOwnPropertySymbols()方法<br/>

```JavaScript
const obj3 = {
    2: 2,
    '1': 1,
    'b': 'b',
    1.1: 1.1,
    0: 0,
    'a': 'a',
    [Symbol('s1')]: 's2',
    [Symbol('s2')]: 's1',
}
Object.keys(obj3) //['0', '1', '2','b', '1.1', 'a']
```

>Map：key是有序的，按照插入的顺序进行返回

```JavaScript
const map3 = new Map();
map3.set(2, 2);
map3.set('1', 1);
map3.set('b', 'b');
map3.set(0, 0);
map3.set('a', 'a');
map3.set(Symbol('s1'), 's1');

for(let key of map3.key()) {
    console.log(key);  //2 1 b 0 a Symbol(s1)
}
```

* 键值对 size<br/>
>Object：只能手动计算，通过Object.key()方法或者通过for...in循环统计
```JavaScript
const obj4 = {
    2: 2,
    '1': 1,
    'b': 'b',
};
Object.keys(obj4).length; //3
```

>Map：直接通过size属性访问
```JavaScript
const map4 = new Map();
map4.set(2, 2);
map4.set('1', 1);
map4.set('b', 'b');
map.size; //3
```

* 键值对访问<br/>
>Object:

```JavaScript
//1.添加或者修改属性，通过点或者中括号的形式
const obj5 = {};
obj5.name = 'zhangsan';
obj5[Symbol('s5')] = 's5';

//判断属性是否存在
obj5.name === undefined;
obj5['name'] === undefined;

//删除属性，通过delete关键字
delete obj5.name;
```

>Map:
```JavaScript
//1.添加和修改key-value
const map5 = new Map();
map5.set('name', 'zhangsan');
map5.set(Symbol('s5'), 's5');

//2.判断属性是否存在
map5.has('name'); //true
map5.has('age');  //false

//取值
map.get('name'); //zhangsan

//删除键值对
map5.delete('name');

//获取所有的属性名
map5.key();

//清空map
map5.clear();
```

* 迭代器 - for...of<br/>
>Object：本身不具有 lterator 特性，默认情况下不能使用 for...of 进行遍历<br/>
>Map：结构的 keys(), values(), entries() 方法返回值都具有 lterator 特性
```JavaScript
const map6 = new Map([
    ['name', 'zhangsan'],
    ['age', 14]
]);
for(let [key, value] of map6.entries()) {
    console.log(key, value);  //name zhangsan
                              //age 14
}
```
* JSON序列化<br/>
>Object: Object类型可以通过JSON.stringify()进行序列化操作
```JavaScript
const obj7 = {
    name: 'zhangsan',
    age: 14,
};
JSON.stringify(obj7); //'{"name":"zhangsan","age":14}'
```
>Map：Map结构不能直接进行JSON序列化
```JavaScript
const map7 = new Map({
    ['name', 'zhangsan'],
    ['age': 14]
});
JSON.stringify(map7); //'{}'
```
* 适用场景

| | 使用场景 |
| ---- | ---- |
|Object| 1. 仅做数据存储，并且属性仅为字符串或者Symbol<br/> 2. 需要进行序列化转换为json传输时<br/> 3. 当做一个对象的实例，需要保留自己的属性和方法时|
|Map| 1. 会频繁更新和删除键值对时<br/> 2. 存储大量数据时，尤其是key类型未知的情况下<br/> 3. 需要频繁进行迭代处理|

* map.keys()函数返回的是键的集合<br/>
* map.values()函数返回的是值的集合<br/>
* map.entries()函数返回的是键值对的集合<br/>

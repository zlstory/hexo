---
layout: post
title: "Array"
tags: [Array]
date: 2017-6-22
comments: true
---

### 创建数组的两种方式
1.调用构造函数Array()

(1)调用时没有参数,即创建一个空数组。
```javascript    
    var arr1 = new Array();
```

(2)调用时有一个参数且为数字，该数字表示数组长度。
```javascript      
    var arr2 = new Array(10);
```

(3)调用时有一个或多个非数值元素、数组元素为参数，此时参数即是该数组的元素。
```javascript      
    var arr3 = new Array("crystal","zilan",1,2,3,true);
```

2.数组字面量

使用数组直接量是创建数组最简单的方法，在方括号中将数组元素用逗号隔开即可。
```javascript      
    var arr1 = [];    

    var arr2 = ["crystal","zilan",1,2,3,true];  

```

### 数组的特点

1.读写属性

使用[ ]操作符来读写数组元素,数组索引(下标)从0开始。

```javascript      
   var value = arr[0];

   arr[1] = 3;

```

2.length属性

每个数组都有一个length属性，代表的是数组中元素的个数。通过此属性可以向数组的末尾移除项或者向数组中添加新项。lengfth值 = 索引值+1

### 数组方法

1.push()与pop()


push()方法：在数组的尾部添加一个或者多个元素，返回值：新数组长度。

pop()方法：删除数组的最后一个元素，返回值：被删除的元素。

```javascript  
    var arr1 = [1,2,3,4,5];
    console.log(arr1.push("last"));     // 6
    console.log(arr1);      // [1, 2, 3, 4, 5, "last"]
    console.log(arr1.pop());    // last
    console.log(arr1);      // [1, 2, 3, 4, 5]

```

2.shift()与unshift()

unshift():在数组的头部添加一个或多个元素，返回值：新数组长度。

shift():删除数组的第一个元素，返回值：被删除的元素。

```javascript  
    var arr2 = [1,2,3,4,5];
    console.log(arr2.unshift("first"));     // 6
    console.log(arr2);      // ["first",1, 2, 3, 4, 5]
    console.log(arr2.pop());    // first
    console.log(arr2);      // [1, 2, 3, 4, 5]

```

3.join()

join():将数组中所有元素都转化为字符串并拼接在一起，返回值：最后生成的字符串。此方法不修改原数组。

```javascript  
    var arr = [1,2,3,4,5];
    console.log(arr.join()); // 1,2,3,4,5
    console.log(arr); // [1, 2, 3, 4, 5]
    console.log(arr.join(" ")); // 1 2 3 4 5
    console.log(arr.join("-")); // 1-2-3-4-5
    console.log(arr); // [1, 2, 3, 4, 5]

```

4.reverse()

reverse():将数组中的元素颠倒顺序,返回值：逆序的数组。

```javascript  
    var arr = [1,2,3,4,5];
    console.log(arr.reverse()); // [5, 4, 3, 2, 1]
    console.log(arr); // [5, 4, 3, 2, 1]   

```

5.sort()

sort():将数组中的元素排序并返回排序后的数组，返回值：排序后的数组。

(1)没有参数时：按照字母表顺序排列

```javascript  
    var arr = ["Travel","on","beyond","the","dawn"];
    console.log(arr.sort()); // [5, 4, 3, 2, 1]
    console.log(arr); // [5, 4, 3, 2, 1]   

```

(2)为了按照其他方式排序，需要给sort()中传入一个比较函数。

```javascript  
    function compare (a,b){
        if(a>b){
            return 1;
        }else if(a<b){
            return -1;
        }else{
            return 0;
        }
    }
    var arr = [1,3,6,8,5,2,9];
    console.log(arr.sort(compare)); //  [1, 2, 3, 5, 6, 8, 9]
    console.log(arr);   //  [1, 2, 3, 5, 6, 8, 9]

```

6.concat()

concat():创建一个新的数组，返回值：新的数组，包括原始数组以及concat()中的参数,但是不会扁平化数组。注：此方法不会修改原数组。

```javascript  
    var a = [1,2,3,4];
    console.log(a.concat(5,6)); // [1, 2, 3, 4, 5, 6]
    console.log(a.concat([7,8]));   // [1, 2, 3, 4, 7, 8]
    console.log(a.concat([9,[10,11]])); //  [1, 2, 3, 4, 9, [10,11]]
    console.log(a); //[1,2,3,4]
```

7.slice()

slice():返回指定数组的一个子数组,第一个参数表示开始的位置，第二个参数表示结束的位置。

```javascript  
    var a = [1,2,3,4,5];
    console.log(a.slice(0,3)); // [1, 2, 3]
    console.log(a.slice(3));   // [4, 5]
    console.log(a.slice(1,-1)); //  [2, 3, 4]
    console.log(a.slice(-3,-2)); //[3]
    console.log(a); // [1,2,3,4,5]

```

8.splice()

splice():在数组中插入或者删除元素，第一个参数表示操作起始位置，第二个参数表示删除元素的个数，其后的参数都是插入到原数组的元素。
```javascript  
    var a = [1,2,3,4,5,6,7,8];
    console.log(a.splice(4)); // [5,6,7,8]
    console.log(a.splice(1,2));   // [2, 3]
    console.log(a.splice(1,1)); //  [4]
    console.log(a.splice(2,0,"a","b")); //[]
    console.log(a); // [1, "a", "b"]

```


### ECMAScript5中的数组方法

ECMAScript5中新定义了9个新的数组方法来遍历、映射、过滤、检测、简化和所搜数组，第一个参数为函数，且数组中的每个元素都调用该函数。

1.forEach()

forEach():从头至尾遍历数组，为每个元素调用指定的函数。

```javascript  
    var data = [1,2,3,4,5,6,7];
    var sum = 0;
    data.forEach(function(value){
        sum += value;
    });
    console.log(sum);   // 28


```

函数中可有三个参数：数组元素value、索引i、数组本身arr。

```javascript  
    var arr = [1,2,3,4,5,6,7];
    arr.forEach(function(value,i,arr){
        arr[i] = value + 1;
    });
    console.log(arr);   // [2, 3, 4, 5, 6, 7, 8]


```

2.map()

map()：将调用的数组的每个元素传递给指定的函数，返回值：一个包含该函数返回值的数组;不修改原数组。

```javascript  
    var arr = [1,2,3,4,5,6,7];
    var arr1 = arr.map(function(x){
        return x*x;
    });
    console.log(arr);   //  [1, 2, 3, 4, 5, 6, 7]
    console.log(arr1);  //  [1, 4, 9, 16, 25, 36, 49]


```


3.filter()

filter():过滤，返回的数组元素是原数组的子集。传递的函数作为筛选的条件，返回true和false。注：不改变原数组。

```javascript  
    var arr = [1,2,3,4,5,6,7];
    var arr1 = arr.filter(function(x){
        return x < 3;
    });
     var arr2 = arr.filter(function(x){
        return x > 3;
    });
    console.log(arr1);  //  [1, 2]
    console.log(arr2);  //  [4, 5, 6, 7]
    console.log(arr);   //  [1, 2, 3, 4, 5, 6, 7]
    
```

4.every()和some()

every():当数组中所有元素调用判定函数返回值都为true时，才返回true；

some():当数组中存在元素调用判定函数返回值为true时，即返回true。

```javascript  
    var arr = [1,2,3,4,5,6,7];
    var arr1 = arr.every(function(x){
        return x < 3;
    });
     var arr2 = arr.some(function(x){
        return x < 3;
    });
    console.log(arr1);  //  false
    console.log(arr2);  // true
    console.log(arr);   //  [1, 2, 3, 4, 5, 6, 7]
    
```


5.reduce()和reduceRight()

reduce()和reduceRight()使用指定的函数将数组元素进行组合，第一个参数是执行简单操作的函数，第二个参数是初始值(可选)，传递的函数有四个参数：前一个参数prev、当前值cur、索引i、数组本身arr。reduceRight()表示执行顺序从右到左。

```javascript  
    var a = [1,2,3,4,5];
		var sum = a.reduce(function(prev,cur,i,arr){
			return prev + cur;
		},1);

		var max = a.reduce(function(x,y){
			return (x>y)?x:y;
		})

		 console.log(sum);  //  16
		 console.log(max);  //  5
    
```

6.indexOf()和lastIndexOf()

indexOf()和lastIndexOf():找到返回第一个符合条件元素的索引，找不到返回-1，第一个参数表示需要搜索的值，第二个是索引，指定从哪开始搜索。lastIndexOf()是从后往前搜索。
	

```javascript  
    var arr = [1,2,3,4,5,6,7];
    var arr1 = arr.indexOf(5);  //  4
    var arr2 = arr.indexOf(8);   // -1
    var arr3 = arr.lastIndexOf(5);  //  4
    var arr4 = arr.lastIndexOf(9);  //  -1
    
```

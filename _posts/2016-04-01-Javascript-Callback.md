---
layout: post
title: Javascript Callback Understanding
date: 2016-04-01
categories: blog
tags: [study]

---

# 回调函数定义

回调函数是指一个函数作为参数传入另一个函数中，并在其中执行。常见的JQuery中的回调函数：

```javascript

$("#button").click(function({
    alert("Button Clicked");
}))

```

如上面代码所展示的，我们把一个函数作为参数传入click函数中，click函数会调用这个传入的函数。

```javascript

var friends = ["Mike", "Stacy", "Andy", "Rick"];

friends.forEach(function(eachName, index){
    console.log(index + 1 + "." + eachName);
})

```

同样，上面代码中，我们传递了一个匿名函数作为forEach的参数

# 回调函数运行原理

我们可以把函数作为参数传递，可以作为返回值，可以在其他函数中调用。当我们把函数作为参数传递的时候，我们并没有执行这个函数，而是把函数的定义作为参数进行了传递，因此这个函数可以在之后的任何时间点执行。

# 使用回调函数的基本原则

### 可以使用命名或匿名函数作为回调函数

最开始的代码是匿名函数的例子，这里给一个命名函数的例子

```javascript

var allUserData = [];

function logStuff(userData) {
    if (typeof userData === "string") {
        console.log(userData);
    } else if (typeof userData === "object") {
        for (var item in userData) {
            console.log(item + "." + userData[item]);
        }
    }
}

function getInput(options, callback) {
    allUserData.push(options);
    callback(options);
}

getInput({name:"Rich",speciality:"JavaScript"}, logStuff);

```

### 可以传递参数到回调函数中

如上面的例子，我们把options传到callback中

### 在回调函数执行前确保参数的确是个函数

```javascript

function getInput(options, callback) {
    allUserData.push(options);

    if (typeof callback === "function") {
        callback(options);
    }
}

```

### 在回调函数中使用this的问题

当回调函数中使用this的时候，我们需要修改callback以保持this中的内容不变。否则this可能指向全局window对象，也可能指向包含它的那个函数。

```javascript

var clientData = {
    id: 094545,
    fullName: "Not Set",

    setUserName: function(firstName, lastName) {
        this.fullName = firstName + " " + lastName;
    }
}

fuction getUserInput(firstName, lastName, callback) {
    callback(firstName, lastName);
}

```

在接下来的代码中，当setUserName被执行的时候，this指向的并不是clientData对象，而是window对象，因为getUserInput是一个全局函数。

```javascript

getUserInput("Barack", "Obama", clientData.setUserName);

console.log(clientData.fullName); //Not Set

console.log(window.fullName);// Barack Obama

```

我们可以使用*Call*或者*Apply*函数：用于设置this对象以及向函数传递参数。Call和Apply的第一个参数都是this对象：

```javascript

function getUserInput(firstName, lastName, callback, callbackObj) {
    callback.apply(callbackObj, [firstName, lastName]);
}

getUserInput("Barack", "Obama", clientData.setUserName, clientData);

console.log(clientData.fullName);

```

上面的代码即可改正之前的this指向错误。

### 允许多个回调函数参数

我们可以在参数中使用多个回调函数

```javascript

function successCallback() {

}

function completeCallback() {

}

function errorCallback() {

}

$.ajax({
    url:"http://fiddle.jshell.net/favicon.png",
    success:successCallback,
    complete:completeCallback,
    error:errorCallback
});

```

#回调地狱

即多层回调导致难以追踪代码，代码以任意顺序运行，下面是MongoDB的一个驱动示例：

```javascript

var p_client = new Db('integration_tests_20', new Server("127.0.0.1", 27017, {}), {'pk':CustomPKFactory});
p_client.open(function(err, p_client) {
    p_client.dropDatabase(function(err, done) {
        p_client.createCollection('test_custom_key', function(err, collection) {
            collection.insert({'a':1}, function(err, docs) {
                collection.find({'_id':new ObjectID("aaaaaaaaaaaa")}, function(err, cursor) {
                    cursor.toArray(function(err, items) {
                        test.assertEquals(1, items.length);
                        p_client.close();
                    });
                });
            });
        });
    });
});

```

解决方法：

1. 不用匿名函数，而是用函数名作为参数
2. 模块化


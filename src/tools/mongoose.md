# mongoose简介

### mongodb 和 mongoose

#### 1、MongoDB

MongoDB是一个基于分布式文件存储的数据库；由C++语言编写。
是一个介于关系数据库和非关系数据库之间的产品；在 mongodb 中，数据的层级是：数据库 -> collection -> document -> 字段。

#### 2、mongoose

mongoose 是个 odm，odm 指对象文档映射。

mongoose 作用就是，在程序代码中，定义一下数据库中的数据格式，然后取数据时通过它们，可以把数据库中的 document 映射成程序中的一个对象，这个对象有 .save .update 等一系列方法和 .title .author 等一系列属性。在调用这些方法时，odm 会根据你调用时所用的条件，自动转换成相应的 mongodb shell 语句帮你发送出去。

mongoose简单理解为是MongoDB的脚本。

### 安装

```
npm i -S mongoose
```

### 初始化
```
let mongoose = require('mongoose');
const location = 'mongodb://localhost:27017/blogdatabase';
mongoose.Promise = global.Promise;
// 连接本地的blogDataBase数据库
mongoose.connect(location,{useNewUrlParser:true});
let db = mongoose.connection;

// 连接异常
db.on('error',err=>{
    console.log('Mongoose 连接失败: ' + err);
});

// 连接成功
db.once('open', ()=>{

    //测试数据是否可以保存成功
    const Dog = mongoose.model('Dog', {name: String});
    const doga = new Dog({name: 'aaa'});
    doga.save().then(()=>console.log('保存成功'));

    console.log('Mongoose 连接成功： ' + location);
});

// 连接断开
db.on('disconnected', function () {
    console.log('Mongoose 已断开连接')
});

module.exports = mongoose;
```

### 数据建模

1、创建一个名称为Schema文件夹并且创建一个user.js文件

```
const mongoose = require('../index');

// 导出 mongoose 定义的数据模式（schema）
module.exports = new mongoose.Schema({
    username: String, // 用户名
    password: String, // 密码
    isAdmin:{
        type:Boolean,
        default:false
    }, //   是不是管理员
});
```

2、创建一个名称为Model文件夹并且创建一个user.js文件

```
const mongoose = require('mongoose');
const userSchema = require('../Schema/user');

// 对上面的导入的user的schema生成一个User的model并导出
module.exports = mongoose.model('User',userSchema);

```
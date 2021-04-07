# MongoDB

## 数据库（Database）

- 数据库是按照数据结构来组织、存储和管理数据的仓库。
- 我们的程序都是在内存中运行的，一旦程序运行结束或者计算机断电，程序运行中的数据都会丢失。
- 所以我们就需要将一些程序运行的数据持久化到硬盘之中，以确保数据的安全性。而数据库就是数据持久化的最佳选择。

## 数据库分类

### 一、关系型数据库（RDBMS）

- MySQL、Oracle、DB2、SQL Server...
- 关系型数据库中全是表

### 二、非关系型数据库（No SQL 、Not Only SQL）

- MongoDB、Redis...
- 键值对数据库
- 文档数据库MongoDB

## MongoDB简介

- MongoDB是为了快速开发互联网Web应用而设计的数据库系统。
- MongoDB的设计目标是极简、灵活、作为Web应用栈的一部分。
- MongoDB的数据模型是面向文档的，所谓面向文档是一种类似于JSON的结构，简单理解MongoDB这个数据库中存的是各种各样的JSON。

## 三个概念

- 数据库（database）

  -- 数据库是一个仓库，在仓库中可以存放**集合**。

- 集合（collection）

  -- 集合类似于数组，在集合中可以存放**文档**。

- 文档（document）

  -- 文档数据库中的最小单位，我们存储和操作的内容都是文档。

---

## 下载MongoDB

- 下载地址

  -- [MongoDB官方下载链接](https://www.mongodb.org/dl/win32)

- MongoDB的版本，奇数版本为开发版。

- MongoDB对于32位系统支持不佳，所以3.2版本之后没有再对32位系统的支持。

## 安装MongoDB

### 一、安装

### 二、配置环境变量到PATH中

- C:\Program Files\MongoDB\Server\3.2\bin

### 三、在C盘根目录

1. 创建一个文件夹`data`
2. 在data中创建一个文件夹`db`

### 四、打开cmd命令行窗口

#### 1、输入`mongod`命令可以启动MongoDB服务

- 可以在启动时设置数据库存放路径

  > mongod --dbpath F:\data\db

- 可以设置启动的端口号

  > mongod --port 端口号（不可超过65535）

#### 2、32位操作系统的注意：

- 启动服务器时，需要输入以下内容，首次启动时输入

  > mongod --storageEngine=mmapv1

#### 3、（可选）将MongoDB设置为系统服务，可以自动在后台启动，不需要每次手动启动，步骤如下：

1. 创建目录，在data下创建两个目录：db和log

2. 创建配置文件，在目录 D:\Program Files\MongoDB\Server\4.4 下添加一个配置文件 mongod.cfg，编写如下内容：

   > systemLog:
   >	destination: file
   > 	path: c:\data\log\mongod.log
   >storage:
   > 	dbPath: c:\data\db
   
3. 以管理员身份打开命令行窗口，执行如下命令

   > sc.exe create MongoDB binPath= "\"C:\Program Files\MongoDB\Server\3.6\bin\mongod.exe\" --service --config=\"C:\Program Files\MongoDB\Server\3.6\mongod.cfg\"" DisplayName= "MongoDB" start= "auto"

4. 启动mongodb服务

   > 任务栏右键	--->	任务管理器	--->	服务	--->	打开服务	--->	找到MongoDB服务	--->	右键启动

5. 如果启动失败，证明上面的操作有误

   在控制台上输入 `sc delete MongoDB` 删除之前配置的服务，然后从头来过

### 五、再打开一个cmd窗口

- 输入`mongo`命令连接mongodb数据库，出现 `>` 表示数据库连接成功

---

## Mongo的基本操作

- 显示当前连接下的所有数据库

  > show  dbs	或	show  databases

- 创建或进入到指定数据库

  > use  数据库名

- 查看当前所处的数据库

  > db

- 查看数据库中的所有集合

  > show  collections

## 数据库的CRUD的操作

### 一、向集合中插入文档

#### 1、向集合中插入一个或多个文档：

- > 语法：db.\<collection\>.insert(document)
  >
  > - 当插入一个文档时，insert()中传递一个对象作为参数
  > - 当插入多个文档时，insert()中传递一个数组作为参数

```sql
use test
-- 向test数据库中的students集合中插入一个学生对象
db.students.insert({name:"xiaozhang",age:21,gender:"男"})
-- 向test数据库中的students集合中插入多个学生对象
db.students.insert([
    {name:"xiaozhang",age:18,gender:"男"},
    {name:"xiaohuang",age:19,gender:"女"},
    {name:"xiaoliu",age:19,gender:"女"}
])
```

#### 2、向集合中插入一个文档：

- > 语法：db.\<collection\>.insertOne(document)

#### 3、向集合中插入多个文档：

- > 语法：db.\<collection\>.insertMany(document)

#### 注意事项：

- 向集合中插入文档时，如果没有给文档指定 `_id`的值，那么系统会自动给文档增加一个 `_id`，该属性用来作为文档的唯一标识。
- `_id` 我们可以自己指定，如果我们指定了改值，那么数据库将不再添加，但必须确保改值的唯一性

### 二、查询当前集合中的所有文档

#### 1、查询集合中所有符合条件的文档

- > 语法：db.\<collection\>.find(document)
  >
  > - find()用来查询集合中所有符合条件的文档
  >
  > - find()可以接收一个对象作为条件参数
  >
  >   { } 表示查询集合中所有的文档
  >
  >   { 属性：属性值，属性：属性值... } 查询属性是指定值得文档
  >
  > - find返回的是一个数组

```sql
-- 查询student集合中所有文档
db.student.find()
db.student.find({})
-- 查询student集合中name值为zhang的文档
db.student.find({name:"zhang"})
-- 查询student集合中name值为zhang，且age值等于18的文档
db.student.find({name:"zhang",age:18})
-- 查询student集合中age值等于18的第一个文档
db.student.find({age:18})[0]
```

#### 2、查询集合中符合条件的一个文档

- > 语法：db.\<collection\>.findOne(document)
  >
  > - 返回的是一个文档对象

```sql
-- 查询student集合中name值为zhang的第一个文档
db.student.findOne({name:"zhang"})
-- 查询student集合中name值为zhang的第一个文档的age值
db.student.findOne({name:"zhang"}).age
```

#### 3、查询集合中文档的总个数

- > 语法：db.\<collection\>.find({}).count() 	或	db.\<collection\>.find({}).length()

###  三、修改集合中文档的内容

#### 1、修改文档内容

- > 语法：db.\<collection\>.update(查询条件，新对象，[修改的配置选项])
  >
  > - update()默认情况下会使用新对象来替换旧的对象
  > - 如果需要修改指定的属性，而不是替换需要使用  ”`修改修饰符`“  来完成修改
  >   - $set  可以用来修改文档中的指定属性
  >   - $unset  可以用来删除文档中的指定属性
  > - update()默认只会修改符合条件的第一个文档
  > - 修改配置选项
  >   - multi  是否修改符合条件的多个文档，默认为false，true表示修改符合条件的多个文档

```sql
-- 将student集合中name值为zhang的文档的age值修改为28，此写法会使用新对象来替换旧的对象
db.student.update({name:"zhang"},{age:28})
-- 将student集合中name值为zhang的文档的age值修改为12，默认只会修改第一个文档
db.student.update(
    {name:"zhang"},
	{
    	$set:{age:12}
    }
)
-- 将student集合中name值为zhang的文档中gender属性删除
db.student.update(
    {name:"zhang"},
	{
    	$unset:{gender:1}
    }
)
-- 将student集合中name值为zhang的文档的age值修改为12，修改符合条件的文档
db.student.update(
    {name:"zhang"},
	{
    	$set:{age:12}
    },
    {
    	multi:true
    }
)
```

#### 2、修改一个符合条件的值

- > 语法：db.\<collection\>.updateOne(查询条件，新对象)
  >
  > - 默认情况下，只会修改集合中符合条件的第一个文档

```sql
-- 将student集合中name值为zhang的文档的age值修改为12，修改符合条件的第一个文档
db.student.updateMany(
    {name:"zhang"},
	{
    	$set:{age:12}
    }
)
```

#### 3、同时修改符合多个符合条件的文档

- > 语法：db.\<collection\>.updateMany(查询条件，新对象)
  >
  > - updateMany()默认会同时修改符合条件多个文档

```sql
-- 将student集合中name值为zhang的文档的age值修改为12，修改符合条件的多个文档
db.student.updateMany(
    {name:"zhang"},
	{
    	$set:{age:12}
    }
)
```

#### 4、替换一个文档

- > 语法：db.\<collection\>.replaceOne(查询条件，新对象)
  >
  > - 会使用新对象来替换旧的对象

```sql
-- 将student集合中age值为18的文档替换age值为28的文档，此写法会使用新对象来替换旧的对象
db.student.replaceOne({age:28},{age:18})
```

### 四、删除集合中的文档

#### 1、删除符合条件的所有文档

- > 语法：db.\<collection\>.remove(条件，删除的配置选项)
  >
  > - remove()可以根据条件来删除文档，传递的条件的方式跟find()一样
  >
  > - 默认删除符合条件多个文档
  >   - 如果remove()的第二个参数传递一个true，则会删除第一个文档
  > - remove()中必须传递参数，否则会报错，而find()可传递也可不传递
  > - 如果只传递一个空对象，那么会清空所有文档，一个个删除，效率低

```sql
-- 将student集合中age值为18的文档删除，默认删除符合条件的所有文档
db.student.remove({age:18})
-- 将student集合中age值为18的文档删除，删除符合条件的第一个文档
db.student.remove({age:18}，true)
-- 将student集合中的所有元素删除,一个个删除
db.student.remove({})
```

#### 2、删除一个文档

- > 语法：db.\<collection\>.deleteOne()

```sql
-- 将student集合中age值为18的文档删除，删除符合条件的第一个文档
db.student.deleteOne({age:18}，true)
```

#### 3、删除多个文档

- > 语法：db.\<collection\>.deleteMany()

```sql
-- 将student集合中age值为18的文档删除，默认删除符合条件的所有文档
db.student.deleteMany({age:18})
```

### 五、删除集合（Collection）

- > 语法：db.\<collection\>.drop()

### 六、删除数据库（DataBase）

- > 语法：db.dropDatabase()

---

##  安装图形化工具

### 一、NoSQL Manager for MongoDB Freeware

#### 1、从[官网](https://www.mongodbmanager.com/download)下载软件

### 二、Studio 3T for MongoDB

---

## 练习

### 1、进入my_test数据库

```sql
use my_test
```

### 2、向数据库的user集合中插入一个文档

```sql
db.user.insert({username:"sunwukong"})
```

### 3、查询user集合中的文档

```sql
db.user.find()
```

### 4、向数据库的user集合中插入一个文档

```sql
db.user.insertOne({username:"zhubajie"})
```

### 5、查询数据库user集合中的文档

```sql
db.user.find()
```

### 6、统计数据库user集合中的文档数量

```sql
db.user.find().count()
```

### 7、查询数据库user集合中username为sunwukong的文档

```sql
db.user.find({username:"sunwukong"})
```

### 8、向数据库user集合中的username为sunwukong的文档，添加一个address属性，属性值为huaguoshan

```sql
db.user.update(
    {username:"sunwukong"},
    {
    	$set:{
    		address:"huaguoshan"
    	}
   	}
)
```

### 9、使用{username:"tangseng"}替换username为zhubajie的文档

```sql
db.user.replaceOne({username:"zhubajie"},{username:"tangseng"})
```

### 10、删除username为sunwukong的文档的address属性

```sql
db.user.update(
    {username:"sunwukong"},
    {
    	$unset:{address:1}
	}
)
```

### 11、向username为sunwukong的文档中，添加一个hobby：{cities:["beijing","shanghai","shenzhen"],movies:["sanguo","hero"]}

```sql
-- MongoDB的文档的属性值是一个文档时，那么这个文档称为内嵌文档
db.user.update(
	{username:"sunwukong"},
    {
    	$set:{
    		hobby：{
    			cities:["beijing","shanghai","shenzhen"],
    			movies:["sanguo","hero"]
    		}
    	}
    }
)
```

### 12、向username为tangseng的文档中，添加一个hobby：{movies:["A Chinese Odyssey","King of comedy"]}

```sql
db.user.update(
    {username:"tangseng"},
    {
    	$set:{
    		hobby：{
    			movies:["A Chinese Odyssey","King of comedy"]
    		}
    	}
    }
)
```

### 13、查询喜欢电影hero的文档

```sql
-- MongoDB支持直接通过内嵌文档的属性进行查询，如果要查询内嵌文档则可以通过 . 的形式来匹配
-- 如果要通过内嵌文档来对文档进行查询，此时属性名必须使用引号
db.user.find({'hobby.movies':"hero"})
```

### 14、向tangseng中添加一个新的电影Interstellar

```sql
-- $push  向数组中添加一个新元素，可重复添加
-- $addToSet  向数组中添加一个新元素，如果数组集合中存在该元素，则不会重复添加
db.user.update(
    {username:"tangseng"},
    {
    	$push:{
    		"hobby.movies":"Interstellar"
    	}
    }
)
```

### 15、删除喜欢beijing的用户

```sql
db.user.remove({"hobby.cities":"beijing"})
```

### 16、删除user集合

```sql
db.user.remove({})	-- 清空集合
db.user.drop()
```

### 17、向numbers中插入20000条数据

```sql
-- 效率低
for(var i = 1;i <= 20000;i++){
	db.numbers.insert({num:i})
}
-- 优化
var arr = []
for(var i = 1;i <= 20000;i++){
	arr.push({num:i})
}
db.numbers.insert(arr)
```

### 18、查询numbers中num为500的文档

```sql
db.numbers.find({num:500})
```

### 19、查询numbers中num大于5000的文档

```sql
db.numbers.find({num:{$gt:5000}})
```

### 20、查询numbers中num小于30的文档

```sql
db.numbers.find({num:{$lt:30}})
```

### 21、查询numbers中num大于40小于50的文档

```sql
db.numbers.find({num:{$gt:40,$lt:50}})
```

### 22、查询numbers中num大于19996的文档

```sql
db.numbers.find({num:{$gt:19996}})
```

### 23、查看numbers集合中的前10条数据

```sql
-- limit()设置显示数据的上限
-- 在开发时，绝对不会执行不带条件的查询
db.numbers.find().limit(10)
```

### 24、查看numbers集合中的第11条到20条的数据

```sql
-- skip()用于跳过指定数量的数据
-- skip((页码-1) * 每页显示的条数).limit(每页显示的条数)
-- MongoDB会自动调整skip和limit的顺序
db.numbers.find().skip(10).limit(10)
```

### 25、查看numbers集合中的第21条到30条的数据

```sql
db.numbers.find().skip(20).limit(10)
```

### 26、查询numbers集合中num大于8000或者num小于2000的文档

```sql
db.numbers.find({$or:[{num:{$gt:8000},num:{$lt:2000}}]})
```

## 文档之间的关系

### 一、一对一（one to one）

> 夫妻（一个丈夫	对应	一个妻子）
>
> 在MongoDB中，可以通过内嵌文档的形式体现出一对一的关系

```sql
db.home.insert({
	name:"黄蓉",
	husband:{
		name:"郭靖"
	}
})
```

### 二、一对多(ont to many)/多对一(many to one)

> 用户	---	订单

### 三、多对多(many to many)

> 分类	---	商品

---

## sort和投影

- > 查询文档时，默认情况是按照 _id 的值进行排序（升序）
  >
  > sort()用来指定文档的排序的规则，sort()需要传递一个对象来指定排序规则：1表示升序，-1表示降序
  >
  > - limit()  skip()  sort()  可以以任意的顺序进行调用

```sql
-- 在集合emp中，先按照sal升序排列，在根据empno降序排列
db.emp.find().sort({sal:1,empno:-1})
```

- > 在查询时，可以在第二个参数的位置来设置查询结果的投影
  >
  > - 1：表示显示
  > - 0：表示不显示

```sql
-- 查询结果中只显示ename，sal字段
db.emp.find({},{ename:1,_id:0,sal:1})
```

---

## Mongoose

### 一、简介

- 之前我们都是通过shell来完成对数据库的各种操作的，在开发中大部分时候我们都需要通过程序来完成对数据库的操作
- 而Mongoose就是一个可以让我们通过Node来操作的MongoDB的模块
- Mongoose是一个对象文档模型（ODM）库，它对Node原生的MongoDB模块进行了进一步的优化封装，并提供了更多的功能
- 在大多数情况下，它被用来把结构化的模式应用到一个MongoDB集合，并提供了验证和类型转换等好处

### 二、Mongoose的好处

- 可以为文档创建一个模式结构（Schema）
- 可以对模型中的对象/文档进行验证
- 数据可以通过类型转换为对象模型
- 可以使用中间件来应用业务逻辑挂钩
- 比Node原生的MongoDB驱动更容易

### 三、新的对象

- mongoose中为我们提供了几个新的对象

  --   Schema（模式对象）

  - Schema对象定义约束了数据库中的文档结构

  --  Model

  - Model对象作为集合中的所有文档的表示，相当于MongoDB数据库中的集合collection

  --  Document

  - Document表示集合中的具体文档，相当于集合中的一个具体的文档






















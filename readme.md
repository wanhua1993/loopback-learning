# loopback
## 安装
>>   安装LoopBack CLI工具
```
npm install -g loopback-cli
```
>>   更新 安装
```
    npm uninstall -g strongloop
    npm cache clear
    npm install -g strongloop 
```
## 初始化项目：
>>如果使用 loopback-cli 
```
    lb
```
>>如果使用 slc 
```
    slc loopback
```
>> 如果使用 apic 
```
    apic loopback
```
## 定义一个实例：
>> 使用loopback工具
```
    lb model <model-name>
```

    1、PersisteModel: 是连接到持久化数据源的所有模型的基础对象
    2、Expose CoffeeShop via the REST API? (Y/n) Y 通过REST暴露模型
    3、Custom plural form (used to build REST URL): 接收默认复数形式
    4、Common model or server only? 系统会询问您是要在服务器上还是在/common目录中创建模型
    5、 Let's add some CoffeeShop properties now.
        Enter an empty property name when done.
        [?] Property name: name 定义属性名字
            invoke   loopback:property
        [?] Property type: (Use arrow keys)  选择属性类型
        ❯ string
          number
          boolean
          object
          array
          date
          buffer
          geopoint
          (other)
        [?] Required? (y/N) 是否需要
        ? Default value[leave blank for none]: 定义默认值
>> 使用 slc
```
    slc loopback:model
```
+ 创建新模型时，生成器将提示您输入模型中的属性。随后，您可以使用属性生成器向其添加更多属性;
        
## lb datasource 定义数据源 数据库

## lb property 定义属性

## 远程注册一个方法
```
User.remoteMethod(
        'getName', // 方法名字
        {
            http: { path: '/getname', verb: 'get' }, // 路径 请求方式
            accepts: { arg: 'id', type: 'number', http: { source: 'query' } },  接收的参数和参数传递的形式
            returns: { arg: 'name', type: 'string' }  返回
        }
    );
```
## loopBack 中间件
+ 1、定义静态中间件来为目录中的文件提供服务  /client
```
"files": {
                "loopback#static": {
                    "params": "$!../client" 
                }
            }
```
            
+ 这些行定义了静态中间件 它使应用程序将目录中的 /client 文件作为静态内容提供。这些  $! 字符表明路径是相对于的位置  middleware.json。

    在 /server/boot/  中定义 router路由 可以在里面定义一些接口和方法； 

    LoopBack框架便于连接到存储身份数据的后端资源，例如数据库和Web服务。
    但是，框架本身不存储实际数据;

    数据采集：LoopBack开发者/用户的责任是确保数据收集符合GDPR并保护他们自己的数据。
    数据存储：LoopBack开发者/用户的责任是保护他们自己的数据。   
    数据访问：LoopBack开发人员/用户的责任是确定他们自己数据的角色和访问权限。
    数据处理：LoopBack开发者/用户的责任是保护他们自己的数据。
    数据删除
    数据监测

##  lb relation 定义关系

##  lb acl 定义访问控制
    LoopBack应用程序通过模型访问数据，因此控制对数据的访问意味着对模型进行限制; 
    即指定谁或什么可以读取和写入数据或在模型上执行方法;
    LoopBack访问控制由访问控制列表 或ACL确定;

    拒绝所有人的所有端点。这通常是定义ACL时的起点，因为您可以选择性地允许访问特定操作。
    ? Select the model to apply the ACL entry to: (all existing models)
    ? Select the ACL scope: All methods and properties
    ? Select the access type: All (match all types)
    ? Select the role: All users
    ? Select the permission to apply: Explicitly deny access

    现在让每个人都可以阅读评论。
    ? Select the model to apply the ACL entry to: Review
    ? Select the ACL scope: All methods and properties
    ? Select the access type: Read
    ? Select the role: All users
    ? Select the permission to apply: Explicitly grant access

    允许经过身份验证的用户阅读咖啡店 ; 也就是说，如果您已登录，则可以查看所有咖啡店。
    ? Select the model to apply the ACL entry to: CoffeeShop
    ? Select the ACL scope: All methods and properties
    ? Select the access type: Read
    ? Select the role: Any authenticated user
    ? Select the permission to apply: Explicitly grant access

    允许经过身份验证的用户撰写评论 ; 也就是说，如果您已登录，则可以添加评论。
    ? Select the model to apply the ACL entry to: Review
    ? Select the ACL scope: A single method
    ? Enter the method name: create
    ? Select the role: Any authenticated user
    ? Select the permission to apply: Explicitly grant access

    现在，使审查的作者（其“所有者”）可以对其进行任何更改。
    $ lb acl
    ? Select the model to apply the ACL entry to: Review
    ? Select the ACL scope: All methods and properties
    ? Select the access type: Write
    ? Select the role: The user owning the object
    ? Select the permission to apply: Explicitly grant access

## 定义一个远程钩子

>> automigrate: 根据模型定义自动创建或重新创建表模式;

>> autoupdate: 根据模型定义自动更改表格模式;

>> 如果数据库具有现有的表，则运行  automigrate() 将丢失并重新创建表,因此可能导致数据丢失。为了避免这个问题的使用  autoupdate();
```
var ds = Model.app.dataSources.oracle;

    ds.createModel(schema_v1.name, schema_v1.properties, schema_v1.options);

    ds.automigrate(function () {
        ds.discoverModelProperties('CUSTOMER_TEST', function (err, props) {
            console.log(props);
        });
    });

    ds.autoupdate(schema_v2.name, function (err, result) {
        ds.discoverModelProperties('CUSTOMER_TEST', function (err, props) {
            console.log(props);
        });
    });

    dataSource.isActual(models, function(err, actual) {
        if (!actual) {
            dataSource.autoupdate(models, function(err, result) {
               // ...
            });
        }
    });
```
    

>   警告：不要将生产数据库凭据显式放入JSON或JavaScript文件中，因为这些文件可能是安全漏洞。相反，加载环境变量的值
```
var DataSource = require('loopback-datasource-juggler').DataSource;
    
    var dataSource = new DataSource({
        connector: require('loopback-connector-mongodb'),
        host: 'localhost',
        port: 27017,
        database: 'mydb'
    }); 
    定义表
    var User = ds.define('User', {
        name: String,
        bio: String,
        approved: Boolean,
        joinedAt: Date,
        age: Number
    });


    数据库事务
    await app.dataSources.db.transaction(async models => {
        const {MyModel} = models;
        console.log(await MyModel.count()); // 0
        await MyModel.create({foo: 'bar'});
        console.log(await MyModel.count()); // 1
    });
    console.log(await app.models.MyModel.count()); // 1

    try {
        await app.dataSources.db.transaction(async models => {
            const {MyModel} = models;
            console.log(await MyModel.count()); // 0
            await MyModel.create({foo: 'bar'});
            console.log(await MyModel.count()); // 1
            throw new Error('Oops');
        });
    } catch (e) {
        console.log(e); // Oops
    }
    console.log(await app.models.MyModel.count()); // 0

    Post.beginTransaction({
        isolationLevel: Transaction.READ_COMMITTED,
        timeout: 30000 // 30000ms = 30s
    }, function(err, tx) {
        tx.observe('timeout', function(context, next) {
            // handle timeout
            next();
        });
    });
    transaction.commit(function(err) {

    });
    或者回滚交易：

    transaction.rollback(function(err) {

    });
    tx.observe('before commit', function(context, next) {
        // ...
        next();
    });

    tx.observe('after commit', function(context, next) {
        // ...
        next();
    });

    tx.observe('before rollback', function(context, next) {
        // ...
        next();
    });

    tx.observe('after rollback', function(context, next) {
        // ...
        next();
    });

    tx.observe('timeout', function(context, next) {
        // ...
        next();
    });
```

## 定义远程方法
    create() replaceOrCreate() pathOrCreate() exsits() findById() find()
    findOne() destroyById() or deleteById() count() replaceById() 
    prototype.patchAttributes()
    createChangeStream()
    updateAll()
    replaceOrCreate()
    replaceById()

    要通过Rest公开模型，则将public属性设置为true;
    隐藏接口方法 可以通过调用disableRemoteMethodByName()来隐藏;
    Location.disableRemoteMethodBYName('deleteById);
    或者 
        "options": {
            "remoting": {
                "sharedMethods": {
                    "*": false,
                    "login": true,
                    "logout": true
                }
            }
        }

## 验证属性的方法
+ validatesAbsenceOf 验证缺少一个或多个指定的属性。模型不应包含被视为有效的财产; 验证字段不为空时失败
+ validatesExclusionOf 验证排除。要求属性值不在指定的数组中；
+ validatesFormatOf 验证格式。要求模型包含与给定格式相匹配的属性。
+ validatesInclusionOf 验证包含在集合中。要求属性的值位于指定的数组中。
+ validatesLengthOf 验证长度。要求属性长度在指定的范围内；
+ validatesNumericalityOf 验证数字性。要求属性的值为整数或数字。
+ validatesPresenceOf 验证是否存在一个或多个指定的属性。要求模型包含一个被认为有效的属性; 验证字段为空时失败。
+ validatesUniquenessOf 验证唯一性。确保该物业的价值对于该模型是唯一的。
+ validatesDateOf 验证属性的值是否为日期。要求属性的模型值为Date类型。
```
module.exports = function(user) {
    user.validatesPresenceOf('name', 'email');
    user.validatesLengthOf('password', {min: 5, message: {min: 'Password is too short'}});
    user.validatesInclusionOf('gender', {in: ['male', 'female']});
    user.validatesExclusionOf('domain', {in: ['www', 'billing', 'admin']});
    user.validatesNumericalityOf('age', {int: true});
    user.validatesUniquenessOf('email', {message: 'email is not unique'});  
};
```
    

## 模型关系
+    BelongsTo relations
+    HasOne relations
+    HasMany relations
+    HasManyThrough relations
+    HasAndBelongsToMany relations
+    Polymorphic relations
+    Embedded relations (embedsOne and embedsMany)

## 扩展用户模型

```
var properties = {
        firstName: {
            type: String,
            required: true        
         }       
    };    

 var options = {
        relations: {    
        accessTokens: {        
            model: accessToken,       
            type: hasMany,
            foreignKey: userId
        },
        account: {
            model: account,
            type: belongsTo
        },
        transactions: {
            model: transaction,
            type: hasMany
        }
    },
        acls: [{
            permission: ALLOW,
            principalType: ROLE,
            principalId: $everyone,
            property: myMethod
        }]
    };       

var user = loopback.Model.extend('user', properties, options);
```
        

## 建立一个自定义模型
```
    MyModel = Model.extend('MyModel');

    MyModel.on('myEvent', function() {
        console.log('meep meep!');
    });

    MyExtendedModel = MyModel.extend('MyExtendedModel');

    MyModel.emit('myEvent');

    MyModel.setup = function() {
        var MyModel = this;
        MyModel.on('myEvent', function() {
            MyModel.printModelName();
        });
    }
```
    

>> 获取相关模型的实例 --------> 关系RESTAPI
+    1、 按照从一个模型到另一个模型的关系来获取关联模型的实例
        GET /<model1-name>/<instanceID>/<model2-name>
        <instanceID> - model1中的实例ID。
        <model1-name> - 第一个模型的名称。 
        <model2-name> - 第二个相关模型的名称。
+    2、 获取许多相关的模型实例
        列出<model-name> 由instance-IDhasMany关系标识的指定的相关模型实例
        GET /<model-name>/<instance-ID>/<hasManyRelationName>

+    3、 创建hasMany相关模型实例
        为hasMany关系创建<model-name>由 指定的指定的相关模型实例 <instance-ID>。
        POST /<model1-name>/<instance-ID>/<hasMany-Relation-Name>

+    4、 删除有许多相关的模型实例
        删除指定的相关模型实例  由`确定`，因为有很多关系。
        DELETE /<model1-name>/<instance-ID>/<hasMany-relation-name>

+    5、 列出属于相关模型实例 
        列出由<instance-ID>hasMany关系标识的给定模型的相关模型实例。
        GET /model-name/<instance-ID>/<belongsTo-relation-name>

+    6、 关系之后的聚合模型
        通常需要在对查询的响应中包含相关的模型实例，以便客户端不必进行多次调用。
        GET /<model1-name>?filter[include]=...
        include - 描述要包含的关系层次的对象

```    
    GET /api/members?filter[include]=posts
    GET /api/members?filter[include][posts]=author
    GET /api/members?filter[include][posts]=author&filter[where][age]=21
    GET /api/members?filter[include][posts]=author&filter[limit]=2
    GET /api/members?filter[include]=posts&filter[include]=passports    
    Report.findById(1, {include: 'lineitems'});
 ```
>>   角色 REST API

## 远程钩子 
    1、beforeRemote     在远程方法之前运行   
    2、afterRemote      在远程方法成功完成后运行。
    3、afterRemoteError 在远程方法完成并发生错误后运行

```
    modelName.beforeRemote( methodName, function( ctx, next) {
        //...
        next();
    });

    modelName.afterRemote( methodName, function( ctx, modelInstance, next) {
        //...
        next();
    });

    modelName.afterRemoteError( methodName, function( ctx, next) {
        //...
        next();
    });
```
+ modelName 是远程钩子所连接模型的名称
+ methodName是触发远程挂钩的方法的名称。
+ ctx是上下文对象。 
+ modelInstance 是受影响的模型实例。
+ 星号  '*' 匹配任何字符，直到分隔符字符'.'（句点）的第一次出现为止。
+ 双星号匹配任何字符，包括分隔符  '.' （句点）。  

```
    module.exports = function(Car) {
        Car.revEngine = function(sound, cb) {
            cb(null, sound - ' ' - sound - ' ' - sound);
        };
        Car.remoteMethod(
            'revEngine',
            {
            accepts: [{arg: 'sound', type: 'string'}],
            returns: {arg: 'engineSound', type: 'string'},
            http: {path:'/rev-engine', verb: 'post'}
            }
        );
        
        Car.beforeRemote('revEngine', function(context, unused, next) {
            console.log('Putting in the car key, starting the engine.');
            next();
        });
        
        Car.afterRemote('revEngine', function(context, remoteMethodOutput, next) {
            console.log('Turning off the engine, removing the key.');
            next();
        });
    }
```
    
在远程方法名称中使用通配符。每当执行名称以“save”结尾的远程方法时，将调用此远程挂接
```
    Customer.beforeRemote('*.save', function(ctx, unused, next) {
        if(ctx.req.accessToken) {
            next();
        } else {
            next(new Error('must be logged in to update'))
        }
    });

    Customer.afterRemote('*.save', function(ctx, user, next) {
        console.log('user has been saved', user);
        next();
    });
```
    
    1、接口调用执行前和执行后，分别对应 beforeRemote 和 afterRemote；
    2、CRUD 操作前或后，注册的方法被执行；

## 中间件
注册中间件的方法：
+ 在middleware.json中注册中间件
+ 在JavaScript中注册中间件 

预定义的阶段是：

+ initial - 中间件可以运行的第一个点。
+ session - 准备会话对象。
+ auth - 处理身份验证和授权。
+ parse - 解析请求正文。
+ routes - 实现应用程序逻辑的HTTP路由。通过快递API注册中间件  app.use，  app.route，  app.get （和其他HTTP动词）运行在这个阶段的开始。将此阶段也用于子应用程序，如   loopback/server/middleware/rest 或  loopback-explorer。

+ files - 提供静态资产（请求在此处访问文件系统）。

+ final - 处理错误和未知URL请求。
除了主要阶段之外，每个阶段都具有“之前”和“之后”子阶段，在阶段名称之后编码，由冒号分隔。例如，对于“初始”阶段，中间件按以下顺序执行：
1、initial:before 
2、initial
3、initial:after
单个子阶段中的中间件按其注册的顺序执行。但是，你不应该依赖这样的命令。在订单重要时，始终使用适当的阶段明确订购中间件。

### 常规中间件
```
module.exports = function() {
  return function myMiddleware(req, res, next) {
    // ...
  }
};
```
### 错误处理中间件
```
function myErrorHandler(err, req, res, next) {
  // ...
}
```
### 静态中间件的示例或条目，用于从multiple 项目根目录中的目录提供内容  ：

```
"files": {
  "loopback#static": [{
    "name": "x",
    "paths": ["/x"],
    "params": "$!../client/x"
  },
  {
    "name": "y",
    "paths": ["/y"],
    "params": "$!../client/y"
  }]
}
```
要使用LoopBack阶段API注册中间件，请使用以下  app 方法：

+ middleware() 
+ middlewareFromConfig()`
+ defineMiddlewarePhases()
例如：服务器/ server.js
```
var loopback = require('loopback');
var morgan = require('morgan');

var app = loopback();

app.middleware('routes:before', morgan('dev'));
app.middleware('routes', loopback.rest());
```
>> 预处理中间件
```module.exports = function() {
  return function tracker(req, res, next) {
    console.log('Request tracking middleware triggered on %s', req.url);
    var start = process.hrtime();
    res.once('finish', function() {
      var diff = process.hrtime(start);
      var ms = diff[0] * 1e3 + diff[1] * 1e-6;
      console.log('The request processing time is %d ms.', ms);
    });
    next();
  };
};
```
```
{
  "initial": {
    "./middleware/tracker": {}
  }
}
```
## 错误处理程序
```{
  "final:after": {
    "strong-error-handler": {
      "params": {
         "debug": false,
         "log": true
       }
    }
  }
}
```
+ 一般来说，strong-error-handler必须是最后注册的中间件功能;

## loopback 组件
### OAuth 2.0
+ OAuth 2.0提供身份验证和授权客户端应用程序和用户访问受保护的API端点。
+ Authorization server: 成功验证资源所有者并获得授权后，将访问令牌颁发给客户端。该组件实现OAuth 2.0协议端点，包括 授权端点 和 令牌端点。
+ Resource server: 承载受保护的资源，并能够使用访问令牌接受和响应受保护的资源请求。该组件提供中间件来保护API端点，以便只接受具有有效OAuth 2.0访问令牌的请求。它还建立身份，例如客户端应用程序ID和用户ID，以进一步访问控制和个性化。
+ 授权服务器可以是与资源服务器相同的服务器或单独的服务器。单个授权服务器可以发出由多个资源服务器接受的访问令牌。
>> 安装
```
npm install loopback-component-oauth2
```
>> 使用
```
var oauth2 = require('loopback-component-oauth2');

var options = {
    dataSource: app.dataSources.db, // Data source for oAuth2 metadata persistence
    loginPage: '/login', // The login page URL
    loginPath: '/login' // The login form processing URL
};

oauth2.oAuth2Provider(
    app, // The app instance
    options // The options
);
```
### 存储组件
>> 安装
```
npm install loopback-component-storage
```
>> 本地存储
```
var ds = loopback.createDataSource({
    connector: require('loopback-component-storage'),
    provider: 'filesystem',
    root: path.join(__dirname, 'storage')
});

var container = ds.createModel('container');
```
>> 亚马逊存储
```
var ds = loopback.createDataSource({
    connector: require('loopback-component-storage'),
    provider: 'amazon',
    key: 'your amazon key',
    keyId: 'your amazon key id'
});
var container = ds.createModel('container');
app.model(container);
```
>> API
```
GET /api/containers 列出当前存储提供程序的所有容器

GET /api/containers/<container-name>  获取有关指定容器的信息。

POST /api/containers  使用当前存储提供程序创建新容器

DELETE /api/containers/<container-name>  删除指定的容器

GET /api/containers/<container-name>/files  按名称列出给定容器中的所有文件

GET /api/containers/<container-name>/files/file-name  按名称获取给定容器中文件的信息

DELETE /api/containers/container-name/files/file-name  删除指定容器中的指定文件

POST /api/containers/container-name/upload  按名称将一个或多个文件上载到给定容器中。请求体应使用 HTML的文件输入类型使用的  multipart / form-data

GET /api/containers/container-name/download/file-name  在指定容器中下载指定文件
```
>> 存储数据持久性
```
{
  "db": {
    "name": "db",
    "connector": "memory",
    "file": "mydata.json"
  }
}
```
```
var memory = loopback.createDataSource({
    connector: loopback.Memory,
    file: "mydata.json"
});
```
+ 默认情况下，内存连接器中的数据是瞬态的。当使用内存连接器的应用程序退出时，所有模型实例都将丢失。要在应用程序重新启动之间维护数据，请file在创建数据源时指定一个JSON文件，在该文件中使用属性存储数据
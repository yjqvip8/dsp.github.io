##### 1. 创建一个angular实例

```HTML
<!-- 以下代码默认已导入angular -->
<div ng-app="myApp" ng-controller="myCtrl">
	{{ myName }}
</div>
<script>
	var app = angular.module("myApp",[]);
    app.controller("myCtrl",function($scope){
        $scope.myName = "张三";
    });
</script>
```

##### 2. 使用 ng-init 初始化数据

```HTML
<!--
	ng-app：指令定义了 AngularJS 应用程序的 根元素。
	ng-init：定义初始值
		name = ""; parse = {type:"",name:""}; god=[1,2,3,4,5]; ... 
-->
<div ng-app="" ng-init="ngName='张三'">
    {{ ngName }}
</div>
```

##### 3. angular - 指令

```HTML
ng-if ng-show ng-click

<!-- ng-model 使用变量需提前声明 -->
<input type="text" name="uname" ng-model="myName" />

<!-- ng-repeat 可便利属性 类似：ng-for v-for -->
<p ng-repeat="note in notes">
	{{ note.title }} : {{ note.text }}
</p>

<!-- 自定义指令 app.directive -->
<body ng-app="nyApp">
    
    <show-name></show-name> // 元素名 张三  --- 默认
    <div show-name></div> // 属性 张三  --- 默认
    <div class="show-name"></div> // 类名 张三
    <!-- directive: runoob-directive --> // 注释 张三
    
    <script>
    	var app = angular.module("myApp",[]);
        app.directive("showName",function(){
            return {
                template:"<h2>张三</h2>"
            }
        })
    </script>
</body>
```

##### 4. $scope - module / $rootScope

```HTMl
<div ng-app="myApp" ng-controller="myCtrl">
    <input type="text" name="uname" ng-model="name" />
    <h1>{{ greeting }}</h1>
    <button ng-click="say()">点我</button>
    <ul>
        <li ng-repeat="note in notes">{{ note }}</li>
    </ul>
</div>
<script>
	var app = angular.module("myApp",[]);
    app.controller("myCtrl",function($scope,$rootScope){
        // $rootScope 连接所有的 $scope 和 controller 可直接使用
        $scope.name = "robot"; // 属性
        $scope.notes = [1,2,3,4,5];
        $scope.say = function(){ // 函数
            $scope.greeting = "Hello" + $scope.name + "!";
        }
    })
</script>
```

##### 5. controller - 控制器 - 主要是外部引用

```HTML
HTML:
<div ng-app="myApp" ng-controller="myCtrl">
    <input type="text" name="uname" ng-model="name" />
</div>
<script src="myController.js"></script>

JS:myController.js
<script>
	var app = angular.module("myApp",[]);
    app.controller("myCtrl",function($scope,$rootScope){
        $scope.name = "请输入姓名";
    })
</script>
```

##### 6. filter -  过滤器

```HTML
<div ng-app="myApp" ng-controller="myCtrl">
    {{ brthDate | index-text }}
</div>
<script>
    // uppercase 大写; lowercase 小写; currency 货币;
	var app = angular.module("myApp",[]);
    app.controller("myCtrl",function($scope){
        $scope.brthDate = new Date();
    })
    // 自定义过滤器
    app.filter("index-text",function(){
        return function(text){
            return "  " + text;
        }
    })
</script>
```

##### 7. service - 服务

```JavaScript
var app = angular.module("myApp",[]);

// 自定义服务
app.service("toStr",function(){
    this.toStr16 = function(x){
        return x.toString(16);
    }
})

app.controller("myCtrl",function($scope,$location,$http,toStr){
    // $location 优化封装的 window.location
    $scope.myUrl = $location.absUrl();
    
    // $http 内置请求模块 - 正式使用时仍需封装
    $http.get("...name").then(function(response){
        $scope.name = response.data;
    })
    
    // $timeOut $interval == setTimeOut SetInterval
    
    $scope.hex = toStr.toStr16(255);
})

// 过滤器使用服务
app.filter("forMat",["toStr",function(toStr){
    return function(x){
        return toStr.toStr16(x);
    }
}]);
```

##### 8. 核心服务 - $http

```JavaScript
var app = angular.module("myApp",[]);
app.controller("myCtrl",function($scope,$http){
    // 基础使用 
    	$http({method:"GET",url:"/someUrl"}).then(res=>{},err=>{});
    // 简写 get post put delete head jsonp patch
    	$http.get('/somUrl',config).then(res=>{},err=>{});
    	$http.post('/someUrl',data,config).then(res=>{},err=>{});
    // 其他版本
     	$http.get("/someUrl.php").success(res=>{});
})
```

##### 9. select - 选择框

```HTML
<div ng-app="myApp" ng-controller="myCtrl">
    <select ng-init="selectedName=names[0]" // 初始化值
            ng-model="selectedName" 	// 双向绑定值
            ng-options="x for x in names">  // 显示选项
    </select>
    
    <select>
        <option ng-repeat="x in names">{{ x }}</option>
    </select>
</div>
```

##### 10. route - 路由

```HTML
<div ng-app="routeDemoApp">
    <div ng-view></div>
</div>
<script>
    var app = angular.module("routeDemoApp",['ngRoute'])
    .config(['$routeProvider',function($routeProvider){
        $routeProvider.when("/",{template:'这是首页'})
                      .when("/home",{temalate:"home页面"})
                      .otherwise({redirectTo:'/'});
    }]);
    /*
    	$routeProvider.when(url,{
            template:string, //在ng-view中插入简单的html内容
            templateUrl:string, //在ng-view中插入html模版文件
            controller:string,function / array, //在当前模版上执行的controller函数
            controllerAs:string, //为controller指定别名
            redirectTo:string,function, //重定向的地址
            resolve:object<key,function> //指定当前controller所依赖的其他模块
        });
    */
</script>
```


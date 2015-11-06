Ionic : 拍照并自动上传照片
 2015年4月19日  APP

http://coolwp.com/ionic-automatically-upload-photo-after-taking-it.html

本文示例 Ionic 如何实现拍照并自动上传照片，数据存储使用的是 Firebase , 

为毛不是七牛等国内存储服务商呢？因为 Firebase 有 AngularJS 库，就这么简单。

准备

已经搭建好了的 Ionnic 平台；

到 ngcordova.com 下载 ng-cordova 库;

注册一个免费的 Firebase 账号并登录，然后下载 AngularFire ( https://www.firebase.com/docs/web/libraries/angular/ ) 和 Web Platform ( https://www.firebase.com/docs/web/  ) ；

装有 SublimeText 或其它编辑器。

新建一个空的 Ionic 应用

比如:

ionic start UploadDemo1 blank

然后进入目录

cd UploadDemo1

 添加视图层文件/代码

添加文件

在www目录下添加目录 templates,进入templates目录，添加两个如下的文件:

Ionic templates

添加代码

在www目录下的文件index.html中，在页面头部载入所需的js:

<!-- ionic/angularjs js -->
<script src="lib/ionic/js/ionic.bundle.js"></script>
<!--  添加 -->
<script src="js/ng-cordova.js"></script>
<!-- cordova script (this will be a 404 during development) -->
<script src="cordova.js"></script>
<!--  添加 -->
<script src="js/firebase.js"></script>
<!--  添加 -->
<script src="js/angularfire.min.js"></script>
<!-- your app's js -->
<script src="js/app.js"></script>
<!--  添加 -->
<script src="js/controllers.js"></script>
并将在上一阶段（“准备”）下载到的 ng-cordova.js 或 ng-cordova.min.js 复制到 www 目录下的 js 目录下，将 firebase.js 也复制到该目录下。

在上一步骤新建的 firebase.html 中添加如下代码：

<ion-view title="登录/注册">
    <ion-content>
        <div>
            <div class="list list-inset">
                <label class="item item-input">
                    <input ng-model="username" type="text" placeholder="邮箱" />
                </label>
                <label class="item item-input">
                    <input ng-model="password" type="password" placeholder="密码" />
                </label>
            </div>
            <div class="padding-left padding-right">
                <div class="button-bar">
                    <a class="button" ng-click="login(username, password)">登录</a>
                    <a class="button" ng-click="register(username, password)">注册</a>
                </div>
            </div>
        </div>
    </ion-content>
</ion-view>
 

在上一步骤新建的　photos.html 中添加如下代码:
<ion-view title="照片" ng-init="">   
    <ion-nav-buttons side="right">   
        <button class="button button-icon icon ion-camera" ng-click="upload()"></button>   
    </ion-nav-buttons>   
    <ion-content>   
        <div class="row" ng-repeat="image in images" ng-if="$index % 4 === 0">   
            <div class="col col-25" ng-if="$index < images.length">   
                <img ng-src="data:image/jpeg;base64,{{images[$index].image}}" width="100%" />   
            </div>   
            <div class="col col-25" ng-if="$index + 1 < images.length">   
                <img ng-src="data:image/jpeg;base64,{{images[$index + 1].image}}" width="100%" />   
            </div>   
            <div class="col col-25" ng-if="$index + 2 < images.length">   
                <img ng-src="data:image/jpeg;base64,{{images[$index + 2].image}}" width="100%" />   
            </div>   
            <div class="col col-25" ng-if="$index + 3 < images.length">   
                <img ng-src="data:image/jpeg;base64,{{images[$index + 3].image}}" width="100%" />   
            </div>   
        </div>   
    </ion-content>   
</ion-view>  

 
清空 app.js ，先为主对象添加依赖并声明为全局变量，再将 filebase 也声明为全局变量：

/*
添加依赖并全局: 
, "ngCordova", "firebase" 
*/
var CoolwpUploadDemo = angular.module("starter", ['ionic', 'ngCordova', 'firebase']);
/*
全局firebaseio
*/
var firebaseio = new Firebase("https://updatedemo1.firebaseio.com/");
 

注意： 请使用自己的应用ID，在本例中，我使用的是 "updatedemo1"(因为 uploaddemo1 貌似已经被别人用了，索性就来个update):

Firebase

加入初始化代码:

CoolwpUploadDemo.run(function($ionicPlatform) {
  $ionicPlatform.ready(function() {
    if(window.cordova && window.cordova.plugins.Keyboard) {
      cordova.plugins.Keyboard.hideKeyboardAccessoryBar(true);
    }
    if(window.StatusBar) {
      StatusBar.styleDefault();
    }
  });
});
 

路由配置，预定义控制器名称:

/*
配置路由
 */
CoolwpUploadDemo.config(function($stateProvider, $urlRouterProvider) {
    $stateProvider
    /*登录注册*/
        .state("firebase", {
            url: "/firebase",
            templateUrl: "templates/firebase.html",
            controller: "FireBaseController",
            cache: false
        })
    /*拍照自动上传+展示已上传的图片*/
        .state("photos", {
            url: "/photos",
            templateUrl: "templates/photos.html",
            controller: "PhotosController"
        });
    $urlRouterProvider.otherwise('/firebase');
});
 

在 js/controllers.js 中添加:

/*
拍照按钮以及功能;
没拍照时显示数据库中存储的已上传照片;
拍照后自动上传并给出弹窗提示;
 */
CoolwpUploadDemo.controller("PhotosController", function($scope, $ionicHistory, $firebaseArray, $cordovaCamera) {
    $ionicHistory.clearHistory();
    $scope.images = [];
    var fbAuth = firebaseio.getAuth();
    if(fbAuth) {
        var userReference = firebaseio.child("users/" + fbAuth.uid);
        var syncArray = $firebaseArray(userReference.child("images"));
        $scope.images = syncArray;
    } else {
        $state.go("firebase");
    }
    $scope.upload = function() {
        var options = {
            quality : 95,
            destinationType : Camera.DestinationType.DATA_URL,
            sourceType : Camera.PictureSourceType.CAMERA,
            allowEdit : true,
            encodingType: Camera.EncodingType.JPEG,
            popoverOptions: CameraPopoverOptions,
            targetWidth: 500,
            targetHeight: 500,
            saveToPhotoAlbum: false
        };
        $cordovaCamera.getPicture(options).then(function(imageData) {
            syncArray.$add({image: imageData}).then(function() {
                alert("图片已上传!");
            });
        }, function(error) {
            console.error(error);
        });
    }
});
 

Firebase 中的鉴权规则以及存储设置

用户鉴权规则:

{
    "rules": {
        "users": {
            ".write": true,
            "$uid": {
                ".read": "auth != null && auth.uid == $uid"
            }
        }
    }
}
 

自己做吧：存储规则请根据上述控制器中的写法自拟。

测试

安卓版示例APP下载：

链接: http://pan.baidu.com/s/1hq3W6DQ

密码: bph5

使用我已注册的测试账户 a@coolwp.com 密码 123456 登录

登录

登录后显示已经拍照并自动上传到 Firebase 的照片：

Ionic Firebase

拍照后自动上传照片到 Firebase ，然后给个弹窗提示:

ionic take a  photo and upload it

PS： 仅在安卓设备上进行了测试。

结论

希望国内的阿里云啊、七牛啊能对 AngularJS 友好些，自己开发 AngularJS 扩展库吧。
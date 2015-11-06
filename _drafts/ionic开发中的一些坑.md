ionic开发中的一些坑(持续更新中。。。) #7

https://github.com/ychow/Blog/issues/7

如果app有使用tabs，那么在真机上运行时，有的android手机tabs跑到了顶部，而在ios或者ipad上tabs还是在底部的，这是因为1.3.2以后版本默认是使用了以ios为模型的，如果想在android上tabs也是显示在底部，添加以下代码即可：
// 在app.js里面添加
.config(function($stateProvider, $urlRouterProvider, $ionicConfigProvider) {
  $ionicConfigProvider.platform.android.tabs.position("bottom");
  $ionicConfigProvider.tabs.style('standard');
});
在把ionic升级到1.3.2版本以后，默认的返回按钮都是带有"text"属性的，之前的版本在index.html中统一配置返回按钮样式取消"text"属性不起作用了，如果想要取消，添加以下代码即可：
.config(function($stateProvider, $urlRouterProvider, $ionicConfigProvider) {
  $ionicConfigProvider.backButton.text('').icon('ion-ios7-arrow-thin-left');
  $ionicConfigProvider.backButton.previousTitleText(false);
  $ionicConfigProvider.navBar.alignTitle('center');
});
最近遇到了一个问题，其实这问题导致的原因还是因为自己没有把 Ionic 的文档仔细看过。在 IONIC beta.14 版本中，Ionic 团队增加了视图缓存。那什么是视图缓存呢？
之前用户一旦在应用程序中执行导航动作,每个退出的视图元素和scope都会被销毁.如果相同的视图再次被访问,应用程序会重新生成元素.现在,视图可以被缓存以提高性能。
现在,当一个视图退出之后,元素将被遗留在DOM中,它的scope在这段时间内会被断开.当导航至一个已经被缓存的视图中,它的scope会被重新链接,并且其遗留在DOM中的元素会被重新激活到当前视图。
但是在实际开发中，有时候这其实是一个坑！ 还好 Ionic 为开发者提供了一个配置选项，你可以在你的路由里添加 ** cache: false**来禁止视图缓存。当然个别的ionView可以通过设置cache-view=”false”属性。你也可以查看详细的文档 ----> ionNavView/
最近使用了slide-box做了一个幻灯片。当然图片也是ajax取的，所以这里大家就会遇到一个问题，当你使用ng-repeat循环的时候，发现宽度为0！不要惊慌！Ionic里面已经有了相关的解决方案 --> $ionicSlideBoxDelegate
$timeout( function() {
    $ionicSlideBoxDelegate.update();
}, 50);
当我们想让我们应用的tabs在底部，让标题居中的时候，我们可以在config里面添加如下几句：
$ionicConfigProvider.navBar.alignTitle("center");
$ionicConfigProvider.platform.android.tabs.position("bottom");
$ionicConfigProvider.tabs.style("standard);
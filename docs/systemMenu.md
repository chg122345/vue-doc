#### 可配置项

首先了解一些本项目配置路由时提供了哪些配置项。

```
//当设置 true 的时候该路由不会再侧边栏出现 如401，login等页面，或者如一些编辑页面/edit/1
hidden: true // (默认 false)

//当设置 noredirect 的时候该路由在面包屑导航中不可被点击
redirect: 'noredirect'

//当你一个路由下面的 children 声明的路由大于1个时，自动会变成嵌套的模式--如组件页面
//只有一个时，会将那个子路由当做根路由显示在侧边栏--如引导页面
//若你想不管路由下面的 children 声明的个数都显示你的根路由
//你可以设置 alwaysShow: true，这样它就会忽略之前定义的规则，一直显示根路由
alwaysShow: true

name: 'router-name' //设定路由的名字，一定要填写不然使用<keep-alive>时会出现各种问题
meta: {
  title: 'title' //设置该路由在侧边栏和面包屑中展示的名字
  icon: 'svg-name' //设置该路由的图标
  noCache: true //如果设置为true，则不会被 <keep-alive> 缓存(默认 false)
  breadcrumb: false // 如果设置为false，则不会在breadcrumb面包屑中显示
}
```
示例：
```
{
  path: '/permission',
  component: Layout,
  redirect: '/permission/index', //重定向地址，在面包屑中点击会重定向去的地址
  hidden: true, // 不在侧边栏线上
  alwaysShow: true, //一直显示根路由
  children: [{
    path: 'index',
    component: ()=>import('@/views/permission/index'),
    name: 'permission',
    meta: {
      title: 'permission',
      icon: 'lock', //图标
      noCache: true // 不会被 <keep-alive> 缓存
    }
  }]
}
```
#### 添加图标

如果你没有在本项目 Icon 中找到需要的图标，可以到 [iconfont.cn]() 上选择并生成自己的业务图标库，再进行使用。或者其它 svg 图标网站，下载 svg 并放到 `src/icons/svg` 文件夹之中就可以了，下载方式如下图：

![添加图标演示](https://i.loli.net/2019/03/28/5c9c93ce6a575.gif)

使用方式：

```
<svg-icon icon-class="password" /> //icon-class 为 icon 的名字
```
#### 添加菜单

本项目支持菜单动态路由，支持添加4级菜单，支持外链，支持自定义图标，这里演示动态添加菜单 **（1.6版本后就不支持在菜单管理中选择角色了，而是在角色管理中分配菜单）**

##### (1) 添加外链

![添加外链](https://i.loli.net/2019/03/28/5c9c93f3015a1.png)

##### (2) 添加内部菜单

组件路径为 src/views

![添加内部菜单](https://i.loli.net/2019/03/28/5c9c94097c0a2.png)

##### (3) 添加多级菜单

添加多级菜单请模仿项目内的多级菜单进行添加

![添加多级菜单](https://i.loli.net/2019/03/28/5c9c9431cf379.png)
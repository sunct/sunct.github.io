---
title: ' 使用mpvue重构个人小程序（三）使用Vue的语法编写'
thumbnail: /assets/articleImg/2019/timg-xcx.jpeg
tags:
  - mpvue
  - 小程序
categories:
  - - 小程序
date: 2019-04-13 17:20:55
---

上一节：[使用mpvue重构个人小程序（二）文件结构](/2019/04/13/使用mpvue重构个人小程序（二）文件结构/)

我们将要对vue-cli生成的代码做一个清理工作，具体如下：

删掉`src/components`、`src/pages`、`src/utils`三个目录下的所有代码文件

提前说一下，微信开发者工具不支持打开vue文件，因为微信开发者工具只是一个用户代理(即显示网站用)，类似Google浏览器一样。所以编写的时候一般会选择一款编辑器，可以使用Sublime、webstorm、vs等。
 
<!--more--> 
# 一、重置
将`src/App.vue`文件中的内容重置：

原来的文件：

![原文件](/assets/articleImg/2019/image-52-1024x591.png)

重置成：

```vue
<script>
export default {}
</script>

<style>
</style>
```

![重置](/assets/articleImg/2019/image-53-1024x342.png)

将`src/main.js`文件中添加内容;

```javascript
export default {
  config: {
    pages: [],
    window: {
      backgroundTextStyle: 'light',
      navigationBarBackgroundColor: '#fff',
      navigationBarTitleText: '第一个小程序',
      navigationBarTextStyle: 'black'
    }
  }
}
```

![添加内容](/assets/articleImg/2019/image-54.png)

# 二、app.json

`app.json`是小程序的全局配置文件，其包含了小程序的页面文件路径配置、窗口的全局样式信息、底部选项卡式菜单栏的配置，以及一些小程序网络超时的配置等等。

在`src/main.js`中，我们就可以完整的进行这些信息的配置，具体可以查看上图文件刚添加的最底部代码。在该代码中通过`export default`导出的对象的`config`属性下的值，就是这些小程序的配置信息了。

# 三、首页

每个小程序都需要至少有一个页面，第一个展示的页面被叫做`首页`。因为前面已经把所有的页面代码都删完了，所以我们现在要新建一个首页。在`src/pages`目录下，我们新建一个名为`index`的子目录（请遵循每个页面放入一个子目录的良好习惯），然后在该子目录下，新建2个文件：一个用于实现页面主体功能的`index.vue`组件，另一个则用于将这个页面组件生成Vue实例并加载的`main.js`。以后的每一个mpvue页面组件都会拥有这样的结构。

![首页文件](/assets/articleImg/2019/image-55-1024x445.png)

没有首页会在命令行中报错，如：

![报错](/assets/articleImg/2019/image-56-1024x576.png)

然后在`main.js`中编写如下代码，的功能是引入`index.vue`并创建Vue实例：

```javascript
import Vue from 'vue'
import App from './index'

const app = new Vue(App)
app.$mount()
```

当然你也可以像在`src/main.js`中去导出一个页面级别的配置，因为小程序的每个页面都可以有一些单独的配置：

```javascript
import Vue from 'vue'
import App from './index'

const app = new Vue(App)
app.$mount()

export default {
  config: {
    // 注意，页面级可配置属性相当于只是`src/main.js`中配置里的`window`部分
    "navigationBarTitleText": "首页"
  }
}
```

接着，我们需要实现`index.vue`页面组件，它的写法是最典型的Vue组件写法。

当我们写到此时，在开发工具中编译运行，结果还不是我们想要的：

![编译运行](/assets/articleImg/2019/image-57-1024x738.png)


改了这么多，为啥还和最初的一样？原因是 我们还没有编译，这个编译不是小程序开发工具里的编译。我们最初运行了 `npm run dev` ，在dist 文件已编译。

![/wx/目录](/assets/articleImg/2019/image-58-1024x368.png)

先清空/wx/该目录，再运行：
```linux
npm run dev
```

当我们实现了这个`index.vue`页面组件后，其实还缺最后一个步骤，就是需要将这个页面组件指定为首页。如果我们的小程序只有一个页面的话，其实也可以省略这一步，因为mpvue会自动将`src/pages`目录下的页面组件路径添加到最终编译出来的小程序配置文件中去（可以打开`dist/app.json`文件观察一下）：

![dist/app.json](/assets/articleImg/2019/image-59-1024x567.png)

和 src/app.json

![src/app.json](/assets/articleImg/2019/image-60-1024x496.png)

点击 界面中的hello，变成了 Clicked，效果如下：

![最终效果](/assets/articleImg/2019/image-61-589x1024.png)


下一节：[使用mpvue重构个人小程序（四）全局配置](/2019/04/16/使用mpvue重构个人小程序（四）全局配置/)

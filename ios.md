# HouseSigma 前端开发面试问卷 李懿哲

### 面试问题1~4
1.关于申请的职位, 有什么疑问需要我们解答?

（1）iOS端原生目前用的开发语言是Objectiv-C还是Swift，以及为什么这样选择呢？

（2）有在领英上看到，贵公司的平均在职时间为1.1年，怎么看待这个时间呢？

2.平时使用的开发，调试，协作工具，一般用哪些手机测试。

（1）开发、调试工具：Xcode，Safari（用于调试Web代码）；

（2）协作工具：Eolinker（API管理），MasterLab（bug追踪管理），Git（代码管理）；

（3）测试手机：iPhone XR、iPhone 5s。

3.为什么想找远程工作？做过远程工作的人应该知道，没做过远程工作的话，想像一下一周5天在github干开源。

（1）我这边目前在郑州居住，这个薪资水平较于本地有优势；

（2）减少了通勤时间。

4.团队会议时间外，自己预计的主要工作时段。

（1）周一 ～ 周五

（2）早9晚6


---
### 面试问题5 
假设有一个原生 iOS app, 类似电商商城，集成了地图。用户频繁使用一段时间后内存占用不断升高，最终崩溃。

5.1
- 怎样确定是不是内存泄漏？
- 如果不是内存泄漏，还有哪些常见问题会造成内存占用高？
- 请结合过往经验说明。

5.2
- objective c ARC内存管理，什么是强引用，弱引用？
- 如果一个 object（对象）在多个view里面被引用，怎样保证内存最终会被释放？


---
### 面试问题7 - 原生/H5 混合开发
> 请下载安装我们的app

目前我们的app是用原生webview壳套在一个 angular app 上面。iOS有几个问题：
- 有的页面性能不好，JS 地图刷新率低不够流畅。
- 由于加载的图片和js组件较多，在某些iphone手机上也会发生内存不足的情况。会闪退。
- 没有办法在应用内直接评价。要跳转商店app。

解决的方法是开发纯原生APP或者是原生和H5混合的APP。请问，如果要从头开始提高app性能，你会怎样改造这个app？
- 一个工程肯定不能一步完成，要分阶段迭代

#### 7.1 补充问题：
- 在使用过我们的app之后，如果让你开始一个项目来改善app的使用体验，你会怎样改？
- 没有明确的预算限制，可以是几个礼拜的小项目，也可以是几个月的大项目
- 答案格式：
  - 相似功能的代码片段
  - 解释原理，对应的文档链接


---
### 面试问题8
很多信息类的APP都有夜间和白天阅读模式。我们的用户主要的使用时间也集中在夜间。如何实现像【知乎app】那样，可以让用户切换阅读模式？
- Auto（跟随系统设置）
- 白天 （不跟随系统设置）
- 夜间 （不跟随系统设置）



##### 答：

> 从代码层来看，白天、夜间模式决定于"Key Window"的`overrideUserInterfaceStyle`属性。
>
> 默认为自动，可以手动切换为`.dark`或`.light`。

代码如下：

```swift
    private var window: UIWindow? {
        UIApplication.shared.keyWindow
    }
    
    @IBAction func auto(_ sender: Any) {
        window?.overrideUserInterfaceStyle = .unspecified
    }
    
    @IBAction func darkTheme(_ sender: Any) {
        window?.overrideUserInterfaceStyle = .dark
    }
    
    @IBAction func lightTheme(_ sender: Any) {
        window?.overrideUserInterfaceStyle = .light
    }
```



#### 8.1 补充问题：

- 如果app里面用了webview，如何让 webview 的昼夜状态跟随app的3种状态？

webview测试范例页面（不修改html内容是否可实现？） https://test.housesigma.com/static/dark_test.html

- 答案格式：
  - 上传代码片段
  - 解释原理，对应的文档链接
  - 自己真机调试通过



##### **答：**

> 测试页面（ https://test.housesigma.com/static/dark_test.html ），不修改任何内容，即可实现跟随App的主题状态自动切换。

**（1）代码片段**

（1.1）iOS原生部分代码

> 只是正常的WKWebView初始化、加载，未做任何特殊处理

```swift
class ViewController: UIViewController {
    
    @IBOutlet weak var webView: WKWebView!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        loadWebViewUrl()
    }
    
    private func loadWebViewUrl() {
        let urlString = "https://test.housesigma.com/static/dark_test.html"
        if let url = URL.init(string: urlString) {
            let request = URLRequest.init(url: url)
            webView.load(request)
        }
    }
}
```

（1.2）测试页面中的部分代码：

> 摘录部分核心代码，并未做任何处理

```css
@media screen and (prefers-color-scheme: dark) {
  p {
    background-color: black;
    color: grey;
  }
  .night{
    display: unset;
    color: black;
  }
  .day{
    display:none;
  }
}
```
**（2）原理及参考文档**

（2.1）原理

测试页面默认的是白天主题，

css中的`@media screen and (prefers-color-scheme: dark) { ... }`部分，意为检测到App为dark mode时，该css中的`p`元素改变背景和颜色，相关的`.night`、`.day` class作相应的展示、隐藏，显示dark样式。

（2.2）参考文档

https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-color-scheme

**（3）已真机调试通过**

参考录屏：

https://user-images.githubusercontent.com/23650458/132179032-8f7ea9c5-5dd9-4720-8645-09857c7ff892.mov

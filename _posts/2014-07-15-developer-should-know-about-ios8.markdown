---
layout: post
title: 开发者所需要知道的 iOS8 SDK 新特性
date: 2014-07-15 22:27:05.000000000 +09:00
tags: 能工巧匠集
---
![](/assets/images/2014/wwdc2014-badge.jpg)

WWDC 2014 已经过去一个多月。最激动人心的莫过于 Swift 这门新语言的发布，我在之前已经写了一些关于这么语言的[第一印象](http://onevcat.com/2014/06/my-opinion-about-swift/)和一些[初步的探索](http://onevcat.com/2014/06/walk-in-swift/)。在写这篇文章的时候，Swift 随着 beta 3 得到了重大的更新，而这门语言现在也还在剧烈的变化之中。对于 Swift，现在大家的探索才刚刚上路，很多背后的机制还并不是非常清楚，或者有可能发生巨大的变化，因此在这里和之后的几篇文章，直到稳定的 1.0 版本出现，我不再打算继续深入针对 Swift 写什么文章。这基本出于对未来可能的变化会容易误导读到文章的新人的考虑，而并不是说建议我们现在可以放下 Swift，而安心等待正式版本。在我的观念里，对于一个尚不稳定的版本的探索和研究，远比之后被动去接受别人的结果要来的有趣得多，理解也会深入得多。因此如果您有时间的话，建议还是能尽早接触和使用会比较好。Github 上有一个[不错的 repo](https://github.com/ksm/SwiftInFlux)，记录了 Swift 一路以来的变化，并探讨了不足以及以后可能的变化，希望深研 Swift 的同学不妨关注看看。

这篇总览先简要介绍下在我看来作为 iOS 开发者应该关注的开发时的变化，在之后一系列文章里我会对其中的某几个部分详细探讨一下，而其余的可能就在本文中做简介。总而言之，这次 WWDC 2014 的相关笔记（现在来说的话是暂定计划要写的内容）大概整理如下：

* [开发者所需要知道的 iOS8 SDK 新特性](http://onevcat/2014/07/developer-should-know-about-ios8)
* [iOS 界面开发的大一统](http://onevcat.com/2014/07/ios-ui-unique/)
* [iOS 通知中心扩展制作入门](http://onevcat.com/2014/08/notification-today-widget/)
* [可视化开发，IB 的新时代](http://onevcat.com/2014/10/ib-customize-view/)
* iOS 和 Mac 整合开发
* 通知中心和应用使用重心的改变

---

## 应用扩展 (Extension)

这是一个千呼万唤始出来的特性，也是一个可以发挥无限想象力的特性。现在 Apple 允许我们在 app 中添加一个新的 target，用来提供一些扩展功能：比如在系统的通知中心中显示一个自己的 widget，在某些应用的 Action 中加入自己的操作，在分享按扭里加入自己的条目，更甚至于添加自定义的键盘等等。每一种操作对应这一个应用扩展的入口，在开发中我们只需要在工程中新建立一个对应相应入口的 target，就能从一个很好的模板开始进行一些列开发，来实现这些传统意义上可能需要越狱才能实现的功能。

对于应用扩展，Apple 将其定义为 App 的功能的自然延伸，因此不是单独存在的，而是随着应用本体的包作为附属而被一同下载和安装到用户的设备中的，用户需要在之后选择将其开启。另外，由于应用扩展和应用是属于两个不同的 target 的，因此它们之间的数据和操作上的交互遵循的是另一套原则。关于应用扩展的更详细的内容，我计划在之后通过一个通知中心的 today 小框体控件的例子来详细说明。

> 专题相关笔记
> 
> [iOS 通知中心扩展制作入门](http://onevcat.com/2014/08/notification-today-widget/)

## App 开发时的统一

随着一代代的 iPhone 和 iPad 的出现，iOS 设备的屏幕尺寸也开始出现分裂的趋势。之前一套屏幕两个方向吃遍全世界的美好时光已然不再，现在至少已经有 3.5 寸，4寸和 10(7) 寸三种分辨率/尺寸的机型需要进行适配，再考虑到每个尺寸的横竖两种方向，以及日益呼声愈高的 4.7 寸和 5.5 寸的 iPhone，可以相见现在的布局方式已然不堪重负。虽然在 iOS 6 Apple 就推出了 Auto Layout 来辅助完成布局工作，解决了原来的相对布局的一些问题，但是在以绝对尺寸为度量的坐标系统中，难免还是有所掣肘。在 iOS 8 中，Apple 的工程师们可以说“极富想象力”地干脆把限制和表征屏幕尺寸的长宽数字给去掉了，取而代之使用 size classes 的概念，将长宽尺寸按照设备类型和方向归类为 regular 和 compact 两类。通过为不同的设备定义尺寸分类，用来定义同类型的操作特性，这使得开发者更容易利用一套 UI 来适配不同的屏幕。

iOS 8 在 UIKit 中添加了一整套使用 size classes 来进行布局的 API，并且将原有的比较复杂（或者说有些冗余）的 API 作废了。结合新的 Interface Builder 和 Auto Layout，可以说对于多尺寸屏幕的适配得到了前所未有的简化。

不仅如此，像是原来 iPad 专有的 SplitController 等也被以适应不同 regular 和 compact 的尺寸类型的形式 port 到了 iPhone 上，在程序设计方面两者更加统一了。另外，一直陪伴我们的 `UIAlertView` 和 `UIActionSheet` 这些老面孔也将退出舞台，取而代之全部统一以 UIViewController 来呈现。

这是一个好的开始，也是一个好的变化。可以看到 Apple 在避免平台碎片化上正在努力。

> 专题相关笔记
>
> [iOS 界面开发的大一统](http://onevcat.com/2014/07/ios-ui-unique/)
>
> [可视化开发，IB 的新时代](http://onevcat.com/2014/10/ib-customize-view/)

## iCloud 相关

作为帮主的最后一件作品，iCloud 其实非常可惜，一直没有能在 Apple 的生态圈中特别出彩。首先主要是由于 iCloud 相关的开发和 API 使用起来有一定难度，另外就是之前的 SDK 在和 iCloud 相关的各种 API 或多或少都有一些小问题。在 iOS 7 中 iCloud，特别是 iCloud 和 CoreData 结合的部分的 API 的稳定性和易用性得到了很大的改善。而在 iOS 8 中，Apple 更进一步，推出了全新的被称为 Cloud Kit 的框架。如果您熟悉或者使用过像 [Parse](https://www.parse.com) 或者 [AVOS Cloud](https://cn.avoscloud.com) 之类的 [BaaS](http://en.wikipedia.org/wiki/Backend_as_a_service) 的话，可能会对这个框架感到亲切。但是和传统的 BaaS 稍有不同的是，Cloud Kit 更多的是倾向于使用 iCloud 对数据进行集成。你可以不更改应用现有的数据模型和结构，而只是使用 Cloud Kit 来从云端获取数据或者向云端存储数据。

相比与 Parse 和 AVOS 的 API，由于可以和系统深度集成，有很多在其他类似 BaaS 中没有的特性 (比如订阅某个公共对象)。但是因为是 Apple 自家产品，其缺点也是显而易见并且致命的 -- 你无法在非 Apple 的平台上使用这个框架。也就是说，如果你的应用火了，想接着出个安卓版的话，那就只能呵呵了。所以虽然 Cloud Kit 看起来很美好，而且基本等同于免费使用，但是因为平台的限制，而它所涉及的内容又是对跨平台需求很强又绕不开的数据，所以可能实际中能实用的机会并不太多。当然，如果应用是 for iOS only 的话，使用 Cloud Kit 应该是很不错的选择。

关于云端存储的另一个新变化是存储源的可变化。以前我们基本别无选择，想使用沙盒外的文件的话，要么就是 iCloud 同一个 container 内的文件，要么就需要来个像 Dropbox 这样的第三方库去做一堆登陆验证什么的。不论那种方式都可以说挺麻烦的。而现在随着 [iCloud Drive](https://www.apple.com/cn/ios/ios8/icloud-drive/) 的引入，在应用间共享访问文件就变得很容易了。更甚，我们现在可以使用 [UIDocumentPickerViewController](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIDocumentPickerViewController_Class/index.html#//apple_ref/occ/cl/UIDocumentPickerViewController) 来从第三方存储 (以及第三方 app 通过应用扩展所实现的存储) 中选取文件。

## Handoff 及其他 iOS 与 Mac 的协同开发

虽然 PC 市场一直疲软，但是得益于 iDevice 的销售和品牌接受度的回升，Mac 的销量反而逆市上扬。这点在国内尤为明显，确实可以感觉到身边开始使用 Mac 的人在逐渐变多，这对于我们这些 iOS 开发者来说其实是一个不错的机会。iOS 8 中的 Handoff 机制（就是可以在 Mac 上继续完成在 iOS 上半途的工作）给 for both iOS and Mac 的应用带来了一个不错的契合点和卖点。而近年来在整合两个系统上的动作，也可以看得出 Apple 确实希望利用庞大的 iOS 的开发人员资源来进一步完善和丰富 Mac。iOS 开发和 Mac 开发其实同根同源，因此在转换的时候并不是很困难的事情。

我们一直以来都可以写出跨两个平台的 Model 部分的代码，而只需要关心在表现上的区别。而现在 Cocoa 和 CocoaTouch 在官方支持自制 framework 后，利用 framework 来完成这一过程可以说更加简单了。

> 专题相关笔记
>
> iOS 和 Mac 整合开发

## Health Kit 和 Home Kit

这是对应两个现在很热的领域 -- 可穿戴式设备和智能家电 -- 所加入的框架。基本上来说 Apple 想做的事情就是以 iOS 为基础，为其他 app 建立一个平台以及成为用户数据的管理者。

Health Kit 就是一个用户体征参数的数据库，第三方应用可以向用户申请权限使用其中的数据或是向其中汇报数据。而 Home Kit 则以家庭，房间和设备的组织形式来管理和控制家中适配了 Home Kit 的智能家电。这两个超级年轻的框架的 API 相对都还比较简单，结构也很好，相信稍有经验的 iOS 开发者都能在很快掌握用法。唯一的限制在于作为普通开发者（比如我这样的只能自己业余玩的）可能手边现在不会有合适的设备来进行测试，所以很多东西其实没有办法验证。不过对于 Home Kit，Apple给我们提供了一个模拟器来模拟智能家电设备，您可以在 Xcode 6 的 Open Developer Tool 菜单中找到 Home Kit Accessory Simulator。使用模拟器可以发现，添加并且控制自定义的智能家电，用来前期开发还是蛮方便的。

如果能入手一些适配于 Health Kit 或者 Home Kit 的设备的话，我可能会补充一些关于这方面的开发心得。

## 游戏方面

最大的改变莫过于 Scene Kit 的加入了。不过游戏天生的容易跨平台的特性 (并且也有这方面的强烈需求)，与平台限制的 Sprite Kit 是冲突的，所以去年的 Sprite Kit 也还没多少人用。暂时看来这个世界现在是，并且在一段时间内还会是被 Cocos2dx/Unity 所统治的。Scene Kit 的未来估计会和 Sprite Kit 比较类似，作为对于一直进行 iOS 应用开发的开发者来说，有着不需要学习和熟悉新语言的优势，容易与系统的其他框架进行集成，所以用来转型还算不错的选择。但除此之外其他方面可能也并没有多少可以吸引人的地方了。

另一个重大改变是对于 A7 和以上级别的 GPU 推出了一套全新的称为 Metal 的绘制 API，从 Keynote 的 Zen Garden 的演示来看，Metal 的性能毋庸置疑是令人折服的，Metal 的渲染方式和着色器也相当有趣。但是其实这些内容更多地是偏向底层以及面向引擎开发的，对于使用游戏引擎来制作游戏的大多数开发者来说，并不需要知道或者理解其中的东西。在 A7 的芯片下使用 Apple 自家的 Sprite Kit 或者 Scene Kit 的话，就可以直接受益于 Metal，而其他一些知名的第三方引擎，比如 Unity 和 UE 也都会在 iOS 8 推出后支持 Metal。因此，作为引擎使用者，并不需要做出除了升级开发使用的游戏引擎之外的任何改变。

## 其他重要改动

### Local 和 Remote 通知的变化

现在需要显示 UI 或者播放声音的通知，包括 Local 通知也需要实现弹窗获得用户许可了。使用 `-registerUserNotificationSettings:` 来向用户获取许可。作为补偿，现在对于不需要打扰用户（也就是 iOS 7 加入的静默通知）的类型不再需要弹框获取用户许可。不过因为本地推送是需要许可的，所以无论怎样如果你想要依靠通知来提高用户留存率的话，现在都绕不开用户许可了。

另外，通知中心加入了非常方便的 Action 特性，用户可以在收到通知后，在不打开应用的情况下完成一些操作。可以说配合通知中心的 Today 扩展，用户现在在很可能可以在不打开应用的情况下就获取到他们想要的信息，并完成互动。这对于开发者可以说是一件喜忧参半的事情，一方面我们可以给用户提供更好更快的使用体验，但是另一方面这将降低用户打开应用的意愿。不过 Apple 现在的总体思路还是 app 的体验才是最重要的，所以正确的道路应该还是优先做好 app 的体验，并且摸索一个应用和通知之间的平衡点，让大家都满意。

> 专题相关笔记
>
> 通知中心和应用使用重心的改变

### CoreLocation
CoreLocation 室内定位。现在 CL 可以给出在建筑物中的楼层定位信息了，直接访问 `CLLocation` 实例的 `floor`，如果当前位置可用的话，会返回一个包含位置信息的非 nil 的 `CLFloor` 以标识当前楼层。这个使得定位应用的可能性大大扩展了，想象一下在复杂的地铁站或者大厦里迷路的时候，还可以依赖定位系统，幸福感涌上心头啊。

### Touch ID
Touch ID API，说是开放了 Touch ID 的验证，但是实际上能做的事情还是比较有限。因为现在提供的 API 只能验证用户是不是手机主人本人，而不能给出一个识别的标志或者唯一编码，所以想用 Touch ID 做注册登陆什么的话可能还是不太现实。不过在进行支付验证之类的已登录后的再次确认操作时就比较好用。现在看来的话这组 API 就是为了简化像 Paypal 或者支付宝这样的第三方支付和确认的流程的。希望之后能继续放开，如果能给一个唯一标识的话，也许就可以干掉整个讨厌的注册和登陆系统了。

### 相机和照片

新增加了 Photos.framework 框架，这个框架用于与系统内置的 Photo 应用进行交互，不仅可以替代原来的 Assets Library 作为照片和视频的选取，还能与 iCloud 照片流进行交互。除此之外，一个很重要的特性是还可以监听其他应用对于照片的改变，可以说整个框架非常灵活。

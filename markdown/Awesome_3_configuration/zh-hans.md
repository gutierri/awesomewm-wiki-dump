---
title: Awesome 3 configuration zh-hans
permalink: /Awesome_3_configuration/zh-hans/
---

背景
====

**awesome 3** 现在开始使用一个基于 [Lua](http://www.lua.org) 语言的配置文件。很多从 awesome 2 迁移过来的用户都将痛苦的发现，使用 libconfuse 格式的旧配置文件已经消失了，还有些人或许会不舒服，因为他们不理解 [ion](http://modeemi.fi/~tuomov/ion/) 配置文件。但是我不会。

为什么选择 Lua 语言？ 因为它丰富的可操控性。例如: 你希望 awesome 在某件事发生时作某事，在以前这是无法实现的。但是现在可以了。你几乎可以做 **任何事** (仅限于一个窗口管理器能做的事)。

配置文件
========

-   ~/.config/awesome/rc.lua
-   /etc/xdg/awesome/rc.lua

Lua 语言基础
============

我们正在讨论 Lua 语言，所以首先，学习 Lua 语言。你不想？那么请不要使用 awesome 3 并且立即停止浏览。(还有一个解决办法就是，从源代码或者其他人哪里获得一份配置文件，然后进行微调，用不到任何 Lua 语言的知识)。

对坚持浏览的用户说一声，很好！Lua 语言是一种 **简单** 的语言。但另一方面，如果你对计算机语言一点也不熟悉，比如说，你不知道 *对象*，*方法* 还有 *参数* 是什么意思，那么，你是很难理解这份文档的，请先学习一些基础知识后再来。

我是精英主义者: awesome 3 被设计给拥有最低限度计算机科学知识的进阶用户。但是如果你非常有积极性，你可以学会足够的基础知识配置和操控 awesome。

学习 Lua 语言最好的方法是花一两个小时阅读 [Programming In Lua](http://www.lua.org/pil/) 这本书，通过这本书你将对 Lua 的用法有一个大概的认知。Do not hesitate to bookmark important pages talking about flow control, useful statements, etc. That will help: it's awful to look for the for syntax when trying to type lines of code.

awesome 对象类型
================

对于那些未曾使用过awesome的来自外太空的人来说，以下是一些awesome提供的，你即将对其控制的基本对象。

Screen 屏幕
-----------

在 awesome 里并没有屏幕(Screen)这个对象，但我们稍后会讨论它。一个屏幕代表了一个和你电脑相连接的物理显示器。它们由索引标识，起始索引是1。

Client 客户
-----------

一个客户(Client)代表一个窗口。我想我已经说清楚了。

Tag 标签
--------

一个标签(Tag)就好像一个工作区或者一个虚拟桌面，但是更加灵活。 每一个客户(Client)都可以被分配(至少)一个标签。每一个屏幕(Screen)都可以被分配(至少)一个标签。 任何时候，在一个屏幕，你可以查看任何标签。查看空标签的话将隐藏所有客户。查看一个或多个标签的话将把分配了这些标签的客户都显示给你。

Widget 零件
-----------

零件就是可以在屏幕上绘制出来的对象。现有几种类型的零件。例如在屏幕上绘制文本的文本零件，在屏幕上绘制图标的图标零件。

Titlebar 标题栏
---------------

标题栏就是附属在客户周围的栏。你可以把零件放置在其中。

Statusbar 状态栏
----------------

状态栏就是固定在屏幕边缘的栏。你可以把零件放置在其中。

建造我们的界面
==============

为了建造我们的界面，我们需要用到 awesome 的函数和方法: **所有这些** 都已经整理成文档 [Lua API 文档](http://awesome.naquadah.org/apidoc/) 和 awesomerc(5) 系统帮助手册。阅读它们，它们是你开发的参考资料。我重复: 阅读它们，所有函数和方法都被整理到系统帮助手册和 lua 文档。

系统帮助手册在哪里？…… awesomerc(5)。很好，你找到了。

标签的创建
----------

如果你在启动 awesome 时没有设置任何 Lua 动作，你将拥有 0 个标签，这是错误的。awesome 需要至少一个标签。

所以首先，我们需要创建一个标签。我们该怎么做呢？我们在文档和系统帮助手册中找到创建标签的函数 tag()。它显示这个函数的参数必须是一个 table，而且这个 table 至少有一个 name 属性。让我们这么做。

`mytagone = tag({ name = "one" })`

现在你将一个对象 *mytagone* 定义成了一个标签对象，而且这个标签对象的名称是 *one*。如果我们想设置它默认的布局，我们可以把这种 key/value 对加入 table，就像文档中说明的那样。

`mytagtwo = tag({name = "two", layout = "floating" })`

现在，我们有了第二个标签对象 *mytagtwo*：它的名称是 *two*，而且默认把所有客户按照 floating 布局排列。

创建对象很有用，但是目前而言，它还没起作用。请记住：每个屏幕都至少要分配一个标签。所以我们需要把这些创建好了的标签添加到我们的首个屏幕。这个 *screen* 参数为我们作了这件事，我们只要把屏幕的索引赋值给它就可以了。

`mytagone.screen = 1`

我们刚刚把那个标签对象添加到屏幕\#1。如果我们拥有第二台显示器，我们可以把我们的第二个标签对象添加进去。

`mytagtwo.screen = 2`

而且如果我们有第三台显示器，我们就可以……好了，我想你已经明白了。

如果你想知道你拥有多少个屏幕，你可以调用这个函数 *screen.count()*。

你通过 *selected* 参数可以操控标签的显示与否。如果你想查看你的标签 *one*：

`mytagone.selected = true`

如果你想隐藏它，你可以赋值 *false*。

你通过这个参数可以操控所有的标签显示与否。请查阅文档。

客户的操控
----------

有时候，你可能会获得客户对象。它在 hook 函数中是非常有用的，我们将会稍后讨论 hook 函数。

一个客户就是一个像标签和其它东西一样拥有方法的对象。我们决定用 *c* 这个变量命名我们的客户对象。 例如，如果我们想把一个对象的显示方式设置为浮动方式，我们可以使用 *floating* 参数，就像这样：

`c.floating = true`

某些方法具有的返回值可以用来进行后期处理。如果我们已经知道一个客户是浮动显示的，我们可以获得这个 *floating* 参数的值：

`is_client_floating = c.floating`

现在这个变量 *is_client_floating* 包含了一个布尔值：*true* 表示这个客户是浮动显示的，或者 *false* 表示它不是浮动显示的。

我们可以接合这两个函数来创建一个开关函数：这个函数将控制客户的浮动显示与否。

`function client_floating_toggle(c)`
`    if c then`
`        c.floating = not c.floating`
`    end`
`end`

我们还可以用它来控制在我们客户上的标签：

`function client_tag_with_tag_one(c)`
`    if c then`
`        local t = c:tags()`
`        table.insert(t, one)`
`        c:tags(t)`
`    end`
`end`

零件的创建
----------

零件是一些很小的对象，它们可以放置在标题栏或者状态栏上。和标签对象一样，如果你不把它们添加到某个地方，它们是不起作用的。创建一个零件需要用到一个函数 *widget()*：

`mytextbox = widget({ type = "textbox", name = "mytextbox" })`

*mytextbox* 现在包含了一个零件对象。对于不同的零件类型，你可以访问不同的参数，下面的设置是在 textbox 上显示文字。

`mytextbox.text = "Hello, world!`

状态栏的创建
------------

状态栏是零件的容器。状态栏可以放置在屏幕的上、下、左、右边缘。

首先，创建一个状态栏：

`mystatusbar = statusbar({ position = "top", name = "mystatusbar" })`

*mystatusbar* 现在是一个包含了状态栏对象的变量。由于我们设置了 *position* 为 *top*，它将会显示在我们屏幕的上方。但是我们还没有把它添加到屏幕上，所以现在它还没起作用。让我们开始吧：

`mystatusbar.screen = 1`

我们的状态栏现在已经显示在我们屏幕的上方了。很好，它什么也不做，但实际上它非常有用。让我们添加一些零件上去！

`-- Create a textbox 创建一个文本零件`
`mytextbox = widget({ type = "textbox", name = "mytextbox" })`
`-- Set text of the textbox 设置文本零件显示的文字`
`mytextbox.text = "你好, 世界!"`
`-- Create a statusbar 创建一个状态栏`
`mystatusbar = statusbar({ position = "top", name = "mystatusbar" })`
`-- Add widgets to the statusbar 把零件添加到状态栏`
`mystatusbar:widgets({ mytextbox })`
`-- Add the statusbar on screen #1 把状态栏添加到屏幕#1`
`mystatusbar.screen = 1`

我们现在已经有一个在屏幕上方显示 *你好，世界!* 的状态栏了。通过这些方法，你可以在你的 awesome 配置文件中设置一个拥有一些零件的个性状态栏了。

标题栏的创建
------------

正如我之前所说，标题栏和状态栏很相似，只不过它附属在一个窗口的边缘。创建一个标题栏，你知道的：

`mytitlebar = titlebar({ position = "top" })`

然后，你可能想放置零件上去。这和状态栏的处理很像：

`mytitlebartitle = widget({ type = "textbox", name = "mytitlebartitle" })`
`mytitlebar:widgets({ mytitlebartitle })`

一个标题栏必须添加一个 *客户对象*。我们可以得到当前焦点的客户：它的信息保存在 *client.focus* 变量中。让我们

`-- Get the focused client 获得当前焦点对象`
`c = client.focus`
`-- Set a titlebar on it 给它设置一个标题栏`
`c.titlebar = mytitlebar`

现在，焦点客户的上方已经有了一个不错的标题栏了！但是标题栏上什么也没有。不是吧，等等！请看，在标题栏中我们已经添加了一个 *textbox* 零件。现在我们可以用它把客户名称显示出来：

`mytitlebartitle.text = c.name`

这个 *name* 属性是一个描述了客户标题名称的字符串，我们可以把它作为文本内容赋值给零件。现在的标题栏已经显示出客户的标题了。

请记住：一个标题栏是一个对象，一个客户也是一个对象。所以你需要为每一个你希望拥有标题栏的客户分别创建一个标题栏。

钩子的使用
----------

正如之前我们所见，你可以创建一个美丽的界面。但是，设想一下你希望更新一个客户上标题栏中显示的标题：你做不到。等等！有办法：钩子函数。

通过钩子，你可以定义一个函数，这个函数将在一个事件发生时被调用执行。让我们通过一个示例尝试一下。

当一个客户在屏幕上弹出来的时候，awesome 运行一个 *manage* 钩子函数。通过传递客户这个参数来调用此函数。这里有个例子：

`function hook_manage(c)`
`    if c.name:find("mplayer") then`
`        c.floating = true`
`    end`
`end`

首先，我们定义了一个 *hook_manage* 函数，有一个参数名为 *c*，这个参数代表一个客户。然后，我们试图找到一个名为 *"mplayer"* 的客户，如果我们找到了，我们就把它的显示方式设置为浮动方式。

` awful.hooks.manage.register(hook_manage)`

再然后，通过上面这行调用，我们定义了当有新的客户出现时，执行一个名为 *hook_manage* 的函数。

这里提供了众多的钩子供你使用和定义：focus, unfocus, manage, unmanage, mouseover, arrange, titleupdate, urgent, timer, 等等。参考系统帮助手册了解更多详细信息。

执行命令或脚本
--------------

你可以在 lua 配置文件中执行命令和脚本。这里有个示例函数，它实现的功能是：运行参数所指定的命令并返回该命令的返回值。对于把命令的返回值放到 textboxes 零件上这类功能来说，这个函数还是很有用的。

    -- Execute command and return its output. You probably won't only execute commands with one
    -- line of output
    function execute_command(command)
       local fh = io.popen(command)
       local str = ""
       for i in fh:lines() do
          str = str .. i
       end
       io.close(fh)
       return str
    end

这里有个例子说明了如何将 /proc/loadavg 信息放置在 mytextbox 零件上：

     mytextbox.text = " " .. execute_command("cat /proc/loadavg") .. " "

awesome Lua 库
==============

awful
-----

[awful](/awful "wikilink") 是和 awesome 交互的默认库，其中包括了像鼠标动作或者键盘事件等。

beautiful
---------

[Beautiful](/Beautiful "wikilink") 是关于 awesome 外观/主题的库。

eminent
-------

[Eminent](/Eminent "wikilink") 是一个 Lua 库，它能让你使用动态标签(dynamic tagging)。动态标签在这里的含义是指你所拥有的标签总数并没有在固定死，你可以先设定一个特殊的标签名称，当你在最后一个标签上时，然后切换到下一个标签，这时它将提供给你一个新的标签。这将允许你在不改变你的配置文件的前提下适应多种用法，给你更多或更少的标签。

naughty
-------

[Naughty](/Naughty "wikilink") 是关于通知的库。

wicked
------

[Wicked](/Wicked "wikilink") 是用于提供更多 awesome 零件所需的库。例如 MPD Widgets，CPU usage，memory usage，等等。

[Category: Awesome3](/Category:_Awesome3 "wikilink")
# Web 入门

## 必备工具软件

进行 Web 开发需要下面工具：

- 计算机，毋庸置疑!

- 文本编辑器，用来写代码。如 [Visual Studio Code](https://code.visualstudio.com)、[Sublime Text](https://www.sublimetext.com)、[Atom](https://github.blog/2022-06-08-sunsetting-atom/) 、[GNU Emacs](https://www.gnu.org/software/emacs/) 、 [VIM](https://www.vim.org) ，或一个混合编辑器（ [Dreamweaver](https://www.adobe.com/products/dreamweaver.html) 或 [WebStorm](https://www.jetbrains.com/webstorm/)）

- 网络浏览器。用来测试代码。目前，最常用的浏览器是 [Firefox](https://www.mozilla.org/zh-CN/firefox/) 、[Chrome](https://www.google.cn/intl/zh-CN/chrome/)、[Opera](https://www.opera.com/zh-cn)、[Safari](https://www.apple.com.cn/safari/)、[Internet Explorer](https://support.microsoft.com/zh-cn/windows/internet-explorer-下载-d49e1f0d-571c-9a7b-d97e-be248806ca70) 和 [Microsoft Edge](https://www.microsoft.com/zh-cn/edge?form=MA13FJ)。我们还应该测试我们的网站在移动设备和我们的目标受众可能仍在使用的任何旧浏览器（如 IE 8-10）上的表现。[Lynx](https://lynx.browser.org)，一个基于文本的终端网络浏览器，对于查看视力障碍用户对你的网站的体验是非常好的。

- 图形编辑器，用来给网页制作图像或图形。如 [GIMP](https://www.gimp.org) 、[Figma](https://www.figma.com)、[Paint.NET](https://www.getpaint.net)、[Photoshop](https://www.adobe.com/products/photoshop.html)、[Sketch](https://www.sketch.com) 或 [XD](https://helpx.adobe.com/support/xd.html)。

- 版本控制系统，用来管理服务器上的文件，与团队合作开展项目，共享代码和资产，避免编辑冲突。现在，[Git](https://git-scm.com) 是最流行的版本控制系统，还有 [GitHub](https://github.com) 或 [GitLab](https://about.gitlab.com) 托管服务。

- FTP 工具，老式 Web 托管账户，用来管理服务器上的文件（Git 正越来越多地取代 FTP 用于此目的）。有大量的 (S)FTP 程序可用，包括 [Cyberduck](https://cyberduck.io)、[Fetch](https://fetchsoftworks.com) 和 [FileZilla](https://filezilla-project.org)。

- 自动化构建工具，用来自动执行重复性任务，简化代码和运行测试，如 [Webpack](https://webpack.js.org) 、[Grunt](https://gruntjs.com) 或 [Gulp](https://gulpjs.com)。

- 库、框架等，用来加快编写常用功能。一个库往往是一个现有的 JavaScript 或 CSS 文件，它提供了现成的功能，供你在代码中使用。框架则更进一步，为你提供一个完整的系统和一些自定义的语法，让你在上面写一个 Web 应用。

- 更多工具...

以上工具安装略...

## 设计网站外观

在编写网站代码之前，需要进行规划和设计工作，包括“网站提供哪些信息？”、“用哪些字体和颜色？”、“网站是做什么的？”等，具体步骤如下：

1. 首先要考虑以下问题：

    1. 网站的主题是什么？你喜欢狗、上海、还是吃豆人？
    
    2. 基于所选主题要展示哪些信息？写下标题和几段文字，再找个你想在网站上展示的图像。
    
    3. 背景颜色用什么，或者，用高级一点的话来说，你的网站外观是什么样的？使用哪种字体比较合适：正式的、卡通的、粗体还是瘦体？

    !!! notes

        复杂的网站需要更详细的指引，深入到颜色、字体、页面上元素的间距、适当的书写风格等等以及其他细节。这有时被称为设计指南、设计系统或品牌手册，可以在[Firefox Photon 设计系统](https://acorn.firefox.com/latest/acorn-aRSAh0Sp)中找到一个示例。

2. 绘制草图。即使在实际的复杂网站中，设计团队通常也是从粗略的草图开始设计的，再使用图形编辑器或 Web 技术建立数字模拟图。Web 团队通常包括一个平面设计师和一个用户体验（UX）设计师。平面设计师把网站的视觉效果放在一起。用户体验设计师则以一种更抽象的模式来解决用户如何体验及与网站交互方面的问题。

3. 选定素材

    1. 文本。

    2. 主题。使用色彩选择器挑选心仪的颜色。当选中某种颜色时，会显示一个六位神秘代码，类似于 #660066。它是一个十六进制数，用于表示颜色。将其复制并暂存。

    3. 图像。注意版权！！！

    4. 字体。注意版权。

## 文件组织

一个网站包含许多文件：文本内容、代码、样式表、媒体内容，等等。当你建立一个网站时，你需要在计算机上将这些文件以合理的结构组织起来，确保文件之间交互畅通，并在你最终将它们上传到服务器之前使你的所有内容看起来正确。处理文件讨论了一些你应该注意的问题，以便你能为你的网站建立一个合理的文件结构。

对于本地网站，应该将所有相关文件放在一个单独文件夹中，可以映射出服务器端点文件结构。这个文件夹可以放在任何自己喜欢的地方，但那我们应该把它放在轻易找到的地方。

1. 选择一个地方来存储你的网站项目。在你选择的地方，创建一个名为 web-projects（或类似）的新文件夹。这是你所有的网站项目的存放地。

2. 在这个文件夹中，创建另一个文件夹来存放你的第一个网站。称之为 test-site（或更有想象力的东西）。

!!! notes

    文件夹和文件名使用小写，用短横线来分隔，这可以避免许多问题。如 my-file.html、test-site


网站的目录结构应该如下：

1. `index.html`：这个文件一般会包含主页内容，也就是人们第一次进入网站时看到的文字和图片。使用文本编辑器，创建一个名为index.html的新文件，并将其保存在test-site文件夹内。

2. `images` 文件夹：这个文件夹包含网站上使用的所有图片。在 test-site 文件夹内创建一个名为 images 的文件夹。

3. `styles` 文件夹：这个文件夹包含用于设置内容样式的 CSS 代码（例如，设置文本和背景颜色）。在你的 test-site 文件夹内创建一个名为 styles 的文件夹。

4. `scripts` 文件夹：这个文件夹包含所有用于向网站添加交互功能的 JavaScript 代码（例如，点击时加载数据的按钮）。在 test-site 文件夹内创建一个名为 scripts 的文件夹。

关于文件路径的通用规则：

- 若引用的目标文件与 HTML 文件同级，只需直接使用文件名，例如：my-image.jpg。

- 要引用子目录中的文件，请在路径前面写上目录名，再加上一个正斜杠。例如：subdirectory/my-image.jpg。

- 若引用的目标文件位于 HTML 文件的上级，需要加上两个点。举个例子，如果 index.html 在 test-site 的一个子文件夹内，而 my-image.jpg 在 test-site 内，你可以使用../my-image.jpg 从 index.html 引用 my-image.jpg。

- 以上方法可以随意组合，比如：../subdirectory/another-subdirectory/my-image.jpg。

!!! notes

     Windows 的文件系统使用反斜杠而不是正斜杠，例如：`C:\Windows`。这在 HTML 中并不重要——即使你在 Windows 系统上进行开发，你也应该在代码中使用正斜杠。

## HTML 基础

超文本标记语言（HyperText Markup Language，简称 HTML）是用来结构化 Web 网页及其内容的标记语言。

网页内容可以是：一组段落、一个重点信息列表、也可以含有图片和数据表。

**HTML 不是一门编程语言，而是一种用来定义内容结构的标记语言。**

HTML 由一系列的**元素**组成，这些元素可以用来包围不同部分的内容，使其以某种方式呈现或者工作。一对**标签**可以为一段文字或一张图片添加超链接，将文字设置为斜体，改变字号，等等。

比如，键入下面一行内容：

```
My cat is very grumpy
```

可以将这行文字封装成一个段落元素来使其在单独一行呈现：

```html
<p>My cat is very grumpy</p>
```

### HTML 元素详解

这个元素 `<p>My cat is very grumpy</p>` 包含的主要部分有：

1. **开始标签（Opening tag）**：包含元素的名称（本例为 p），被大于号、小于号所包围。表示元素从这里开始或者开始起作用——在本例中即段落由此开始。

2. **结束标签（Closing tag）**：与开始标签相似，只是其在元素名之前包含了一个斜杠。这表示着元素的结尾——在本例中即段落在此结束。

3. **内容（Content）**：元素的内容，本例中就是所输入的文本本身。

4. **元素（Element）**：开始标签、结束标签与内容相结合，便是一个完整的元素。

元素也可以有属性（Attribute）：

属性包含了关于元素的一些额外信息，这些信息本身不应显现在内容中。本例中，class 是属性名称，editor-note 是属性的值。class 属性可为元素提供一个标识名称，以便进一步为元素指定样式或进行其他操作时使用。

属性应该包含：

在属性与元素名称（或上一个属性，如果有超过一个属性的话）之间的空格符。
属性的名称，并接上一个等号。
由引号所包围的属性值。
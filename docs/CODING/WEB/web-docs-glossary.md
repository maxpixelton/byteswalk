# Web 相关术语的定义

## Abstraction

Abstraction，抽象编程。

在计算机编程领域中，抽象编程指的是在研发大型复杂软件系统时，通过抽象的方法来降低编程复杂度，实现系统快速高效设计和开发的编程模式。

它将系统各功能实现的技术细节隐藏在相对简单的 [API 接口]()之后

数据抽象的好处：

- 避免写出低等级代码。

- 避免代码重复，增加代码的复用性。

- 在不影响用户的前提下可以独立修改类的内部实现。

- 有效提升应用程序的安全性，因为只向用户提供重要的细节。


参见：

- 维基百科上的[抽象化](https://zh.wikipedia.org/wiki/抽象化_(計算機科學))


```js title="示例"
class ImplementAbstraction {
  // 设置一系列内部成员的值的方法
  set(x, y) {
    this.a = x;
    this.b = y;
  }
  display() {
    console.log(`a = ${this.a}`);
    console.log(`b = ${this.b}`);
  }
}
const obj = new ImplementAbstraction();
obj.set(10, 20);
obj.display();
// a = 10
// b = 20
```

## Accent

## Accessibility

## API

一个 API（Application Programming Interface，应用编程接口）是软件（应用）中的一系列特性和规则，这些特性和规则允许其他软件与之交互（与用户界面相对）。API 可被视为提供它的应用与其他软硬件之间的一个简单的合约（接口）。

在 Web 开发中，API 通常是开发者能用在应用（app）中的一系列代码特性（如 方法、属性、事件 和 URL），这些特性被用于与用户的 web 浏览器中的组件、用户电脑上的其他软硬件或者第三方软件与服务交互。

例如：

- getUserMedia API 能被用于从用户的摄像头采集音视频。接下来开发者可以任意使用这些音视频，例如记录视频和音频、在视频会议中向其他用户广播，或者从视频中截图。

- Geolocation API 能被用于从用户的可用的任意定位设备（如 GPS）获取位置信息，然后可以再用 Google Maps APIs 将这些位置信息用于在一个自定义的地图上标记出用户的位置和展示用户所在地区的旅游景点。

- Twitter APIs 能被用于从用户的 twitter 账户获取数据，然后可以在一个网页上展示他们最近的 tweet。

- Web Animations API 能被用于制作一个网页中的动画，例如让网页中的图片移动或旋转。

## Computer Programming

Computer Programming，计算机（电脑）编程。是将一系列指令整合起来的过程。这些指令以电脑能够理解的语言告诉电脑或者软件该做些什么。这些指令可由不同的语言呈现，例如 C++、Java、JavaScript、HTML、Python、Ruby 和 Rust。

使用一门合适的语言，我们可以编程/创建任意一种软件。例如，帮助科学家进行复杂运算的程序、存储大量数据的数据库、可以供人下载音乐的网站或可以供人制作电影的动画软件。
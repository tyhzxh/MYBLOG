+++
title = 'GameProgramming'
date = 2024-04-08T17:24:45+08:00
draft = true
+++

从校科协群里收集到的游戏开发相关的书籍《游戏编程与算法技巧》

又是一个新的栏目 阅读了一下本书的目录 感觉并不是很难的一本书（面向有一定编程基础的小白）

借此来实践一下学习的c++ c# lua 算法 线代 物理 英语等理论知识

本书提供了两个游戏demo 2D滚轴ios游戏（OBJECTIVE-C Cocos2D） 3D塔防游戏(C# XNA/MonoGame)

游戏之旅 启动！

## 游戏编程历史

### Atari

从1971年的第一款商业游戏Computer Space开始，作者也就是Atari的创始人，他们又开发了“Atari 2600”的游戏系统（不是在传统的PC上开发的，聚焦于家用机，刚推出的时候，硬件会被锁定5年多，称为一个“世代”）早期的游戏使用汇编语言编写

### NES和SNES

北美游戏市场崩溃后（主要原因在于，市场上充斥着大量质量低下的游戏。比如Atari版的Pac-Man及名声不佳的E.T）1985年推出的红白机(NES)把产业带回正轨

NES比Atari强大得多，这需要更多的“人时”去开发游戏。很多NES时期的游戏都需要不少程序员去开发。比如初代《塞尔达传说》需要3位著名程序员去开发。

到了超级任天堂(SNES)时代，开发人员进一步扩大，程序员会分工专门为某个游戏的某个功能进行深度开发。

NES和SNES的游戏仍然完全用汇编语言开发，因为内存依然不足。幸运的是任天堂有提供SDK(开发代码库)和开发工具，开发者不再像Atari时期那么痛苦。

### PS和PS2

在20世纪90年代中期推出的PS和N64终于可以用高级语言去开发游戏了。两大平台的游戏开发语言以C语言为主，但仍然有一些性能敏感的功能得用汇编语言完成。

由于高级语言带来了生产力提升，开发团队规模的增长在这个时期有所减缓

### Xbox,PS3,Wii

家用机的高画质表现导致游戏开发进入了两种境地。AAA游戏变得更加庞大，也有着相应庞大的团队和费用。而独立游戏则跟过去的小团队开发规模相仿。

对于AAA游戏，开发团队的规模增长让人震惊。《刺客信条：启示录》(Assassin's Creed: Revelations)参与开发的程序员总人数达75人。

这里有一个非常重要的概念-中间件

有的中间件是完整的游戏引擎，当下常见的游戏引擎 UE unity这些

有的则是专门地做某个系统，比如物理引擎Havok Physics。这样就节省了大量的人力、财力。但是缺点就是某个策划功能可能会受到中间件的限制。

### 未来

移动设备的性能提升得飞快，这就导致了越来越多的3D游戏在移动平台开发。

## 基本概念

20年来，早期的一些概念还是没有变化：游戏循环，游戏时间管理和游戏对象模型

### 游戏循环

一个传统的游戏循环可以分成3部分：处理输入、更新游戏世界、生成输出。一个基本的游戏循环可能是这样的:

```c#
while game is running
    process inputs
    update game world
    generate outputs
loop
```

process inputs会检查各种输入设备

update game world会执行所有激活并且需要更新的对象。

generate outputs

1.输出通常就是将图形渲染成2D或3D 

2.还有，家用机手柄通常会有振动效果，随着游戏进程振动，它也是一个需要输出的效果，也叫作力回馈。

3.多人游戏，游戏中需要输出数据给其他玩家。

### 游戏时间管理

真实时间和游戏时间

举其中一个例子

#### 游戏时间

由于硬件问题(处理器不同)，fps对真实时间有很大影响

故引入增量时间（从上一帧起流逝的时间）

```c#
// 更新x位置150像素/每秒
enemy.position.x += 150 * deltaTime
```

现在代码不管帧率如何都能正常工作

#### 物理行为异常

当然还有其他问题

虽然能够在不同帧率下都正确执行，但还是会有一些问题。首先是任何与物理相关的游戏(比如平台跳跃类)在帧率的表现上都会很不一样。这是数值积分的原因。这会导致奇怪的角色行为，比如在低帧率下跳得更高。还有就是那些支持多人互动的游戏，可能在不同帧率下导致行为异常。

这个问题最简单的解决方法就是限制帧率，强制游戏循环等待到指定帧率才继续。比如一个目标帧率为30FPS的游戏，如果游戏循环本身只用了30ms，那么还要等待额外的3.3ms才能开始执行下一次的游戏循环。

```c#
//30FPS每帧33.3ms
targetFrameTime = 33.3f
while game is running
    realDeltaTime = time since last frame
    gameDeltaTime = realDeltaTime * gameTimeFactor

    //处理输入
    ···
    update game world with gameDeltaTime

    //渲染输出
    ···
    while (time spent this frame) < targetFrameTime
        //做一些事情将多出的时间用完
        ···
    loop
loop
```

#### 卡顿

游戏突然遇上复杂情形，导致某帧比目标帧率的时长长怎么办？有很多解决办法，最常见的就是为了跟上目标帧率，而丢弃这一帧的渲染。这就是有名的卡帧，这么做会引起视觉上的卡顿。你可能会注意到有时候玩游戏的时候，做某些事情就会卡一下。

### 游戏对象模型

在3种游戏对象中，最常见的就是更新和绘制都需要的对象。任何角色、生物或者可以移动的物体都需要在游戏循环中的update game world阶段更新，还要在generate outputs阶段渲染。

只绘制不更新的对象，称为静态对象。这些对象就是那些玩家可以看到，但是永远不需要更新的对象。它可以是游戏背景中的建筑。一栋建筑不会移动也不会攻击玩家，但是需要绘制。

第三种游戏对象，就是那些需要更新，但不需要绘制的对象。例如，摄像机和触发器。

游戏循环中的游戏对象
有很多方法来表示游戏对象。其中一种就是面向对象中的接口。接口更像是一种契约。如果一个类实现了这个接口，那么就相当于要按照契约去实现这个接口中所有的函数。

首先，需要由一个基础的游戏对象类，使得3种游戏对象都可以从这里继承:

```c++
class GameObject
    //成员变量/函数
    ...
end
```

任何游戏对象公用的功能，不管什么对象类型，都应该放在基类里。这样就可以声明两个接口，一个是Drawable对象，一个是Updateable对象:

```c++
interface Drawable
    function Draw()
end

interface Updateable
    function Update(float deltaTime)
end
```

一旦有了这两个接口，就可以通过两个接口和一个基类来表示3种游戏对象：

```c++
//只更新的游戏对象
class UGameObject inherits GameObject, implements Drawable
    //重载绘制函数
    ...
end

//只渲染的游戏对象
class DGameObject inherits GameObject, implements Drawable
    //重载绘制函数
    ...
end

//更新且绘制的游戏对象
class DUGameObject inherits UGameObject, implements Drawable
    //重载绘制和更新函数
    ...
end
```

在实现这3种对象后，把它们整合在游戏循环中是很简单的。GameWorld类拥有两个列表，分别在游戏世界中管理Updateable对象和Drawable对象:

```c++
class GameWorld
    List updateableObjects
    List drawableObjects
end
```

游戏对象被创建出来后，必须添加到相应的列表中。相应的，删除游戏对象也要讲其从列表移除。在存储了所有游戏对象之后，就可以将游戏循环中的update game world部分完成。

```c#
while game is running
    realDeltaTime = time since last frame
    gameDeltaTime = realDeltaTime * gameTimeFactor

    //处理输入
    ...

    //更新游戏世界
    foreach Updateable  o in GameWorld.updateableObjects
        o.Update(gameDeltaTime)
    loop

    //渲染输出
    foreach Drawable o in GameWorld.drawableObejcts
        o.Draw()
    loop

    //帧数限制代码
    ...
```

## 拓展

一些关于以上模块的资料

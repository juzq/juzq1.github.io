---
layout: post
title: 射击游戏中准心与子弹弹道的探索
date: 2019-11-5 8:00:01 +0800
categories: 总结归纳
tag: UE4
---

* content
{:toc}

![](https://gitee.com/juzii/res/raw/master/pic/2019/10/shoot-game-bullet-track/1.png)

# 前言 {#preface}

&emsp;&emsp;现如今，精品游戏竞争激烈，各大游戏厂商都在争相推出“3A”级品质的游戏。而目前比较热门的科幻、战争、动作等品类大多都有枪械射击内容，因此，“玩枪”便成了很大一部分游戏中常见的玩法。当前比较热门的使命召唤系列、战地系列、无主之地系列、战争机器系列还有最近流行的吃鸡类游戏（绝地求生、堡垒之夜）等等都是玩枪，核心玩法类型上都是属于射击类游戏。因此做好武器与射击体验，还原逼真的射击情景，便成了这类游戏的核心卖点。

# 射击游戏的分类 {#classify}

&emsp;&emsp;目前的射击游戏主要分为两种，一种为“第一人称射击游戏”（FPS）与“第三人称射击游戏”（TPS）。

## 第一人称 {#fps}

&emsp;&emsp;第一人称射击游戏是以玩家主视角进行的射击游戏。玩家不再像别的游戏类型一样操纵屏幕中的虚拟人物来进行游戏，而是身临其境的主视角，体验游戏带来的视觉冲击，这就大大增强了游戏的主动性和真实感。

![](https://gitee.com/juzii/res/raw/master/pic/2019/10/shoot-game-bullet-track/2.png)

![](https://gitee.com/juzii/res/raw/master/pic/2019/10/shoot-game-bullet-track/3.png)

&emsp;&emsp;如上两张图分别是1999年发售的反恐精英（ Counter-Strike  ）与2019年发售的使命召唤：现代战争  （ Call of Duty: Modern Warfare ）。可以看到，时隔20年，场景与枪械变得更加精细与逼真，界面变得精美与合理，但不变的是，屏幕中间都做有一个“准心”。

## 第三人称 {#tps}

 &emsp;&emsp;第三人称射击游戏与第一人称区别在于，屏幕上显示的主角的视野不同，并且第三人称中玩家控制的游戏人物在游戏屏幕上是可见的，因而第三人称射击游戏加强调更强调动作感。 

![](https://gitee.com/juzii/res/raw/master/pic/2019/10/shoot-game-bullet-track/4.png)

![](https://gitee.com/juzii/res/raw/master/pic/2019/10/shoot-game-bullet-track/5.png)

![](https://gitee.com/juzii/res/raw/master/pic/2019/10/shoot-game-bullet-track/6.png)

&emsp;&emsp;如上3张图都是“幽灵行动：断点”的游戏画面，可以看到，在没有瞄准时屏幕中央是没有准心的，这时可以将注意力集中在环境、角色动作、游戏剧情等等。当打开瞄准时，就会出现屏幕中央的准心，还可以通过Alt键切换第一人称与第三人称（第二张图与第三张图）。

# 准心 {#concentric}

&emsp;&emsp;通过上述的介绍可以看到，无论是第一人称还是第三人称，屏幕中央都会有“准心”以方便玩家瞄准。那为什么要有准心呢？为什么有了准心之后，我们在游戏中就可以瞄准目标呢？

## 现实中的射击 {#real-world-shoot}



![](https://gitee.com/juzii/res/raw/master/pic/2019/10/shoot-game-bullet-track/6.5.jpg)

&emsp;&emsp;我们在物理课程中了解过，如果抛去枪的复杂结构不谈，枪的工作原理简单来讲就是，弹头在枪管中受到推进力后做抛物线运动。

![](https://gitee.com/juzii/res/raw/master/pic/2019/10/shoot-game-bullet-track/7.gif)

&emsp;&emsp;如果弹头在运动过程中碰撞到目标，则表示击中；若未碰撞到目标，则表示未击中。那么我们如何瞄准才能击中目标呢？

![](https://gitee.com/juzii/res/raw/master/pic/2019/10/shoot-game-bullet-track/8.png)

&emsp;&emsp;如图所示，蓝色的水平线则是瞄准线，代表瞄准方向；绿色是枪管轴心线，代表枪管的指向方向；红色则是子弹的飞行轨迹。假定子弹每次从枪口中射出的速度是固定的，空气阻力也是固定的，子弹的重力也是固定的，那么按照上图的瞄准方式，射击命中的“远交点”也是固定的。换句话说，用这把枪和这个瞄准角度的话，只能击中距离枪口x米的目标。那么如果我们射击同一方向上比x米更近或更远的目标怎么办呢？

![](https://gitee.com/juzii/res/raw/master/pic/2019/10/shoot-game-bullet-track/9.png)

![](https://gitee.com/juzii/res/raw/master/pic/2019/10/shoot-game-bullet-track/10.png)

&emsp;&emsp;如上图所示，分别为步枪和狙击枪的射击距离调整方法。步枪中有瞄准距离刻度线，调节刻度即可；狙击枪在瞄准高倍镜中有刻度线，根据目标的距离，使用对应的瞄准刻度线即可；手枪因为射击距离短（大部分射击距离在50米以内），瞄准误差较小，所以不用调节。

## 游戏中的射击 {#in-game-shoot}

![](https://gitee.com/juzii/res/raw/master/pic/2019/10/shoot-game-bullet-track/11.png)

&emsp;&emsp;而在游戏中，未开瞄准镜的情形下，角色的持枪动作往往如上图所示，将枪固定于肘关节处。为什么要这样持枪呢？因为这样持枪姿势可以减小在第一人称人下枪所遮住的视野，提升游戏体验，而这样的持枪动作，在现实中是打不准目标的（因为没有三点一线的瞄准）。在游戏里为了能更加便捷开枪，因此加入了准心的概念。

![](https://gitee.com/juzii/res/raw/master/pic/2019/10/shoot-game-bullet-track/12.jpg)

&emsp;&emsp;有了准心之后，玩家就可以更加方便地瞄准目标，从而获得更好的射击体验。但这种准心“指哪打哪”的游戏效果是如何实现的呢？

# UE4中的子弹弹道 {#bullet-trace-in-ue4}

&emsp;&emsp;我刚开始在UE4中实现子弹弹道时，想法很简答，既然子弹是从枪中射出的，那么子弹的飞行方向当然就是枪口的朝向。为了使子弹能够击中“准心”的位置，我调整了枪在手中的朝向。

## 问题 {#problems}

<iframe src="//player.bilibili.com/player.html?aid=74612974&cid=127622995&page=1&high_quality=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width="800" height="550"> </iframe>
&emsp;&emsp;经过反复测试后发现，无论怎么调，只能保证在某一固定射击距离时，弹着点能与准心重合。当射击距离改变时，子弹就无法击中准心位置，这是什么原因呢？

![](https://gitee.com/juzii/res/raw/master/pic/2019/10/shoot-game-bullet-track/13.png)

&emsp;&emsp;如上图所示，假定玩家（摄像机）与枪处在同一水平面，从俯视的角度来看，玩家的准心永远指向正前方（用红色线条表示），而枪则在玩家左手或右手，其朝向（用绿色线条表示）与玩家朝向存在夹角Θ。在该夹角下，只有玩家与目标距离y时，弹着点才能刚好在准心上。无论怎么调整夹角Θ，都只有某一个距离下弹着点刚好落在准心上（因为两条不平行的直线永远只有一个交点）。而实际上枪与玩家（摄像头）并不在同一水平面，在三维坐标系下，两条不平行的直线最多有一个交点（可能没有交点）。

&emsp;&emsp;那么该如何解决该问题呢？既然两条线只有一个交点，那么能不能根据不同的射击距离改变枪的朝向，使交点始终在准心瞄准位置呢？

![](https://gitee.com/juzii/res/raw/master/pic/2019/10/shoot-game-bullet-track/18.png)

&emsp;&emsp;要想调整枪的朝向，就必须调整枪在玩家动画的骨骼中的相对位置。而且要根据射击距离实时调整（玩家准心瞄到的障碍物的距离），这个运算量会非常大，在现阶段是不可能实现的。而且，角色在“静止状态”时，会随着呼吸而晃动枪的朝向，会对计算结果产生偏差，所以实时调整枪的朝向这条路是走不通的，那该怎么办呢？

## 解决方案 {#solution}

![](https://gitee.com/juzii/res/raw/master/pic/2019/10/shoot-game-bullet-track/14.png)

&emsp;&emsp;既然枪的方向不方便调整，子弹的方向是可以控制的。可以在开枪时，先根据玩家（摄像机）的朝向，做射线检测，确定目标弹着点，然后再控制子弹的飞行方向为枪口飞向弹着点，这不就可以实现无论玩家瞄向哪里，子弹都可以击中“准心”的位置的需求吗。

## 计算弹着点 {#hit-point}

![](https://gitee.com/juzii/res/raw/master/pic/2019/10/shoot-game-bullet-track/15.png)

![](https://gitee.com/juzii/res/raw/master/pic/2019/10/shoot-game-bullet-track/16.png)

&emsp;&emsp;如上蓝图代码所示，用摄像机方向（即准心的朝向）做射线检测，返回击中的弹着点坐标与材质（以便后续播放击中特效），若未击中任何物体（例如朝天上开枪），则返回射线检测的终点，以便客户端展示弹道。

## 广播开枪 {#broadcast-shoot}

![](https://gitee.com/juzii/res/raw/master/pic/2019/10/shoot-game-bullet-track/17.png)

&emsp;&emsp;如上蓝图代码所示，获得弹着点坐标与材质后进行广播，在所有客户端上播放该玩家的开枪动画（武器特效），并调用C++函数，利用GamePlayTask生成弹道。

```c++
void AWeapon::GenerateBulletTrack(FVector HitLocation, UPhysicalMaterial* HitMaterial)
{
    // 枪口坐标
    FVector MuzzleLocation = Mesh->GetSocketLocation("Muzzle");
    // 向量方向：终点 - 起点
    FVector Direction = HitLocation - MuzzleLocation;
    // 飞行速度
    FVector Speed = Direction.Rotation().Vector() * 10000;
    // 飞行时间：距离 / 速度
    float Time = Direction.Size() / Speed.Size();
    // 击中点特效
    UParticleSystem* HitFX = nullptr;
    // 击中点材质
    if (HitMaterial)
    {
        EPhysicalSurface SurfaceType = HitMaterial->SurfaceType;
        // 该枪已设置该材质类型的击中特效
        if (VarHitFXs.Contains(SurfaceType))
        {
            HitFX = VarHitFXs[SurfaceType];
        }
    }
    // 运行子弹轨迹的GamePlayTask
    UBulletTrackTask * TrackTask = UBulletTrackTask::InitBulletTrack(BulletTrackComponent, MuzzleLocation, Speed, TargetFX, HitFX, Time, this, HitLocation);
    TrackTask->ReadyForActivation();
}
```

&emsp;&emsp;如上C++代码所示，根据击中点坐标，计算出子弹的飞行方向与距离，然后计算出飞行时间，并用这些参数开启子弹轨迹的GamePlayTask，用于显示客户端的子弹轨迹及击中特效。

## 子弹弹道 {#bullet-trace}

```c++
void UBulletTrackTask::TickTask(float DeltaTime)
{
    // 记录飞行时间
    ActiveTime += DeltaTime;
    UWorld* World = GetWorld();
    check(World);
    const FVector OldLocation = CurrentLocation;
    // 匀速距离公式：距离 = 速度 * 时间
    FVector MoveDistance = CurrentVelocity * DeltaTime;
    // 新位置
    CurrentLocation = OldLocation + MoveDistance;
    // 更新轨迹特效的位置与朝向
    if (TrackComponent)
    {
         TrackComponent->SetWorldLocationAndRotation(CurrentLocation, UKismetMathLibrary::MakeRotFromX(CurrentVelocity.GetSafeNormal()));
    }
    // 超过飞行时间
    if (ActiveTime >= LifeTime)
    {
        // 有击中特效
        if (HitFX)
        {
            // 显示击中特效
            UGameplayStatics::SpawnEmitterAtLocation(World, HitFX, HitLocation);
        }
        EndTask();
    }
}
```

&emsp;&emsp;通过运行该子弹轨迹的GamePlayTask，每帧会更新子弹轨迹特效的位置，显示子弹的飞行过程，如果超过了子弹的飞行时间，则在服务器已计算出的击中点产生击中特效（受伤害特效）等等，实现了子弹总是能击中游戏准心瞄准的目标。

<iframe src="//player.bilibili.com/player.html?aid=74613873&cid=127622995&page=1&high_quality=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width="800" height="550"> </iframe>
# 展望 {#outlook}

&emsp;&emsp;上述解决方案是弹道实现的较简化版本，因为没有考虑子弹的飞行时间、子弹重力、枪械后坐力、空气阻力等诸多因素。如果考虑这些因素的话，就不适合直接在服务器用射线检测来计算弹着点，而是同样改用GamePlayTask来实现，以便能够精确计算子弹飞行过程中的受力、飞行碰撞等问题。

![](https://gitee.com/juzii/res/raw/master/pic/2019/10/shoot-game-bullet-track/19.gif)

&emsp;&emsp;如果是单机游戏则不会有影响，因为客户端本地计算，不会有延迟问题，但如果是多人在线游戏，这样做会增加服务器弹着点计算耗时（因为会增加子弹飞行时间），从而加大客户端的表现困难（让玩家感受到开枪有延时），本方案的缺点就显露了出来。

![](https://gitee.com/juzii/res/raw/master/pic/2019/10/shoot-game-bullet-track/20.png)

&emsp;&emsp;那么，可行的优化思路是怎样的呢？可以先在服务器预计算出（或者客户端根据当前摄像机朝向自行计算）弹着点，根据预计算的弹着点作为子弹飞行方向，然后在客户端与服务器中同步运行子弹飞行任务，根据子弹运行过程中的碰撞点作为实际弹着点，最后服务器再同步碰撞结果。因此，处理好客户端与服务器的弹道同步，就成为了后续工作的关键。

&emsp;&emsp;以上就是针对游戏开发中子弹弹道实现的探索，若有分析得不妥之处，还请共同探讨。
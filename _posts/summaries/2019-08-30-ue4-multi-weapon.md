
# 回顾

&emsp;&emsp;学习UE4已有近2周的时间，跟着数天学院“UE4游戏开发”课程的学习，已经完成了UE4蓝图方面比较基础性的学习。通过UE4蓝图的开发，我实现了类似CS的单人版射击游戏，效果如下视频：  

<iframe src="//player.bilibili.com/player.html?aid=66018584&cid=114524544&page=1&high_quality=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width="800" height="550"> </iframe>

&emsp;&emsp;不得不说UE4蓝图功能的强大，无需写一句代码，就能实现一个基本的游戏玩法。并且使用门槛极低，只要熟悉蓝图的API，通过“拖拖，连连”就能完成游戏玩法的开发，对游戏策划（设计师）及其友好，与C++相比，生产效率极高。



# 多武器系统

&emsp;&emsp;目前的游戏设定是开场后，角色身上就自动装备了一把武器，为了实现类似于CS雪地地图一样的设定：开场没有武器，地图中摆放着多把武器，需要从地上拾取后才能使用，就需要将当前的武器系统进行扩展，创建多个蓝图来实现不同武器的模型、特效、属性等等。

## 武器玩法逻辑的处理

&emsp;&emsp;之前只有单个武器时，武器蓝图包含有玩法逻辑例如弹道计算、粒子显示、开枪动画、伤害控制等等，这些玩法（GamePlay）的逻辑是写在武器蓝图里面的。如果按照以前的设计，创建多个武器蓝图时，这些武器中的玩法逻辑也需要拷贝多份。

### 拷贝

&emsp;&emsp;然而，“拷贝”这种开发方式在程序编码中是非常不推荐的。所谓拷贝，意味着需要维护多份同样逻辑实现的代码，如果后续需要对该部分玩法进行调整 优化~~（策划又要改需求）~~，那么拷贝了多少次，就需要修改多少次。例如，项目后期有50把武器（参考穿越火线），如果在前期为了省事将武器的玩法逻辑拷贝了49次，那么如果某天有该逻辑的修改需求时，就需要将该修改操作重复50次，这是让人崩溃的一件事。**因此，在软件开发中，不要轻易使用Ctrl-C + Ctrl-V。**

### 引用

&emsp;&emsp;那么该如何解决该问题呢？软件开发中常用的方法是将拷贝转变为引用。所谓引用，意思就是将公共的部分剥离出来，形成函数（方法），在需要该逻辑的地方引用该函数即可。如果后期有修改需求，只需要修改该函数一个地方，就可以实现多处逻辑被同时修改。同样，在UE4的蓝图中也提供了函数（Functions）这样的功能，通过创建函数可以将蓝图中公共的逻辑封装 ，从而实现多处引用。但是，UE4的蓝图是面向对象的，不同的武器（对象）之间是不能共用函数的，因此，将公共逻辑改为引用的方式是行不通的。

### 继承

&emsp;&emsp;既然蓝图是面向对象的，那么可以使用面向对象的编程特点：继承。所谓继承，就是将函数、变量封装到父类，从该父类集成出来的子类可以使用父类暴露出来的方法与属性。利用继承的特性，于是我们就可以从蓝图的Actor类中继承出Weapon类，而我们游戏中的各种武器，例如手枪、冲锋枪、狙击枪、火箭炮等等可以使用武器中封装的一些逻辑，比如开枪，换弹匣，等等，继承图如下：

![](https://gitee.com/juzii/res/raw/master/pic/2019/08/ue4_multi_weapon/1.png)

&emsp;&emsp;

&emsp;&emsp;可以看到，各种武器继承了Weapon之后，就拥有的子弹的变量，也拥有了开枪的函数，从而实现了武器逻辑的复用。

## 武器属性的设置

&emsp;&emsp;不同的武器除了模型（Mesh）不同以外，还有子弹数量、开枪动画、装弹动画、子弹撞击粒子、子弹伤害等等不同的属性，不同的武器这些属性都不同，而这些属性都需要在父类Weapon中进行处理。那么如何才能为不同的武器配置这些属性呢？这就涉及到C++变量如何暴露给蓝图使用。

&emsp;&emsp;根据[官方文档：虚幻反射系统](https://docs.unrealengine.com/zh-CN/Programming/Introduction/#%E8%99%9A%E5%B9%BB%E5%8F%8D%E5%B0%84%E7%B3%BB%E7%BB%9F)，C++中的变量可以被`UPROPERTY()`宏修饰，就可以暴露给蓝图使用，还可以根据需要设定访问权限。

C++代码如下：

```c++
	// 击中目标粒子
	UPROPERTY(EditDefaultsOnly)
	UParticleSystem* TargetFX;

	// 开枪动画
	UPROPERTY(EditDefaultsOnly)
	UAnimationAsset* ShootAnim;

	// 开枪间隔
	UPROPERTY(EditDefaultsOnly)
	float ShootInterval;

	// 换弹匣动画
	UPROPERTY(EditDefaultsOnly)
	UAnimationAsset* ReloadAnim;

	// 换弹匣时间
	UPROPERTY(EditDefaultsOnly)
	float ReloadTime;

	// 每颗子弹伤害值
	UPROPERTY(EditDefaultsOnly)
	float Damage;

	// 最大子弹数
	UPROPERTY(EditDefaultsOnly)
	int8 MaxBullet;

	// 是否正在换弹匣
	UPROPERTY(BlueprintReadOnly)
	bool Reloading = false;

	// 是否正在射击
	UPROPERTY(BlueprintReadOnly)
	bool Shooting = false;

	// 当前子弹数（因为要暴露给蓝图获取，所以类型扩充到int32）
	UPROPERTY(BlueprintReadOnly)
	int32 CurrentBullet;
```

自动生成的蓝图设置如下：

![](https://gitee.com/juzii/res/raw/master/pic/2019/08/ue4_multi_weapon/2.png)

&emsp;&emsp;可以看到，如果变量是粒子指针和动画的指针，蓝图中则直接生成了对应的可视化选择框，太方便了有木有。这不就是策划所需要的配置表吗，还是可视化的，再也不用担心把文件名配错了。

## 武器逻辑

&emsp;&emsp;除了变量，C++函数有类似的处理方法，宏`UFUNCTION()`可以将函数暴露给蓝图，供蓝图调用。因此，上文中提到的换弹匣的逻辑，就可以移植到C++中，从而给予所有武器具有开枪与换弹匣能力。

开枪的核心代码如下（已精简）：

```c++
// 获取坐标与朝向
World->GetFirstPlayerController()->GetPlayerViewPoint(Location, Rotation);
// 播放开枪动画
Mesh->PlayAnimation(ShootAnim, false);
// 计算终点坐标 = 起点坐标 + 方向 * 距离
FVector EndLocation = Location + Rotation.Vector() * 10000;
// 发射射线
World->LineTraceSingleByChannel(Result, Location, EndLocation, ECC_WorldStatic, ccq);
// 已击中
if (Result.Actor.IsValid())
{
    // 播放粒子
    FRotator EmitterRotation = FRotator(0, 0, 0);
    AActor* Actor = Result.Actor.Get();
    UGameplayStatics::SpawnEmitterAtLocation(Actor, TargetFX, Result.Location, EmitterRotation);
    // 中弹的是Character
    if (dynamic_cast<ACharacter*>(Actor) != NULL)
    {
        // 受伤害
        ACharacter* Shooter = dynamic_cast<ACharacter*>(WeaponOwner);
        UGameplayStatics::ApplyDamage(Actor, Damage, Shooter->GetController(), this, NULL);
    }
}
```

接下来是换弹匣：

```c++
Reloading = true;
// 播放动画
Mesh->PlayAnimation(ReloadAnim, false);
// 设置延时回调
GetWorldTimerManager().SetTimer(ReloadTimer, this, &AWeapon::ReloadFinish, ReloadTime);

void AWeapon::ReloadFinish()
{
	CurrentBullet = MaxBullet;
	Reloading = false;
}
```

因为换弹匣并不是瞬间换好（按了R键需要等一定时间后子弹才会恢复），因此使用了定时器来实现。

# 遇到的问题

*   蓝图中绑定的Mesh无法传递到C++

&emsp;&emsp;我按照之前的方法，在C++中定义好骨骼Mesh指针：

```c++
// 武器mesh
UPROPERTY(EditDefaultsOnly)
USkeletalMeshComponent* MeshComponent;
```

&emsp;&emsp;编译之后，兴冲冲的跑到蓝图中准备选择Mesh，然而却发现蓝图中却没有出现MeshComponent这个字段，以为是C++代码没有编译到，于是就反复试了几次，结果还是没有。怎么办呢？怀疑是UE4的这个反射系统不支持`USkeletalMeshComponent*`这种变量类型，把变量类型改为`int32`后，果然，这个字段出现了。

&emsp;&emsp;SkeletalMeshComponent不行，那么父类MeshComponent呢？

```c++
// 武器mesh
UPROPERTY(EditDefaultsOnly)
UMeshComponent* MeshComponent;
```

&emsp;&emsp;然而，编译之后蓝图中还是没有......看来的确是不支持`USkeletalMeshComponent*`或者`UMeshComponent*`这种类型。怎么办呢？我突然想到，既然不支持在蓝图中直接选择默认值，那么在蓝图中调用set方法来设置该变量吧！

&emsp;&emsp;于是，我将C++代码修改为：

```c++
// 武器mesh
UPROPERTY(BlueprintReadWrite)
USkeletalMeshComponent* MeshComponent;
```

&emsp;&emsp;蓝图如下：

![](https://gitee.com/juzii/res/raw/master/pic/2019/08/ue4_multi_weapon/3.png)

&emsp;&emsp;这样虽然实现了需求，但是需要在每一把武器蓝图中做一样的调用，如果有50把武器呢？100把武器呢？这样做明显与我的期望不一致，因此这种方法虽然可行，但是不可取。

&emsp;&emsp;还有其他办法吗？通过查询UE4 C++的API，我找到了`AActor::GetComponentByClass`这个函数，官方文档的描述是：

>   Searches components array and returns first encountered component of the specified class

&emsp;&emsp;即查找并获取本Actor的指定类型的组件的第一个。这不正是我需要的吗？如果我指定查找类型为USkeletalMeshComponent，而每个武器只有一个USkeletalMeshComponent，那这样不就从蓝图中获取到了绑定的Mesh吗？实现代码如下：

```
void AWeapon::BeginPlay()
{
	Super::BeginPlay();
	// 获得Mesh
	MeshComponent = dynamic_cast<USkeletalMeshComponent*>(GetComponentByClass(USkeletalMeshComponent::StaticClass()));
}
```

&emsp;&emsp;通过这种方式，实现了武器的全部逻辑从蓝图中去除。当新加一件武器时，只需要在武器蓝图属性中配置动画、粒子、子弹容量等属性后，就可以直接在游戏中使用。

<iframe src="//player.bilibili.com/player.html?aid=66014316&cid=114517140&page=1&high_quality=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width="800" height="550"> </iframe>


# 蓝图与C++选择的思考

&emsp;&emsp;既然UE4同时支持蓝图与C++，那么我们在开发时应该如何选择呢？官方文档有如下的解释：

>   程序员利用C++即可添加基础Gameplay系统，然后设计师可基于这些系统进行构建或利用这些系统为某个特定关卡或游戏本身创建自定义Gameplay。

&emsp;&emsp;也就是说，程序员用C++开发一些基础的系统，例如本文当中的武器系统（Weapon），设计师（策划）即可利用该武器系统在蓝图上进行武器的扩充，设计出不同的武器；设计师（策划）也可以利用C++开发的基础系统，将这些系统在蓝图上进行组装，以构建更丰富的玩法系统。除此之外，

1.  蓝图的可重构性非常非常非常**低**，如果某个模块的逻辑比较复杂，就会出现各种线条乱飞，非常的凌乱，过段时间再过来看，可能作者自己都看不明白了。因此我建议，对于比较复杂的逻辑，最好使用C++来实现，如果一定要用蓝图，请将该部分逻辑拆分为几段小逻辑来实现（充分利用蓝图的函数与宏）。
2.  蓝图本身是作为二进制文件来保存（.uasset），在版本管理工具中（Git）无法进行差异性对比，如果对于某段频繁修改（升级）的逻辑，又想看到每次修改的变化，最好也使用C++来实现，可以对比每个版本的修改内容。
3.  对于复杂的数学运算或循环次数较多的逻辑，也最好采用C++来实现，以保证运行效率。
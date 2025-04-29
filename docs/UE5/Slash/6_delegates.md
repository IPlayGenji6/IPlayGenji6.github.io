---
title: 委托
---

**委托**是一种泛型但类型安全的方式，可在C++对象上调用成员函数。可使用委托动态绑定到任意对象的成员函数，之后在该对象上调用函数，即使调用程序不知对象类型也可进行操作。复制委托对象很安全。你也可以利用值传递委托，但这样操作需要在堆上分配内存，因此通常并不推荐。请尽量通过引用传递委托。虚幻引擎共支持三种类型的委托：
单点委托
组播委托
动态(UObject, serializable)

**单播（Single-cast）**：类型安全地封装对单一函数的调用引用，只能绑定一个回调函数。

**多播（Multicast）**：同一委托上可绑定多个回调，触发时按绑定顺序逐一调用。​
    
**动态（Dynamic）**：支持通过 AddDynamic 在运行时绑定 UObject 上标记了 UFUNCTION() 的成员函数，可在 Blueprint 中可视化绑定。​
    
稀疏（Sparse）：与普通多播委托相比，稀疏委托使用按组件实例索引稀疏存储监听器，减少空插槽开销，适合大量组件场景。

## 声明委托
```c++
DECLARE_DYNAMIC_MULTICAST_SPARSE_DELEGATE_SixParams(
    FComponentBeginOverlapSignature,
    UPrimitiveComponent,
    OnComponentBeginOverlap,
    UPrimitiveComponent*, OverlappedComponent,
    AActor*,        OtherActor,
    UPrimitiveComponent*, OtherComp,
    int32,           OtherBodyIndex,
    bool,            bFromSweep,
    const FHitResult&, SweepResult
);
```
**参数含义**：
FComponentBeginOverlapSignature
生成的委托类型名，后续可用此类型声明属性或成员。​

UPrimitiveComponent
委托所属类，稀疏存储会以该类的实例索引作为键。​

OnComponentBeginOverlap
委托成员名称，通常伴随 UPROPERTY(BlueprintAssignable) 标记以便在 Blueprint 中使用。​

参数列表（类型与名称）
UPrimitiveComponent* OverlappedComponent：触发重叠事件的组件指针。​

AActor* OtherActor：与之发生重叠的另一个 Actor。​

UPrimitiveComponent* OtherComp：另一个 Actor 的触发组件。​

int32 OtherBodyIndex：另一个组件的身体索引，用于复杂的物理子体情况。​

bool bFromSweep：如果是移动中触发，表明是否进行扫射检测。​

const FHitResult& SweepResult：扫射检测的详细碰撞信息。

## 使用方式
在头文件中声明
```c++
UPROPERTY(BlueprintAssignable, Category="Collision")
FComponentBeginOverlapSignature OnComponentBeginOverlap;
```

在cpp中绑定
```c++
// 必须在类中声明符合签名的函数，并加 UFUNCTION()
UFUNCTION()
void HandleOverlap(
  UPrimitiveComponent* OverlappedComponent,
  AActor* OtherActor,
  UPrimitiveComponent* OtherComp,
  int32 OtherBodyIndex,
  bool bFromSweep,
  const FHitResult& SweepResult
);

// 在初始化时绑定
MyPrimitiveComponent->OnComponentBeginOverlap.AddDynamic(
  this, &MyClass::HandleOverlap
);
```
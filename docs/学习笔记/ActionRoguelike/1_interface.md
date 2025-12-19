# UE5 学习笔记：解耦利器——蓝图与 C++ 接口

在软件设计中，接口（Interface）是实现 **解耦 (Decoupling)** 的核心工具。它符合 **SOLID** 原则中的：
* **依赖倒置原则 (DIP)**：高层模块（玩家）不应依赖底层模块（具体的箱子），两者都应依赖于抽象（接口）。
* **接口隔离原则 (ISP)**：不强迫一个类实现它用不到的方法。

### 场景对比：
* **不用接口**：玩家检测到物体 -> `Cast To BP_Chest` -> `Cast To BP_Door` ... 每增加一个物体都要修改玩家代码，产生强耦合。
* **使用接口**：玩家检测到物体 -> 询问“你实现了交互接口吗？” -> 实现了就执行，不管对方是谁。

---

## 2. 蓝图接口 (Blueprint Interface) 实现

### 步骤一：定义接口
1. 右键菜单：`Blueprints` -> `Blueprint Interface`，命名为 `BPI_Interactable`。
2. 添加函数 `Interact`。
   * **注意**：无返回值的函数在实现时表现为 **Event (事件)**；有返回值的表现为 **Function (函数)**。

### 步骤二：实现接口 (以箱子为例)
1. 打开 `BP_Chest` -> `Class Settings` -> `Interfaces` 中添加 `BPI_Interactable`。
2. 在左侧列表找到 `Interact`，右键选择 **Implement Event**。
3. 连入逻辑：`Print String` 或播放开关盖动画。

### 步骤三：调用接口 (发信)
1. 玩家通过射线检测获取 `Hit Actor`。
2. 拖出引脚，搜索并调用 `Interact (Message)`。
   * **关键点**：必须使用带 **Message** 后缀的节点，这样即使目标没实现接口也不会报错。

---

## 3. C++ 接口 (UInterface) 深度解析

C++ 接口采用“双类结构”：`U` 类处理反射，`I` 类处理逻辑。

### 第一步：定义接口
```cpp
// InteractableInterface.h
UINTERFACE(MinimalAPI, Blueprintable)
class UInteractableInterface : public UInterface { GENERATED_BODY() };

class IInteractableInterface {
    GENERATED_BODY()
public:
    // BlueprintNativeEvent 允许 C++ 实现默认逻辑，同时允许蓝图重写
    UFUNCTION(BlueprintCallable, BlueprintNativeEvent, Category = "Interaction")
    void Interact(AActor* Interactor);
};
```

### 第二步：类实现接口
```c++
// Chest.h 继承 I 类
class AChest : public AActor, public IInteractableInterface {
    GENERATED_BODY()
public:
    // C++ 实现必须带 _Implementation 后缀
    virtual void Interact_Implementation(AActor* Interactor) override;
};
```

```c++
// Chest.cpp
void AChest::Interact_Implementation(AActor* InInteractor)
{
    // 实现具体逻辑，例如打开箱子
    UE_LOG(LogTemp, Log, TEXT("箱子被交互了！交互者是: %s"), *InInteractor->GetName());
}
```

### 第三步：安全地调用接口 (Character.cpp)

在 C++ 中，为了兼容蓝图实现的接口，必须使用静态执行函数 `Execute_` + `函数名`。

```cpp
void AMyCharacter::PerformInteraction(AActor* TargetActor)
{
    // 1. 检查目标是否有效，且是否实现了该接口
    // 使用 Implements<UInterfaceClass> 可以同时检测 C++ 和蓝图类
    if (TargetActor && TargetActor->Implements<UInteractableInterface>())
    {
        // 2. 调用接口
        // 格式：I接口类名::Execute_函数名(目标对象, 参数...)
        IInteractableInterface::Execute_Interact(TargetActor, this);
    }
}
```
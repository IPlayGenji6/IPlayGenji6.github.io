---
title: 摄像机与视角
---

## 摄像机组件与弹簧臂

- **CameraComponent**：可附加到任意 Actor（如 Pawn），用于第一/第三人称视角。
  
```c++
#include "Camera/CameraComponent.h"
```
```c++
	// .h
	UPROPERTY(VisibleAnywhere)
	UCameraComponent* ViewCamera;

	//.cpp
	// 创建摄像机组件附加到弹簧臂组件下
	ViewCamera = CreateDefaultSubobject<UCameraComponent>(TEXT("ViewCamera"));
	ViewCamera->SetupAttachment(SpringArm);
```
- **SpringArmComponent (弹簧臂)**：自动保持摄像机与目标的距离，避免穿模，可控制旋转和缩放。

```c++
#include "GameFramework/SpringArmComponent.h"
```

```c++
	// .h
	UPROPERTY(VisibleAnywhere)
	USpringArmComponent *SpringArm;

	//.cpp
	// 创建弹簧臂组件附加到根组件下
	SpringArm = CreateDefaultSubobject<USpringArmComponent>(TEXT("SpringArm"));
	SpringArm->SetupAttachment(RootComponent);
	SpringArm->TargetArmLength = 100.f;
```



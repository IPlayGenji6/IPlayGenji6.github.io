---
title: LOG，调试输出
---

## 输出日志，屏幕消息  
```c++
UE_LOG(LogTemp, Warning, TEXT("LOG Test: Begin from C++"));
```
```c++
	if (GEngine)
	{
		GEngine->AddOnScreenDebugMessage(1, 10.f, FColor::Cyan, FString::Printf(TEXT("GroundSpeed: %.2f"), GroundSpeed));
	}
```
## 绘制调试几何  
需要包含头文件 **DrawDebugHelpers.h**
```c++
	// 绘制调试球体
	UWorld* World = GetWorld();
	FVector Center = GetActorLocation();
	if (World)
	{
		DrawDebugSphere(World, Center, 30.f, 24, FColor::Cyan, true, 10.f);
	}
    // 此外还有线，点
```
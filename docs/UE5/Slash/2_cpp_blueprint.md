---
title: C++与蓝图交互
---

## 类成员变量参与反射系统  
[属性文档 ](https://dev.epicgames.com/documentation/zh-cn/unreal-engine/unreal-engine-uproperties)
```c++
    // 编辑器和蓝图均可编辑
    UPROPERTY(EditAnywhere, BlueprintReadWrite, Category="MyCategory")
    MemberVariable;
```

## 类成员函数参与反射系统  
[UFunction文档 ](https://dev.epicgames.com/documentation/zh-cn/unreal-engine/ufunctions-in-unreal-engine)
```c++
	UFUNCTION(BlueprintCallable)
	int32 BlueprintCallableConstFunction() const
```
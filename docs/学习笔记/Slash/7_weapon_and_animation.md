---
title: 武器和动画
---

## 附加武器到角色骨骼

[USceneComponent::AttachToComponent](https://dev.epicgames.com/documentation/en-us/unreal-engine/API/Runtime/Engine/Components/USceneComponent/AttachToComponenthttps://dev.epicgames.com/documentation/en-us/unreal-engine/API/Runtime/Engine/Components/USceneComponent/AttachToComponent) 用于在运行时将一个场景组件（USceneComponent）或 Actor 的根组件附加到另一个场景组件上，支持指定挂点（Socket）和变换规则（FAttachmentTransformRules）。
 
例如角色进入到武器的球形组件触发重叠事件时让角色捡起武器。
```c++
 void AWeapon::OnSphereBeginOverlap(UPrimitiveComponent* OverlappedComponent, AActor* OtherActor, UPrimitiveComponent* OtherComp, int32 OtherBodyIndex, bool bFromSweep, const FHitResult& SweepResult)
{
	Super::OnSphereBeginOverlap(OverlappedComponent, OtherActor, OtherComp, OtherBodyIndex, bFromSweep, SweepResult);

	// 检测是否是角色重叠球体
	ASlashCharacter *SlashCharacter = Cast<ASlashCharacter>(OtherActor);
	if (SlashCharacter)
	{
		// 把武器网格体附加到角色骨骼网格体
		FAttachmentTransformRules rule(EAttachmentRule::SnapToTarget, true);
		ItemMesh->AttachToComponent(SlashCharacter->GetMesh(), rule, TEXT("hand_l_socket"));
	}
}
```
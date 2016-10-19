---
author: Mtoepke
title: "Xbox One의 UWP 앱 및 게임에 대한 시스템 리소스"
description: "Xbox의 UWP 시스템 리소스"
translationtype: Human Translation
ms.sourcegitcommit: 9187e39e1be8b98ad8315487633dfebd068491e6
ms.openlocfilehash: c3ca70936e30ce67b19971e5ccbb01fa89253f35

---

# Xbox One의 UWP 앱 및 게임에 대한 시스템 리소스

Xbox One에서 실행되는 UWP 앱 및 게임은 시스템 및 다른 앱과 리소스를 공유합니다. 따라서 UWP 앱 및 게임은 다음 리소스에 액세스할 수 있습니다.

* 포그라운드에서 실행되는 앱에서 사용할 수 있는 최대 메모리는 1GB입니다.
    * 백그라운드에서 실행되는 앱에서 사용할 수 있는 최대 메모리는 128MB입니다.
    * 이러한 메모리 요구 사항을 초과하는 앱에서는 메모리 할당 오류가 발생합니다. 메모리 사용 모니터링에 대한 자세한 내용은 [MemoryManager 클래스](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.aspx) 참조를 참조하세요.
    
    > [!NOTE]
    > Visual Studio 디버거에서 응용 프로그램 또는 게임을 실행할 때는 이러한 메모리 제약 조건이 적용되지 않습니다. 이 제한은 디버깅 모드에서 실행하지 않는 경우에만 적용됩니다.

* 시스템에서 실행되는 앱과 게임 수에 따라 CPU 코어 2-4개가 공유됩니다.

* 시스템에서 실행되는 앱과 게임 수에 따라 GPU의 45%가 공유됩니다.

* Xbox One의 UWP는 DirectX 11 기능 수준 10을 지원합니다. 지금은 DirectX 12가 지원되지 않습니다. 

**응용 프로그램 개발**의 경우 사용 가능한 리소스가 표준 PC에 비해 제한될 수 있다는 것에 유의해야 합니다.

**게임 개발**의 경우 다른 게임 콘솔과 마찬가지로 Xbox One은 전체 잠재 기능에 액세스하기 위해 특정 하드웨어 기반 개발 키트가 필요한 특수 하드웨어라는 것에 유의해야 합니다. Xbox One 하드웨어의 최대 잠재 기능에 액세스해야 하는 게임을 개발하는 경우 DirectX 12 지원을 포함하는 Xbox One 개발 키트에 액세스하기 위해 [ID@Xbox](http://www.xbox.com/Developers/id) 프로그램에 등록할 수 있습니다.

## 참고 항목
- [Xbox One의 UWP](index.md)



<!--HONumber=Aug16_HO3-->



---
author: QuinnRadich
title: "UWP 버전 선택"
description: "Microsoft Visual Studio에서 UWP 앱을 작성하는 경우 대상 버전을 선택할 수 있습니다. 다른 UWP 버전 간 차이점과 새 프로젝트 및 기존 프로젝트에서 선택을 구성하는 방법을 알아봅니다."
redirect_url: ../updates-and-versions/choose-a-uwp-version/
translationtype: Human Translation
ms.sourcegitcommit: a86002c944841536d37735bb8c4b657905582144
ms.openlocfilehash: d6d2be6c91ddf5fb85cdec759c753db1561f066f

---

# UWP 버전 선택

**이 페이지는 ../updates-and-versions/choose-a-uwp-version/으로 이동되었습니다.**

Microsoft Visual Studio에서 UWP 앱을 작성하는 경우 대상 버전을 선택할 수 있습니다. 다른 UWP 버전 간 차이점과 새 프로젝트 및 기존 프로젝트에서 선택을 구성하는 방법을 알아봅니다.

## 각 UWP 버전의 차이점은 무엇인가요?

UWP의 새 API 및 변경된 API는 Windows 10의 모든 후속 버전에서 사용할 수 있습니다. 버전에 추가된 기능에 대한 특정 정보는 [Windows 10 개발자를 위한 새로운 기능](../whats-new/windows-10-version-1607.md)을 참조하세요.

모든 디바이스 패밀리 및 해당 버전과 모든 API 계약 및 해당 버전을 열거하는 참조 항목은 [디바이스 패밀리](https://msdn.microsoft.com/library/windows/apps/dn706137.aspx) 및 [API 계약](https://msdn.microsoft.com/library/windows/apps/dn706135.aspx)을 참조하세요.

## 앱에 사용할 버전 선택

Visual Studio의 **새 유니버설 Windows 프로젝트** 대화 상자에서 **대상 버전** 및 **최소 버전**용 버전을 선택할 수 있습니다.

* **대상 버전**. 프로젝트 파일에서 *TargetPlatformVersion* 설정을 설정합니다. 또한 앱 패키지 매니페스트에서 *TargetDeviceFamily@MaxVersionTested* 특성의 값을 결정합니다. 선택하는 값은 프로젝트가 대상으로 하는 UWP 플랫폼의 버전을 지정하므로(따라서 앱에 사용할 수 있는 API 집합) 가능한 한 최신 버전을 선택하는 것이 좋습니다. 앱 패키지 매니페스트에 대한 자세한 내용과 TargetDeviceFamily를 수동으로 구성하는 데 대한 몇 가지 지침은 [TargetDeviceFamily](https://msdn.microsoft.com/library/windows/apps/dn986903)를 참조하세요.
* **최소 버전**. 프로젝트 파일에서 *TargetPlatformMinVersion* 설정을 설정합니다. 또한 앱 패키지 매니페스트에서 *TargetDeviceFamily@MinVersion* 특성의 값을 결정합니다. 선택하는 값은 프로젝트가 작동할 수 있는 UWP 플랫폼의 최소 버전을 지정합니다.

앱이 **최소 버전**에서 **대상 버전**까지의 범위에 있는 모든 버전의 Windows에서 작동함을 선언하는 것입니다. 두 가지가 동일한 버전인 경우 특별한 작업을 수행할 필요가 없습니다. 다른 경우에는 다음과 같이 몇 가지 사항을 주의해야 합니다.

* 코드에서 자유롭게,즉 조건부 확인 없이 **최소 버전**에 지정된 버전에 있는 모든 API를 호출할 수 있습니다.
* **대상 버전** 값은 프로젝트를 컴파일하는 데 사용되는 모든 참조(계약 winmds)를 식별하기 위해 사용합니다. 하지만 해당 참조를 사용하면 **최소 버전**을 통해 지원한다고 선언한 디바이스에 반드시 존재하지 않을 수도 있는 API에 대한 호출로 코드를 컴파일할 수 있습니다. 따라서 **최소 버전** 후 도입된 모든 API는 적응 코드를 통해 호출해야 합니다. 적응 코드에 대한 자세한 내용은 [UWP(유니버설 Windows 플랫폼) 앱 지침](universal-application-platform-guide.md)을 참조하세요.


<!--HONumber=Aug16_HO5-->



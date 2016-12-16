---
author: QuinnRadich
title: "UWP 버전 선택"
description: "Microsoft Visual Studio에서 UWP 앱을 작성하는 경우 대상 버전을 선택할 수 있습니다. 다른 UWP 버전 간 차이점과 새 프로젝트 및 기존 프로젝트에서 선택을 구성하는 방법을 알아봅니다."
translationtype: Human Translation
ms.sourcegitcommit: 006b5d01c2474591a81e4d7a83c5735dc0b3d9d8
ms.openlocfilehash: 5d05c427ecc1ec57856b7c3909be50c3d87daa28

---

# <a name="choose-a-uwp-version"></a>UWP 버전 선택

Microsoft Visual Studio에서 UWP 앱을 작성하는 경우 대상 버전을 선택할 수 있습니다. 현재는 세 개의 버전만 사용 가능합니다.

| 버전 | 설명 |
| --- | --- |
| 빌드 14393(Anniversary Edition) | 2016년 7월에 릴리스된 Windows 10의 최신 버전입니다. 이 릴리스의 몇 가지 주요 기능은 다음과 같습니다. </br> \* **Windows Ink:** 새 InkCanvas 및 InkToolbar 컨트롤입니다. </br> \* **Cortana API:** 새 Cortana 작업을 사용하여 Cortana 지원과 앱의 특정 기능을 통합합니다. </br> \* **Windows Hello:** 이제 Microsoft Edge에서 Windows Hello를 지원하여 웹 개발자가 생체 인식 인증에 액세스할 수 있습니다. </br> 이러한 사항과 Windows의 이 릴리스에 추가된 다른 많은 기능에 대한 자세한 내용은 [개발자 센터](https://developer.microsoft.com/en-us/windows/windows-10-for-developers) 또는 [개발자를 위한 Windows 10의 새로운 기능](../whats-new/windows-10-version-1607.md)에 있는 세부 정보 페이지를 방문하세요.  |
| 빌드 10586 | Windows 10의 이 버전은 2015년 11월에 릴리스되었습니다. 주요 기능으로 Microsoft Edge의 비디오 통신을 위한 ORTC(개체 실시간 통신) API와 앱에서 Windows Hello 얼굴 인증을 사용할 수 있도록 해주는 공급자 API가 있습니다. [이 빌드에 도입된 기능에 대한 자세한 내용입니다.](../whats-new/windows-10-version-1511.md) |
| 빌드 10240 | 이 버전은 2015년 7월의 Windows 10 초기 릴리스 버전입니다. [이 빌드에 도입된 기능에 대한 자세한 내용입니다.](../whats-new/windows-10-version-1507.md) |

새 개발자와 모든 연령 시청가용 코드를 작성하는 개발자는 항상 Windows의 최신 빌드(14393)를 사용하는 것이 좋습니다. 엔터프라이즈 앱을 작성하는 개발자는 이전 **최소 버전**을 지원하는 것을 고려해야 합니다.

## <a name="whats-different-in-each-uwp-version"></a>각 UWP 버전의 차이점은 무엇인가요?

UWP의 새 API 및 변경된 API는 Windows 10의 모든 후속 버전에서 사용할 수 있습니다. 버전에 추가된 기능에 대한 특정 정보는 [Windows 10 개발자를 위한 새로운 기능](../whats-new/windows-10-version-1607.md)을 참조하세요.

모든 디바이스 패밀리 및 해당 버전과 모든 API 계약 및 해당 버전을 열거하는 참조 항목은 [디바이스 패밀리](https://msdn.microsoft.com/library/windows/apps/dn706137.aspx) 및 [API 계약](https://msdn.microsoft.com/library/windows/apps/dn706135.aspx)을 참조하세요.

## <a name="choose-which-version-to-use-for-your-app"></a>앱에 사용할 버전 선택

Visual Studio의 **새 유니버설 Windows 프로젝트** 대화 상자에서 **대상 버전** 및 **최소 버전**용 버전을 선택할 수 있습니다.

* **대상 버전**. 프로젝트 파일에서 *TargetPlatformVersion* 설정을 설정합니다. 또한 앱 패키지 매니페스트에서 *TargetDeviceFamily@MaxVersionTested* 특성의 값을 결정합니다. 선택하는 값은 프로젝트가 대상으로 하는 UWP 플랫폼의 버전을 지정하므로(따라서 앱에 사용할 수 있는 API 집합) 가능한 한 최신 버전을 선택하는 것이 좋습니다. 앱 패키지 매니페스트에 대한 자세한 내용과 TargetDeviceFamily를 수동으로 구성하는 데 대한 몇 가지 지침은 [TargetDeviceFamily](https://msdn.microsoft.com/library/windows/apps/dn986903)를 참조하세요.
* **최소 버전**. 프로젝트 파일에서 *TargetPlatformMinVersion* 설정을 설정합니다. 또한 앱 패키지 매니페스트에서 *TargetDeviceFamily@MinVersion* 특성의 값을 결정합니다. 선택하는 값은 프로젝트가 작동할 수 있는 UWP 플랫폼의 최소 버전을 지정합니다.

앱이 **최소 버전**에서 **대상 버전**까지의 범위에 있는 모든 버전의 Windows에서 작동함을 선언하는 것입니다. 두 가지가 동일한 버전인 경우 특별한 작업을 수행할 필요가 없습니다. 다른 경우에는 다음과 같이 몇 가지 사항을 주의해야 합니다.

* 코드에서 자유롭게,즉 조건부 확인 없이 **최소 버전**에 지정된 버전에 있는 모든 API를 호출할 수 있습니다.
* **대상 버전**에만 있는 API 없이도 코드가 작동하는지 확인하기 위해 **최소 버전**을 실행하는 디바이스에서 코드를 테스트해야 합니다.
* **대상 버전** 값을 사용하여 프로젝트를 컴파일하는 데 사용되는 모든 참조(계약 winmds)를 식별합니다. 하지만 해당 참조를 사용하면 **최소 버전**을 통해 지원한다고 선언한 디바이스에 반드시 존재하지 않을 수도 있는 API에 대한 호출로 코드를 컴파일할 수 있습니다. 따라서 **최소 버전** 후 도입된 모든 API는 적응 코드를 통해 호출해야 합니다. 적응 코드에 대한 자세한 내용은 [UWP(유니버설 Windows 플랫폼) 앱 지침](../get-started/universal-application-platform-guide.md)을 참조하세요.



<!--HONumber=Dec16_HO1-->



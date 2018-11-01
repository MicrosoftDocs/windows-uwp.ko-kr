---
author: stevewhims
description: 디바이스 및 센서와 통합되는 코드는 사용자의 입력과 사용자에 대한 출력을 포함합니다.
title: I/O, 디바이스 및 앱 모델에 대해 Windows 런타임 8.x를 UWP로 포팅
ms.assetid: bb13fb8f-bdec-46f5-8640-57fb0dd2d85b
ms.author: stwhi
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8e15014e39ed6d980cbe80daa0a129ff83a021b9
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5870815"
---
# <a name="porting-windows-runtime-8x-to-uwp-for-io-device-and-app-model"></a>I/O, 디바이스 및 앱 모델에 대해 Windows 런타임 8.x를 UWP로 포팅




이전 항목에서는 [XAML 및 UI 포팅](w8x-to-uwp-porting-xaml-and-ui.md)에 대해 살펴봤습니다.

디바이스 및 센서와 통합되는 코드는 사용자의 입력과 사용자에 대한 출력을 포함합니다. 데이터 처리를 포함할 수도 있습니다. 하지만 이 코드는 UI 계층 *또는* 데이터 계층으로 간주되지 않습니다. 이 코드는 진동 컨트롤러, 가속도계, 자이로스코프, 마이크와 스피커(음성 인식 및 음성 합성과 교차), 지리적 위치, 입력 형식(예제: 터치, 마우스, 키보드, 펜) 등과의 통합을 포함합니다.

## <a name="application-lifecycle-process-lifetime-management"></a>응용 프로그램 수명 주기(프로세스 수명 관리)


유니버설 8.1 앱의 경우 비활성화되는 앱과 일시 중단 이벤트를 발생시키는 시스템 간에 2초의 "디바운스 기간"이 있습니다. 일시 중단 상태에 대한 추가 시간으로 이 디바운스 기간을 사용하는 것은 안전하지 않으며 UWP(유니버설 Windows 플랫폼) 앱의 경우 디바운스 기간이 없습니다. 즉, 앱이 비활성화되는 즉시 일시 중단 이벤트가 발생합니다.

자세한 내용은 [앱 수명 주기](https://msdn.microsoft.com/library/windows/apps/mt243287)를 참조하세요.

## <a name="background-audio"></a>백그라운드 오디오


[**MediaElement.AudioCategory**](https://msdn.microsoft.com/library/windows/apps/br227352) 속성에 대 한 **ForegroundOnlyMedia** 및 **에서** 는 Windows10 앱에 대 한 사용 되지 않습니다. Windows Phone 스토어 앱 모델을 대신 사용합니다. 자세한 내용은 [배경 오디오](https://msdn.microsoft.com/library/windows/apps/mt282140)를 참조하세요.

## <a name="detecting-the-platform-your-app-is-running-on"></a>앱이 실행되고 있는 플랫폼 검색


Windows10 사용 하 여 앱을 대상으로 변경에 대 한 생각 하는 방법은 합니다. 새로운 개념적 모델에서는 앱이 UWP(유니버설 Windows 플랫폼)를 대상으로 하고 모든 Windows 디바이스에서 실행됩니다. 그런 다음 특정 디바이스 패밀리에 독점적으로 사용되는 기능을 돋보이도록 선택할 수 있습니다. 또한 필요한 경우 앱에는 특별히 하나 이상의 디바이스 패밀리를 대상으로 하도록 자체적으로 제한하는 옵션도 있습니다. 디바이스 패밀리와 대상으로 할 디바이스 패밀리를 결정하는 방법에 대한 자세한 내용은 [UWP 앱 지침](https://msdn.microsoft.com/library/windows/apps/dn894631)을 참조하세요.

실행되고 있는 운영 체제를 검색하는 유니버설 8.1 앱의 코드가 있다면 논리에 대한 원인에 따라 코드를 변경해야 할 수 있습니다. 앱이 값에 대해 작업하지 않고 통과시킨다면 계속해서 운영 체제 정보를 수집할 수 있습니다.

**참고**  를 사용 하면 운영 체제 또는 디바이스 패밀리 기능이 있는지 검색 하는 것이 좋습니다. 일반적으로 현재 운영 체제 또는 디바이스 패밀리를 식별하는 방법이 특정 운영 체제 또는 디바이스 패밀리 기능이 있는지 확인하는 가장 좋은 방법은 아닙니다. 운영 체제 또는 디바이스 패밀리(및 버전 번호)를 검색하는 대신 기능 자체의 존재에 대해 테스트합니다([조건부 컴파일 및 적응 코드](w8x-to-uwp-porting-to-a-uwp-project.md) 참조). 특정 운영 체제 또는 디바이스 패밀리가 필요한 경우 해당 버전에 대한 테스트를 디자인하지 않고 해당 버전을 지원되는 최소 버전으로 사용해야 합니다.

 

앱의 UI를 서로 다른 디바이스에 맞게 구성하는 데 권장되는 여러 기술이 있습니다. 기존과 마찬가지로 자동 크기 조정 요소 및 동적 레이아웃 패널을 계속 사용할 수 있습니다. XAML 태그에서 유효 픽셀(이전의 보기 픽셀)로 크기를 사용하므로 다른 해상도 및 배율에 맞게 UI를 조정할 수 있습니다(참조 [유효 픽셀, 가시거리 및 배율 인수](w8x-to-uwp-porting-xaml-and-ui.md) 참조). Visual State Manager의 적응 트리거 및 setter를 사용하여 창 크기에 맞게 UI를 조정할 수 있습니다([UWP 앱 지침](https://msdn.microsoft.com/library/windows/apps/dn894631) 참조).

그러나 디바이스 패밀리 검색이 반드시 필요한 경우에는 검색을 수행할 수 있습니다. 이 예제에서는 [**AnalyticsVersionInfo**](https://msdn.microsoft.com/library/windows/apps/dn960165) 클래스를 사용하여 적절한 경우 디바이스 패밀리에 맞게 조정된 페이지를 탐색하고 그렇지 않은 경우 다시 기본 페이지를 사용할 수 있는지 확인합니다.

```csharp
   if (Windows.System.Profile.AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Mobile")
        rootFrame.Navigate(typeof(MainPageMobile), e.Arguments);
    else
        rootFrame.Navigate(typeof(MainPage), e.Arguments);
```

또한 앱이 적용되는 리소스 선택 요소에서 실행되고 있는 디바이스 패밀리를 결정할 수도 있습니다. 아래 예제는 명령을 통해 이를 수행하는 방법을 보여 주며, [**ResourceContext.QualifierValues**](https://msdn.microsoft.com/library/windows/apps/br206071) 항목에서는 디바이스 패밀리 요소에 따라 디바이스 패밀리별 리소스를 로드하는 데 있어 클래스의 일반적인 사용 사례에 관해 설명합니다.

```csharp
var qualifiers = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues;
string deviceFamilyName;
bool isDeviceFamilyNameKnown = qualifiers.TryGetValue("DeviceFamily", out deviceFamilyName);
```

또한 [조건부 컴파일 및 적응 코드](w8x-to-uwp-porting-to-a-uwp-project.md)를 참조하세요.

## <a name="location"></a>위치


해당 앱 패키지 매니페스트에서 위치 기능을 선언 하는 앱 Windows10에서 실행 되 면 최종 사용자의 동의에 메시지가 나타납니다. 이 앱은 Windows Phone 스토어 앱 또는 Windows10 앱 인지 true입니다. 따라서 앱에서 고유한 사용자 지정 동의 확인 프롬프트가 표시되거나 켜기-끄기 토글이 제공되면 최종 사용자에게 한 번만 묻도록 제거할 수 있습니다.

 

 





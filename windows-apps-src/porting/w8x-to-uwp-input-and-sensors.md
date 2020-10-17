---
description: I/o, 장치 및 앱 모델을 위해 UWP에서 Windows 런타임 .x로 이식 하는 방법에 대해 알아봅니다.
title: I/O, 디바이스 및 앱 모델을 위해 Windows 런타임 8.x를 UWP로 포팅
ms.assetid: bb13fb8f-bdec-46f5-8640-57fb0dd2d85b
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: fe0d78f40fc7e4ca28e5ef766ff713b3aaf9f189
ms.sourcegitcommit: 0c4bbaf1c119a84002748cdcf02e1449835559c3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/16/2020
ms.locfileid: "92133076"
---
# <a name="porting-windows-runtime-8x-to-uwp-for-io-device-and-app-model"></a>I/o, 장치 및 앱 모델을 위해 UWP에 Windows 런타임 .x 포팅




이전 항목에서는 [XAML 및 UI를 이식](w8x-to-uwp-porting-xaml-and-ui.md)했습니다.

장치 자체 및 센서와 통합 되는 코드는 사용자의 입력 및 출력을 포함 합니다. 데이터 처리도 포함 될 수 있습니다. 그러나이 코드는 일반적으로 UI 계층 *또는* 데이터 계층으로 간주 되지 않습니다. 이 코드에는 진동 컨트롤러,가 속도계, 자이로스코프가, 마이크 및 스피커 (음성 인식 및 합성과 교차), (지역) 위치, 터치, 마우스, 키보드, 펜 등의 입력 소프트웨어나와의 통합이 포함 됩니다.

## <a name="application-lifecycle-process-lifetime-management"></a>응용 프로그램 수명 주기 (프로세스 수명 관리)


유니버설 8.1 앱의 경우 앱이 비활성 상태가 되는 시간과 일시 중단 이벤트를 발생 시키는 시스템 사이에 2 초 "debounce 창"이 있습니다. 일시 중단 상태에 대 한 추가 시간으로이 debounce 창을 사용 하는 것은 안전 하지 않으며, UWP (유니버설 Windows 플랫폼) 앱의 경우에는 debounce 창이 없습니다. 일시 중단 이벤트는 앱이 비활성화 되는 즉시 발생 합니다.

자세한 내용은 [앱 수명 주기](../launch-resume/app-lifecycle.md)를 참조 하세요.

## <a name="background-audio"></a>배경 오디오


[**AudioCategory**](/uwp/api/windows.ui.xaml.controls.mediaelement.audiocategory) 속성의 경우 **ForegroundOnlyMedia** 및 **BackgroundCapableMedia** 는 Windows 10 앱에서 사용 되지 않습니다. 대신 Windows Phone 스토어 앱 모델을 사용 합니다. 자세한 내용은 [배경 오디오](../audio-video-camera/background-audio.md)를 참조 하세요.

## <a name="detecting-the-platform-your-app-is-running-on"></a>앱이 실행 되 고 있는 플랫폼 검색


Windows 10의 앱 대상 변경에 대해 생각 하는 방법입니다. 새 개념적 모델은 앱이 UWP (유니버설 Windows 플랫폼)를 대상으로 하 고 모든 Windows 장치에서 실행 되는 것입니다. 그런 다음 특정 장치 제품군에만 적용 되는 기능을 선택할 수 있습니다. 필요한 경우 앱은 하나 이상의 장치 패밀리를 대상으로 하는 것을 제한 하는 옵션도 있습니다. 장치 제품군에 대 한 자세한 내용 및 대상으로 할 장치 패밀리를 결정 하는 방법에 대 한 자세한 내용은 [UWP 앱에 대 한 가이드를](../get-started/universal-application-platform-guide.md)참조 하세요.

유니버설 8.1 앱에 실행 중인 운영 체제를 검색 하는 코드가 있는 경우 논리의 이유에 따라이를 변경 해야 할 수 있습니다. 앱에서 값을 전달 하 고 작동 하지 않는 경우 운영 체제 정보를 계속 수집할 수 있습니다.

**참고**    운영 체제 또는 장치 패밀리를 사용 하 여 기능이 있는지 검색 하지 않는 것이 좋습니다. 현재 운영 체제 또는 장치 제품군을 식별 하는 것은 일반적으로 특정 운영 체제 또는 장치 패밀리 기능이 있는지 여부를 확인 하는 가장 좋은 방법이 아닙니다. 운영 체제 또는 장치 패밀리 (및 버전 번호)를 검색 하는 대신 기능 자체의 존재 여부를 테스트 합니다 ( [조건부 컴파일 및 적응 코드](w8x-to-uwp-porting-to-a-uwp-project.md)참조). 특정 운영 체제 또는 장치 패밀리가 필요한 경우 해당 버전에 대 한 테스트를 설계 하는 대신 지원 되는 최소 버전으로 사용 해야 합니다.

 

앱의 UI를 다른 장치에 맞게 조정 하기 위해 권장 되는 몇 가지 기술이 있습니다. 자동 크기 조정 요소와 동적 레이아웃 패널은 항상 그대로 사용 합니다. XAML 태그에서 UI가 다른 해상도 및 배율 요소에 맞게 조정 되도록 (이전의 픽셀) 크기를 계속 사용 합니다 ( [효과적인 픽셀, 거리 보기 및 배율 인수](w8x-to-uwp-porting-xaml-and-ui.md)참조). 그리고 시각적 상태 관리자의 적응 트리거와 setter를 사용 하 여 UI를 창 크기로 조정 합니다 ( [UWP 앱에 대 한 가이드](../get-started/universal-application-platform-guide.md)참조).

그러나 장치 패밀리를 피할 수 없는 시나리오가 있는 경우이 작업을 수행할 수 있습니다. 이 예제에서는 [**AnalyticsVersionInfo**](/uwp/api/Windows.System.Profile.AnalyticsVersionInfo) 클래스를 사용 하 여 해당 하는 경우 모바일 장치 제품군에 맞게 조정 된 페이지로 이동 하 고, 그렇지 않은 경우 기본 페이지로 대체 해야 합니다.

```csharp
   if (Windows.System.Profile.AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Mobile")
        rootFrame.Navigate(typeof(MainPageMobile), e.Arguments);
    else
        rootFrame.Navigate(typeof(MainPage), e.Arguments);
```

앱은 적용 되는 리소스 선택 요소에서 실행 중인 장치 제품군을 확인할 수도 있습니다. 아래 예제에서는이 작업을 수행 하는 방법을 보여 줍니다. [**QualifierValues**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.qualifiervalues) 항목에서는 장치 패밀리 요소를 기준으로 장치 패밀리 관련 리소스를 로드 하는 클래스에 대 한 일반적인 사용 사례에 대해 설명 합니다.

```csharp
var qualifiers = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues;
string deviceFamilyName;
bool isDeviceFamilyNameKnown = qualifiers.TryGetValue("DeviceFamily", out deviceFamilyName);
```

또한 [조건부 컴파일 및 적응 코드](w8x-to-uwp-porting-to-a-uwp-project.md)를 참조 하세요.

## <a name="location"></a>위치


앱 패키지 매니페스트의 위치 기능을 선언 하는 앱이 Windows 10에서 실행 되는 경우 시스템은 최종 사용자에 게 동의 여부를 묻는 메시지를 표시 합니다. 앱이 Windows Phone 스토어 앱 인지 아니면 Windows 10 앱 인지에 해당 합니다. 따라서 앱에서 고유한 사용자 지정 동의 확인 프롬프트를 표시 하거나, 설정/해제 기능을 제공 하는 경우 최종 사용자에 게 한 번만 표시 되도록 해당 정보를 제거 하는 것이 좋습니다.

 

 
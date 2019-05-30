---
ms.assetid: b7333924-d641-4ba5-92a2-65925b44ccaa
description: 이 문서에서는 앱이 백그라운드에서 실행되는 동안 미디어를 재생하는 방법을 보여 줍니다.
title: 백그라운드에서 미디어 재생
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 19b3aa80bee643087a0aa92f714349004f6ec1c1
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66359157"
---
# <a name="play-media-in-the-background"></a>백그라운드에서 미디어 재생
이 문서에서는 앱이 포그라운드에서 백그라운드로 이동될 때 미디어가 계속 재생되도록 앱을 구성하는 방법을 보여 줍니다. 즉, 사용자가 홈 화면에서 반환된 앱을 최소화했거나 다른 방법으로 앱에서 외부로 이동한 후에도 앱은 오디오를 계속 재생할 수 있습니다. 

백그라운드 오디오 재생 시나리오는 다음과 같습니다.

-   **장기 실행 재생:** 사용자 선택 하 고 시작 재생 목록에는 사용자가 백그라운드에서 재생을 계속 하 여 재생을 기대 하는 포그라운드 앱을 간단 하 게 나타납니다.

-   **작업 전환기를 사용합니다.** 간단 하 게 사용자 오디오 재생을 시작 하는 포그라운드 응용 프로그램을 표시 하 작업 전환기를 사용 하 여 다른 앱 열기 전환 됩니다. 사용자는 예상대로 백그라운드에서 오디오를 계속 재생할 수 있습니다.

이 문서에 설명된 배경 오디오 구현을 사용하면 모바일, 데스크톱, Xbox 등의 모든 Windows 디바이스에서 앱을 실행할 수 있습니다.

> [!NOTE]
> 이 문서의 코드는 UWP [배경 오디오 샘플](https://go.microsoft.com/fwlink/p/?LinkId=800141)에서 조정되었습니다.

## <a name="explanation-of-one-process-model"></a>단일 프로세스 모델에 대한 설명
Windows 10 버전 1607에서는 배경 오디오를 사용하도록 설정하는 프로세스를 훨씬 간소화하는 새로운 단일 프로세스 모델이 도입되었습니다. 이전에는 앱이 포그라운드 앱뿐 아니라 백그라운드 프로세스도 관리한 다음 두 프로세스 간에 상태 변경을 수동으로 통신해야 했습니다. 새 모델에서는 앱 매니페스트에 배경 오디오 접근 권한 값을 추가하기만 하면 앱이 백그라운드로 이동될 때 자동으로 오디오 재생을 계속합니다. 새로운 두 가지 응용 프로그램 수명 주기 이벤트인 [**EnteredBackground**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.enteredbackground) 및 [**LeavingBackground**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.leavingbackground)를 사용하면 앱이 백그라운드로 이동하고 나가는 시기를 알 수 있습니다. 앱이 백그라운드로 이동하거나 나갈 때 시스템에서 적용하는 메모리 제약 조건이 변경될 수 있으므로 이러한 이벤트를 사용하여 현재 메모리 사용을 확인하고 제한 아래로 유지하기 위해 리소스를 해제할 수 있습니다.

새 모델을 사용하면 복잡한 프로세스 간 통신 및 상태 관리가 제거되므로 코드 작업이 훨씬 감소하며 배경 오디오를 보다 신속하게 구현할 수 있습니다. 그러나 이전 버전과의 호환성을 위해 현재 릴리스에서는 두 프로세스 모델도 계속 지원됩니다. 자세한 내용은 [레거시 배경 오디오 모델](legacy-background-media-playback.md)을 참조하세요.

## <a name="requirements-for-background-audio"></a>배경 오디오에 대한 요구 사항
앱이 백그라운드에 있는 동안 오디오를 재생하려면 다음 요구 사항을 충족해야 합니다.

* 이 문서의 뒷부분에 설명된 대로 앱 매니페스트에 **백그라운드 미디어 재생** 접근 권한 값을 추가합니다.
* 앱이 [**CommandManager.IsEnabled**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackcommandmanager.isenabled) 속성을 false로 설정하는 등 **MediaPlayer**와 SMTC(시스템 미디어 전송 컨트롤)의 자동 통합을 사용하지 않도록 설정하는 경우 백그라운드 미디어 재생을 사용하려면 SMTC와 수동 통합을 구현해야 합니다. **MediaPlayer** 이외의 API(예: [**AudioGraph**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioGraph))를 사용하여 오디오를 재생하는 경우에도 앱이 백그라운드로 이동할 때 오디오 재생을 계속하려면 수동으로 SMTC와 통합해야 합니다. 최소 SMTC 통합 요구 사항은 [시스템 미디어 전송 컨트롤의 수동 컨트롤](system-media-transport-controls.md)에서 "배경 오디오에 대한 시스템 미디어 전송 컨트롤 사용" 섹션에 설명되어 있습니다.
* 앱이 백그라운드에 있는 동안 시스템에서 백그라운드 앱에 대해 설정한 메모리 사용량 제한 아래로 유지해야 합니다. 백그라운드에 있는 동안 메모리를 관리하기 위한 지침은 이 문서의 뒷부분에 제공되어 있습니다.

## <a name="background-media-playback-manifest-capability"></a>백그라운드 미디어 재생 매니페스트 접근 권한 값
배경 오디오를 사용하려면 Package.appxmanifest 앱 매니페스트 파일에 백그라운드 미디어 재생 접근 권한 값을 추가해야 합니다. 

**매니페스트 디자이너를 사용 하 여 앱 매니페스트에 기능을 추가 하려면**

1.  Microsoft Visual Studio의 **솔루션 탐색기**에서 **package.appxmanifest** 항목을 두 번 클릭하여 응용 프로그램 매니페스트 디자이너를 엽니다.
2.  **접근 권한 값** 탭을 선택합니다.
3.  **백그라운드 미디어 재생** 확인란을 선택합니다.

앱 매니페스트 xml을 수동으로 편집하여 접근 권한 값을 설정하려면 먼저 *uap3* 네임스페이스 접두사가 **Package** 요소에 정의되어 있는지 확인합니다. 정의되지 않은 경우 아래와 같이 추가합니다.
```xml
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap uap3 mp">
```

**Capabilities** 요소에 *backgroundMediaPlayback* 접근 권한 값을 추가합니다.
```xml
<Capabilities>
    <uap3:Capability Name="backgroundMediaPlayback"/>
</Capabilities>
```

## <a name="handle-transitioning-between-foreground-and-background"></a>포그라운드와 백그라운드 간 전환 처리
앱이 포그라운드에서 백그라운드로 이동하면 [**EnteredBackground**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.enteredbackground) 이벤트가 발생합니다. 앱이 포그라운드로 반환되면 [**LeavingBackground**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.leavingbackground) 이벤트가 발생합니다. 이러한 이벤트는 앱 수명 주기 이벤트이기 때문에 앱을 만들 때 해당 이벤트 처리기를 등록해야 합니다. 기본 프로젝트 템플릿에서 이는 App.xaml.cs의 **App** 클래스 생성자에 추가하는 것을 의미합니다. 

[!code-cs[RegisterEvents](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetRegisterEvents)]

현재 백그라운드에서 실행 중인지 여부를 추적하기 위한 변수를 만듭니다.

[!code-cs[DeclareBackgroundMode](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetDeclareBackgroundMode)]

[  **EnteredBackground**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.enteredbackground) 이벤트가 발생하면 추적 변수를 설정하여 현재 백그라운드에서 실행 중임을 나타냅니다. 백그라운드로의 전환 속도가 사용자에게 느려지는 것처럼 보일 수 있으므로 **EnteredBackground** 이벤트에서 장기 실행 작업을 수행하면 안 됩니다.

[!code-cs[EnteredBackground](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetEnteredBackground)]

[  **LeavingBackground**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.leavingbackground) 이벤트 처리기에서 추적 변수를 설정하여 앱이 더 이상 백그라운드에서 실행되지 않음을 나타내야 합니다.

[!code-cs[LeavingBackground](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetLeavingBackground)]

### <a name="memory-management-requirements"></a>메모리 관리 요구 사항
포그라운드와 백그라운드 간의 전환 처리 작업 중 가장 중요한 부분은 앱에서 사용하는 메모리를 관리하는 것입니다. 백그라운드에서 실행할 경우 시스템에서 앱에 허용하는 메모리 리소스가 줄어들기 때문에 [**AppMemoryUsageIncreased**](https://docs.microsoft.com/uwp/api/windows.system.memorymanager.appmemoryusageincreased) 및 [**AppMemoryUsageLimitChanging**](https://docs.microsoft.com/uwp/api/windows.system.memorymanager.appmemoryusagelimitchanging) 이벤트도 등록해야 합니다. 이러한 이벤트는 발생하는 경우 앱의 현재 메모리 사용량과 현재 제한을 확인한 다음 필요한 경우 메모리 사용을 줄이세요. 백그라운드에서 실행하는 동안 메모리 사용량을 감소하는 방법에 대한 자세한 내용은 [앱이 백그라운드로 이동할 때 메모리 회수](../launch-resume/reduce-memory-usage.md)를 참조하세요.

## <a name="network-availability-for-background-media-apps"></a>백그라운드 미디어 앱에 대한 네트워크 가용성
스트림이나 파일에서 생성되지 않은 모든 네트워크 인식 미디어 원본은 원격 콘텐츠를 검색하는 동안 네트워크 연결을 활성 상태로 유지하고 검색하지 않을 때 해제합니다. [**MediaStreamSource**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaStreamSource), 특히 응용 프로그램을 사용 하 여 플랫폼에 올바른 버퍼링 된 범위를 올바르게 보고할 의존 [ **SetBufferedRange**](https://docs.microsoft.com/uwp/api/windows.media.core.mediastreamsource.setbufferedrange)합니다. 전체 콘텐츠가 완전히 버퍼링된 후에는 더 이상 앱을 위해 네트워크가 예약되지 않습니다.

미디어에서 다운로드하지 않을 때 백그라운드에서 발생하는 네트워크 호출을 만들려면 [**MaintenanceTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.MaintenanceTrigger) 또는 [**TimeTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.TimeTrigger)와 같은 적절한 작업에 포함되어야 합니다. 자세한 내용은 [백그라운드 작업을 사용하여 앱 지원](https://docs.microsoft.com/windows/uwp/launch-resume/support-your-app-with-background-tasks)을 참조하세요.

## <a name="related-topics"></a>관련 항목
* [미디어 재생](media-playback.md)
* [오디오 및 MediaPlayer를 사용 하 여 비디오를 재생 합니다.](play-audio-and-video-with-mediaplayer.md)
* [전송 컨트롤 시스템 미디어를 사용 하 여 통합](integrate-with-systemmediatransportcontrols.md)
* [백그라운드 오디오 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundMediaPlayback)

 

 





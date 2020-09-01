---
ms.assetid: b7333924-d641-4ba5-92a2-65925b44ccaa
description: 이 문서에서는 앱이 백그라운드에서 실행 되는 동안 미디어를 재생 하는 방법을 보여 줍니다.
title: 백그라운드에서 미디어 재생
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 72687db5bed8303b672ed8ed009108708cb126be
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161187"
---
# <a name="play-media-in-the-background"></a>백그라운드에서 미디어 재생
이 문서에서는 앱이 포그라운드에서 백그라운드에서 이동할 때 미디어가 계속 재생 되도록 앱을 구성 하는 방법을 보여 줍니다. 즉, 사용자가 앱을 최소화 하거나, 홈 화면으로 반환 하거나, 다른 방식으로 앱에서 벗어난 경우에도 앱에서 오디오를 계속 재생할 수 있습니다. 

배경 오디오 재생 시나리오는 다음과 같습니다.

-   **장기 실행 재생 목록:** 사용자는 전경 앱을 잠깐 눌러 재생 목록을 선택 하 고 시작 합니다. 그러면 사용자가 재생 목록이 백그라운드에서 계속 재생 될 것으로 예상 합니다.

-   **작업 전환기 사용:** 사용자는 전경 앱을 잠깐 열어 오디오 재생을 시작한 다음, 작업 전환기를 사용 하 여 열려 있는 다른 앱으로 전환 합니다. 사용자는 오디오를 백그라운드에서 계속 재생할 것으로 예상 합니다.

이 문서에서 설명 하는 배경 오디오 구현을 통해 모바일, 데스크톱 및 Xbox를 비롯 한 모든 Windows 장치에서 앱을 실행할 수 있습니다.

> [!NOTE]
> 이 문서의 코드는 UWP [배경 오디오 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundMediaPlayback)에서 도입 되었습니다.

## <a name="explanation-of-one-process-model"></a>단일 프로세스 모델에 대 한 설명
Windows 10 버전 1607에서는 백그라운드 오디오를 사용 하도록 설정 하는 프로세스를 크게 간소화 하는 새로운 단일 프로세스 모델이 도입 되었습니다. 이전에는 앱이 포그라운드 앱 외에 백그라운드 프로세스를 관리 하 고 두 프로세스 간에 상태 변경을 수동으로 전달 해야 했습니다. 새 모델에서는 백그라운드 오디오 기능을 앱 매니페스트에 추가 하기만 하면 앱이 백그라운드로 이동할 때 자동으로 오디오를 재생 합니다. 두 개의 새로운 응용 프로그램 수명 주기 [**이벤트 인 응용 프로그램에서 응용**](/uwp/api/windows.applicationmodel.core.coreapplication.enteredbackground) 프로그램을 시작 하 고 배경을 벗어날 때 앱에 [**알릴 수 있습니다**](/uwp/api/windows.applicationmodel.core.coreapplication.leavingbackground) . 앱이 백그라운드에서 또는 백그라운드에서 전환로 이동 하는 경우 시스템에 의해 적용 되는 메모리 제약 조건이 변경 될 수 있으므로 이러한 이벤트를 사용 하 여 현재 메모리 사용을 확인 하 고 리소스를 확보 하 여 한도 미만으로 유지할 수 있습니다.

새 모델을 사용 하면 복잡 한 프로세스 간 통신 및 상태 관리를 제거 하 여 코드를 크게 줄여 백그라운드 오디오를 훨씬 더 신속 하 게 구현할 수 있습니다. 그러나 두 프로세스 모델은 이전 버전과의 호환성을 위해 현재 릴리스에서 여전히 지원 됩니다. 자세한 내용은 [레거시 배경 오디오 모델](legacy-background-media-playback.md)을 참조 하세요.

## <a name="requirements-for-background-audio"></a>배경 오디오 요구 사항
앱이 백그라운드에 있는 동안에는 앱이 오디오 재생에 대 한 다음 요구 사항을 충족 해야 합니다.

* 이 문서의 뒷부분에 설명 된 대로 **백그라운드 미디어 재생** 기능을 앱 매니페스트에 추가 합니다.
* 앱이 [**IsEnabled**](/uwp/api/windows.media.playback.mediaplaybackcommandmanager.isenabled) 속성을 false로 설정 하 여와 같이 Smtc (시스템 미디어 전송 제어)와 함께 **MediaPlayer** 의 자동 통합을 사용 하지 않도록 설정한 경우에는 백그라운드 미디어 재생을 사용 하기 위해 smtc와 수동 통합을 구현 해야 합니다. 앱이 배경으로 이동할 때 오디오를 계속 재생 하려는 경우 오디오를 재생 하려면 **MediaPlayer**  [**Graph**](/uwp/api/Windows.Media.Audio.AudioGraph)와 같이 MediaPlayer 이외의 API를 사용 하는 경우에는 smtc와도 수동으로 통합 해야 합니다. 최소 SMTC 통합 요구 사항은 [시스템 미디어 전송 컨트롤의 수동 제어](system-media-transport-controls.md)의 "배경 오디오에 시스템 미디어 전송 컨트롤 사용" 섹션에 설명 되어 있습니다.
* 앱이 백그라운드에 있는 동안 백그라운드 앱에 대해 시스템에서 설정한 메모리 사용 제한을 유지 해야 합니다. 백그라운드에서 메모리 관리에 대 한 지침은이 문서의 뒷부분에서 제공 됩니다.

## <a name="background-media-playback-manifest-capability"></a>백그라운드 미디어 재생 매니페스트 기능
배경 오디오를 사용 하도록 설정 하려면 응용 프로그램 매니페스트 파일 appxmanifest.xml에 백그라운드 미디어 재생 기능을 추가 해야 합니다. 

**매니페스트 디자이너를 사용 하 여 앱 매니페스트에 기능을 추가 하려면**

1.  Microsoft Visual Studio의 **솔루션 탐색기**에서 **package.appxmanifest** 항목을 두 번 클릭하여 응용 프로그램 매니페스트 디자이너를 엽니다.
2.  **기능** 탭을 선택합니다.
3.  **백그라운드 미디어 재생** 확인란을 선택 합니다.

응용 프로그램 매니페스트 xml을 수동으로 편집 하 여 기능을 설정 하려면 먼저 **Package** 요소에 *uap3* 네임 스페이스 접두사가 정의 되어 있는지 확인 합니다. 그렇지 않은 경우 아래와 같이 추가 합니다.
```xml
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap uap3 mp">
```

다음으로,  *backgroundMediaPlayback* 기능을 **Capabilities** 요소에 추가 합니다.
```xml
<Capabilities>
    <uap3:Capability Name="backgroundMediaPlayback"/>
</Capabilities>
```

## <a name="handle-transitioning-between-foreground-and-background"></a>포그라운드 및 백그라운드 간 전환 처리
앱이 포그라운드에서 배경으로 이동 하면, 사용자의 [**배경**](/uwp/api/windows.applicationmodel.core.coreapplication.enteredbackground) 이벤트가 발생 합니다. 앱이 포그라운드로 반환 될 때 [**Leavingbackground**](/uwp/api/windows.applicationmodel.core.coreapplication.leavingbackground) 이벤트가 발생 합니다. 앱 수명 주기 이벤트 이므로 앱을 만들 때 이러한 이벤트에 대 한 처리기를 등록 해야 합니다. 기본 프로젝트 템플릿에서이는 App.xaml.cs의 **App** class 생성자에 추가 하는 것을 의미 합니다. 

[!code-cs[RegisterEvents](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetRegisterEvents)]

현재 백그라운드에서 실행 중인지 여부를 추적 하는 변수를 만듭니다.

[!code-cs[DeclareBackgroundMode](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetDeclareBackgroundMode)]

지정 된 [**background**](/uwp/api/windows.applicationmodel.core.coreapplication.enteredbackground) 이벤트를 발생 시키면 현재 백그라운드에서 실행 중임을 나타내기 위해 추적 변수를 설정 합니다. 이로 인해 백그라운드로 전환 하면 사용자에 게 느리게 표시 될 수 있으므로이 작업을 수행 하면 오래 실행 **되는 작업** 을 수행 하지 않아도 됩니다.

[!code-cs[EnteredBackground](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetEnteredBackground)]

[**Leavingbackground**](/uwp/api/windows.applicationmodel.core.coreapplication.leavingbackground) 이벤트 처리기에서 앱이 백그라운드에서 더 이상 실행 되지 않음을 나타내도록 추적 변수를 설정 해야 합니다.

[!code-cs[LeavingBackground](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetLeavingBackground)]

### <a name="memory-management-requirements"></a>메모리 관리 요구 사항
포그라운드 및 백그라운드 간의 전환을 처리 하는 가장 중요 한 부분은 앱에서 사용 하는 메모리를 관리 하는 것입니다. 백그라운드에서 실행 하면 응용 프로그램이 시스템에서 유지할 수 있도록 허용 하는 메모리 리소스를 줄일 수 있기 때문에 [**AppMemoryUsageIncreased**](/uwp/api/windows.system.memorymanager.appmemoryusageincreased) 및 [**AppMemoryUsageLimitChanging**](/uwp/api/windows.system.memorymanager.appmemoryusagelimitchanging) 이벤트도 등록 해야 합니다. 이러한 이벤트가 발생 하면 응용 프로그램의 현재 메모리 사용량과 현재 제한을 확인 하 고 필요한 경우 메모리 사용량을 줄여야 합니다. 백그라운드에서 실행 하는 동안 메모리 사용량을 줄이는 방법에 대 한 자세한 내용은 [앱이 백그라운드로 이동할 때 사용 가능한 메모리](../launch-resume/reduce-memory-usage.md)를 참조 하세요.

## <a name="network-availability-for-background-media-apps"></a>백그라운드 미디어 앱에 대 한 네트워크 가용성
스트림 또는 파일에서 생성 되지 않은 모든 네트워크 인식 미디어 소스는 원격 콘텐츠를 검색 하는 동안 네트워크 연결을 활성 상태로 유지 하 고, 그렇지 않은 경우 해제 합니다. 특히 [**Mediastreamsource**](/uwp/api/Windows.Media.Core.MediaStreamSource)는 응용 프로그램을 사용 하 여 [**SetBufferedRange**](/uwp/api/windows.media.core.mediastreamsource.setbufferedrange)를 사용 하는 플랫폼에 올바른 버퍼링 된 범위를 올바르게 보고 합니다. 전체 콘텐츠가 완전히 버퍼링 된 후에는 더 이상 앱을 대신 하 여 네트워크를 예약 하지 않습니다.

미디어를 다운로드 하지 않을 때 백그라운드에서 발생 하는 네트워크 호출을 수행 해야 하는 경우 [**MaintenanceTrigger**](/uwp/api/Windows.ApplicationModel.Background.MaintenanceTrigger) 또는 [**TimeTrigger**](/uwp/api/Windows.ApplicationModel.Background.TimeTrigger)와 같은 적절 한 작업으로 래핑해야 합니다. 자세한 내용은 [백그라운드 작업을 사용 하 여 앱 지원](../launch-resume/support-your-app-with-background-tasks.md)을 참조 하세요.

## <a name="related-topics"></a>관련 항목
* [미디어 재생](media-playback.md)
* [MediaPlayer를 사용하여 오디오 및 비디오 재생](play-audio-and-video-with-mediaplayer.md)
* [시스템 미디어 전송 컨트롤과 통합](integrate-with-systemmediatransportcontrols.md)
* [배경 오디오 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundMediaPlayback)

 

 
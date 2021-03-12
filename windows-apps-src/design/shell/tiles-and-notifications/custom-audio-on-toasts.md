---
description: 알림 메시지에 사용자 지정 오디오를 사용 하 여 앱이 브랜드의 고유한 음향 효과를 표현할 수 있도록 하는 방법을 알아봅니다.
title: 알림을의 사용자 지정 오디오
label: Custom audio on toasts
template: detail.hbs
ms.date: 12/15/2017
ms.topic: article
keywords: windows 10, uwp, 알림, 사용자 지정 오디오, 알림, 오디오, 소리
ms.localizationpriority: medium
ms.openlocfilehash: 905292155dfc43a82c464edb651b2d176aeab960
ms.sourcegitcommit: 5e718720d1032a7089dea46a7c5aefa6cda3385f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/12/2021
ms.locfileid: "103226127"
---
# <a name="custom-audio-on-toasts"></a>알림을의 사용자 지정 오디오

알림 메시지에는 사용자 지정 오디오를 사용할 수 있습니다. 그러면 앱이 사용자의 브랜드 고유의 소리 효과를 표현할 수 있습니다. 예를 들어, 메시징 앱은 알림 메시지에 자신의 메시지 소리를 사용할 수 있으므로 사용자는 일반적인 알림 소리를 인식 하지 않고 앱에서 알림을 받았음을 즉시 알 수 있습니다.

## <a name="install-uwp-community-toolkit-nuget-package"></a>UWP Community Toolkit NuGet 패키지 설치

코드를 통해 알림을 만들려면 알림 XML 콘텐츠에 대 한 개체 모델을 제공 하는 UWP 커뮤니티 도구 키트 알림 라이브러리를 사용 하는 것이 좋습니다. 알림 XML을 수동으로 생성할 수도 있지만 오류가 발생 하기 쉬우며 복잡 합니다. UWP 커뮤니티 도구 키트 내의 알림 라이브러리는 Microsoft에서 알림을 소유 하는 팀에서 빌드하고 유지 관리 합니다.

NuGet의 [Microsoft Toolkit](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) .


## <a name="add-namespace-declarations"></a>네임스페이스 선언 추가

```csharp
using Microsoft.Toolkit.Uwp.Notifications;
```


## <a name="add-the-custom-audio"></a>사용자 지정 오디오를 추가 합니다.

Windows Mobile은 알림 메시지에서 항상 지원 되는 사용자 지정 오디오를 지원 합니다. 그러나 데스크톱은 버전 1511 (빌드 10586)에서 사용자 지정 오디오에 대 한 지원도 추가 했습니다. 버전 1511 이전 데스크톱 장치에 사용자 지정 오디오를 포함 하는 알림을 보내는 경우 알림이 자동으로 수행 됩니다. 따라서 데스크톱 1511 이전 버전의 경우 알림 메시지에 사용자 지정 오디오를 포함 해서는 안 됩니다. 그러면 알림이 최소한 기본 알림 소리를 사용 하 게 됩니다.

**알려진 문제**: 데스크톱 버전 1511을 사용 하는 경우 사용자 지정 알림 오디오는 앱이 스토어를 통해 설치 된 경우에만 작동 합니다. 즉, 저장소에 제출 하기 전에 바탕 화면에서 사용자 지정 오디오를 로컬로 테스트할 수 없지만, 해당 오디오는 스토어에서 설치 된 후에도 제대로 작동 합니다. 이 문제는 지역에서 배포 된 앱의 사용자 지정 오디오가 제대로 작동 하도록 기념일 업데이트에서 수정 되었습니다.

```csharp
var contentBuilder = new ToastContentBuilder()
    .AddText("New message");

    
bool supportsCustomAudio = true;
 
// If we're running on Desktop before Version 1511, do NOT include custom audio
// since it was not supported until Version 1511, and would result in a silent toast.
if (AnalyticsInfo.VersionInfo.DeviceFamily.Equals("Windows.Desktop")
    && !ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 2))
{
    supportsCustomAudio = false;
}
 
if (supportsCustomAudio)
{
    contentBuilder.AddAudio(new Uri("ms-appx:///Assets/Audio/CustomToastAudio.m4a"));
}

// Send the toast
contentBuilder.Show();
```

지원 되는 오디오 파일 형식에는 다음이 포함 됩니다.

- . aac
- . flac
- . m4a
- .mp3
- .wav
- .wma


## <a name="send-the-notification"></a>알림 보내기

오디오가 포함 된 알림을 보내는 것은 일반 알림을 보내는 것과 같습니다 (Show 메서드만 호출). 자세한 내용은 [로컬 알림 보내기](send-local-toast.md) 를 참조 하세요.


## <a name="related-topics"></a>관련 항목

- [GitHub의 전체 코드 샘플](https://github.com/WindowsNotifications/quickstart-toast-with-custom-audio)
- [로컬 알림 보내기](send-local-toast.md)
- [알림 콘텐츠 설명서](adaptive-interactive-toasts.md)

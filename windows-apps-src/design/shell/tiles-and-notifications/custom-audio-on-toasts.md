---
author: andrewleader
Description: Learn how to use custom audio on your toast notifications.
title: 알림에 대한 사용자 지정 오디오
label: Custom audio on toasts
template: detail.hbs
ms.author: mijacobs
ms.date: 12/15/2017
ms.topic: article
keywords: Windows 10, uwp, 알림, 사용자 지정 오디오, 알림, 오디오, 사운드
ms.localizationpriority: medium
ms.openlocfilehash: 8ef27dfed400715256d1d9cfa51f383a9b72c90d
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/03/2018
ms.locfileid: "5994564"
---
# <a name="custom-audio-on-toasts"></a>알림에 대한 사용자 지정 오디오

알림 메시지는 앱이 브랜드의 고유한 사운드 효과를 나타낼 수 있도록 사용자 지정 오디오를 사용할 수 있습니다. 예를 들어 메시지 앱은 일반 알림 사운드보다는 알림 메시지에서 자체 메시지 사운드를 사용할 수 있으므로 사용자는 앱의 알림을 수신하는 즉시 알 수 있습니다.

## <a name="install-uwp-community-toolkit-nuget-package"></a>UWP 커뮤니티 도구 키트 NuGet 패키지 설치

코드를 통해 알림을 만들려면 알림 XML 콘텐츠를 제공하는 UWP 커뮤니티 도구 키트 알림 라이브러리를 사용하는 것이 가장 좋습니다. 수동으로 알림 XML을 구성할 수 있지만 오류가 발생하기 쉽고 지저분합니다. UWP 커뮤니티 도구 키트의 알림 라이브러리는 Microsoft에서 알림을 소유한 팀에서 빌드하고 유지합니다.

NuGet에서 [Microsoft.Toolkit.Uwp.Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)를 설치합니다(이 문서에서는 버전 1.0.0 사용).


## <a name="add-namespace-declarations"></a>네임스페이스 선언 추가

`Windows.UI.Notifications` 타일 및 알림 API를 포함합니다. `Microsoft.Toolkit.Uwp.Notifications` 알림 라이브러리를 포함합니다.

```csharp
using Microsoft.Toolkit.Uwp.Notifications;
using Windows.UI.Notifications;
```


## <a name="construct-the-notification"></a>알림 구성

알림 메시지 콘텐츠에는 텍스트와 이미지뿐 아니라 단추와 입력도 포함되어 있습니다. [로컬 알림 보내기](send-local-toast.md)를 참조하여 전체 코드 조각을 확인하세요.

```csharp
ToastContent toastContent = new ToastContent()
{
    Visual = new ToastVisual()
    {
        ... (omitted)
    }
};
```


## <a name="add-the-custom-audio"></a>사용자 지정 오디오 추가

Windows Mobile은 항상 알림 메시지에서 사용자 지정 오디오를 지원해 왔습니다. 그렇지만 Desktop은 버전 1511(빌드 10586)에서 사용자 지정 오디오 지원만을 추가했습니다. 버전 1511 이전의 Desktop 디바이스에 사용자 지정 오디오를 포함한 알림을 보내면 이 알림은 무음으로 전달됩니다. 따라서 버전 1511 이전 Desktop의 경우에는 알림이 최소한 기본 알림 사운드를 사용할 것이므로 알림 메시지에 사용자 지정 오디오를 포함해서는 안 됩니다.

**알려진 문제**: Desktop 버전 1511을 사용하는 경우 Microsoft Store를 통해 앱을 설치할 때에만 사용자 지정 메시지 오디오가 작동합니다. 즉, Microsoft Store에 제출하기 전에 Desktop에서 사용자 정의 오디오를 로컬에서 테스트할 수 없지만 일단 Microsoft Store에서 설치하고 나면 오디오가 제대로 작동합니다. Anniversary Update에서 이 문제를 해결했으므로 로컬에 배포된 앱의 사용자 지정 오디오는 올바르게 작동합니다.

```csharp
?
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
    toastContent.Audio = new ToastAudio()
    {
        Src = new Uri("ms-appx:///Assets/Audio/CustomToastAudio.m4a")
    };
}
```

다음 오디오 파일 형식을 지원합니다.

- .aac
- .flac
- .m4a
- .mp3
- .wav
- .wma


## <a name="send-the-notification"></a>알림 보내기

이제 알림 콘텐츠를 완료했으므로 알림 보내기는 매우 간단합니다.

```csharp
// Create the toast notification from the previous toast content
ToastNotification notification = new ToastNotification(toastContent.GetXml());
             
// And then send the toast
ToastNotificationManager.CreateToastNotifier().Show(notification);
```


## <a name="related-topics"></a>관련 항목

- [GitHub의 전체 코드 샘플](https://github.com/WindowsNotifications/quickstart-toast-with-custom-audio)
- [로컬 알림 보내기](send-local-toast.md)
- [알림 콘텐츠 설명서](adaptive-interactive-toasts.md)
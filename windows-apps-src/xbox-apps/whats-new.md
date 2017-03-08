---
author: v-angraf
title: "Xbox One의 UWP에 대한 새로운 기능"
description: "Xbox One의 UWP 앱에 대한 새로운 기능을 강조하여 설명합니다."
ms.author: v-angraf
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: fe63c527-8f06-43a5-868f-de909f5664b3
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 96f9c9ef355382c72423187a7f81635571762071
ms.lasthandoff: 02/08/2017

---

# <a name="whats-new-for-developers-in-the-latest-update-of-uwp-on-xbox-one"></a>개발자를 위한 Xbox One의 UWP 최신 업데이트의 새로운 기능

Xbox One의 UWP(유니버설 Windows 플랫폼)에 대한 2016년 7월 릴리스에는 다음과 같은 새로운 기능, 기존 기능의 업데이트 및 버그 수정이 포함되어 있습니다.

## <a name="networking-using-tcpudp-sockets-is-now-available"></a>TCP/UDP 소켓을 사용하는 네트워킹이 출시되었습니다.  
이제 기존의 TCP/UDP 소켓(WinSock, Windows.Networking.Sockets)을 사용하는 콘솔에서 인바운드 및 아웃바운드 네트워크 액세스를 사용할 수 있습니다.

## <a name="fiddler-support"></a>Fiddler 지원
이제 Xbox One의 UWP를 사용하는 콘솔의 프록시로 Fiddler를 사용할 수 있습니다. Fiddler를 통해 Xbox 서비스와 신뢰 당사자 웹 서비스에서 나가고 들어오는 모든 HTTP/HTTPS 트래픽을 기록하고 검사할 수 있습니다. 자세한 내용은 [UWP용으로 개발하는 경우 Xbox One에서 Fiddler를 사용하는 방법](uwp-fiddler.md) 항목을 참조하세요.

## <a name="mouse-mode-is-now-enabled-by-default"></a>마우스 모드가 이제 기본적으로 사용하도록 설정됨
마우스 모드가 이제 XAML 및 호스트된 웹앱에 대해 기본적으로 사용하도록 설정됩니다.
마우스 모드를 끄고 방향 컨트롤러 탐색에 최적화하는 것이 좋습니다.
마우스 모드를 끄는 방법을 알아보려면 [마우스 모드를 사용하지 않도록 설정하는 방법](how-to-disable-mouse-mode.md)을 참조하세요.
멋진 Xbox용 앱을 빌드하는 방법은 [Xbox 및 TV용 디자인](../input-and-devices/designing-for-tv.md#mouse-mode)을 참조하세요.

## <a name="extended-uwp-api-surface-area-is-now-functional-on-the-console"></a>확장된 UWP API 노출 영역이 이제 콘솔에서 작동함
추가 UWP API가 이제 Xbox 콘솔에서 작동합니다. UWP API 지원에 대한 자세한 내용은 [Xbox에서 아직 지원되지 않는 UWP 기능](http://go.microsoft.com/fwlink/p/?LinkID=760755)을 참조하세요. 

## <a name="background-music-and-audio-capabilities"></a>백그라운드 음악 및 오디오 기능
이제 백그라우드에서 실행되는 앱에서 음악과 오디오를 재생할 수 있습니다.

## <a name="xaml-improvements"></a>XAML 개선 사항
XAML 플랫폼이 다음과 같이 개선되었습니다.
-    포커스 사각형의 스타일이 이제 텔레비전 10피트 환경에 맞게 지정되었습니다.
-    Xbox 소리가 이제 XAML 플랫폼에 포함되어 있습니다.
-    UI 요소 간 XY 포커스 탐색이 개선되었습니다. 

## <a name="you-can-now-change-the-size-of-allocated-developer-storage-on-the-console"></a>이제 콘솔에서 할당된 개발자 저장소의 크기를 변경할 수 있습니다.
개발자 홈 앱의 새로운 설정을 사용하여 콘솔에서 할당된 개발자 저장소의 크기를 늘리거나 줄일 수 있습니다. 할당된 개발자 저장소의 크기를 변경하는 방법에 대한 자세한 내용은 [Xbox One 도구 소개](introduction-to-xbox-tools.md)를 참조하세요.

## <a name="wdp-tool-enhancements"></a>WDP 도구 향상
Xbox용 WDP(Windows Device Portal) 도구가 다음과 같이 향상되었습니다.
 - 도구에 추가 콘솔 설정이 포함되어 있습니다. 콘솔 설정에 대한 자세한 내용은 [/ext/settings](wdp-xboxsettings-api.md) 참조 항목을 참조하세요. 
 - 사용자가 콘솔에서 로그인 및 로그아웃할 수 있습니다. 사용자에 대한 자세한 내용은 [/ext/user](wdp-user-management.md) 참조 항목을 참조하세요.
 - 이제 콘솔의 스크린샷을 캡처할 수 있습니다. 스크린샷 작성에 대한 자세한 내용은 [/ext/screenshot](wdp-media-capture-api.md) 참조 항목을 참조하세요.
 - 도구에서 앱의 느슨한 파일 빌드를 배포할 수 있습니다. 느슨한 파일 빌드에 대한 자세한 내용은 [/api/app/packagemanager/register](wdp-loose-folder-register-api.md) 참조 항목을 참조하세요.
 - 개발 PC의 파일 탐색기에서 콘솔의 개발자 파일에 액세스할 수 있습니다. 파일 탐색기를 통해 파일에 액세스하는 방법에 대한 자세한 내용은 [/ext/smb/developerfolder](wdp-smb-api.md) 참조 항목을 참조하세요.

## <a name="see-also"></a>참고 항목
- [알려진 문제](known-issues.md)
- [Xbox One의 UWP](index.md)


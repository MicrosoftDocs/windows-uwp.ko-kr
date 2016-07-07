---
author: v-angraf
title: "Xbox One의 UWP에 대한 새로운 기능"
description: "Xbox One의 UWP 앱에 대한 새로운 기능을 강조하여 설명합니다."
area: Xbox
ms.sourcegitcommit: 59019f209729b56e02ebdbdfd53a8fbf835c69f7
ms.openlocfilehash: dfa94ad42a79d0f6b3f72fbf2efe9ce043532c56

---

# Xbox One의 UWP에 대한 2016년 6월 Developer Preview의 새로운 기능

Xbox One의 UWP(유니버설 Windows 플랫폼)에 대한 2016년 6월 Developer Preview 릴리스에는 다음과 같은 새로운 기능, 기존 기능의 업데이트 및 버그 수정이 포함되어 있습니다.

## 마우스 모드가 이제 기본적으로 사용하도록 설정됨
마우스 모드가 이제 XAML 및 호스트된 웹앱에 대해 기본적으로 사용하도록 설정됩니다.
마우스 모드를 끄고 방향 컨트롤러 탐색에 최적화하는 것이 좋습니다.
마우스 모드를 끄는 방법을 알아보려면 [마우스 모드를 사용하지 않도록 설정하는 방법](how-to-disable-mouse-mode.md)을 참조하세요.
멋진 Xbox용 앱을 빌드하는 방법에 대한 자세한 내용은 [Xbox 및 TV용 디자인](https://msdn.microsoft.com/en-us/windows/uwp/input-and-devices/designing-for-tv?f=255&MSPPError=-2147217396#mouse-mode)을 참조하세요.

## 확장된 UWP API 노출 영역이 이제 콘솔에서 작동함
추가 UWP API가 이제 Xbox 콘솔에서 작동합니다. UWP API 지원에 대한 자세한 내용은 [Xbox에서 아직 지원되지 않는 UWP 기능](http://go.microsoft.com/fwlink/?LinkID=760755)을 참조하세요. 

## 백그라운드 음악 및 오디오 기능
이제 백그라우드에서 실행되는 앱에서 음악과 오디오를 재생할 수 있습니다.

## XAML 개선 사항
XAML 플랫폼이 다음과 같이 개선되었습니다.
-   포커스 사각형의 스타일이 이제 텔레비전 10피트 환경에 맞게 지정되었습니다.
-   Xbox 소리가 이제 XAML 플랫폼에 포함되어 있습니다.
-   UI 요소 간 XY 포커스 탐색이 개선되었습니다. 

## 이제 콘솔에서 할당된 개발자 저장소의 크기를 변경할 수 있습니다.
개발자 홈 앱의 새로운 설정을 사용하여 콘솔에서 할당된 개발자 저장소의 크기를 늘리거나 줄일 수 있습니다. 할당된 개발자 저장소의 크기를 변경하는 방법에 대한 자세한 내용은 [Xbox One 도구 소개](introduction-to-xbox-tools.md)를 참조하세요.

## WDP 도구 향상
Xbox용 WDP(Windows Device Portal) 도구가 다음과 같이 향상되었습니다.
 - 도구에 추가 콘솔 설정이 포함되어 있습니다. 콘솔 설정에 대한 자세한 내용은 [/ext/settings](wdp-xboxsettings-api.md) 참조 항목을 참조하세요. 
 - 사용자가 콘솔에서 로그인 및 로그아웃할 수 있습니다. 사용자에 대한 자세한 내용은 [/ext/user](wdp-user-management.md) 참조 항목을 참조하세요.
 - 이제 콘솔의 스크린샷을 캡처할 수 있습니다. 스크린샷 작성에 대한 자세한 내용은 [/ext/screenshot](wdp-media-capture-api.md) 참조 항목을 참조하세요.
 - 도구에서 앱의 느슨한 파일 빌드를 배포할 수 있습니다. 느슨한 파일 빌드에 대한 자세한 내용은 [/api/app/packagemanager/register](wdp-loose-folder-register-api.md) 참조 항목을 참조하세요.
 - 개발 PC의 파일 탐색기에서 콘솔의 개발자 파일에 액세스할 수 있습니다. 파일 탐색기를 통해 파일에 액세스하는 방법에 대한 자세한 내용은 [/ext/smb/developerfolder](wdp-smb-api.md) 참조 항목을 참조하세요.

## 참고 항목
- [Xbox One의 UWP](index.md)
- [알려진 문제](known-issues.md)



<!--HONumber=Jun16_HO4-->



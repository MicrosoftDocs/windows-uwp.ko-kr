---
author: mijacobs
Description: 알림 시각화 도우미는 스토어의 UWP(유니버설 Windows 플랫폼) 앱으로, 개발자가 Windows 10용 적응형 라이브 타일을 디자인하는 데 도움을 줍니다.
title: 알림 시각화 도우미
ms.assetid: FCBB7BB1-2C79-484B-8FFC-26FE1934EC1C
label: TBD
template: detail.hbs
---

# 알림 시각화 도우미





알림 시각화 도우미는 [스토어](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1)의 UWP(유니버설 Windows 플랫폼) 앱으로, 개발자가 Windows 10용 적응형 라이브 타일을 디자인하는 데 도움을 줍니다.

## <span id="Overview"></span><span id="overview"></span><span id="OVERVIEW"></span>개요


알림 시각화 도우미 앱은 Visual Studio의 XAML 편집기/디자인 뷰와 유사하게 편집 시 타일을 시각적 미리 보기를 통해 즉각적으로 확인할 수 있도록 합니다. 이 앱은 또한 오류를 검사하여 유효한 타일 페이로드를 만들 수 있도록 돕습니다.

이 앱 스크린샷은 XML 페이로드 및 선택한 디바이스에서 타일 크기가 표시되는 방식을 보여 줍니다.

![알림 시각화 도우미 앱 편집기 스크린샷(코드 및 타일 포함)](images/notif-visualizer-001.png)

 

알림 시각화 도우미를 사용하면 앱 자체를 편집 및 배포하지 않고도 적응형 타일 페이로드를 만들어 테스트할 수 있습니다. 원하는 시각적 결과가 포함된 페이로드를 만들었다면 이를 앱에 통합할 수 있습니다. 자세한 내용은 [로컬 타일 알림 보내기](tiles-and-notifications-sending-a-local-tile-notification.md)를 참조하세요.

**참고** 알림 시각화 도우미의 Windows 시작 메뉴 시뮬레이션은 항상 완전히 정확하지는 않으며 [baseUri](https://msdn.microsoft.com/library/windows/apps/br208712)와 같은 일부 페이로드 속성을 지원하지 않습니다. 원하는 타일 디자인을 얻으면 실제 시작 메뉴에 타일을 고정해 보고 테스트하여 의도한 대로 나타나는지 확인합니다.

 

## <span id="Features"></span><span id="features"></span><span id="FEATURES"></span>기능


알림 시각화 도우미에는 적응형 라이브 타일을 사용하여 수행할 수 있는 작업을 보여 주고 시작하는 데 도움을 주는 다양한 샘플 페이로드가 함께 제공됩니다. 다른 모든 텍스트 옵션, 그룹/하위 그룹, 배경 이미지 등을 실험해 볼 수 있으며 다양한 디바이스와 화면에서 타일이 조정되는 방식도 확인할 수 있습니다. 변경 후에는 업데이트된 페이로드를 나중에 사용하도록 파일에 저장할 수 있습니다.

편집기에서는 실시간 오류 및 경고가 나타납니다. 예를 들어 앱 페이로드가 5KB(플랫폼 제한) 미만으로 제한된 경우 알림 시각화 도우미는 페이로드가 이 제한을 넘으면 경고를 표시합니다. 잘못된 특성 이름 또는 값에 대해서도 경고를 제공하여 시각적 문제 디버깅에 도움을 줍니다.

표시 이름, 색, 로고, ShowName, 배지 값과 같은 타일 속성을 제어할 수 있습니다. 이러한 옵션을 사용하면 타일 속성과 타일 알림 페이로드가 상호 작용하는 방식 및 여기서 생성되는 결과를 즉시 이해할 수 있습니다.

이 앱 스크린샷은 타일 편집기를 보여 줍니다.

![알림 시각화 도우미 편집기 스크린샷(타일 포함)](images/notif-visualizer-004.png)

 

## <span id="related_topics"></span>관련 항목


* [스토어에서 알림 시각화 도우미 가져오기](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1)
* [적응형 타일 만들기](tiles-and-notifications-create-adaptive-tiles.md)
* [적응형 타일 템플릿: 스키마 및 설명서](tiles-and-notifications-adaptive-tiles-schema.md)
* [타일 및 알림(MSDN 블로그)](http://blogs.msdn.com/b/tiles_and_toasts/)
* [NotificationsExtensions 라이브러리(MSDN 블로그)](http://blogs.msdn.com/b/tiles_and_toasts/archive/2015/08/20/introducing-notificationsextensions-for-windows-10.aspx)
 

 






<!--HONumber=May16_HO2-->



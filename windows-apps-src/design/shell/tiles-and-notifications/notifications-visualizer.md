---
author: anbare
Description: Notifications Visualizer is a new Universal Windows Platform (UWP) app in the Store that helps developers design adaptive live tiles for Windows 10.
title: 알림 시각화 도우미
ms.assetid: FCBB7BB1-2C79-484B-8FFC-26FE1934EC1C
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10 uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1d8fde35a8a288bf28c6c5fad1b2ab7d80519285
ms.sourcegitcommit: eead3c00b27d9f66f79ec08c81a97e91dc1fdb3c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/18/2018
ms.locfileid: "1522912"
---
# <a name="notifications-visualizer"></a>알림 시각화 도우미

 


알림 시각화 도우미는 [Microsoft Store의](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1) UWP(유니버설 Windows 플랫폼) 앱으로, 개발자가 Windows 10용 적응형 Live Tile 및 대화형 알림 메시지를 디자인하는 데 도움을 줍니다.


## <a name="overview"></a>개요

알림 시각화 도우미는 Visual Studio의 XAML 편집기/디자인 뷰와 유사하게 XML 페이로드를 편집할 때 타일과 알림 메시지를 시각적 미리 보기를 통해 즉각적으로 확인할 수 있도록 합니다. 이 앱은 또한 오류를 검사하여 유효한 타일 또는 알림 메시지 페이로드를 만들 수 있도록 돕습니다.

이 앱 스크린샷은 XML 페이로드 및 선택한 디바이스에서 타일 크기가 표시되는 방식을 보여 줍니다.

![알림 시각화 도우미 앱 편집기 스크린샷(코드 및 타일 포함)](images/notif-visualizer-001.png)

 

알림 시각화 도우미를 사용하면 자체 앱을 편집 및 배포하지 않고도 적응형 타일 및 알림 페이로드를 만들어 테스트할 수 있습니다. 원하는 시각적 결과가 포함된 페이로드를 만들었다면 이를 앱에 통합할 수 있습니다. [로컬 타일 알림 보내기](sending-a-local-tile-notification.md)와 [로컬 알림 보내기](send-local-toast.md)를 참조하여 자세히 알아보세요.

**참고**   알림 시각화 도우미의 Windows 시작 메뉴 및 알림 메시지 시뮬레이션은 항상 완전히 정확하지는 않으며 일부 고급 페이로드 속성을 지원하지 않습니다. 원하는 타일 또는 알림이 있으면 실제 시작 메뉴에 타일을 고정하거나 알림을 표시하여 테스트하면서 의도한 대로 나타나는지 확인합니다.

 

## <a name="features"></a>기능

알림 시각화 도우미에는 적응형 Live Tile과 대화형 알림을 사용하여 수행할 수 있는 작업을 보여 주고 시작하는 데 도움을 주는 다양한 샘플 페이로드가 함께 제공됩니다. 다른 모든 텍스트 옵션, 그룹/하위 그룹, 배경 이미지 등을 실험해 볼 수 있으며 다양한 디바이스와 화면에서 타일이 조정되는 방식도 확인할 수 있습니다. 변경 후에는 업데이트된 페이로드를 나중에 사용하도록 파일에 저장할 수 있습니다.

편집기에서는 실시간 오류 및 경고가 나타납니다. 예를 들어 앱 페이로드가 5KB(플랫폼 제한) 이상인 경우 알림 시각화 도우미는 페이로드가 이 제한을 초과하면 경고를 표시합니다. 잘못된 특성 이름 또는 값에 대해서도 경고를 제공하여 시각적 문제 디버깅에 도움을 줍니다.

표시 이름, 색, 로고, ShowName, 배지 값과 같은 타일 속성을 제어할 수 있습니다. 이러한 옵션을 사용하면 타일 속성과 타일 알림 페이로드가 상호 작용하는 방식 및 여기서 생성되는 결과를 즉시 이해할 수 있습니다.

이 앱 스크린샷은 타일 편집기를 보여 줍니다.

![알림 시각화 도우미 편집기 스크린샷(타일 포함)](images/notif-visualizer-004.png)

 

## <a name="related-topics"></a>관련 항목

* [스토어에서 알림 시각화 도우미 가져오기](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1)
* [적응형 타일 만들기](create-adaptive-tiles.md)
* [대화형 알림](adaptive-interactive-toasts.md)
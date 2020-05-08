---
Description: 알림 시각화 도우미는 Store의 Windows 앱으로, 개발자가 Windows 10용 적응형 라이브 타일을 디자인하는 데 도움이 됩니다.
title: 알림 시각화 도우미
ms.assetid: FCBB7BB1-2C79-484B-8FFC-26FE1934EC1C
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c8d355570ef7002d1424457bf29f8161680f2c77
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82971038"
---
# <a name="notifications-visualizer"></a>알림 시각화 도우미

 


알림 시각화 도우미는 개발자가 Windows 10에 대 한 적응 라이브 타일 및 대화형 알림 [메시지를 디자인 하는](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1) 데 도움이 되는 새로운 windows 앱 앱입니다.


## <a name="overview"></a>개요

알림 시각화 도우미는 Visual Studio의 XAML 편집기/디자인 뷰와 마찬가지로 XML 페이로드를 편집할 때 타일 및 알림 메시지에 대 한 즉각적인 시각적 미리 보기를 제공 합니다. 또한 앱은 유효한 타일 또는 알림 메시지를 만들도록 하는 오류를 확인 합니다.

이 앱의 스크린샷은 XML 페이로드 및 선택한 장치에 타일 크기를 표시 하는 방법을 보여 줍니다.

![코드 및 타일이 있는 알림 시각화 도우미 앱 편집기의 스크린샷](images/notif-visualizer-001.png)

 

알림 시각화 도우미를 사용 하 여 사용자 고유의 앱을 편집 하 고 배포할 필요 없이 적응 타일 및 알림 페이로드를 만들고 테스트할 수 있습니다. 이상적인 시각적 결과를 사용 하 여 페이로드를 만들었으면 앱에 통합할 수 있습니다. 자세한 내용은 [로컬 타일 알림 보내기](sending-a-local-tile-notification.md) 및 [로컬 알림 보내기](send-local-toast.md) 를 참조 하세요.

**참고**    알림 시각화 도우미는 Windows 시작 메뉴의 시뮬레이션 및 알림 알림은 항상 정확 하지는 않지만 일부 고급 페이로드 속성은 지원 하지 않습니다. 원하는 타일 또는 알림이 있으면 타일을 고정 하거나 알림 메시지를 팝 하 여 원하는 대로 나타나는지 확인 합니다.

 

## <a name="features"></a>기능

알림 시각화 도우미는 다양 한 샘플 페이로드를 제공 하 여 시작 하는 데 도움이 되는 적응 라이브 타일 및 대화형 알림을에서 가능한 작업을 보여 줍니다. 다른 모든 텍스트 옵션, 그룹/하위 그룹, 배경 이미지를 시험해 보고 타일이 다른 장치 및 화면에 어떻게 적응 하는지 확인할 수 있습니다. 변경을 수행한 후에는 나중에 사용할 수 있도록 업데이트 된 페이로드를 파일에 저장할 수 있습니다.

편집기에서 실시간 오류와 경고를 제공 합니다. 예를 들어 페이로드가 5KB (플랫폼 제한) 보다 큰 경우 알림 시각화 도우미는 페이로드가 해당 제한을 초과 한다는 경고를 표시 합니다. 시각적 문제를 디버그 하는 데 도움이 되는 잘못 된 특성 이름 또는 값에 대 한 경고를 제공 합니다.

표시 이름, 색, 로고, ShowName, 배지 값 등의 타일 속성을 제어할 수 있습니다. 이러한 옵션을 통해 타일 속성과 타일 알림 페이로드가 상호 작용 하는 방식과 생성 되는 결과를 즉시 파악할 수 있습니다.

앱의이 스크린샷은 타일 편집기를 표시 합니다.

![타일을 사용 하는 알림 시각화 도우미 편집기의 스크린샷](images/notif-visualizer-004.png)

 

## <a name="related-topics"></a>관련 항목

* [스토어에서 알림 시각화 도우미 가져오기](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1)
* [적응 타일 만들기](create-adaptive-tiles.md)
* [대화형 알림을](adaptive-interactive-toasts.md)

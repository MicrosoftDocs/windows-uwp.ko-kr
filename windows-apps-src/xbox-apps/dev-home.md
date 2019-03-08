---
ms.assetid: a56156e4-7adb-bf37-527b-fc3243e04b46
title: 콘솔의 개발자 홈(개발자 홈)
description: Xbox One용 개발자 홈 앱에 대한 정보를 제공합니다.
ms.date: 08/09/2017
ms.topic: article
keywords: windows 10, uwp
permalink: en-us/docs/xdk/dev-home.html
ms.localizationpriority: medium
ms.openlocfilehash: 4113df37446d93883cf395e7c1e86b1de6c1b328
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57620848"
---
# <a name="developer-home-on-the-console-dev-home"></a>콘솔의 개발자 홈(개발자 홈)
   
  
개발자 홈은 개발자의 생산성을 향상하도록 설계된 Xbox One 개발 키트의 도구 환경입니다. 개발자 홈에서는 개발 키트를 관리 및 구성하고, 사용자를 관리하고, 설치된 타이틀을 실행하고, 캡처 및 추적을 수행하는 기능을 제공합니다. 향후 릴리스에서는 여러분의 의견에 따라 추가 기능을 계속 확장할 것이며, 또한 개발자 고유의 도구를 확장하고 추가할 수 있도록 할 방침입니다.   
   
  
개발자 홈 및 개발자 홈에서 지원하기를 기대하는 시나리오에 대한 여러분의 많은 의견을 기다리고 있습니다. 앱 주 메뉴의 **사용자 의견 보내기**에 설명된 방법을 통해 또는 DAM(Developer Account Manager)을 통해 의견을 남겨 주세요.   
   
  
2015년 11월 이상 복구에서 개발자 홈을 시작하려면:  
 
   1. 홈에서 왼쪽으로 이동하거나 Nexus 단추를 두 번 클릭하여 가이드를 엽니다.  
   1. 아래로 **설정**(기어 아이콘)으로 이동합니다.   
   1. **모든 설정**을 선택합니다.  
   1. 기본 **개발자** 페이지에서 **개발자 홈**(홈 아이콘)을 선택합니다.   

 ![](images/dev_home_icons.png)   
  
이전 복구에서, **추천 콘텐츠**의 홈 화면 오른쪽에서 개발자 홈 타일을 선택하거나 Xbox One 관리자에서 응용 프로그램 목록을 보고 **개발자 홈**을 시작합니다.   
 ![](images/dev_home_1.png) 
<a id="ID4EBC"></a>

   

## <a name="user-interface"></a>사용자 인터페이스  
   
  
개발자 홈 사용자 인터페이스의 헤더에는 개발 콘솔에 대한 다음과 같은 중요한 "개요" 정보가 포함되어 있습니다.   
 
   *  **콘솔 IP:** 콘솔의 현재 IP 주소를 지정 합니다.   
   *  **콘솔 이름:** 콘솔의 현재 호스트 이름입니다.  
   *  **샌드박스:** 콘솔에 있는 sandbox의 이름입니다.  
   *  **OS 버전:** 콘솔에서 실행 되는 현재 복구 버전.
   *  현재 시스템 시간.   

   
  
개발자 홈 UI의 나머지 부분은 다음 페이지로 구분됩니다. 이러한 페이지의 도구에 대한 자세한 내용은 해당 개별 항목을 참조하세요.   
 
   *  [홈](devhome-home.md)  
   *  [Xbox Live](devhome-live.md)  
   *  [설정](devhome-settings.md)  
   *  [미디어 캡처](devhome-capture.md)  
   *  [네트워킹](devhome-networking.md)  
   *  [성능](devhome-performance.md)  

  
<a id="ID4EKE"></a>

   

## <a name="main-menu"></a>주 메뉴  
   
  
컨트롤러의 **메뉴** 단추를 눌러 앱 작업 영역, 네트워크 위치에 액세스하는 데 사용되는 자격 증명을 관리하는 기능 및 앱 관련 피드백 제공에 대한 정보를 구성하는 주 메뉴에 액세스할 수 있습니다.   
  
<a id="ID4EUE"></a>

   

## <a name="snap-mode-ux"></a>끌기 모드 UX  
   
  
네트워킹 및 멀티 플레이 같은 개발자 홈의 여러 기존 도구 및 향후 출시될 도구는 테스트 중에 도구에 쉽게 액세스할 수 있도록 타이틀을 실행하는 동안 측면으로 끌어서 사용할 수 있도록 설계되었습니다.   
   
  
끌기 모드에 액세스하려면 적절한 도구의 타이틀을 강조 표시하고, 컨트롤러에서 **보기** 단추를 누르고, 상황에 맞는 메뉴에서 **끌기**를 선택합니다.  
 ![](images/dev_home_4.png)   
  
개발자 홈이 오른쪽에 맞춰집니다. 평소대로 Nexus 단추를 두 번 탭하여 컨텍스트를 전환할 수 있습니다.  
 ![](images/dev_home_5.png)  
<a id="ID4EKF"></a>

   

## <a name="customizing-dev-home"></a>개발자 홈 사용자 지정  
   
  
개발자 홈은 사용자 지정 가능하고 개인적으로 설계되었습니다. 개발자의 워크플로에 맞게 앱을 구성한 다음 작업 영역으로 저장할 수 있습니다. 이 작업 영역을 내보내고 가져와서 필요에 따라 레이아웃을 다른 콘솔로 복사할 수 있습니다. 이러한 옵션은 **작업 영역** 아래의 주 메뉴에 있습니다. 내보낸 파일은 `Dev Home\Workspaces`디렉터리의 시스템 스크래치 드라이브에 있습니다.   
 
<a id="ID4EVF"></a>

   

### <a name="resizing-and-reordering-tools"></a>도구 크기 조정 및 다시 정렬  
   
  
도구의 크기나 위치를 변경하려면 타이틀에 포커스가 있는 동안 상황에 맞는 메뉴 단추(컨트롤러의 보기 단추)를 사용합니다. 상황에 맞는 메뉴에서 **이동** 또는 **크기 조정**을 선택합니다.   
 ![](images/dev_home_6.png)  
<a id="ID4EEG"></a>

   

### <a name="changing-theme-color-and-background-image"></a>테마 색 및 배경 이미지 변경  
   
  
주 메뉴에서 **작업 영역**을 선택한 다음 **테마 색 변경**을 선택할 수 있습니다 새로운 색을 선택하고 **저장**을 선택하여 포커스 강조 표시에 사용된 테마 색을 업데이트합니다.   
 ![](images/dev_home_7.png)  
<a id="ID4EVG"></a>

   

### <a name="setting-the-default-application-for-a-package"></a>패키지에 대한 기본 응용 프로그램 설정  
   
  
패키지에 여러 응용 프로그램이 들어 있는 경우 실행할 기본 응용 프로그램을 개발자 홈에서 설정할 수 있습니다. 시작 관리자에서 패키지를 강조 표시하고 **A** 단추를 눌러 사용 가능한 응용 프로그램 목록을 엽니다. 기본 응용 프로그램으로 설정할 응용 프로그램을 강조 표시하고, **보기** 단추를 누른 다음 상황에 맞는 메뉴에서 **기본값으로 설정**을 선택합니다.   
 ![](images/dev_home_setdefault.png)  
<a id="ID4EGH"></a>

   

### <a name="using-dev-home-to-register-and-launch-titles-from-a-network-share"></a>개발자 홈을 사용하여 네트워크 공유의 타이틀을 등록하고 시작  
   
  
시작 관리자 맨 아래에 있는 설치된 앱 및 게임 목록에서 **네트워크 공유의 게임 등록** 옵션을 선택하여 타이틀의 느슨한 파일 버전을 원격으로 실행할 수 있습니다.   
 ![](images/dev_home_8.png)   
  
그런 다음 등록하려는 타이틀의 appxmanifest.xml 파일에 대한 네트워크 경로를 입력할 수 있습니다. 개발자 홈은 해당 공유의 기존 자격 증명을 사용하여 타이틀을 등록하려고 시도하며, 필요한 경우 새로운 네트워크 자격 증명을 요구합니다. 추가 공유에 액세스해야 하는 경우(예를 들어 별도 서버의 기호로 연결된 리소스에 액세스해야 하는 경우) 아래 옵션을 통해 해당 공유를 추가해야 합니다.   
   
  
주 메뉴의 **네트워크 자격 증명 관리** 옵션을 통해 콘솔에 저장된 자격 증명을 관리(그리고 새 자격 증명을 추가)할 수 있습니다.   
 ![](images/dev_home_9.png)   
  
현재 콘솔에 있는 자격 증명을 보고, 자격 증명을 편집하고(자격 증명의 경로를 선택한 후 **A** 단추 누르기), 자격 증명을 제거(제거 링크를 선택한 후 **A** 단추 누르기)할 수 있습니다.   
   
<a id="ID4EGAAC"></a>

   

## <a name="in-this-section"></a>이 섹션의 내용  
  
[홈 페이지 (개발자 홈)](devhome-home.md)  


&nbsp;&nbsp;개발 콘솔에서 정기적으로 수행 되는 작업에 대 한 빠른 액세스를 제공 합니다. 
  
  
[Xbox Live 페이지 (개발자 홈)](devhome-live.md)  


&nbsp;&nbsp;멀티 플레이 정보를 캡처하고 Xbox Live 서비스의 현재 상태를 표시 합니다. 
  
  
[설정 페이지 (개발자 홈)](devhome-settings.md)  


&nbsp;&nbsp;개발 콘솔에 대 한 다양 한 설정에 대 한 액세스를 제공합니다. 
  
  
[미디어 캡처 (개발자 홈) 페이지](devhome-capture.md)  


&nbsp;&nbsp;합니다 **미디어 캡처** 개발자 홈 페이지는 콘솔에서 현재 실행 되는 제목의 비디오를 캡처합니다. 
  
  
[네트워킹 페이지 (개발자 홈)](devhome-networking.md)  


&nbsp;&nbsp;문제 해결을 위해 다양 한 네트워킹 조건을 시뮬레이션 합니다. 
  
  
[성능 페이지 (개발자 홈)](devhome-performance.md)  


&nbsp;&nbsp;다양 한 디스크 작업 및 문제 해결을 위해 CPU 사용량 조건을 시뮬레이션 합니다. 
 
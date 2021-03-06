---
ms.assetid: a56156e4-7adb-bf37-527b-fc3243e04b46
title: 콘솔의 개발자 홈 (Dev Home)
description: Xbox One 콘솔 개발 키트의 개발 홈 도구 환경에서 개발자 생산성을 지원 하는 방법을 알아보세요.
ms.date: 08/09/2017
ms.topic: article
keywords: windows 10, uwp
permalink: en-us/docs/xdk/dev-home.html
ms.localizationpriority: medium
ms.openlocfilehash: 291b25e962aa8ac37705fd0db544f138036949d2
ms.sourcegitcommit: 6cb20dca1cb60b4f6b894b95dcc2cc3a166165ad
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/01/2020
ms.locfileid: "91636613"
---
# <a name="developer-home-on-the-console-dev-home"></a>콘솔의 개발자 홈 (Dev Home)
   
  
Dev Home은 개발자의 생산성을 지원 하도록 설계 된 Xbox One 개발 키트의 도구 환경입니다. 개발자 홈에서는 개발 키트를 관리 및 구성 하 고, 사용자를 관리 하 고, 설치 된 타이틀을 시작 하 고, 캡처 및 추적을 수행할 수 있는 기능을 이후 릴리스에서는 사용자 의견에 따라 추가 기능을 사용할 수 있도록 기능을 계속 확장 하 고 확장성 및 사용자 고유의 도구 추가를 사용 하도록 설정 합니다.   
   
  
개발자 홈에 대 한 피드백 및 it 지원에 가장 관심이 있는 시나리오에 대해 매우 관심이 있습니다. 앱의 주 메뉴 또는 개발자 계정 관리자 (DAM)를 통해 **사용자 의견 보내기** 아래에 설명 된 방법을 통해 의견을 제공 해 주세요.   
   
  
11 월 2015 이상 복구에서 개발자 홈을 시작 하려면 다음을 수행 합니다.  
 
   1. 홈에서 왼쪽으로 이동 하거나 Nexus 단추를 두 번 클릭 하 여 가이드를 엽니다.  
   1. **설정** (기어 아이콘)으로 아래로 이동   
   1. **모든 설정** 선택  
   1. 기본 **개발자** 페이지에서 **개발자 홈** (홈 아이콘)을 선택 합니다.   

 ![설정에서 개발자 페이지의 스크린샷](images/dev_home_icons.png)   
  
이전 복구에서 **추천 콘텐츠의** 홈 화면 오른쪽에 있는 Dev home 타일을 선택 하거나 Xbox one Manager의 응용 프로그램 목록을 보고 **개발자 홈**을 시작 합니다.   
 ![시작 옵션이 강조 표시 된 응용 프로그램 탭의 스크린샷](images/dev_home_1.png) 
<a id="ID4EBC"></a>

   

## <a name="user-interface"></a>사용자 인터페이스  
   
  
개발자 홈 사용자 인터페이스의 헤더에는 개발 콘솔에 대 한 다음과 같은 중요 한 "간략 한" 정보가 포함 되어 있습니다.   
 
   *  **콘솔 IP:** 콘솔의 현재 IP 주소입니다.   
   *  **콘솔 이름:** 콘솔의 현재 호스트 이름입니다.  
   *  **샌드박스:** 콘솔이 있는 샌드박스의 이름입니다.  
   *  **OS 버전:** 콘솔에서 실행 중인 현재 복구 버전입니다.
   *  현재 시스템 시간입니다.   

   
  
개발자 홈 UI의 나머지 부분은 다음 페이지로 나뉘어 있습니다. 이러한 페이지의 도구에 대 한 자세한 내용은 개별 항목을 참조 하세요.   
 
   *  [홈](devhome-home.md)  
   *  [Xbox Live](devhome-live.md)  
   *  [설정](devhome-settings.md)  
   *  [미디어 캡처](devhome-capture.md)  
   *  [네트워킹](devhome-networking.md)  
   *  [성능](devhome-performance.md)  

  
<a id="ID4EKE"></a>

   

## <a name="main-menu"></a>주 메뉴  
   
  
컨트롤러의 **메뉴** 단추를 눌러 앱 작업 영역의 구성을 허용 하는 주 메뉴, 네트워크 위치에 액세스 하기 위한 자격 증명을 관리 하는 기능 및 앱에 대 한 피드백 제공에 대 한 정보를 액세스할 수 있습니다.   
  
<a id="ID4EUE"></a>

   

## <a name="snap-mode-ux"></a>맞추기 모드 UX  
   
  
네트워킹 및 멀티 플레이어와 같은 개발자 홈의 일부 기존 및 예정 된 도구는 사용자가 테스트 하는 동안 도구에 쉽게 액세스할 수 있도록 타이틀을 실행 하는 동안 면에 맞춰 사용 하도록 설계 되었습니다.   
   
  
맞추기 모드에 액세스 하려면 적절 한 도구의 제목을 강조 표시 하 고 컨트롤러에서 **보기** 단추를 누른 다음 상황에 맞는 메뉴에서 **맞추기** 를 선택 합니다.  
 ![강조 표시 된 맞추기 옵션을 보여 주는 개발자 홈 페이지의 스크린샷](images/dev_home_4.png)   
  
개발자 홈이 오른쪽에 맞춰집니다. 일반적인 방법으로 Nexus 단추를 두 번 눌러 컨텍스트를 전환할 수 있습니다.  
 ![테스트 하는 동안 사용자가 도구에 액세스할 수 있다는 것을 보여 주는 오른쪽에 맞춘 개발자 홈 페이지를 보여 주는 스크린샷](images/dev_home_5.png)  
<a id="ID4EKF"></a>

   

## <a name="customizing-dev-home"></a>개발자 홈 사용자 지정  
   
  
개발자 홈은 사용자 지정이 가능 하 고 개인 사용자로 설계 되었습니다. 워크플로에 맞게 앱을 구성한 다음 작업 영역으로 저장할 수 있습니다. 이 작업 영역을 내보내고 가져올 수 있으므로 필요에 따라 다른 콘솔에 레이아웃을 복사할 수 있습니다. 이러한 옵션은 주 메뉴의 **작업 영역**아래에 있습니다. 내보낸 파일은 시스템 스크래치 드라이브의 디렉터리에 있습니다 `Dev Home\Workspaces` .   
 
<a id="ID4EVF"></a>

   

### <a name="resizing-and-reordering-tools"></a>크기 조정 및 다시 정렬 도구  
   
  
도구의 크기나 위치를 변경 하려면 제목에 포커스가 있을 때 상황에 맞는 메뉴 단추 (컨트롤러의 보기 단추)를 사용 합니다. 상황에 맞는 메뉴에서 **이동** 또는 **크기 조정**을 선택 합니다.   
 ![이동 옵션이 강조 표시 된 오른쪽에 맞춰진 개발자 홈 페이지의 스크린샷](images/dev_home_6.png)  
<a id="ID4EEG"></a>

   

### <a name="changing-theme-color-and-background-image"></a>테마 색 및 배경 이미지 변경  
   
  
주 메뉴에서 **작업 영역** 을 선택한 다음 **테마 색을 변경할**수 있습니다. 새 색을 선택 하 고 **저장** 을 선택 하 여 포커스를 강조 표시 하는 데 사용 되는 테마 색을 업데이트 합니다.   
 ![작업 영역을 표시 하 고 테마 색 옵션을 선택 하는 개발자 홈 페이지의 스크린샷](images/dev_home_7.png)  
<a id="ID4EVG"></a>

   

### <a name="setting-the-default-application-for-a-package"></a>패키지에 대 한 기본 응용 프로그램 설정  
   
  
패키지에 여러 응용 프로그램이 포함 되어 있는 경우 개발자 홈에서 기본 응용 프로그램을 시작할 수 있도록 설정할 수 있습니다. 시작 관리자에서 패키지를 강조 표시 하 고 **A** 단추를 눌러 사용 가능한 응용 프로그램 목록을 엽니다. 기본값으로 설정 하려는 항목을 강조 표시 하 고 **보기** 단추를 누른 다음 상황에 맞는 메뉴에서 **기본값으로 설정** 을 선택 합니다.   
 ![기본으로 설정 옵션이 강조 표시 된 개발자 홈 페이지의 스크린샷](images/dev_home_setdefault.png)  
<a id="ID4EGH"></a>

   

### <a name="using-dev-home-to-register-and-launch-titles-from-a-network-share"></a>Dev Home을 사용 하 여 네트워크 공유에서 제목 등록 및 시작  
   
  
설치 된 앱 및 게임 목록의 맨 아래에 있는 시작 관리자에서 **네트워크 공유에서 게임 등록** 옵션을 선택 하 여 느슨한 파일 버전의 타이틀을 원격으로 실행할 수 있습니다.   
 ![네트워크 공유에서 게임 등록 옵션이 강조 표시 된 개발자 홈 페이지의 스크린샷](images/dev_home_8.png)   
  
그런 다음 등록 하려는 제목의 appxmanifest.xml 파일에 대 한 네트워크 경로를 입력할 수 있습니다. Dev Home은 해당 공유에 대 한 기존 자격 증명을 사용 하 여 타이틀을 등록 하려고 하며, 필요한 경우 새 네트워크 자격 증명을 입력 하 라는 메시지가 표시 됩니다. 추가 공유 (예: 별도의 서버에서 연결 된 리소스에 액세스 하는 경우)에 액세스 해야 하는 경우 아래 옵션을 통해 추가 공유를 추가 해야 기호로.   
   
  
주 메뉴의 **네트워크 자격 증명 관리** 옵션을 통해 콘솔에서 이러한 저장 된 자격 증명을 관리 하 고 다른 자격 증명을 추가할 수 있습니다.   
 ![네트워크 자격 증명 관리 옵션이 강조 표시 된 개발자 홈 페이지의 스크린샷](images/dev_home_9.png)   
  
현재 콘솔에서 자격 증명을 확인 하 고, 자격 증명의 경로를 선택 하 고, 단추 **를** 클릭 하 여 자격 증명을 편집 하 고, 제거 링크를 선택 하 고 단추 **를** 클릭 하 여 자격 증명을 제거할 수 있습니다.   
   
<a id="ID4EGAAC"></a>

   

## <a name="in-this-section"></a>섹션 내용  
  
[홈페이지 (Dev Home)](devhome-home.md)  


&nbsp;&nbsp;개발 콘솔에서 정기적으로 수행 되는 작업에 빠르게 액세스할 수 있습니다. 
  
  
[Xbox Live 페이지 (Dev Home)](devhome-live.md)  


&nbsp;&nbsp;멀티 플레이 정보를 캡처하고 Xbox Live 서비스의 현재 상태를 표시 합니다. 
  
  
[설정 페이지 (Dev Home)](devhome-settings.md)  


&nbsp;&nbsp;개발 콘솔에 대 한 다양 한 설정에 대 한 액세스를 제공 합니다. 
  
  
[미디어 캡처 페이지 (Dev Home)](devhome-capture.md)  


&nbsp;&nbsp;개발자 홈의 **미디어 캡처** 페이지는 콘솔에서 현재 실행 중인 제목의 비디오를 캡처합니다. 
  
  
[네트워킹 페이지 (Dev Home)](devhome-networking.md)  


&nbsp;&nbsp;문제 해결을 위해 다양 한 네트워킹 조건을 시뮬레이션 합니다. 
  
  
[성능 페이지 (Dev Home)](devhome-performance.md)  


&nbsp;&nbsp;문제 해결을 위해 다양 한 디스크 작업 및 CPU 사용 조건을 시뮬레이션 합니다. 
 
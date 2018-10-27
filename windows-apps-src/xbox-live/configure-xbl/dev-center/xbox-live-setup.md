---
title: Xbox Live 설정 구성
author: shrutimundra
description: Windows 개발자 센터에서 Xbox Live 설치를 구성 하는 방법을 설명 합니다.
ms.assetid: ''
ms.author: kevinasg
ms.date: 10/30/2017
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox live, Xbox, 게임, uwp, windows 10, Xbox one, Windows 개발자 센터, Xbox Live 설정
ms.openlocfilehash: 761d5e721220a3094c455273a71d029fc8c26ba1
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5692647"
---
# <a name="configure-xbox-live-setup-on-windows-dev-center"></a>Windows 개발자 센터에서 Xbox Live 설정 구성

연결 된 게임에 Xbox Live 속성의 초기 집합을 구성 하려면 [Windows 개발자 센터](https://developer.microsoft.com/dashboard) 를 사용할 수 있습니다. 다음을 수행 하 여 구성을 추가 합니다.

1. **서비스**아래의 타이틀에 **Xbox Live 설정** 섹션으로 이동 > **Xbox Live** > **Xbox Live 설정**합니다.
2. 이 페이지에서 제목 이름, 기본 로캘로, 제품 유형, 디바이스 패밀리 및 금지 날짜를 설정할 수 있습니다. 완료 되 면 변경 내용을 제출 하려면 **저장** 단추를 클릭 하 여 구성을 설정 하는 합니다.

## <a name="title-names"></a>제목 이름
**지역화 된 제목 추가**클릭 하 여 제품에 대 한 이름을 입력 하 고 지역화 되 게 하는 언어를 선택할 수 있습니다. Note 여기 제목 이름은 제출의 속성 페이지에 정의 된 지역화 된 제품 이름에 매핑해야 합니다. 기본값은 영어 (EN-US)입니다.

![개발자 센터에서 추가 지역화 된 제목 대화의 이미지](../../images/dev-center/xbox-live-setup/xbox-live-setup-1.png)

## <a name="default-locale"></a>기본 로캘로
이 옵션을 사용 하면 Xbox Live 서비스 구성에서 모든 문자열을 구성 하는 데 사용할 기본 언어를 설정할 수 있습니다. 예를 들어, 스페인어 (ES-ES)로 기본 로캘로 설정한 경우 성과 구성 하려면 다음 최소한 도전 과제 이름과 설명을 것 스페인어에서 수입니다. 즉, 스페인어로이 옵션을 설정할 수 있지만 영어로 도전 과제 정보만 제공 수 없습니다. Xbox Live 서비스 구성의 모든 기본 로캘로와 동일한 버전에서 제공 되어야 합니다. 기본적으로 기본 로캘로 영어 (EN-US)로 설정 됩니다.
> [!NOTE]
> 또한 지역화 된 문자열 페이지에서 모든 문자열을 지역화할 수 있습니다.  

![개발자 센터에서 사용자의 기본 로케일 선택 드롭다운 선택의 이미지](../../images/dev-center/xbox-live-setup/xbox-live-setup-2.png)

## <a name="product-type"></a>제품 유형
드롭다운 메뉴 제품 유형을 변경할 수 있습니다. **게임**유형 기본값으로 사용 됩니다. 선택 하는 옵션을 사용할 수 있는 XboxLivefeatures 영향을 줍니다. 선택할 수는 세 가지 옵션이 있습니다.
1. 앱 
2. Game 
3. 게임 데모 

![개발자 센터에서 제품 유형을 선택 드롭다운 선택의 이미지](../../images/dev-center/xbox-live-setup/xbox-live-setup-3.png)

## <a name="device-families"></a>디바이스 패밀리
이 구성에서는 타이틀 Xbox Live에 액세스할 수는 장치 유형을 선택할 수 있습니다. 기본적으로 모든 디바이스 패밀리에서 사용할 수 있습니다. 사용 하 여 장치를 확인할 수 있습니다.

![개발자 센터에서 디바이스 패밀리를 선택 하 여 선택 확인란의 이미지](../../images/dev-center/xbox-live-setup/xbox-live-setup-4.png)

## <a name="embargo-date"></a>금지 날짜
Xbox Live 구성과 공개적으로 라이브 이동할 때 선택한 날짜를 결정 합니다. 것는 소매에 변경 내용을 게시 하는 경우에 이러한 들어가지 것입니다 라이브 금지 날짜를 충족 하지 않는 한 유의 해야 합니다. 추가로 설명할 수 있습니다.
1. 나중에 날짜를 선택 하는 경우 변경 내용을 익숙해 공개적으로 사용할 수 있는 해당 날짜에입니다.
2. 과거에 날짜를 선택 하는 경우 일반 정품 변경 내용을 게시 되는 즉시 변경 공개적으로 사용할 수 있습니다.

날짜 및 시간 선택기를 클릭 하 고 정확한 날짜와 시간을 선택할 수 있도록 확장 됩니다. **확인**을 클릭 한 후 금지 날짜 설정 됩니다.

![개발자 센터에서 금지 날짜를 설정의 이미지](../../images/dev-center/xbox-live-setup/xbox-live-setup-5.png)

## <a name="advanced-settings"></a>고급 설정

**여러 점의 존재 여부를**설정 하는 **옵션 표시** 를 클릭할 수 있습니다. 여러 점의 존재 여부에 로그인 할 Xbox Live 여러 장치에서 동시에 동일한 사용자 수 있습니다. 와 같은 Xbox Live 기능 도전 과제 및 멀티 플레이어는 액세스가 제한 됩니다. 따라서이 옵션은 게임에 대 한 권장 되지 않습니다. 이 옵션을 사용 하도록 설정 하 여 확인란을 선택 합니다.

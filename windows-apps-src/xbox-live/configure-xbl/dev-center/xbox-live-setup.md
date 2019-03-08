---
title: Xbox Live 설치 구성
description: 파트너 센터에서 Xbox Live 설치를 구성 하는 방법을 설명 합니다.
ms.assetid: ''
ms.date: 10/30/2017
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox live, Xbox, 게임, uwp, windows 10, 하나는 Xbox, 파트너 센터, Xbox Live 설치
ms.openlocfilehash: 9a846a4b7f0069216e92eb123b33d9fc0f7f67c9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612828"
---
# <a name="configure-xbox-live-setup-in-partner-center"></a>파트너 센터에서 Xbox Live 설치 구성

사용할 수 있습니다 [파트너 센터](https://developer.microsoft.com/dashboard) Xbox Live 게임에 연관 된 속성의 초기 집합을 구성할 수 있습니다. 다음을 수행 하 여 구성을 추가 합니다.

1. 로 이동 합니다 **Xbox Live 설치** 아래에 있는 제목으로 섹션 **Services** > **Xbox Live** > **Xbox Live 설치** .
2. 이 페이지에서 제목 이름, 기본 로캘, 제품 유형, 장치 제품군 및 내려져 날짜를 설정할 수 있습니다. 완료 되 면 구성 설정을 클릭 합니다 **저장** 변경 내용을 제출 단추.

## <a name="title-names"></a>제목 이름
클릭 하 여 **추가 제목을 지역화**, 제품의 이름을 입력 하 고 지역화 하는 언어를 선택 합니다. 제목 이름을 제출의 속성 페이지에서 정의한 지역화 한 제품 이름에 매핑하는 다음 note 합니다. 기본값은 영어 (EN-US)입니다.

![파트너 센터에서 추가 지역화 된 제목 대화 상자의 이미지](../../images/dev-center/xbox-live-setup/xbox-live-setup-1.png)

## <a name="default-locale"></a>기본 로캘
이 옵션을 사용 하면 Xbox Live 서비스 구성에서 모든 문자열을 구성 하는 데 사용할 기본 언어를 설정할 수 있습니다. 예를 들어, 기본 로캘이 스페인어 (ES-ES)로 성과 구성 하려는 경우 다음에 최소한 성과 이름 및 설명을 것 스페인어에서 수입니다. 즉, 스페인어로이 옵션을 설정할 수 없지만 영어로 성과 달성 하셨습니다 정보만 제공 합니다. Xbox Live 서비스 구성의 모든 기본 로캘을와 동일한 버전에서 제공 되어야 합니다. 기본적으로 기본 로캘은 영어 (EN-US)로 설정 됩니다.
> [!NOTE]
> 또한 지역화 된 문자열 페이지에서 모든 문자열을 지역화할 수 있습니다.  

![파트너 센터에서 사용자 기본 로캘을 선택 하려면 드롭다운 목록 선택 이미지](../../images/dev-center/xbox-live-setup/xbox-live-setup-2.png)

## <a name="product-type"></a>제품 유형
드롭다운 메뉴 유형의 제품을 변경할 수 있습니다. 형식에 기본값으로 **게임**합니다. 선택한 항목에는 Xbox Live 기능 할 수 있는 영향을 줍니다. 선택 하는 세 가지 옵션이 있습니다.
1. 앱 
2. 게임 
3. 게임 데모 

![파트너 센터에서 제품 유형을 선택 하려면 드롭다운 목록 선택 이미지](../../images/dev-center/xbox-live-setup/xbox-live-setup-3.png)

## <a name="device-families"></a>디바이스 패밀리
이 구성을 사용 하는 제목 Xbox Live 액세스할 수는 장치의 종류를 선택할 수 있습니다. 기본적으로 모든 장치 패밀리에서 사용 됩니다. 사용 하도록 설정 하려면 장치를 확인할 수 있습니다.

![파트너 센터에서 장치 제품군을 선택 하려면 선택 확인란의 이미지](../../images/dev-center/xbox-live-setup/xbox-live-setup-4.png)

## <a name="embargo-date"></a>내려져 날짜
Xbox Live 구성에 공용 라이브 되 면 선택한 날짜가 결정 됩니다. 일반 정품으로 변경 내용을 게시 하는 경우에 이러한 다루지 것입니다 라이브 내려져 날짜에 도달 하지 않으면 하는 것이 반드시 합니다. 추가 설명:
1. 나중에 날짜를 선택 하면 변경 내용을 사용할 수 있게 됩니다 공용 해당 날짜에 있습니다.
2. 과거의 날짜를 선택 하는 경우 일반 정품으로 변경 내용을 게시 하는 즉시 변경 내용을 공개적으로 사용할 수 있습니다.

날짜 / 시간 선택기를 클릭 하 고 정확한 날짜 및 시간을 선택할 수 있도록 확장 됩니다. 클릭 하면 **확인**, 내려져 날짜 설정이 적용 됩니다.

![파트너 센터에서 내려져 날짜를 설정 하는 이미지](../../images/dev-center/xbox-live-setup/xbox-live-setup-5.png)

## <a name="advanced-settings"></a>고급 설정

클릭할 수 **Show options** 설정 하는 **여러 지점**합니다. 여러 지점에 로그인 할 Xbox Live 여러 장치에서 동시에 동일한 사용자를 수 있습니다. 기능 Xbox Live와 같은 성과 및 멀티 플레이어는 액세스가 제한 합니다. 따라서이 옵션은 게임에 대 한 권장 되지 않습니다. 상자를 확인 하 여이 옵션을 사용 합니다.

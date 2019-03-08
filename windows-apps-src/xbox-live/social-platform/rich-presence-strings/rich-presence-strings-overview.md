---
title: 다양한 상태
description: Xbox Live 상대의 어떻게 도움이 되는지 승격을 제목에 알아봅니다.
ms.assetid: 00042359-f877-4b26-9067-58834590b1dd
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 다기능 현재 상태, xbox
ms.localizationpriority: medium
ms.openlocfilehash: 0ae0d0189954ed8fa9a0bc7651d6a90b9789c388
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57604118"
---
# <a name="rich-presence"></a>다양한 상태

서식 있는 현재 상태를 사용 하 여 게임 플레이어를 수행 하는 지금 보급할 수 있습니다. 게임이 표시할 모든 게이머 게임의 플레이어의 상태와 같은 다양 한 현재 상태 문자열을 사용할 수는 예를 들어 *자리 비움*합니다. 다양 한 상태 정보 게이머 Xbox Live에 연결 되어 표시 됩니다. 이상적으로 대화 상대의 문자열로 플레이어를 수행 하는, 및는 게임에서 플레이어는 그렇게 다른 Xbox Live 게이머에 알려줍니다. 대화 상대의 문자열 개념이 같습니다 Xbox One에 Xbox 360에 있었지만 새 구현은 서비스 이니셔티브로 엔터테인먼트를 따릅니다. 이 섹션의에서 항목에서는 다양 한 현재 상태 문자열을 구성 하는 방법 및 다음 재생을 제목 사용자에 대 한 문자열을 설정 하는 방법을 설명 합니다.


## <a name="definitions"></a>정의

**열거형**  
열거형은 일부 게임 내 차원의 목록. 이러한 게임 내 차원의 예로 무기, 문자 클래스, 구조 및 등이 있습니다. 게임에서는 모든 가능한 문자 클래스 또는 구조 목록 가능한 무기의 목록을 보려면 등에 하려고 합니다.

**로캘 문자열 쌍**  
모든 사용할 수 있는 다양 한 현재 상태 문자열에는 어떤 로캘에서 문자열 수/사용 되도록 지정 하기 위해 연결 된 로캘이 있어야 합니다. 각 열거도 로캘 문자열 쌍의 집합을 갖게 됩니다.

**String-set**  
문자열 집합을 그룹 로캘 문자열 쌍으로 구성 됩니다. 이 설정 가능한 모든 로캘에 대 한 다양 한 현재 상태 문자열의 가능한 값 또는 모든 가능한 로캘에 대 한 열거형의 가능한 값을 정의합니다.

**친숙 한 이름**  
친숙 한 이름의 다음과 같은 두 종류가 있습니다.

**대화 상대의 문자열**  
문자열 집합에 대 한 이름에는 문자열 집합을 참조 하는 데 사용 하는 문자열 형식의 고유 식별자입니다.

**열거형**  
이러한 이름은 고유 하 게 식별 무기 열거형 또는 문자 클래스 열거와 같은 특정 열거형을 사용 됩니다.


## <a name="in-this-section"></a>이 섹션의 내용

[대화 상대의 구성](rich-presence-strings-configuration.md)  
제목에 사용 하기 위해 다양 한 현재 상태를 구성 하는 방법입니다.

[문자열을 업데이트 하는 다양 한 현재 상태](rich-presence-strings-updating-strings.md)  
제목에서 다양 한 현재 상태 문자열을 업데이트 하는 방법입니다.

[서식 있는 현재 상태에 대 한 유용한 정보](rich-presence-strings-best-practices.md)  
에 대 한 모범 사례에 제목에 다양 한 현재 상태를 사용합니다.

[서식 있는 현재 상태 정책 및 제한 사항](rich-presence-strings-policies-and-limitations.md)  
서식 있는 현재 상태를 사용 하 여 제목에 하는 방법에 대 한 정책입니다.

[대화 상대의 부록](rich-presence-strings-appendix.md)  
추가 예제 및 다양 한 현재 상태와 관련 된 데이터 플랫폼에 대 한 세부 정보입니다.

[프로그래밍 Xbox Live 서식 있는 현재 상태](programming-rich-presence.md)  
Xbox Live를 사용한 다양 한 현재 상태를 사용 하는 방법을 보여 줍니다.

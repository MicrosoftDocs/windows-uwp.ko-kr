---
title: 다양한 상태
author: KevinAsgari
description: 어떻게 Xbox Live 다양 한 상태 것을 돕는 타이틀 홍보에 대해 알아봅니다.
ms.assetid: 00042359-f877-4b26-9067-58834590b1dd
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, 풍부한 상태, xbox
ms.localizationpriority: medium
ms.openlocfilehash: 57ee9c43b4a78fc68f6da29721d38ac11c440624
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2018
ms.locfileid: "4430288"
---
# <a name="rich-presence"></a>다양한 상태

다양 한 상태를 사용 하 여 플레이어를 수행 하는 지금 바로 게임 알릴 수 있습니다. 예를 들어 게임 상태를 표시할 게이머 *비움*등 게임의 플레이어의 다양 한 상태 문자열을 사용할 수 있습니다. 다양 한 상태 정보는 Xbox Live에 연결 하는 게이머에 게 표시 합니다. 이상적으로 다양 한 상태 문자열 플레이어를 수행 하는, 여기서 게임에서 플레이어는 것과 다른 Xbox Live 게이머를 알려줍니다. 다양 한 상태 문자열의 개념은 Xbox One에서 동일 하 게 Xbox 360에 있지만 새 구현으로 서비스 이니셔티브 엔터테인먼트를 따릅니다. 이 섹션의에서 항목에서는 다양 한 상태 문자열을 구성 하는 방법 및 타이틀을 재생 하는 사용자에 대 한 문자열을 설정 하는 방법을 설명 합니다.


## <a name="definitions"></a>정의

**열거형**  
열거형에는 게임에서 차원의 일부 목록입니다. 이러한 게임에서 크기의 예로 무기, 문자 클래스, 지도, 등. 게임, 모든 가능한 문자 클래스 또는 지도, 목록에서에서 가능한 무기 목록을 보려면 등에 하려고 합니다.

**로캘 문자열 쌍**  
모든 가능한 다양 한 상태 문자열 연결 지역을 문자열 사용 수/해야 지정 된 로캘이 있어야 합니다. 각 열거형도 로캘 문자열 쌍도 집합이 있습니다.

**문자열 집합**  
문자열 집합 그룹 로캘 문자열 쌍으로 구성 됩니다. 이 설정 가능한 모든 로캘에 대 한 다양 한 상태 문자열의 가능한 값 또는 모든 가능한 로캘에 대 한 열거형에 대 한 가능한 값을 정의합니다.

**이름**  
두 종류의 식별 이름 가지 있습니다.

**다양 한 상태 문자열**  
설정 문자열에 대 한 이름을 문자열 집합을 참조 하는 데 사용 하는 문자열의 숫자 형태로 고유 식별자.

**열거형**  
이러한 이름은 고유 하 게 식별 무기 열거 또는 문자 클래스 열거와 같은 특정 열거형 사용 됩니다.


## <a name="in-this-section"></a>이 섹션 내용

[다양 한 상태 구성](rich-presence-strings-configuration.md)  
타이틀에서 사용 하기 위해 다양 한 상태를 구성 하는 방법입니다.

[다양 한 상태 문자열 업데이트](rich-presence-strings-updating-strings.md)  
타이틀에서 다양 한 상태 문자열을 업데이트 하는 방법입니다.

[다양 한 상태에 대 한 유용한 정보](rich-presence-strings-best-practices.md)  
모범 사례에 대 한 제목에 다양 한 상태를 활용합니다.

[다양 한 상태 정책 및 제한 사항](rich-presence-strings-policies-and-limitations.md)  
타이틀에서 다양 한 상태를 사용 하는 방법에 대 한 정책입니다.

[다양 한 상태 부록](rich-presence-strings-appendix.md)  
추가 예제와 관련 된 다양 한 상태 데이터 플랫폼에 대해 자세히 설명 합니다.

[프로그래밍 Xbox Live 다양 한 상태](programming-rich-presence.md)  
Xbox live 다양 한 상태를 사용 하는 방법을 보여 줍니다.
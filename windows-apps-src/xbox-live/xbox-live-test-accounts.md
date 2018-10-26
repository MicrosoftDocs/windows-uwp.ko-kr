---
title: Xbox Live 테스트 계정
author: KevinAsgari
description: 개발 중 테스트 계정을 사용 하 여 Xbox Live 테스트를 위해 게임을 만드는 방법을 알아봅니다.
ms.assetid: e8076233-c93c-4961-86ac-27ec74917ebc
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 테스트 계정, xbox
ms.localizationpriority: medium
ms.openlocfilehash: 2316a40e9524240b20645419ca8952d27374ca04
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5555024"
---
# <a name="xbox-live-test-accounts"></a>Xbox Live 테스트 계정

타이틀에서 개발 하는 동안 기능 테스트 하는 경우 추가 Xbox Live 계정을 만드는 것이 유용할 수 있습니다.  예를 들어 없는 도전 과제를 사용 하 여 새 계정을 좋습니다.  또는 여러 계정을 만들고 소셜 시나리오를 테스트 하는 것에 대 한 서로 친구 점에도 하는 것이 좋습니다.

한 번에 많은 테스트 계정을 만들려면 쉽게 제공 하므로 여러 Microsoft 계정 (MSA)를 만드는 데 시간이 오래 걸릴 수 있습니다.

테스트 계정에는 몇 가지 다른 이점을.  보안 제한으로 인해 일반 MSA 수 없습니다. 반면 *개발 샌드박스*를 서명할 수 있습니다.  어떤는 *개발 샌드박스* 를 모르는 경우 이면 하세요 [Xbox Live 샌드박스](xbox-live-sandboxes.md)

## <a name="types-of-test-accounts"></a>테스트 계정 유형

테스트 계정의 두 가지 옵션이 있습니다.  개발 샌드박스에서 작동 하도록는 프로 비전 하는 일반 msa Msa 또는 개발 샌드박스만 사용할 수 있는 테스트 계정.

크리에이터 스 프로그램 타이틀을 개발 하는 경우에 개발 샌드박스를 제공 하는 일반 msa Msa를 사용할 수 있습니다.

다음 두 형식을 만드는 방법에 대해 알아봅니다.

## <a name="provisioning-regular-msas"></a>일반 msa Msa를 프로 비전

기존 Xbox Live 계정이 있는 경우 시작점 개발 샌드박스를 사용 하 여 사용 하기 위해 프로 비전 하는 것입니다.

기존 Xbox Live 계정이 하지 않거나 추가 msa Msa를 필요로 하는 경우에 일부를 만들 수 있습니다 [https://account.microsoft.com/account](https://account.microsoft.com/account).

## <a name="creating-test-accounts"></a>테스트 계정 만들기

경우는 ID@Xbox 개발자는 다음에서 작성할 수도 있습니다 테스트 계정 사용을 위해 단독으로 개발 샌드박스 합니다.  한 번에 여러 테스트 계정을 만들 수도 있습니다.

개발자 센터에서 테스트 계정 관리 페이지로 이동 합니다.
1. 개발자 센터 대시보드로 이동
2. 맨 위에 있는 기어 아이콘을 클릭 계정 설정으로 이동 하려면 오른쪽
3. "테스트 계정"를 클릭 합니다.

이 제공 하는 곳을 보여 주는 스크린샷 아래를 참조 하세요

![](images/getting_started/devcenter_testaccount_nav.png)

"테스트 계정"을 클릭 하면 테스트 계정이 있는 경우 기존 요약을 표시 됩니다.  새 테스트 계정을 만들 수가 있습니다.

![](images/getting_started/devcenter_testaccount_summary.png)

"새 테스트 계정"을 클릭할 수 및 테스트 계정을 만드는 데 사용할 수 있는 형식으로 제공 됩니다.

![](images/getting_started/devcenter_testaccount_new.png)

모든 테스트 계정을 만든 개발 샌드박스 이름을 접두사가 및 자동 개발 샌드박스에 대 한 액세스를 가집니다.

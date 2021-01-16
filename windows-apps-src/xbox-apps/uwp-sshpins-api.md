---
title: 장치 포털 SSH pin API 참조
description: /Ext/app/sshpins Xbox Device Portal REST API를 사용 하 여 프로그래밍 방식으로 모든 SSH (신뢰할 수 있는 Secure Shell) pin을 제거 하는 방법을 알아봅니다.
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 999a77051b0eebe6ae5d8be9e4c2640709431b6d
ms.sourcegitcommit: b0a82c2a132212eb5fb72b67f0789cac1014642f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/16/2021
ms.locfileid: "98254208"
---
# <a name="ssh-pins-api-reference"></a>SSH Pin API 참조

이 REST API를 사용 하 여 devkit에서 모든 신뢰할 수 있는 SSH pin을 제거할 수 있습니다.

## <a name="remove-trusted-ssh-pins"></a>신뢰할 수 있는 SSH pin 제거

**요청**

| 방법 | 요청 URI |
|--------|-------------|
| Delete | /ext/app/sshpins |

**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**Response**

- 없음

**상태 코드**

이 API는 다음과 같은 예상 상태 코드를 포함 합니다.

| HTTP 상태 코드 | Description |
|------------------|-------------|
| 204 | Pin을 지우는 요청이 성공 했습니다. |
| 4XX | 오류 코드 |
| 5XX | 오류 코드 |

**사용 가능한 장치 패밀리**

* Windows Xbox

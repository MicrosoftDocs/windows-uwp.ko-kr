---
title: 오류 처리
author: KevinAsgari
description: Xbox Live 서비스 호출을 만들 때 오류를 처리 하는 방법을 알아봅니다.
ms.assetid: e433dfbd-488b-44ff-8333-1dcf0329cd60
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 오류, 서비스 호출
ms.localizationpriority: medium
ms.openlocfilehash: 1728af14afb0840975ad175608da3f0e9b03645d
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5705754"
---
# <a name="error-handling"></a>오류 처리

서비스 호출을 만들 때 개발자 특별 한 주의 수행 해야 합니다. 항상 타이틀은 네트워크 호출을 만들 때 오류를 처리 해야 합니다. 개발 하는 동안 계속 작업 서비스 호출에 대해서도 서비스 호출 실제 환경에서 여러 가지 다양 한 이유로 실패할 수 있습니다. 예를 들어 한 호출으로 인해 실패할 수 있습니다.

* 네트워크를 사용할 수 없는 경우
* 사용자가 너무 많아서 503 HTTP 상태 코드를 반환 합니다.
* 요청이 유효 하지 400 HTTP 상태 코드를 반환 합니다.
* 사용자가 없는 적절 한 권한이 403 HTTP 상태 코드를 반환 합니다.
* 사용자가 차단 된, 401 HTTP 상태 코드를 반환 합니다.
IXMLHTTPRequest2 WinRT 서비스 Api가 호출 하는 (예: ERROR_TIMEOUT, RPC_S_CALL_FAILED_DNE) 요청을 보낼 수 없는
* 위의 목록이 아니며 하지 않습니다. 대부분의 이러한 문제는 서비스 API 계층에서 예외가 발생 됩니다. 타이틀 이러한 예외 캡처하고 적절히 처리 해야 합니다.

여기의 오류 처리에서 Xbox 서비스 API (XSAPI) API에 따라 두 가지 스타일 사용 하는:

[C + + API 오류 처리](error-handling-cpp.md)를 참조 하세요.

[WinRT API 오류 처리](error-handling-winrt.md)를 참조 하세요.

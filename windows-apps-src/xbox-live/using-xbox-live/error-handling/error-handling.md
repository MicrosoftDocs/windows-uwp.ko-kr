---
title: 오류 처리
description: Xbox Live 서비스 호출을 수행할 때 오류를 처리 하는 방법을 알아봅니다.
ms.assetid: e433dfbd-488b-44ff-8333-1dcf0329cd60
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox 하나, 오류, 서비스 호출
ms.localizationpriority: medium
ms.openlocfilehash: 637b9da84ef3ae50ea6235ca86e6318ae62acd11
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57637008"
---
# <a name="error-handling"></a>오류 처리

개발자는 서비스 호출을 수행할 때 특별 한 주의 수행 해야 합니다. 제목 네트워크 호출을 수행할 때 항상 오류를 처리 해야 합니다. 개발 하는 동안 지속적으로 노력 하는 서비스 호출에 대해서도 여러 가지 다른 이유로 실제 환경에서 서비스 호출이 실패할 수 있습니다. 예를 들어 호출으로 인해 실패할 수 있습니다.

* 네트워크를 사용할 수 없는 경우
* 서비스 사용량이 많아서 503 HTTP 상태 코드를 반환 합니다.
* 요청이 올바르지 400 HTTP 상태 코드를 반환 합니다.
* 사용자 없는 적절 한 권한이 403 HTTP 상태 코드를 반환 합니다.
* 사용자 금지 되어, 401 HTTP 상태 코드를 반환 합니다.
IXMLHTTPRequest2 WinRT 서비스 Api에서 호출 되는 (예: ERROR_TIMEOUT, RPC_S_CALL_FAILED_DNE) 요청을 보낼 수 없는 경우
* 위의 목록은 완전 하지 않습니다. 이러한 문제의 대부분은 서비스 API 계층에서 예외가 발생 됩니다. 제목 이러한 예외를 캡처합니다 하 고이 적절히 처리 해야 합니다.

있는 오류 처리에서 Xbox 서비스 API (XSAPI) API에 따라 두 가지 다른 스타일을 사용 하는:

참조 [c + + API 오류 처리](error-handling-cpp.md)합니다.

참조 [WinRT API 오류 처리](error-handling-winrt.md)합니다.

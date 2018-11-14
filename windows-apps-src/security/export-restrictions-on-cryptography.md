---
title: 암호화에 대한 내보내기 제한
description: 이 정보를 사용하여 앱이 Microsoft Store에 나열될 수 없는 방식으로 암호화를 사용하는지 확인할 수 있습니다.
ms.assetid: 204C7D1D-6F08-4AEE-A333-434D715E7617
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 보안
ms.localizationpriority: medium
ms.openlocfilehash: a29c4aeb5a5928e04e0018d68884fdb4a4876332
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6662746"
---
# <a name="export-restrictions-on-cryptography"></a>암호화에 대한 내보내기 제한



이 정보를 사용하여 앱이 Microsoft Store에 나열될 수 없는 방식으로 암호화를 사용하는지 확인할 수 있습니다.

미국 상무부의 BIS(Bureau of Industry and Security)에서는 특정 유형의 암호화를 사용하는 기술의 수출을 규제합니다. 앱 파일은 미국에 저장되므로 Microsoft Store에 나열된 모든 앱은 이러한 법과 규정을 따라야 합니다. 미국 외부에 배포하기 위해 다른 나라에서 앱 개발자가 업로드한 앱도 이러한 규정을 따라야 합니다. 따라서 모든 앱 개발자는 Microsoft Store에 앱을 제출할 때 이러한 규정에 따라 제한되는 기술이 앱에 포함되지 않도록 해야 합니다.

> **참고**여기에 제공 된 정보는 일부 지침을 제공 하지만 앱 모든 관련 법률 및 규정을 준수 하는지 확인 하려면 Microsoft Store에 앱을 게시 하는 앱 개발자의 책임입니다.

 

미국 상무부 및 BIS(Bureau of Industry and Security)에 대한 자세한 내용은 [Bureau of Industry and Security 정보](http://go.microsoft.com/fwlink/p/?LinkID=245644)를 참조하세요.

암호화가 포함된 기술의 수출을 규제하는 EAR(수출 관리 규정)에 대한 자세한 내용은 [암호화를 사용하는 항목에 대한 EAR 통제](http://go.microsoft.com/fwlink/p/?LinkID=245645)를 참조하세요.

## <a name="governed-uses"></a>규제된 사용

먼저 앱이 EAR(수출 관리 규정)에서 통제되는 암호화 유형을 사용하는지 확인합니다. 이 질문에는 아래 목록에 표시된 예가 포함되지만 가능한 모든 암호화 응용이 목록에 포함되어 있는 것은 아닙니다.

> **중요 한**앱을 하지만 모든 소프트웨어 라이브러리, 유틸리티 및 앱을 포함 하거나 연결 하는 운영 체제 구성 요소에 대해 작성 한 코드 뿐 아니라는 것이 좋습니다.

-   디지털 서명 사용(예: 인증 또는 무결성 검사)
-   앱이 사용하거나 액세스하는 데이터 또는 파일 암호화
-   키 관리, 인증서 관리 또는 공개 키 인프라와 상호 작용하는 모든 항목
-   보안 통신 채널 사용(예: NTLM, Kerberos, SSL(Secure Sockets Layer) 또는 TLS(전송 계층 보안))
-   암호 또는 다른 형태의 정보 보안 암호화
-   복사 방지 또는 DRM(디지털 권한 관리)
-   바이러스 백신 보호

암호화 응용 프로그램의 최신 전체 목록은 [EAR Controls for Items That Use Encryption](http://go.microsoft.com/fwlink/p/?LinkID=245645)을 참조하세요.

## <a name="non-restricted-uses"></a>비 제한 사용

일부 암호화 응용 프로그램은 제한되지 않습니다. 제한되지 않는 작업은 다음과 같습니다.

-   암호의 암호화
-   복사 방지
-   인증
-   디지털 권한 관리
-   디지털 서명 사용

암호화 응용 프로그램의 최신 전체 목록은 [EAR Controls for Items That Use Encryption](http://go.microsoft.com/fwlink/p/?LinkID=245645)을 참조하세요.

앱이 이 목록에 없는 작업에 대해 암호화를 호출, 지원, 포함 또는 사용하는 경우 앱에 ECCN(Export Commodity Classification Number)이 필요합니다.

ECCN이 없는 경우 [ECCN Questions and Answers](http://go.microsoft.com/fwlink/p/?LinkID=245646)를 참조하세요.

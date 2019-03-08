---
title: Xbox Live Api를 사용 하는 XDK에 기본 제공
description: Xbox 개발자 키트 (XDK) 프로젝트의 기본 제공 Xbox Live Api를 사용 하는 방법에 알아봅니다.
ms.assetid: 539caca3-58bc-49d9-8432-ca8e57755be2
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c762dd8abbbc80948d232610e4123b6e4893936d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598408"
---
# <a name="using-xbox-live-apis-built-into-the-xdk"></a>Xbox Live Api를 사용 하는 XDK에 기본 제공

1. Visual Studio에서 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 "참조"를 선택 합니다.
1. "새 참조 추가" 선택
1. "Durango 클릭. <build number>" 왼쪽된 패널에서 "확장" 하 고
1. 중간에 하나를 선택 합니다.
- WinRT XSAPI API를 사용 하려는 경우 "Xbox 서비스 API" 선택
- C + + XSAPI API를 사용 하려는 경우 "Xbox 서비스 API Cpp" 선택
1. 확인을 클릭합니다.

참고: 전처리기 정 및 라이브러리에 표시 된 대로 수동으로 추가 해야 빌드 시스템 props 파일을 지원 하지 않으면 `%XboxOneExtensionSDKLatest%\ExtensionSDKs\Xbox.Services.API.Cpp\8.0\DesignTime\CommonConfiguration\Neutral\Xbox.Services.API.Cpp.props`

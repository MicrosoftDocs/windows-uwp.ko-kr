---
title: Xbox Live Api를 사용 하 여 XDK에 기본 제공
author: KevinAsgari
description: Xbox 개발자 키트 (XDK) 프로젝트에서 기본 제공 Xbox Live Api를 사용 하는 방법을 알아봅니다.
ms.assetid: 539caca3-58bc-49d9-8432-ca8e57755be2
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: e127dd2adb53e8fdf5fb0469ce9deae5759eb899
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/23/2018
ms.locfileid: "7552900"
---
# <a name="using-xbox-live-apis-built-into-the-xdk"></a>Xbox Live Api를 사용 하 여 XDK에 기본 제공

1. Visual Studio에서 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 "참조"를 선택 합니다.
1. "새 참조 추가"를 선택 합니다.
1. "Durango 클릭. <build number>" 왼쪽된 창에서 "확장" 및
1. 중간에 중 하나를 선택 합니다.
- XSAPI WinRT API를 사용 하려는 경우 "Xbox 서비스 API"을 선택 합니다.
- C + + XSAPI API를 사용 하려는 경우 "Xbox 서비스 API Cpp"을 선택 합니다.
1. 확인을 클릭 합니다.

참고: 빌드 시스템에서 속성 파일을 지원 하지 않으면 수동으로 추가 해야 전처리기 정 및 라이브러리에 표시 된 대로
`%XboxOneExtensionSDKLatest%\ExtensionSDKs\Xbox.Services.API.Cpp\8.0\DesignTime\CommonConfiguration\Neutral\Xbox.Services.API.Cpp.props`

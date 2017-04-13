---
author: drewbat
ms.assetid: 
title: "시험판 API를 사용한 UWP 앱 개발"
description: "UWP SDK 미리 보기에 포함된 시험판 API 사용에 따른 장점과 주의 사항을 이해합니다."
ms.openlocfilehash: ede7e8d5e9cce39850edbdb70a715d76c78f0c01
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="developing-uwp-apps-with-pre-release-apis"></a>시험판 API를 사용한 UWP 앱 개발

Microsoft는 새로운 플랫폼 기능이 완성되기 전에 개발자가 이를 사용해 볼 수 있도록 UWP(유니버설 Windows 플랫폼)의 미리 보기 릴리스를 게시했습니다. 이는 기능을 앱에 통합하고 공식 RTM 버전의 SDK가 릴리스된 후 신속하게 앱을 게시할 수 있는 시작점이 될 것입니다. 또한 시험판 API를 사용해 보고 플랫폼의 향후 릴리스 방향에 영향을 주는 피드백을 Microsoft에 제공할 수 있습니다.

## <a name="important-limitations-on-the-use-of-pre-release-apis"></a>시험판 API 사용에 대한 중요 제한 사항
앱에서 시험판 API를 사용하기 전에, 사용으로 인한 다음과 같은 중요 영향을 고려하십시오. 
* 시험판 API를 사용하는 앱은 API가 RTM 릴리스로 공식 게시될 때까지 Windows 스토어에 제출할 수 없습니다. 현재 게시된 앱의 코드와 시험판 개발 코드를 분리하는 것을 적극 권장합니다. 
* 미리 보기 SDK를 사용하여 개발한 앱은 어떤 시험한 API도 사용하지 않았더라도 스토어에 제출할 수 없습니다. 이전 개발에 사용한 프로덕션 컴퓨터와는 별개의 컴퓨터나 VM에 미리 보기 도구를 설치해야 합니다. 
* 시험판 API는 RTM 전에 변경될 수 있습니다. 미리 보기 SDK에 포함된 API는 최종 SDK에 포함되거나 사용할 수 있도록 설정될 가능성이 높습니다. 하지만 API의 이름, 서명 및 특정 동작은 최종 릴리스 전에 변경될 수 있으며, API가 완전히 제거될 수도 있습니다. 

## <a name="how-to-identify-a-prerelease-api"></a>시험판 API를 식별하는 방법 
시험판 API는 UWP의 API 참조 설명서에 다음 텍스트로 레이블이 지정됩니다. 

이 문서에서는 상업적으로 출시하기 전에 크게 수정될 수 있는 시험판 API 또는 기능에 대해 설명합니다. 지금 바로 이 기능을 개발과 테스트에 사용할 수 있지만, 이를 사용한 앱은 기능이 상업적으로 릴리스되기 전에 Windows 스토어에 제출할 수 없습니다. Microsoft는 여기에 제공된 정보에 대해 명시적 또는 묵시적 보증을 하지 않습니다. 시험판 API를 사용한 개발에 대한 자세한 내용은 시험판 API를 사용한 UWP 앱 개발을 참조하세요. 

## <a name="get-the-latest-sdk-insider-preview"></a>최신 SDK 참가자 미리 보기 다운로드 
1. SDK의 미리 보기 빌드에 액세스하려면 [Windows 참가자 프로그램에 등록](https://insider.windows.com/)하세요. 
3. 개발자 미리 보기 도구를 설치하기 전에 [현재 SDK 및 모바일 에뮬레이터의 릴리스 정보](http://go.microsoft.com/fwlink/?LinkId=829180)를 검토해 보세요.
4. [SDK 참가자 미리 보기](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewSDK)를 설치합니다.
5. [Windows Insider Preview 커뮤니티 포럼](http://go.microsoft.com/fwlink/p/?LinkId=507620)을 확인합니다.

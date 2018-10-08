---
title: Xbox Live 크리에이터 스에 대 한 단계별 지침
author: KevinAsgari
description: 크리에이터 스 프로그램에 대 한 Xbox Live를 통합 하는 단계에 대 한 지침을 제공 합니다.
ms.assetid: 7f9d093e-479a-4ad4-9965-a4ea6f0b2ac3
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 크리에이터 스
ms.localizationpriority: medium
ms.openlocfilehash: bca840455af68b8584901476de2e8e26973b9b0f
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/08/2018
ms.locfileid: "4416383"
---
# <a name="step-by-step-guide-to-integrate-xbox-live-creators-program"></a>Xbox Live 크리에이터스 프로그램을 통합하는 방법에 대한 단계별 가이드

이 섹션은 Xbox live 타이틀 시작 및 실행 하는 데 도움이 됩니다.

## <a name="1-ensure-you-have-a-title-created-on-dev-center"></a>1. 개발자 센터에서 만든 타이틀이 있는지 확인
모든 Xbox Live 타이틀은 로그인하고 Xbox Live 서비스 전화를 걸기 전에 개발자 센터에서 정의되어야 합니다.  [새 크리에이터스 타이틀 만들기](create-and-test-a-new-creators-title.md)에서 이 작업을 수행하는 방법을 설명합니다.

## <a name="2-follow-the-appropriate-guide-to-setup-your-ide-or-game-engine"></a>2. IDE 또는 게임 엔진 설정 적절 한 가이드를 따라
플랫폼 및 엔진에 대 한 적절 한 시작 가이드를 따라 하 고 진행 하면서 Xbox Live의 기본 사항을 알아볼 수 있습니다.

* [Visual Studio로 크리에이터스 타이틀 개발](develop-creators-title-with-visual-studio.md)은 Visual Studio 프로젝트를 개발자 센터의 Xbox Live 구성과 연결하는 방법을 설명합니다.

* [Unity로 크리에이터 스 타이틀 개발](develop-creators-title-with-unity.md) 하면 새 Xbox Live를 만들려면 Unity 제목을 사용 하는 방법 제목, 순위표 등의 기능을 추가 하 고 기본 Visual Studio 프로젝트를 생성 표시 됩니다.

## <a name="3-xbox-live-concepts"></a>3. Xbox Live 개념
타이틀을 만들었으면 타이틀 개발 경험에 영향을 주는 Xbox Live 개념에 대해 알아 두어야 합니다.

- [Xbox Live 테스트 환경](../xbox-live-sandboxes.md)
- [Xbox Live 계정에 권한을 부여](authorize-xbox-live-accounts.md)

## <a name="4-add-xbox-live-features"></a>4. Xbox Live 기능을 추가 합니다.

게임에 Xbox Live 기능을 추가 합니다.

- [Xbox Live 소셜 플랫폼 - 프로필, 친구, 상태](../social-platform/social-platform.md)
- [Xbox Live 데이터 플랫폼-통계, 순위표, 도전 과제](../data-platform/data-platform.md)
- [Xbox Live 저장소 플랫폼-연결 된 저장소, 타이틀 저장소](../storage-platform/storage-platform.md)

## <a name="5-release-your-title"></a>5. 타이틀을 릴리스 합니다.

Xbox Live 크리에이터 스 프로그램을 사용 하는 경우이 프로세스는 다른 UWP를 해제 하면 차이가 없습니다.  프로세스에 대 한 자세한 내용은 [Windows 앱 게시](https://developer.microsoft.com/en-us/store/publish-apps)를 볼 수 있습니다.

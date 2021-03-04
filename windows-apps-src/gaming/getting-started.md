---
title: 시작
description: 이 빠른 시작 가이드에 따라 Windows 또는 Xbox 용 게임 개발을 바로 시작 하는 방법을 알아봅니다.
ms.assetid: 40490837-6c7f-4f82-96b5-14f6858982b3
ms.date: 01/25/2018
ms.topic: article
keywords: windows 10, uwp, 게임, 시작
localizationpriority: medium
ms.openlocfilehash: 6927bc6910d95a0d7499d6706e1f92c1f379b2b8
ms.sourcegitcommit: 4ea59d5d18f79800410e1ebde28f97dd5e45eb26
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101824097"
---
# <a name="getting-started"></a>시작

이 문서는 Windows 또는 Xbox에서 게임을 개발 하려는 작성자를 위한 시작 가이드입니다. 

필요한 정보를 찾는 데 도움이 되는 몇 가지 질문은 다음과 같습니다.
* 게임 개발자에 게 모든 세부 정보가 필요 한가요? [Windows 10 게임 개발 가이드](e2e.md)를 참조 하세요.
* 코딩을 완전히 처음 접하는 가요? [코드 자습서의 Minecraft 시간](https://code.org/minecraft) 처럼 흥미로운 점이 있을 수 있습니다.
* 멋진 게임을 찾고 있나요? [Microsoft Store](https://www.microsoft.com/store)를 확인 하세요.
* 멋진 Windows 또는 Xbox 게임 개발을 시작할 준비가 되셨습니까?  올바른 장소에 있습니다.

## <a name="quick-start-guide"></a>빠른 시작 가이드

게임을 바로 개발 하기 위한 단계입니다.

### <a name="step-1-get-the-software-and-tools"></a>1 단계: 소프트웨어 및 도구 다운로드

장치에 Windows 10이 설치 되어 있고 최신 업데이트가 설치 되어 있는지 확인 합니다.

Visual Studio와 같은 적합 한 IDE를 설치 합니다. Visual Studio Community 2017은 무료로 다운로드할 수 있습니다. 자세한 내용은 [Visual Studio 다운로드](https://visualstudio.microsoft.com/downloads/)를 참조 하세요.

게임 엔진과 기타 미들웨어를 사용 하려는 경우 [Windows 10 게임 개발 가이드](e2e.md)의 [브리지, 게임 엔진 및 미들웨어](e2e.md#bridges-game-engines-and-middleware) 섹션을 참조 하세요. 특정 게임 엔진을 사용 하 여 Windows 및 Xbox 게임을 개발 하는 방법에 대 한 자세한 내용은 게임 엔진의 설명서로 이동 해야 합니다.

### <a name="step-2-prepare-your-hardware-for-development"></a>2 단계: 개발을 위한 하드웨어 준비

처음으로 개발 하는 경우 장치에서 개발자 모드를 사용 하도록 설정 해야 합니다. 자세한 내용은 [개발에 디바이스 사용](/windows/apps/get-started/enable-your-device-for-development)을 참조하세요.

소매 Xbox 본체를 사용 하 여 Xbox 게임을 개발 하려는 사용자의 경우 개발자 모드를 활성화 하 고 활성화 해야 합니다. 자세한 내용은 xbox [One Developer 모드 정품 인증](../xbox-apps/devkit-activation.md) 및 [xbox에서 UWP 앱 개발 시작](../xbox-apps/getting-started.md)을 참조 하세요. 

> [!Note]
> Xbox 본체에서 개발자 모드를 사용 하도록 설정 하려면 먼저 [파트너 센터](https://partner.microsoft.com/dashboard)  계정에 등록 해야 합니다. 파트너 센터 계정에 등록 하는 방법에 대 한 자세한 내용은 아래의 [5 단계](#step-5-sign-up-for-a-partner-center-account) 를 참조 하세요.

### <a name="step-3-run-a-sample-and-see-how-it-works"></a>3 단계: 샘플 실행 및 작동 방식 확인

UWP DirectX 개발을 시작 하려면 [directx를 사용 하 여 간단한 UWP 게임 만들기](tutorial--create-your-first-uwp-directx-game.md)를 참조 하세요. 버퍼와 같은 DirectX 개념을 읽고 숙지 하려면 [Direct3D 그래픽 개념](../graphics-concepts/index.md)을 참조 하세요.

더 많은 샘플을 보려면 [Game samples](e2e.md#game-samples)를 참조 하십시오.

### <a name="step-4-consider-joining-a-program"></a>4 단계: 프로그램 참여 고려

Xbox 게임을 개발 하거나 게임에서 Xbox Live 기능을 사용 하려는 경우 [Xbox Live 크리에이터 프로그램](https://developer.microsoft.com/games/xbox/xboxlive/creator) 또는 프로그램 중 하나를 참여 [ID@Xbox](https://www.xbox.com/Developers/id) 하세요. 

각 프로그램에서 사용할 수 있는 Xbox Live 기능에 대해 자세히 알아보려면 [기능 표](/gaming/xbox-live/get-started/join-dev-program/live-feature-comparison-table)를 참조 하세요. 자세한 내용은 [개발자 프로그램](e2e.md#developer-programs)을 참조 하세요.

> [!Note]
> Xbox Live 크리에이터 프로그램은 모든 개발자가 사용할 수 있습니다. **누구나** Xbox 게임을 게시할 수 있습니다. Xbox Live 크리에이터 프로그램의 제목을 만들려면 파트너 센터에서이 옵션을 사용 하도록 설정 하기만 하면 됩니다. 파트너 센터 계정에 등록 하는 방법에 대 한 자세한 내용은 아래의 [5 단계](#step-5-sign-up-for-a-partner-center-account) 를 참조 하세요.

### <a name="step-5-sign-up-for-a-partner-center-account"></a>5 단계: 파트너 센터 계정에 등록

파트너 센터 계정은 [파트너 센터](https://partner.microsoft.com/dashboard)에 대 한 액세스를 제공 하 여 모든 Windows 장치용 앱과 게임을 한 곳에서 관리 하 고 제출할 수 있습니다.

Windows 게임 개발의 경우 파트너 센터에 액세스할 수 있을 때까지 기다리거나, 게임에서 Xbox Live 기능을 사용 하려는 경우에만 대기 하도록 선택할 수 있습니다.

Xbox 게임 개발의 경우 파트너 센터 계정에 등록 해야 합니다 .이는 개발을 위해 소매점 Xbox를 설정 하는 데 필요 하기 때문입니다. 자세한 내용은 [2 단계](#step-2-prepare-your-hardware-for-development) 를 참조 하세요.

자세한 내용은 [Windows 앱 및 게임 게시](../publish/index.md)를 참조하세요.

## <a name="useful-links"></a>유용한 링크

* [Windows 10 게임 개발 가이드](e2e.md)
* [UWP 앱이란?](../get-started/universal-application-platform-guide.md)
* [UWP 게임에 클라우드 서비스 사용](cloud-for-games.md)
* [게임에 액세스할 수 있도록 설정](accessibility-for-games.md)

---
title: 시작
description: 게임 개발 학습 가이드
ms.assetid: 40490837-6c7f-4f82-96b5-14f6858982b3
ms.date: 01/25/2018
ms.topic: article
keywords: windows 10, uwp, 게임을 시작 하기
localizationpriority: medium
ms.openlocfilehash: 596d0b1eb371fec98825b23a214683421e388506
ms.sourcegitcommit: e63fbd7a63a7e8c03c52f4219f34513f4b2bb411
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58162658"
---
# <a name="getting-started"></a>시작

이 문서는 시작 Windows 또는 Xbox 용 게임 개발 하려는 작성자 용 가이드입니다. 

다음은 필요한 정보를 찾을 수 있도록 몇 가지 질문입니다.
* 모든 세부 정보를 들고 숙련된 된 게임 개발자 이십니까? 참조 된 [Windows 10 게임 개발 가이드](e2e.md)합니다.
* 초보 코딩? 이와 같은 재미 합니다 [Minecraft 시간의 코드 자습서](https://code.org/minecraft) 유용할 수 있습니다.
* 방금 게임 플레이를 찾으시나요? 체크 아웃 합니다 [Microsoft Store](https://www.microsoft.com/store)합니다.
* Windows 또는 Xbox 용 게임 개발을 시작할 준비가 되셨습니까?  적절 한 위치에 완료 되었습니다!

## <a name="quick-start-guide"></a>빠른 시작 가이드

지금 바로 게임 개발에 대 한 단계입니다.

### <a name="step-1-get-the-software-and-tools"></a>1단계: 소프트웨어 및 도구

Windows 10 장치에 설치 하 고 최신 업데이트가 설치 되어 있습니다.

Visual Studio와 같은 적합 한 IDE를 설치 합니다. Visual Studio Community 2017는 무료로 다운로드할 수 있습니다. 자세한 내용은 [Visual Studio 다운로드](https://www.visualstudio.com/downloads/)합니다.

게임 엔진 및 다른 미들웨어를 사용 하려는 경우 참조 [브리지, 게임 엔진 및 미들웨어](e2e.md#bridges-game-engines-and-middleware) 섹션을 [Windows 10 게임 개발 가이드](e2e.md)합니다. 특정 게임 엔진을 사용 하 여 Windows 및 Xbox 게임 개발에 대 한 내용은 게임 엔진 설명서로 이동 해야 합니다.

### <a name="step-2-prepare-your-hardware-for-development"></a>2단계: 하드웨어 개발을 위한 준비

처음으로 개발을 수행 하는 경우 장치에서 개발자 모드가 사용 하도록 설정 해야 합니다. 자세한 내용은 [개발용 장치를 사용 하도록 설정](../get-started/enable-your-device-for-development.md)합니다.

사용자에 게 소매 Xbox 본체를 사용 하 여 Xbox 게임을 개발 하려는는 활성화 하 고 개발자 모드가 사용 하도록 설정에 해야 합니다. 자세한 내용은 [Xbox 하나 개발자 모드가 활성화](../xbox-apps/devkit-activation.md) 하 고 [Xbox에서 UWP 앱 개발을 시작 하기](../xbox-apps/getting-started.md)합니다. 

> [!Note]
> 에 등록 해야 합니다는 [파트너 센터](https://partner.microsoft.com/dashboard) 계정을 Xbox 본체에서 개발자 모드를 사용할 수 있습니다. 파트너 센터 계정에 등록 하는 방법에 대 한 자세한 내용은 참조 하세요. [5 단계](#step-5-sign-up-for-a-partner-center-account) 아래.

### <a name="step-3-run-a-sample-and-see-how-it-works"></a>3단계: 샘플을 실행 하 고 어떻게 작동 하는지 확인

UWP DirectX 개발 시작을 참조 하세요 [DirectX를 사용 하 여 간단한 UWP 게임을 만들기](tutorial--create-your-first-uwp-directx-game.md)합니다. 단순히를 읽고 어떤 버퍼는 DirectX 개념을 익히는 참조 하려는 경우 [Direct3D 그래픽 개념](../graphics-concepts/index.md)합니다.

추가 샘플을 참조 하세요 [샘플 게임](e2e.md#game-samples)합니다.

### <a name="step-4-consider-joining-a-program"></a>4단계: 프로그램을 조인 하는 것이 좋습니다.

Xbox 게임을 개발 하거나 게임에 Xbox Live 기능을 사용 하려는 경우 조인 하거나 합니다 [Xbox Live 크리에이터 스 프로그램](https://developer.microsoft.com/games/xbox/xboxlive/creator) 하거나 [ ID@Xbox ](https://www.xbox.com/Developers/id) 프로그램입니다. 

각 프로그램에 사용할 수 있는 Xbox Live 기능에 대 한 자세한 내용은 참조 하세요 [기능 테이블](https://docs.microsoft.com/gaming/xbox-live//developer-program-overview.md#feature-table)합니다. 자세한 내용은 [개발자 프로그램](e2e.md#developer-programs)합니다.

> [!Note]
> Xbox Live 크리에이터 스 프로그램은 모든 개발자에 게 제공 합니다. **누구나** Xbox 게임을 게시할 수 있습니다. 제목 부분은 Xbox Live 크리에이터 스 프로그램을 만들려면 파트너 센터에서이 옵션을 사용 하도록 설정 하기만 하면 됩니다. 파트너 센터 계정에 등록 하는 방법에 대 한 자세한 내용은 참조 하세요. [5 단계](#step-5-sign-up-for-a-partner-center-account) 아래.

### <a name="step-5-sign-up-for-a-partner-center-account"></a>5단계: 파트너 센터 계정에 등록

파트너 센터 계정에 액세스할 수 있습니다 [파트너 센터](https://partner.microsoft.com/dashboard), 관리 및 제출 하는 앱 및 게임 한곳에서 Windows 장치에 대 한 모든 수 있습니다.

Windows 게임 개발에 대 한 파트너 센터 또는 게임에 Xbox Live 기능을 사용 하려는 경우 액세스 하려는 때까지 대기할 수도 있습니다.

Xbox 게임 개발을 위해 등록 해야 파트너 센터 계정에 대 한 개발을 위한 retail Xbox 프로그램을 설정 하는 데 필요한 대로 합니다. 참조 [2 단계](#step-2-prepare-your-hardware-for-development) 세부 정보에 대 한 합니다.

자세한 내용은 [게시할 Windows 앱과 게임](../publish/index.md)합니다.

## <a name="useful-links"></a>유용한 링크

* [Windows 10 게임 개발 가이드](e2e.md)
* [UWP 앱이란 무엇인가요?](../get-started/universal-application-platform-guide.md)
* [UWP 게임을 위한 클라우드 서비스를 사용 하 여](cloud-for-games.md)
* [내게 필요한 옵션이 게임](accessibility-for-games.md)
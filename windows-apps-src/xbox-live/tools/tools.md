---
title: Xbox Live에 대 한 개발 도구
description: 개발 및 Xbox Live를 테스트 하는 데 제목 사용을 제공 하는 도구에 알아봅니다.
ms.assetid: 380a29bf-41a7-4817-9c57-f48f2b824b52
ms.date: 6/13/2018
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 도구, 플레이어 재설정, 추적 분석기 LTA, xbox live 계정 도구인 라이브
ms.localizationpriority: medium
ms.openlocfilehash: ad43ecbd3bfd266d4a237253380bca223302fe54
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623518"
---
# <a name="development-tools-for-xbox-live"></a>Xbox Live에 대 한 개발 도구

이 섹션에서는 Xbox live 개발 하는 데 사용할 수 있는 다양 한 도구에 설명 합니다. 사용할 수 있는 다양 한 도구를 [Xbox Live 개발자 도구 GitHub](https://github.com/Microsoft/xbox-live-developer-tools) 리포지토리. 사용할 수도 있습니다는 [개발 도구 라이브러리](https://www.nuget.org/packages/Microsoft.Xbox.Services.DevTools) 사용자 고유의 사용자 지정 도구를 만들려고 합니다. 모든 독립 실행형 개발자 도구에서 다운로드할 수 있습니다 [ https://aka.ms/xboxliveuwptools ](https://aka.ms/xboxliveuwptools)합니다.

> [!NOTE]
> 관리 되는 파트너가 다운로드에 포함 된 MatchSim 및 XboxLiveCompute 도구만 사용할 수 있습니다 또는 파트너에 등록 합니다 [ ID@Xbox ](https://www.xbox.com/Developers/id) 프로그램입니다. 사용 가능한 개발자 프로그램에 대 한 자세한 내용은를 참조 하십시오 합니다 [개발자 프로그램 개요](https://docs.microsoft.com/windows/uwp/xbox-live/developer-program-overview)합니다. 

## <a name="global-storage"></a>전역 저장소
전역 제목 저장소 명단, 맵, 문제, 또는 최신 리소스와 같은 모든 사용자가 읽을 수 있는 데이터를 저장할 사용 됩니다. 형식인 [Title 저장소](../storage-platform/xbox-live-title-storage/xbox-live-title-storage.md)합니다. 전역 저장소 도구는 전역 제목 저장소 테스트 샌드박스를 관리 하에 사용 됩니다. 데이터에서 파트너 센터 또는 Xbox 개발자 포털 (XDP)을 통해 일반 정품 버전 여전히 게시 되어야 합니다. 이 도구는 명령줄을 통해 사용할 수 있는의 일부로 합니다 [개발 도구](https://aka.ms/xboxliveuwptools) zip입니다. 사용자 지정 도구를 사용 하 여 만들 수 있습니다 합니다 [개발 도구 라이브러리](https://www.nuget.org/packages/Microsoft.Xbox.Services.DevTools)합니다.

## <a name="multiplayer-session-history-viewer"></a>멀티 플레이 세션 기록 뷰어
다중 접속 세션 기록 뷰어 멀티 플레이 세션 문서의 기록은 삭제 된 문서 등 모든 변경 내용을 기록 타임 라인을 볼 수가 있습니다. 이 도구를 사용 하 고 시간이 지남에 따라 변경 될 때 MPSD 세션 문서를 사용 하 여 상황을 깊이 있는 이해가 제공 됩니다. 독립 실행형 도구로 사용할 수는 [개발 도구](https://aka.ms/xboxliveuwptools) zip입니다.

## <a name="player-data-reset"></a>플레이어 데이터 다시 설정
테스트 샌드박스의 플레이어의 데이터를 다시 설정 하려면 플레이어 데이터 다시 설정 도구를 사용할 수 있습니다. 와 같은 데이터를 다시 설정할 수 있습니다. 도전 과제, 순위표, 통계 및 제목 기록 합니다. 이 도구는 명령줄을 통해 사용할 수 있는의 일부로 합니다 [개발 도구](https://aka.ms/xboxliveuwptools) zip입니다. 사용자 지정 도구를 사용 하 여 만들 수 있습니다 합니다 [개발 도구 라이브러리](https://www.nuget.org/packages/Microsoft.Xbox.Services.DevTools)합니다.

## <a name="xbox-live-developer-account"></a>Xbox Live 개발자 계정
Xbox Live 개발자 계정 도구는 개발자 계정의 인증을 관리 하려면 사용 됩니다. 플레이어 다시 설정 하 고 글로벌 저장소와 같은 개발자 자격 증명을 필요로 하는 다른 개발자 도구와 상호 작용에 필요 합니다. 이 도구는 명령줄을 통해 사용할 수 있는의 일부로 합니다 [개발 도구](https://aka.ms/xboxliveuwptools) zip입니다.

## <a name="xbox-live-trace-analyzer"></a>Xbox Live 추적 분석기
사용 하 여 [Xbox Live 추적 분석기](analyze-service-calls.md), 서비스에 대 한 모든 호출을 캡처하고 호출 패턴에 위반에 대 한 분석 하는 오프 라인 상태로 전환 수 있습니다. 서비스 호출 추적 xbtrace 명령줄 도구를 사용 하 여 활성화 또는 자세한 프로토콜 활성화를 통해 고급 시나리오 수 있습니다. 활성화 제목 코드에서 직접 추적 하는 서비스 호출도 지원 됩니다. 이 도구는 명령줄을 통해 사용할 수 있는의 일부로 합니다 [개발 도구](https://aka.ms/xboxliveuwptools) zip입니다.

## <a name="xbox-live-account-tool"></a>Xbox Live 계정 도구  
합니다 [Xbox Live 계정을 도구](xbox-live-account-tool.md) 게임 시나리오를 테스트 하는 것에 대 한 기존 테스트 계정을 설정할 수 있도록 설계 됩니다. 예를 들어, Xbox Live 계정을 도구를 사용 하 여 계정의 게이머 태그를 변경 하 하거나 신속 하 게 계정의 친구 목록에 1000 팔로 워를 추가할 수 있습니다. 이 도구는 명령줄을 통해 사용할 수 있는의 일부로 합니다 [개발 도구](https://aka.ms/xboxliveuwptools) zip입니다.

## <a name="config-as-source"></a>원본으로 구성
[원본으로 구성](https://github.com/Microsoft/xbox-live-developer-tools/blob/master/CONFIGASSOURCE.md) 는 제품군 구성 서비스에 통합 하기 위한 공식적으로 지원 되는 도구와 Api를 제공 하 여 고급 사용자를 수용 하기 위해 Microsoft 개발 도구입니다. Xbox Live 서비스 이러한 성과에 대 한, 웹 서비스와 신뢰 당사자에 순위표에 이르는 서비스를 포함 하 여 파트너 센터에서 제목에 대 한 일반적으로 구성 됩니다. 대부분의 게임 개발자, 파트너 센터를 사용 하 여으로 충분 합니다. 고급 사용자를 위해 그런데 자체 프로세스 및 도구에 일반적인 구성 태스크를 통합 하려고 합니다.  명령줄 도구와 기존 워크플로 및 파이프라인에 사용자 지정 통합을 지 원하는 새 Api를 제공 하 여 이러한 시나리오를 지 원하는 원본으로 구성이 됩니다. 이 도구는 명령줄을 통해 사용할 수 있는의 일부로 합니다 [개발 도구](https://aka.ms/xboxliveuwptools) zip입니다.

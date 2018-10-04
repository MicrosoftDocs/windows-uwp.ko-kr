---
title: Xbox live 개발 도구
author: StaceyHaffner
description: 개발 및 Xbox Live 테스트 하는 데 사용할 제목 제공 되는 도구에 알아봅니다.
ms.assetid: 380a29bf-41a7-4817-9c57-f48f2b824b52
ms.author: kevinasg
ms.date: 6/13/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 도구, 플레이어 재설정, live 추적 분석기, LTA, xbox live 계정 도구
ms.localizationpriority: medium
ms.openlocfilehash: 98b21eda55c6122104c9ec79cda10708e362f3a4
ms.sourcegitcommit: e6daa7ff878f2f0c7015aca9787e7f2730abcfbf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/04/2018
ms.locfileid: "4339785"
---
# <a name="development-tools-for-xbox-live"></a>Xbox live 개발 도구

이 섹션에서는 Xbox live 개발 하는 데 사용할 수 있는 다양 한 도구를 다룹니다. 이러한 도구는 [Xbox Live 개발자 도구 GitHub](https://github.com/Microsoft/xbox-live-developer-tools) 리포지토리에서 사용할 수 있습니다. 또한 고유한 사용자 지정 도구가 만드는 [개발자 도구 라이브러리](https://www.nuget.org/packages/Microsoft.Xbox.Services.DevTools) 를 사용할 수 있습니다. 모든 독립 실행형 개발자 도구에서 다운로드할 수 있습니다 [https://aka.ms/xboxliveuwptools](https://aka.ms/xboxliveuwptools).

> [!NOTE]
> 관리 파트너에서 다운로드에 포함 된 MatchSim 및 XboxLiveCompute 도구만 사용할 수 또는에 등록 된 파트너는 [ID@Xbox](http://www.xbox.com/Developers/id) 프로그램. 사용 가능한 개발자 프로그램에 대 한 자세한 내용은 [개발자 프로그램 개요](https://docs.microsoft.com/windows/uwp/xbox-live/developer-program-overview)를 참조 하십시오. 

## <a name="global-storage"></a>글로벌 저장소
글로벌 타이틀 저장소 명단, 지도, 문제, 또는 아트 리소스 등의 모든 사용자가 읽을 수 있는 데이터를 저장 하는 데 사용 됩니다. [타이틀 저장소](../storage-platform/xbox-live-title-storage/xbox-live-title-storage.md)의 형식입니다. 테스트 샌드박스의 전역 타이틀 저장소를 관리 하는 글로벌 저장소 도구 사용 됩니다. 여전히 Windows 개발자 센터 또는 Xbox 개발자 포털 (XDP)을 통해 정품에 데이터를 게시 합니다. 명령줄을 통해 사용할 수 있는 도구는 개발 도구의 일부로 (https://aka.ms/xboxliveuwptools) zip 합니다. [개발자 도구 라이브러리](https://www.nuget.org/packages/Microsoft.Xbox.Services.DevTools)를 사용 하 여 사용자 지정 도구를 만들 수 있습니다.

## <a name="multiplayer-session-history-viewer"></a>멀티 플레이 세션 기록 뷰어
멀티 플레이어 세션 기록 뷰어 (삭제 된 문서 포함)는 멀티 플레이 세션 문서 기록을 통해 모든 변경 내용 기록 타임 라인을 볼 수가 있습니다. 이 도구를 사용 하 여 시간이 지남에 따라 변경 되 MPSD 세션 문서와 일어나는지 더 깊게 이해를 제공 됩니다. 것이 [개발 도구](https://aka.ms/xboxliveuwptools) zip에서 독립 실행형 도구로 사용할 수 있습니다.

## <a name="player-data-reset"></a>플레이어 데이터 초기화
플레이어 데이터 초기화 도구 테스트 샌드박스에서 플레이어의 데이터를 다시 사용할 수 있습니다. 데이터와 같은; 재설정할 수 있습니다. 도전 과제, 순위표, 통계 및 제목 기록 합니다. 명령줄을 통해 사용할 수 있는 도구는 [개발 도구](https://aka.ms/xboxliveuwptools) zip의 일환으로 합니다. [개발자 도구 라이브러리](https://www.nuget.org/packages/Microsoft.Xbox.Services.DevTools)를 사용 하 여 사용자 지정 도구를 만들 수 있습니다.

## <a name="xbox-live-developer-account"></a>Xbox Live 개발자 계정
개발자 계정에 인증을 관리 하는 Xbox Live 개발자 계정 도구 사용 됩니다. 플레이어 초기화 및 글로벌 저장소와 같은 개발자 자격 증명을 요구 하는 다른 개발자 도구와 상호 작용할 필요 합니다. 명령줄을 통해 사용할 수 있는 도구는 [개발 도구](https://aka.ms/xboxliveuwptools) zip의 일환으로 합니다.

## <a name="xbox-live-trace-analyzer"></a>Xbox Live 추적 분석기
[Xbox Live 추적 분석기](analyze-service-calls.md)를 사용 하 여 모든 서비스 호출을 캡처할 수 있으며 호출 패턴에 위반 분석 하는 오프 수도 있습니다. 서비스 호출 추적 xbtrace 명령줄 도구를 사용 하 여 활성화 하거나 더 프로토콜 활성화를 통해 고급 시나리오 될 수 있습니다. 서비스 호출 제목 코드에서 직접 추적을 활성화도 지원 됩니다. 명령줄을 통해 사용할 수 있는 도구는 [개발 도구](https://aka.ms/xboxliveuwptools) zip의 일환으로 합니다.

## <a name="xbox-live-account-tool"></a>Xbox Live 계정 도구  
[Xbox Live 계정 도구](xbox-live-account-tool.md) 는 게임 시나리오를 테스트를 위해 기존 테스트 계정을 설정할 수 있도록 설계 되었습니다. 예를 들어 Xbox Live 계정 도구를 사용 하 여 계정의 게이머를 변경 하거나 신속 하 게 1000 팔로 워 계정의 친구 목록에 추가 수 있습니다. 명령줄을 통해 사용할 수 있는 도구는 [개발 도구](https://aka.ms/xboxliveuwptools) zip의 일환으로 합니다.

## <a name="config-as-source"></a>소스로 구성
[구성 소스로](https://github.com/Microsoft/xbox-live-developer-tools/blob/master/CONFIGASSOURCE.md) 구성 서비스에 통합 하는 것에 대 한 공식적으로 지원 되는 도구 및 Api를 제공 하 여 고급 사용자를 수용 하기 위해 Microsoft 개발 도구 제품군입니다. 이러한 Xbox Live 서비스와 신뢰 당사자 웹 서비스에, 도전 과제를 순위표에 이르는 서비스를 포함 하 여 개발자 센터에서 타이틀 정상적으로 구성 됩니다. 대부분의 게임 개발자, 개발자 센터를 사용 하 여 면 충분 합니다. 그러나 고급 사용자를 위한는 자체 프로세스 및 도구에 일반적인 구성 작업을 통합 하기 원하는 합니다.  소스로 구성은 명령줄 도구 및 워크플로 기존 및 파이프라인에 사용자 지정 통합을 지원 하기 위해 새로운 Api를 제공 하 여 이러한 시나리오를 지원 하기 위한 것입니다. 명령줄을 통해 사용할 수 있는 도구는 [개발 도구](https://aka.ms/xboxliveuwptools) zip의 일환으로 합니다.

---
title: 게임 채팅 2 개요
author: KevinAsgari
description: 게임에 Xbox Live 게임 채팅 2, 게임 채팅의 업데이트 된 버전을 사용 하 여 음성 통신을 추가 하는 방법을 알아봅니다.
ms.author: tomco
ms.date: 10/20/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 게임 채팅, 게임 채팅 2, 음성 통신
ms.localizationpriority: medium
ms.openlocfilehash: 252cd33b0504a586852381a6c5ace3f5700890f7
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/22/2018
ms.locfileid: "5396204"
---
# <a name="game-chat-2-overview"></a>게임 채팅 2 개요

게임 채팅 2를 사용 하면 플레이어의 개인 정보 설정을 사용 하 고 Xbox One 게임 및 음성 및 텍스트 채팅에 관련 된 허브 앱에 대 한 Xbox 요구를 수행 하는 동안 앱에 음성 및 문자 채팅 통신을 쉽게 추가할 수 있습니다. 접근성-게임 채팅 기록 설정을 통해 음성-텍스트 또는 텍스트-음성 변환을 사용 하도록 설정한 플레이어에 게 게임 채팅 2는 투명 하 게 수행 채팅 텍스트 메시지 수신 음성 오디오 및 플레이 나타내는 만드는 번역 송신 채팅 텍스트 메시지에 대 한 음성 오디오를 각각 합성.

- **통신 관계** -Game Chat 2 플레이어와 서로 통신할 수는 어떻게 세부적으로 제어를 제공 합니다. 게임 채팅 2에 팀 또는 채널을 지정 하는 대신 명시적 사용자의 각 쌍 관계를 정의 하는 것에 필요 합니다. 게임 채팅 2 통신 관계 uni 및 플레이어의 쌍 간의 양방향 통신을 지원합니다. 음성 및 텍스트 통신 관계는 서로 독립적으로 설정할 수 있습니다.

- 음성-텍스트 및 텍스트 음성 변환 **접근성** -Game Chat 2를 지원합니다. 게임 채팅 2 사용 하면 플레이어의 "게임 채팅 기록" 기본 설정을 존중 하 고 채팅 텍스트 메시지 수신 음성 오디오 및 플레이 합성 음성 오디오 송신 채팅 텍스트를 나타내는 만드는 번역을 투명 하 게 수행 됩니다. 메시지를 각각 합니다.

- **Xbox Live 통합** -Game Chat 2 Xbox Live 서비스를 사용 하 여 각 플레이어의 기본 설정 및 권한을 적용 하면.

- **음성 활동 감지** -Game Chat 2는 오디오 데이터 음성 활동을 포함 하는 시기를 결정 하는 음성 활동 감지를 수행 합니다.

- **자동 게인 제어** -Game Chat 2 사용자의 마이크 출력의 변형을 최소화 하기 위해 자동 게인 제어를 수행 합니다.

- **코덱** -Game Chat 2는 앱의 원격 인스턴스를 제공 해야 하는 오디오 데이터를 인코딩합니다. Xbox One에서이 인코딩 (및, 수신측에서 디코딩)는 하드웨어 가속입니다.

- `chat_manager::start/finish_processing_state_changes` - 비동기 작업을 수행하고, `game_chat_state_change` 구조의 형태로 처리할 결과를 검색한 다음, 완료되면 관련 리소스를 비우기 위해 앱 각 UI 프레임에 의해 호출되는 메서드 쌍.

- `chat_manager::start/finish_processing_data_frames` -쌍 Game Chat 2 앱의 전송 계층에 연결 하는 데 사용 되는 방법입니다. 이러한 메서드를 검색 하 여 배포할 네트워크 프레임 마다 앱에서 호출 됩니다 `game_chat_data_frame` 개체 원격 장치에서 앱의 인스턴스를 차례로 완료 되 면 관련된 리소스를 해제 합니다.

- `chat_manager::process_incoming_data` -Game Chat 2의 원격 인스턴스에서 앱의 전송 계층을 통해 제공 하는 게임 채팅 2로 데이터를 제공 하는 데 메서드.

- **실시간 오디오 조작** -게임 채팅 2 검사할 수도 채팅 오디오 데이터를 조작할 수 있도록 채팅 오디오 파이프라인에 삽입 하도록 앱을 수 있습니다. 자세한 내용은 [실시간 오디오 조작](real-time-audio-manipulation.md) 를 참조 하세요.

앱은 로컬 장치에는 사용자와 함께 채팅 것으로 예상 되는 원격 디바이스에서 사용자의 라이브러리를 알립니다. 앱은 다음 각 사용자 간의 관계를 구성합니다.

구성 되 면 게임 채팅 2 로컬 사용자의 microphone(s)에서 오디오를 폴링합니다, 수행한 자동 이득 제어 및 음성 활동 감지, 데이터를 인코딩하고 다음을 통해 게임 채팅 2의 원격 인스턴스에 배달 오디오 데이터를 노출 `chat_manager::start/finish_processing_data_frames`. 앱에 포함 된 데이터를 제공 해야 합니다 `game_chat_state_change` Game Chat 2 동일한 개체에 지정 된 원격 인스턴스에 개체. 데이터를 받으면 앱의 인스턴스를 원격으로 데이터를 통해 Game Chat 2의 자체 인스턴스를 제출 해야 합니다 `chat_manager::process_incoming_data`. 게임 채팅 2 들어오는 데이터를 디코딩하고 로컬 사용자의 오디오 장치에 렌더링 합니다.

게임 채팅 2는 Xbox Live 하 권한 및 개인 정보 보호 요구 사항을 적용 합니다. 오디오 데이터 통신 권한이 없는 사용자가 생성 되지 않습니다 및 오디오 데이터 개인 정보 설정을 통해 차단 되는 사용자 로부터 렌더링 되지 않습니다.

시작 하려면 [게임 채팅 2를 사용 하 여](using-game-chat-2.md)참조 하세요. C#을 사용 하는 경우 [를 사용 하 여 게임 채팅 2 WinRT 프로젝션](using-game-chat-2-winrt.md)를 참조 하세요. 게임 채팅 구현 이미 있는 경우 게임 채팅 개념 및 호출 패턴 Game Chat 2 아날로그 매핑할 [게임 채팅 2 마이그레이션 가이드](game-chat-2-migration.md) 를 사용 합니다.

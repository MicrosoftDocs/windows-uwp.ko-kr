---
author: mcleblanc
ms.assetid: 41ac0142-4d86-4bb3-b580-36d0d6956091
title: "HoloLens용 디바이스 포털 API 참조"
description: "데이터에 액세스하고 디바이스를 프로그래밍 방식으로 제어하는 데 사용할 수 있는 HoloLens REST API의 Windows Device Portal에 대해 알아봅니다."
ms.openlocfilehash: 638ebca167b2ca56f00a83aab13b15c57b2dca2a
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="device-portal-api-reference-for-hololens"></a>HoloLens용 디바이스 포털 API 참조

Windows Device Portal의 모든 작업은 데이터에 액세스하고 디바이스를 프로그래밍 방식으로 제어하는 데 사용할 수 있는 REST API를 기반으로 합니다.

## <a name="holographic-os"></a>홀로그램 OS
---
### <a name="get-https-requirements-for-the-device-portal"></a>디바이스 포털에 대한 HTTPS 요구 사항 가져오기

**요청**

다음 요청 형식을 사용하여 디바이스 포털에 대한 HTTPS 요구 사항을 가져올 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/holographic/os/webmanagement/settings/https


**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

- 없음

**상태 코드**

- 표준 상태 코드입니다.

---
### <a name="get-the-stored-interpupillary-distance-ipd"></a>저장된 IPD(동공 간 거리) 가져오기

**요청**

다음 요청 형식을 사용하여 저장된 IPD 값을 가져올 수 있습니다. 값은 밀리미터 단위로 반환됩니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/holographic/os/settings/ipd


**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

- 없음

**상태 코드**

- 표준 상태 코드입니다.

---
### <a name="get-a-list-of-hololens-specific-etw-providers"></a>HoloLens 특정 ETW 공급자 목록 가져오기

**요청**

다음 요청 형식을 사용하여 시스템에 등록되지 않은 HoloLens 특정 ETW 공급자의 목록을 가져올 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/holographic/os/etw/customproviders


**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

- 없음

**상태 코드**

- 표준 상태 코드입니다.

---
### <a name="return-the-state-for-all-active-services"></a>모든 활성 서비스의 상태를 반환합니다.

**요청**

다음 요청 형식을 사용하여 현재 실행 중인 모든 서비스의 상태를 가져올 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/holographic/os/services


**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

- 없음

**상태 코드**

- 표준 상태 코드입니다.

---
### <a name="set-the-https-requirement-for-the-device-portal"></a>디바이스 포털에 대한 HTTPS 요구 사항 설정

**요청**

다음 요청 형식을 사용하여 디바이스 포털에 대한 HTTPS 요구 사항을 설정할 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
POST | /api/holographic/management/settings/https


**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

URI 매개 변수 | 설명
:---          | :---
required   | (**필수**) HTTPS가 디바이스 포털에 필요한지 여부를 결정합니다. 가능한 값은 **yes**, **no**, **default**입니다.

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

- 없음

**상태 코드**

- 표준 상태 코드입니다.

---
### <a name="set-the-interpupillary-distance-ipd"></a>IPD(I동공 간 거리)를 설정합니다.

**요청**

다음 요청 형식을 사용하여 저장된 IPD를 설정할 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
POST | /api/holographic/os/settings/ipd


**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

URI 매개 변수 | 설명
:---          | :---
ipd   | (**필수**) 저장될 새 IPD 값입니다. 이 값은 밀리미터 단위여야 합니다.

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

- 없음

**상태 코드**

- 표준 상태 코드입니다.

---
## Holographic perception
---
### <a name="accept-websocket-upgrades-and-run-a-mirage-client-that-sends-updates"></a>Websocket 업그레이드를 수락하고 업데이트를 보내는 mirage 클라이언트 실행

**요청**

다음 요청 형식을 사용하여 Websocket 업그레이드를 수락하고 30fps로 업데이트를 보내는 mirage 클라이언트를 실행할 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
GET/WebSocket | /api/holographic/perception/client


**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

URI 매개 변수 | 설명
:---          | :---
clientmode   | (**필수**) 추적 모드를 결정합니다. **active** 값은 수동적으로 연결할 수 없는 경우 시각적 추적 모드로 강제 지정됩니다.

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

- 없음

**상태 코드**

- 표준 상태 코드입니다.

---
## Holographic thermal
---
### <a name="get-the-thermal-stage-of-the-device"></a>디바이스의 열 단계 가져오기

**요청**

다음 요청 형식을 사용하여 디바이스의 열 단계를 가져올 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/holographic/

**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

다음 표에 가능한 값이 표시됩니다.

값 | 설명
--- | ---
1 | 일반
2 | 준비
3 | 위험

**상태 코드**

- 표준 상태 코드입니다.

---
## HSimulation control
---
### <a name="create-a-control-stream-or-post-data-to-a-created-stream"></a>컨트롤 스트림을 만들거나 만든 스트림에 데이터 게시

**요청**

다음 요청 형식을 사용하여 컨트롤 스트림을 만들거나 만든 스트림에 데이터를 게시할 수 있습니다. 게시된 데이터는 **application/octet-stream** 형식이어야 합니다.
 
메서드      | 요청 URI
:------     | :-----
POST | /api/holographic/simulation/control/stream


**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

URI 매개 변수 | 설명
:---          | :---
priority   | (**컨트롤 스트림을 만드는 경우 필수**) 스트림의 우선 순위를 나타냅니다.
streamid   | (**만든 스트림에 게시하는 경우 필수**) 게시할 스트림 식별자입니다.

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

- 없음

**상태 코드**

- 표준 상태 코드입니다.

---
### <a name="delete-a-control-stream"></a>컨트롤 스트림 삭제

**요청**

다음 요청 형식을 사용하여 컨트롤 스트림을 삭제할 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
DELETE | /api/holographic/simulation/control/stream


**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

- 없음

**상태 코드**

- 표준 상태 코드입니다.

---
### <a name="get-a-control-stream"></a>컨트롤 스트림 가져오기

**요청**

다음 요청 형식을 사용하여 컨트롤 스트림에 대한 웹 소켓 연결을 열 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
GET/WebSocket | /api/holographic/simulation/control/stream


**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

- 없음

**상태 코드**

- 표준 상태 코드입니다.

---
### <a name="get-the-simluation-mode"></a>시뮬레이션 모드 가져오기

**요청**

다음 요청 형식을 사용하여 시뮬레이션 모드를 가져올 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/holographic/simulation/control/mode


**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

- 없음

**상태 코드**

- 표준 상태 코드입니다.

---
### <a name="set-the-simluation-mode"></a>시뮬레이션 모드 설정

**요청**

다음 요청 형식을 사용하여 시뮬레이션 모드를 설정할 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
POST | /api/holographic/simluation/control/mode


**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

URI 매개 변수 | 설명
:---          | :---
모드   | (**필수**) 시뮬레이션 모드를 나타냅니다. 가능한 값은 **default**, **simulation**, **remote**, **legacy**입니다.

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

- 없음

**상태 코드**

- 표준 상태 코드입니다.

---
## HSimulation playback
---
### <a name="delete-a-recording"></a>기록 삭제

**요청**

다음 요청 형식을 사용하여 기록을 삭제할 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
DELETE | /api/holographic/simulation/playback/file


**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

URI 매개 변수 | 설명
:---          | :---
recording   | (**필수**) 삭제할 기록의 이름입니다.

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

- 없음

**상태 코드**

- 표준 상태 코드입니다.

---
### <a name="get-all-recordings"></a>모든 기록 가져오기

**요청**

다음 요청 형식을 사용하여 사용 가능한 모든 기록을 가져올 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/holographic/simulation/playback/files


**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

- 없음

**상태 코드**

- 표준 상태 코드입니다.

---
### <a name="get-the-types-of-data-in-a-loaded-recording"></a>로드된 기록의 데이터 유형 가져오기

**요청**

다음 요청 형식을 사용하여 로드된 기록의 데이터 유형을 가져올 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/holographic/simulation/playback/session/types


**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

URI 매개 변수 | 설명
:---          | :---
recording   | (**필수**) 관심 있는 기록의 이름입니다.

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

- 없음

**상태 코드**

- 표준 상태 코드입니다.

---
### <a name="get-all-the-loaded-recordings"></a>로드된 모든 기록 가져오기

**요청**

다음 요청 형식을 사용하여 로드된 모든 기록을 가져올 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/holographic/simulation/playback/session/files


**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

- 없음

**상태 코드**

- 표준 상태 코드입니다.

---
### <a name="get-the-current-playback-state-of-a-recording"></a>기록의 현재 재생 상태 가져오기 

**요청**

다음 요청 형식을 사용하여 기록의 현재 재생 상태를 가져올 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/holographic/simulation/playback/session


**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

URI 매개 변수 | 설명
:---          | :---
recording   | (**필수**) 관심 있는 기록의 이름입니다.

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

- 없음

**상태 코드**

- 표준 상태 코드입니다.

---
### <a name="load-a-recording"></a>기록 로드

**요청**

다음 요청 형식을 사용하여 기록을 로드할 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
POST | /api/holographic/simulation/playback/session/file


**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

URI 매개 변수 | 설명
:---          | :---
recording   | (**필수**) 로드할 기록의 이름입니다.

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

- 없음

**상태 코드**

- 표준 상태 코드입니다.

---
### <a name="pause-a-recording"></a>기록 일시 중지

**요청**

다음 요청 형식을 사용하여 기록을 일시 중지할 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
POST | /api/holographic/simulation/playback/session/pause


**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

URI 매개 변수 | 설명
:---          | :---
recording   | (**필수**) 일시 중지할 기록의 이름입니다.

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

- 없음

**상태 코드**

- 표준 상태 코드입니다.

---
### <a name="play-a-recording"></a>기록 재생

**요청**

다음 요청 형식을 사용하여 기록을 재생할 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
POST | /api/holographic/simulation/playback/session/play


**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

URI 매개 변수 | 설명
:---          | :---
recording   | (**필수**) 재생할 기록의 이름입니다.

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

- 없음

**상태 코드**

- 표준 상태 코드입니다.

---
### <a name="stop-a-recording"></a>기록 중지

**요청**

다음 요청 형식을 사용하여 기록을 중지할 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
POST | /api/holographic/simulation/playback/session/stop


**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

URI 매개 변수 | 설명
:---          | :---
recording   | (**필수**) 중지할 기록의 이름입니다.

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

- 없음

**상태 코드**

- 표준 상태 코드입니다.

---
### <a name="unload-a-recording"></a>기록 언로드

**요청**

다음 요청 형식을 사용하여 기록을 언로드할 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
DELETE | /api/holographic/simulation/playback/session/file


**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

URI 매개 변수 | 설명
:---          | :---
recording   | (**필수**) 언로드할 기록의 이름입니다.

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

- 없음

**상태 코드**

- 표준 상태 코드입니다.

---
### <a name="upload-a-recording"></a>기록 업로드

**요청**

다음 요청 형식을 사용하여 기록을 업로드할 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
POST | /api/holographic/simulation/playback/file


**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

- 없음

**상태 코드**

- 표준 상태 코드입니다.

---
## HSimulation recording
---
### <a name="get-the-recording-state"></a>기록 상태 가져오기

**요청**

다음 요청 형식을 사용하여 현재 기록 상태를 가져올 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/holographic/simulation/recording/status


**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

- 없음

**상태 코드**

- 표준 상태 코드입니다.

---
### <a name="start-a-recording"></a>기록 시작

**요청**

다음 요청 형식을 사용하여 기록을 시작할 수 있습니다. 한 번에 하나의 활성 기록만 있을 수 있습니다. 
 
메서드      | 요청 URI
:------     | :-----
POST | /api/holographic/simulation/recording/start


**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

URI 매개 변수 | 설명
:---          | :---
head   | (**아래 참조**) 시스템이 헤드 데이터를 기록하도록 이 값을 1로 설정합니다.
hands   | (**아래 참조**) 시스템이 핸즈 데이터를 기록하도록 이 값을 1로 설정합니다.
spatialMapping   | (**아래 참조**) 시스템이 공간 매핑 데이터를 기록하도록 이 값을 1로 설정합니다.
environment   | (**아래 참조**) 시스템이 환경 데이터를 기록하도록 이 값을 1로 설정합니다.
name   | (**필수**) 기록의 이름입니다.
singleSpatialMappingFrame   | (**선택**) 단일 공간 매핑 프레임만 기록되도록 이 값을 1로 설정합니다.

이러한 매개 변수는 *head*, *hands*, *spatialMapping*, *environment* 매개 변수 중 하나만 1로 설정해야 합니다.

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

- 없음

**상태 코드**

- 표준 상태 코드입니다.

---
### <a name="stop-the-current-recording"></a>현재 기록 중지

**요청**

다음 요청 형식을 사용하여 현재 기록을 중지할 수 있습니다. 기록이 파일로 반환됩니다.
 
메서드      | 요청 URI
:------     | :-----
POST | /api/holographic/simulation/recording/stop


**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

- 없음

**상태 코드**

- 표준 상태 코드입니다.

---
## Mixed reality capture
---
### <a name="delete-a-mixed-reality-capture-mrc-recording-from-the-device"></a>디바이스에서 MRC(혼합 현실 캡처)를 삭제합니다.

**요청**

다음 요청 형식을 사용하여 MRC 기록을 삭제할 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
DELETE | /api/holographic/mrc/file


**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

URI 매개 변수 | 설명
:---          | :---
filename   | (**필수**) 삭제할 동영상 파일의 이름입니다. 이름은 hex64로 인코드되어야 합니다.

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

- 없음

**상태 코드**

- 표준 상태 코드입니다.

---
### <a name="download-a-mixed-reality-capture-mrc-file"></a>MRC(혼합 현실 캡처) 파일 다운로드

**요청**

다음 요청 형식을 사용하여 디바이스에서 MRC 파일을 다운로드할 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/holographic/mrc/file


**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

URI 매개 변수 | 설명
:---          | :---
filename   | (**필수**) 가져오려는 동영상 파일의 이름입니다. 이름은 hex64로 인코드되어야 합니다.
op   | (**선택**) 스트림을 다운로드하려면 이 값을 **stream**으로 설정합니다.

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

- 없음

**상태 코드**

- 표준 상태 코드입니다.

---
### <a name="get-the-mixed-reality-capture-mrc-settings"></a>MRC(혼합 현실 캡처) 설정 가져오기

**요청**

다음 요청 형식을 사용하여 MRC 설정을 가져올 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/holographic/mrc/settings


**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

- 없음

**상태 코드**

- 표준 상태 코드입니다.

---
### <a name="get-the-status-of-the-mixed-reality-capture-mrc-recording"></a>MRC(혼합 현실 캡처) 기록 상태를 가져옵니다.

**요청**

다음 요청 형식을 사용하여 MRC 기록 상태를 가져올 수 있습니다. 가능한 값은 **running** 및 **stopped**입니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/holographic/mrc/status


**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

- 없음

**상태 코드**

- 표준 상태 코드입니다.

---
### <a name="get-the-list-of-mixed-reality-capture-mrc-files"></a>MRC(혼합 현실 캡처) 파일 목록 가져오기

**요청**

다음 요청 형식을 사용하여 디바이스에 저장된 MRC 파일을 가져올 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/holographic/mrc/files


**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

- 없음

**상태 코드**

- 표준 상태 코드입니다.

---
### <a name="set-the-mixed-reality-capture-mrc-settings"></a>MRC(혼합 현실 캡처) 설정 지정

**요청**

다음 요청 형식을 사용하여 MRC 설정을 지정할 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
POST | /api/holographic/mrc/settings


**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

- 없음

**상태 코드**

- 표준 상태 코드입니다.

---
### <a name="starts-a-mixed-reality-capture-mrc-recording"></a>MRC(혼합 현실 캡처) 기록 시작

**요청**

다음 요청 형식을 사용하여 MRC 기록을 시작할 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
POST | /api/holographic/mrc/video/control/start


**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

- 없음

**상태 코드**

- 표준 상태 코드입니다.

---
### <a name="stop-the-current-mixed-reality-capture-mrc-recording"></a>현재 MRC(혼합 현실 캡처) 기록 중지

**요청**

다음 요청 형식을 사용하여 현재 MRC 기록을 중지할 수 있습니다.
 
메서드      | 요청 URI
:------     | :-----
POST | /api/holographic/mrc/video/control/stop


**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

- 없음

**상태 코드**

- 표준 상태 코드입니다.

---
### <a name="take-a-mixed-reality-capture-mrc-photo"></a>MRC(혼합 현실 캡처) 사진 촬영

**요청**

다음 요청 형식을 사용하여 MRC 사진을 찍을 수 있습니다. 사진은 JPEG 형식으로 반환됩니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/holographic/mrc/photo


**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

- 없음

**상태 코드**

- 표준 상태 코드입니다.

---
## Mixed reality streaming
---
### <a name="initiates-a-chunked-download-of-a-fragmented-mp4"></a>조각난 mp4의 청크 분할 다운로드를 시작합니다.

**요청**

다음 요청 형식을 사용하여 조각난 mp4의 청크 분할 다운로드를 시작할 수 있습니다. 이 API는 기본 품질을 사용합니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/holographic/stream/live.mp4


**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

URI 매개 변수 | 설명
:---          | :---
pv   | (**선택**) PV 카메라 캡처 여부를 나타냅니다. **true** 또는 **false**여야 합니다.
holo   | (**선택**) 홀로그램 캡처 여부를 나타냅니다. **true** 또는 **false**여야 합니다.
mic   | (**선택**) 마이크 캡처 여부를 나타냅니다. **true** 또는 **false**여야 합니다.
loopback   | (**선택**) 응용 프로그램 오디오 캡처 여부를 나타냅니다. **true** 또는 **false**여야 합니다.

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

- 없음

**상태 코드**

- 표준 상태 코드입니다.

---
### <a name="initiates-a-chunked-download-of-a-fragmented-mp4"></a>조각난 mp4의 청크 분할 다운로드를 시작합니다.

**요청**

다음 요청 형식을 사용하여 조각난 mp4의 청크 분할 다운로드를 시작할 수 있습니다. 이 API는 고품질을 사용합니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/holographic/stream/live_high.mp4


**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

URI 매개 변수 | 설명
:---          | :---
pv   | (**선택**) PV 카메라 캡처 여부를 나타냅니다. **true** 또는 **false**여야 합니다.
holo   | (**선택**) 홀로그램 캡처 여부를 나타냅니다. **true** 또는 **false**여야 합니다.
mic   | (**선택**) 마이크 캡처 여부를 나타냅니다. **true** 또는 **false**여야 합니다.
loopback   | (**선택**) 응용 프로그램 오디오 캡처 여부를 나타냅니다. **true** 또는 **false**여야 합니다.

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

- 없음

**상태 코드**

- 표준 상태 코드입니다.

---
### <a name="initiates-a-chunked-download-of-a-fragmented-mp4"></a>조각난 mp4의 청크 분할 다운로드를 시작합니다.

**요청**

다음 요청 형식을 사용하여 조각난 mp4의 청크 분할 다운로드를 시작할 수 있습니다. 이 API는 저품질을 사용합니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/holographic/stream/live_low.mp4


**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

URI 매개 변수 | 설명
:---          | :---
pv   | (**선택**) PV 카메라 캡처 여부를 나타냅니다. **true** 또는 **false**여야 합니다.
holo   | (**선택**) 홀로그램 캡처 여부를 나타냅니다. **true** 또는 **false**여야 합니다.
mic   | (**선택**) 마이크 캡처 여부를 나타냅니다. **true** 또는 **false**여야 합니다.
loopback   | (**선택**) 응용 프로그램 오디오 캡처 여부를 나타냅니다. **true** 또는 **false**여야 합니다.

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

- 없음

**상태 코드**

- 표준 상태 코드입니다.

---
### <a name="initiates-a-chunked-download-of-a-fragmented-mp4"></a>조각난 mp4의 청크 분할 다운로드를 시작합니다.

**요청**

다음 요청 형식을 사용하여 조각난 mp4의 청크 분할 다운로드를 시작할 수 있습니다. 이 API는 중간 품질을 사용합니다.
 
메서드      | 요청 URI
:------     | :-----
GET | /api/holographic/stream/live_med.mp4


**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

URI 매개 변수 | 설명
:---          | :---
pv   | (**선택**) PV 카메라 캡처 여부를 나타냅니다. **true** 또는 **false**여야 합니다.
holo   | (**선택**) 홀로그램 캡처 여부를 나타냅니다. **true** 또는 **false**여야 합니다.
mic   | (**선택**) 마이크 캡처 여부를 나타냅니다. **true** 또는 **false**여야 합니다.
loopback   | (**선택**) 응용 프로그램 오디오 캡처 여부를 나타냅니다. **true** 또는 **false**여야 합니다.

**요청 헤더**

- 없음

**요청 본문**

- 없음

**응답**

- 없음

**상태 코드**

- 표준 상태 코드입니다.

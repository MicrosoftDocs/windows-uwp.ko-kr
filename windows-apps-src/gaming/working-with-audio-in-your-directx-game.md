---
title: 게임의 오디오
description: DirectX 게임에 음악과 소리를 개발 하 고 통합 하는 방법과 오디오 신호를 처리 하 여 동적 및 위치 소리를 만드는 방법을 알아봅니다.
ms.assetid: ab29297a-9588-c79b-24c5-3b94b85e74a8
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 오디오, directx
ms.localizationpriority: medium
ms.openlocfilehash: f0493c68fcb3453890f95c123b0c870cf321fe7e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162917"
---
# <a name="audio-for-games"></a>게임의 오디오

DirectX 게임에 음악과 소리를 개발 하 고 통합 하는 방법과 오디오 신호를 처리 하 여 동적 및 위치 소리를 만드는 방법을 알아봅니다.

오디오 프로그래밍의 경우 DirectX의 [XAudio2](/windows/win32/xaudio2/xaudio2-apis-portal) 라이브러리 또는 Windows 런타임 [오디오 그래프](../audio-video-camera/audio-graphs.md) api를 사용 하는 것이 좋습니다. 여기서는 XAudio2를 사용 합니다. XAudio2는 게임의 신호 처리 및 혼합 토대를 제공 하는 하위 수준 오디오 라이브러리로, 다양 한 형식을 지원 합니다.

[Microsoft 미디어 파운데이션](/windows/desktop/medfound/microsoft-media-foundation-sdk)를 사용 하 여 간단한 소리와 음악 재생을 구현할 수도 있습니다. Microsoft 미디어 파운데이션는 오디오 및 비디오와 같은 미디어 파일 및 스트림을 재생할 수 있도록 설계 되었지만 게임에도 사용할 수 있으며, 게임의 시네마 또는 비 대화형 구성 요소에 특히 유용 합니다.

## <a name="concepts-at-a-glance"></a>한눈에 개념 보기

이 섹션에서 사용 하는 몇 가지 오디오 프로그래밍 개념은 다음과 같습니다.

-   신호는 그래픽의 픽셀과 유사 하 게 사운드 프로그래밍의 기본 단위입니다. 이를 처리 하는 Dsp (디지털 신호 프로세서)는 게임 오디오의 픽셀 셰이더와 비슷합니다. 신호를 변형 하거나 결합 하거나 필터링 할 수 있습니다. Dsp를 프로그래밍 하 여 게임의 소리 효과와 음악을 필요에 따라 거의 또는 거의 복잡 하 게 변경할 수 있습니다.
-   음성은 두 개 이상의 신호에 대 한 submixed 복합입니다. XAudio2 음성 개체에는 원본, 서브 믹스 및 마스터링 음성의 3 가지 유형이 있습니다. 원본 음성은 클라이언트에서 제공 하는 오디오 데이터에 대해 작동 합니다. 원본 및 서브 믹스 음성은 하나 이상의 서브 믹스 또는 마스터링 음성으로 출력을 보냅니다. 서브 믹스 및 마스터링 음성은 오디오를 공급 하는 모든 음성에서 오디오를 혼합 하 고 결과에 대해 작동 합니다. 오디오 오디오 데이터를 오디오 장치에 기록 합니다.
-   믹싱은 음향 효과와 장면에서 재생 되는 배경 오디오와 같은 여러 불연속 음성을 단일 스트림으로 결합 하는 프로세스입니다. Submixing은 엔진 소음의 구성 요소 소리와 같은 여러 불연속 신호를 결합 하 고 음성을 만드는 프로세스입니다.
-   오디오 형식. 음악과 음향 효과는 게임에 대해 다양 한 디지털 형식으로 저장할 수 있습니다. WAV와 같은 압축 되지 않은 형식 및 MP3 및 OGG와 같은 압축 된 형식이 있습니다. 더 많은 샘플이 압축 됩니다. 일반적으로 비트 전송률에 의해 지정 되 고, 비트 전송률이 낮을수록, 압축이 더 손실 되 고,이에 따라 더 심한 충실도가 발생 합니다. 충실도는 압축 체계 및 비트 전송률에 따라 다를 수 있으므로 게임에 가장 적합 한 것을 찾기 위해 실험 합니다.
-   샘플링 주기 및 품질. 소리는 서로 다른 속도로 샘플링할 수 있으며, 더 낮은 속도로 샘플링 된 소리는 훨씬 쿼리보다 성능이. CD 품질의 샘플링 주기는 44.1 Khz (44100 Hz)입니다. 사운드에 대해 고화질이 필요 하지 않은 경우 더 낮은 샘플 주기를 선택할 수 있습니다. 전문 오디오 응용 프로그램에는 더 높은 요금이 적용 될 수 있지만 게임에서 전문가의 품질을 요구 하지 않는 한 필요 하지 않을 수 있습니다.
-   소리 생성기 (또는 소스). XAudio2에서 소리 송신기는 소리를 내보내는 위치 이며, 게임 중인 주크박스에서 재생 된 배경 노이즈와 snarling 바위 트랙의 단순한 배경입니다. 전역 좌표로 송신기를 지정 합니다.
-   사운드 수신기. 사운드 수신기는 종종 수신기에서 받은 소리를 처리 하는 고급 게임에서 플레이어 또는 AI 엔터티입니다. 플레이어를 재생 하기 위해 오디오 스트림에 해당 소리를 서브 믹스 하거나,이를 사용 하 여 활성화으로 표시 된 AI guard와 같은 특정 게임 내 작업을 수행할 수 있습니다.

## <a name="design-considerations"></a>설계 고려 사항

오디오는 게임 디자인 및 개발의 크게 중요 한 부분입니다. 많은 게이머는 기억 하기 쉬운 사운드 트랙, 좋은 음성 작업 및 소리 혼합 또는 전체 뛰어난 오디오 생산으로 인 한 mediocre 게임을 전설의 상태에 회수할 수 있습니다. 음악과 소리는 게임의 개성을 정의 하 고 게임을 정의 하 고 다른 유사한 게임과 분리 하는 기본 목표을 설정 합니다. 게임의 오디오 프로필을 디자인 하 고 개발 하는 데 소요 되는 노력은 매우 가치가 있습니다.

위치 3D 오디오는 3D 그래픽에서 제공 하는 것 이상의 집중 교육 수준을 추가할 수 있습니다. 전 세계를 시뮬레이트하는 복잡 한 게임을 개발 하거나 시네마 스타일을 요구 하는 경우 3D 위치 오디오 기술을 사용 하 여에서 플레이어를 실제로 그리는 것이 좋습니다.

## <a name="directx-audio-development-roadmap"></a>DirectX 오디오 개발 로드맵

### <a name="xaudio2-conceptual-resources"></a>XAudio2 개념 리소스

XAudio2는 DirectX 용 오디오 믹싱 라이브러리 이며 주로 게임을 위한 고성능 오디오 엔진을 개발 하기 위한 것입니다. 최신 게임에 음향 효과와 배경 음악을 추가 하려는 게임 개발자를 위해 XAudio2는 오디오 그래프를 제공 하 고 대기 시간이 짧고 동적 버퍼에 대 한 지원, 동기식 샘플-정확한 재생 및 암시적 소스 요율 변환을 지원 합니다.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">항목</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-introduction">XAudio2 소개</a></p></td>
<td align="left"><p>항목은 XAudio2에서 지원 되는 오디오 프로그래밍 기능 목록을 제공 합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/getting-started">XAudio2 시작</a></p></td>
<td align="left"><p>이 항목에서는 주요 XAudio2 개념, XAudio2 버전 및 RIFF 오디오 형식에 대 한 정보를 제공 합니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/common-audio-concepts">일반적인 오디오 프로그래밍 개념</a></p></td>
<td align="left"><p>이 항목에서는 오디오 개발자에 게 친숙 한 일반적인 오디오 개념의 개요를 제공 합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-voices">XAudio2 음성</a></p></td>
<td align="left"><p>이 항목에는 오디오 데이터의 서브 믹스, 작동 및 마스터에 사용 되는 XAudio2 음성의 개요가 포함 되어 있습니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-callbacks">XAudio2 콜백</a></p></td>
<td align="left"><p>이 항목에서는 오디오 재생의 중단을 방지 하는 데 사용 되는 XAudio 2 콜백에 대해 설명 합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/audio-graphs">XAudio2 오디오 그래프</a></p></td>
<td align="left"><p>이 항목에서는 클라이언트에서 입력으로 오디오 스트림 집합을 사용 하 고, 처리 하 고, 최종 결과를 오디오 장치로 전달 하는 XAudio2 오디오 처리 그래프에 대해 설명 합니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-audio-effects">XAudio2 오디오 효과</a></p></td>
<td align="left"><p>이 항목에서는 들어오는 오디오 데이터를 사용 하 고 데이터에 대 한 일부 작업 (예: 반향 효과)을 전달 하기 전에이를 수행 하는 XAudio2 오디오 효과를 다룹니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-streaming-audio-data">XAudio2를 사용 하 여 오디오 데이터 스트리밍</a></p></td>
<td align="left"><p>이 항목에서는 XAudio2를 사용 하는 오디오 스트리밍을 다룹니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/x3daudio">X3DAudio</a></p></td>
<td align="left"><p>이 항목에서는 3D 공간의 점에서 오는 소리의 효과를 만들기 위해 XAudio2와 함께 사용 되는 API 인 X3DAudio에 대해 설명 합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/programming-reference">XAudio2 프로그래밍 참조</a></p></td>
<td align="left"><p>이 섹션에는 XAudio2 Api에 대 한 전체 참조가 포함 되어 있습니다.</p></td>
</tr>
</tbody>
</table>

### <a name="xaudio2-how-to-resources"></a>XAudio2 "방법" 리소스

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">항목</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--initialize-xaudio2">방법: XAudio2 초기화</a></p></td>
<td align="left"><p>XAudio2 엔진의 인스턴스를 만들고 마스터링 음성을 만들어 오디오 재생을 위해 XAudio2를 초기화 하는 방법에 대해 알아봅니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--load-audio-data-files-in-xaudio2">방법: XAudio2에서 오디오 데이터 파일 로드</a></p></td>
<td align="left"><p>XAudio2에서 오디오 데이터를 재생 하는 데 필요한 구조를 채우는 방법에 대해 알아봅니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--play-a-sound-with-xaudio2">방법: XAudio2를 사용 하 여 소리 재생</a></p></td>
<td align="left"><p>XAudio2에서 이전에 로드 한 오디오 데이터를 재생 하는 방법에 대해 알아봅니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--use-submix-voices">방법: 서브 믹스 음성 사용</a></p></td>
<td align="left"><p>음성 그룹을 설정 하 여 출력을 동일한 서브 믹스 음성으로 보내는 방법에 대해 알아봅니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--use-source-voice-callbacks">방법: 소스 음성 콜백 사용</a></p></td>
<td align="left"><p>XAudio2 원본 음성 콜백을 사용 하는 방법에 대해 알아봅니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--use-engine-callbacks">방법: 엔진 콜백 사용</a></p></td>
<td align="left"><p>XAudio2 엔진 콜백을 사용 하는 방법에 대해 알아봅니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--build-a-basic-audio-processing-graph">방법: 기본 오디오 처리 그래프 빌드</a></p></td>
<td align="left"><p>단일 마스터링 음성 및 단일 원본 음성에서 구성 된 오디오 처리 그래프를 만드는 방법에 대해 알아봅니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--dynamically-add-or-remove-voices-from-an-audio-graph">방법: 동적으로 오디오 그래프에서 음성 추가 또는 제거</a></p></td>
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--build-a-basic-audio-processing-graph">방법: 기본 오디오 처리 그래프 빌드</a>의 단계에 따라 만든 그래프에서 서브 믹스 음성을 추가 하거나 제거 하는 방법에 대해 알아봅니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--create-an-effect-chain">방법: 효과 체인 만들기</a></p></td>
<td align="left"><p>음성에 효과 체인을 적용 하 여 해당 음성에 대 한 오디오 데이터의 사용자 지정 처리를 허용 하는 방법을 알아봅니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--create-an-xapo">방법: XAPO 만들기</a></p></td>
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/xapo/nn-xapo-ixapo"><strong>IXAPO</strong></a> 를 구현 하 여 Xapo (XAudio2 audio processing object)를 만드는 방법에 대해 알아봅니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--add-run-time-parameter-support-to-an-xapo">방법: XAPO에 런타임 매개 변수 지원 추가</a></p></td>
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/xapo/nn-xapo-ixapoparameters"><strong>IXAPOParameters</strong></a> 인터페이스를 구현 하 여 xapo에 런타임 매개 변수 지원을 추가 하는 방법에 대해 알아봅니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--use-an-xapo-in-xaudio2">방법: XAudio2에서 XAPO 사용</a></p></td>
<td align="left"><p>XAudio2 효과 체인에서 XAPO로 구현 된 효과를 사용 하는 방법에 대해 알아봅니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--use-xapofx-in-xaudio2">방법: XAudio2에서 XAPOFX 사용</a></p></td>
<td align="left"><p>XAudio2 효과 체인에서 XAPOFX에 포함 된 효과 중 하나를 사용 하는 방법에 대해 알아봅니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--stream-a-sound-from-disk">방법: 디스크에서 소리 스트리밍</a></p></td>
<td align="left"><p>별도의 스레드를 만들어 오디오 버퍼를 읽고 콜백을 사용 하 여 해당 스레드를 제어 함으로써 XAudio2에서 오디오 데이터를 스트리밍하는 방법에 대해 알아봅니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--integrate-x3daudio-with-xaudio2">방법: X3DAudio와 XAudio2 통합</a></p></td>
<td align="left"><p>X3DAudio를 사용 하 여 XAudio2 음성의 볼륨 및 피치 값과 XAudio2 기본 제공 반향 효과에 대 한 매개 변수를 제공 하는 방법을 알아봅니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--group-audio-methods-as-an-operation-set">방법: 오디오 메서드를 작업 집합으로 그룹화</a></p></td>
<td align="left"><p>XAudio2 작업 집합을 사용 하 여 메서드 호출 그룹을 동시에 적용 하는 방법에 대해 알아봅니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/debugging-audio-glitches-in-xaudio2">XAudio2에서 오디오 결함 디버깅</a></p></td>
<td align="left"><p>XAudio2에 대 한 디버그 로깅 수준을 설정 하는 방법에 대해 알아봅니다.</p></td>
</tr>
</tbody>
</table>

### <a name="media-foundation-resources"></a>미디어 파운데이션 리소스

MF (미디어 파운데이션)는 오디오 및 비디오 재생을 스트리밍하는 미디어 플랫폼입니다. 미디어 파운데이션 Api를 사용 하 여 다양 한 알고리즘으로 인코딩 및 압축 된 오디오 및 비디오를 스트리밍할 수 있습니다. 실시간 게임 플레이 시나리오에는 적합 하지 않습니다. 대신, 오디오 및 비디오 구성 요소의 선형 캡처 및 표시를 위한 강력한 도구와 광범위 한 코덱을 지원 합니다.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">항목</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/medfound/about-the-media-foundation-sdk">미디어 파운데이션 정보</a></p></td>
<td align="left"><p>이 섹션에는 미디어 파운데이션 Api에 대 한 일반 정보와 이러한 Api를 지 원하는 데 사용할 수 있는 도구가 포함 되어 있습니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/medfound/media-foundation-programming--essential-concepts">미디어 파운데이션: 필수 개념</a></p></td>
<td align="left"><p>이 항목에서는 미디어 파운데이션 응용 프로그램을 작성 하기 전에 이해 해야 하는 몇 가지 개념을 소개 합니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/medfound/media-foundation-architecture">미디어 파운데이션 아키텍처</a></p></td>
<td align="left"><p>이 섹션에서는 Microsoft 미디어 파운데이션의 일반적인 디자인과 미디어 기본 형식 및이에서 사용 하는 처리 파이프라인에 대해 설명 합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/medfound/audio-video-capture">오디오/비디오 캡처</a></p></td>
<td align="left"><p>이 항목에서는 Microsoft 미디어 파운데이션를 사용 하 여 오디오 및 비디오 캡처를 수행 하는 방법에 대해 설명 합니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/medfound/audio-video-playback">오디오/비디오 재생</a></p></td>
<td align="left"><p>이 항목에서는 앱에서 오디오/비디오 재생을 구현 하는 방법에 대해 설명 합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/medfound/supported-media-formats-in-media-foundation">미디어 파운데이션에서 지원 되는 미디어 형식</a></p></td>
<td align="left"><p>이 항목에서는 Microsoft 미디어 파운데이션에서 기본적으로 지 원하는 미디어 형식을 나열 합니다. 제 3 자가 사용자 지정 플러그 인을 작성 하 여 추가 형식을 지원할 수 있습니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/medfound/encoding-and-file-authoring">인코딩 및 파일 제작</a></p></td>
<td align="left"><p>이 항목에서는 Microsoft 미디어 파운데이션를 사용 하 여 오디오 및 비디오 인코딩을 수행 하 고 미디어 파일을 제작 하는 방법에 대해 설명 합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/medfound/windows-media-codecs">Windows Media 코덱</a></p></td>
<td align="left"><p>이 항목에서는 Windows Media 오디오 및 비디오 코덱의 기능을 사용 하 여 압축 된 데이터 스트림을 생성 하 고 사용 하는 방법에 대해 설명 합니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/medfound/media-foundation-programming-reference">미디어 파운데이션 프로그래밍 참조</a></p></td>
<td align="left"><p>이 섹션에는 미디어 파운데이션 Api에 대 한 참조 정보가 포함 되어 있습니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/medfound/media-foundation-sdk-samples">미디어 파운데이션 SDK 샘플</a></p></td>
<td align="left"><p>Media Foundation 사용 방법을 보여 주는 샘플 앱을 제공합니다.</p></td>
</tr>
</tbody>
</table>

### <a name="windows-runtime-xaml-media-types"></a>Windows 런타임 XAML 미디어 형식

[Directx-xaml interop](/previous-versions/windows/apps/hh825871(v=win.10))를 사용 하는 경우 간단한 게임 시나리오를 위해 c + +와 함께 directx를 사용 하 여 Windows 런타임 XAML 미디어 API를 UWP 앱에 통합할 수 있습니다.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">항목</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement"><strong>Windows .Xaml.. x m l.</strong></a></p></td>
<td align="left"><p>오디오, 비디오 또는 둘 다를 포함 하는 개체를 나타내는 XAML 요소입니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/uwp/audio-video-camera/index">오디오 비디오 및 카메라</a></p></td>
<td align="left"><p>유니버설 Windows 플랫폼 (UWP) 앱에서 기본 오디오 및 비디오를 통합 하는 방법에 대해 알아봅니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/media-playback">MediaElement</a></p></td>
<td align="left"><p>UWP 앱에서 로컬로 저장 된 미디어 파일을 재생 하는 방법에 대해 알아봅니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/media-playback">MediaElement</a></p></td>
<td align="left"><p>UWP 앱에서 짧은 대기 시간으로 미디어 파일을 스트리밍하는 방법에 대해 알아봅니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/uwp/audio-video-camera/media-casting">미디어 캐스팅</a></p></td>
<td align="left"><p>Play To contract를 사용 하 여 UWP 앱에서 다른 장치로 미디어를 스트리밍하는 방법에 대해 알아봅니다.</p></td>
</tr>
</tbody>
</table>

## <a name="reference"></a>참조

-   [XAudio2 소개](/windows/desktop/xaudio2/xaudio2-introduction)
-   [XAudio2 프로그래밍 가이드](/windows/desktop/xaudio2/programming-guide)
-   [Microsoft 미디어 파운데이션 개요](/windows/desktop/medfound/microsoft-media-foundation-sdk)

## <a name="related-topics"></a>관련 항목

-   [XAudio2 프로그래밍 가이드](/windows/desktop/xaudio2/programming-guide)
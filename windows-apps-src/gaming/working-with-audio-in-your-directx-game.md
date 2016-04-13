---
title: 게임의 오디오
description: 음악 및 사운드를 개발하여 DirectX 게임에 통합하는 방법 및 오디오 신호를 처리하여 동적 및 위치 사운드를 만드는 방법을 알아봅니다.
ms.assetid: ab29297a-9588-c79b-24c5-3b94b85e74a8
---

# 게임의 오디오


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

음악 및 사운드를 개발하여 DirectX 게임에 통합하는 방법 및 오디오 신호를 처리하여 동적 및 위치 사운드를 만드는 방법을 알아봅니다.

오디오 프로그래밍의 경우 DirectX의 XAudio2 라이브러리를 사용하는 것이 좋으며, 여기서도 사용합니다. XAudio2는 게임에 신호 처리 및 믹싱 기초를 제공하는 하위 수준 오디오 라이브러리이며, 다양한 형식을 지원합니다.

또한 [Microsoft 미디어 파운데이션](https://msdn.microsoft.com/library/windows/desktop/ms694197)을 사용하여 간단한 오디오 및 음악 재생을 구현할 수 있습니다. Microsoft Media Foundation은 오디오 및 비디오의 미디어 파일과 스트림 재생을 위해 설계되었지만 게임에서 사용할 수도 있으며, 영화 장면이나 대화형이 아닌 게임 구성 요소에 특히 유용합니다.

## 개념 개요


다음은 이 섹션에서 사용되는 몇 가지 오디오 프로그래밍 개념입니다.

-   신호는 사운드 프로그래밍의 기본 단위이며, 그래픽의 픽셀과 유사합니다. 신호를 처리하는 DSP(디지털 신호 처리기)는 게임 오디오의 픽셀 셰이더와 유사합니다. 이 처리기는 신호를 변환하거나 결합하거나 필터링할 수 있습니다. DSP로 프로그래밍하면 게임의 사운드 효과 및 음악을 필요한 만큼 복잡하거나 덜 복잡하게 변경할 수 있습니다.
-   음성은 둘 이상의 신호가 서브믹스된 합성입니다. 원본, 서브믹스 및 마스터링 음성이라는 세 가지 유형의 XAudio2 음성 개체가 있습니다. 원본 음성은 클라이언트에서 제공한 오디오 데이터에서 작동합니다. 원본 및 서브믹스 음성은 해당 출력을 하나 이상의 서브믹스 또는 마스터링 음성으로 보냅니다. 서브믹스 및 마스터링 음성은 해당 음성을 공급하는 모든 음성으로부터 오디오를 믹싱하고 그 결과에서 작동합니다. 마스터링 음성은 오디오 데이터를 오디오 장치에 기록합니다.
-   믹싱은 사운드 효과 및 장면의 뒤에서 재생되는 배경 오디오 같은 여러 가지 별도의 음성을 단일 스트림으로 결합하는 프로세스입니다. 서브믹싱은 엔진 노이즈의 구성 요소 사운드 같은 여러 가지 별도의 음성을 결함하여 음성을 만드는 프로세스입니다.
-   오디오 형식. 음악 및 사운드 효과는 게임에 대한 다양한 디지털 형식으로 저장할 수 있습니다. WAV 같은 압축되지 않은 형식 및 MP3 및 OGG 같은 압축된 형식이 있습니다. 샘플이 더 많이 압축될 수록 품질은 저하됩니다. 일반적으로 비트 속도로 지정되는 경우 비트 속도가 낮을 수록 압축 데이터가 더 많이 손실됩니다. 품질은 압축 구조 및 비트 속도에 따라 달라질 수 있으므로 게임에 가장 적합한 압축을 찾으려면 실험해 보아야 합니다.
-   샘플 속도 및 품질. 사운드는 서로 다른 속도로 샘플링할 수 있으며 낮은 속도로 샘플링된 사운드의 품질이 훨씬 더 나쁩니다. CD 품질의 샘플 속도는 44.1Khz(44100Hz)입니다. 높은 품질의 사운드가 필요하지 않은 경우 낮은 샘플 속도를 선택할 수 있습니다. 속도가 높을수록 전문적인 오디오 응용 프로그램에 적합할 수 있지만 게임에 전문적인 품질의 사운드가 필요하지 않으면 사용할 필요가 없습니다.
-   사운드 송신기(또는 원본). XAudio2에서 사운드 송신기는 사운드를 방출하는 위치입니다. 여기서 사운드는 단순히 배경 노이즈의 삑하는 소리이거나 게임 내 주크박스에서 재생되는 락 음악 트랙일 수 있습니다. 송신기는 월드 좌표로 지정합니다.
-   사운드 수신기. 사운드 수신기는 플레이어이거나 고급 게임에서는 수신기로부터 받은 사운드를 처리하는 AI 엔터티일 수 있습니다. 해당 사운드를 오디오 스트림으로 서브믹싱하여 플레이어에게 재생하거나 수신기로 표시된 AI 가드 활성화 같이 게임 내에서 특정 작업을 수행하는 데 사용할 수 있습니다.

## 디자인 고려 사항


오디오는 게임 디자인 및 개발에서 엄청나게 중요한 부분입니다. 많은 게이머는 단순히 인상적인 사운드 트랙, 뛰어난 음성 작업 및 사운드 믹싱 또는 전체적으로 우수한 오디오 생성 때문에 전설적인 상태로 향상된 평범한 게임을 다시 즐길 수 있습니다. 음악 및 사운드는 게임의 개성을 정의하며 게임을 정의하는 주요 동기를 설정하고 다른 유사한 게임과 구분되도록 합니다. 게임의 오디오 프로필을 디자인하고 개발하는 데 들이는 노력은 매우 가치가 있습니다.

위치 3D 오디오는 3D 그래픽에서 제공하는 수준 이상으로 몰입을 추가할 수 있습니다. 월드를 시뮬레이트하거나 영화 스타일이 필요한 복잡한 게임을 개발하고 있는 경우 3D 위치 오디오 기술을 사용하여 플레이어가 실제로 몰입하도록 만들어 보세요.

## DirectX 오디오 개발 로드맵


### XAudio2 개념 리소스

XAudio2는 DirectX용 오디오 믹싱 라이브러리로, 주 용도는 게임용 고성능 오디오 엔진을 개발하는 것입니다. 자신의 게임에 소리 효과 및 배경 음악을 추가하고자 하는 개발자에게 XAudio2는 대기 시간이 짧고 동적 버퍼, 동기식 고밀도 샘플 재생, 암시적 원본 속도 변환을 지원하는 믹싱 엔진과 오디오 그래프를 제공합니다.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">항목</th>
<th align="left">설명</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[Introduction to XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415813)</p></td>
<td align="left"><p>XAudio2에서 지원되는 오디오 프로그래밍 기능의 목록을 제공합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Getting Started with XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415762)</p></td>
<td align="left"><p>주요 XAudio2 개념, XAudio2 버전 및 RIFF 오디오 형식에 대한 정보를 제공합니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Common Audio Programming Concepts](https://msdn.microsoft.com/library/windows/desktop/ee415692)</p></td>
<td align="left"><p>오디오 개발자가 숙지해야 할 일반적인 오디오 개념에 대한 개요를 제공합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[XAudio2 Voices](https://msdn.microsoft.com/library/windows/desktop/ee415825)</p></td>
<td align="left"><p>오디오 데이터 서브믹스, 조작 및 마스터링에 사용되는 XAudio2 음성에 대한 개요를 제공합니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[XAudio2 Callbacks](https://msdn.microsoft.com/library/windows/desktop/ee415745)</p></td>
<td align="left"><p>오디오 재생에서 중단을 방지하는 데 사용되는 XAudio2 콜백에 대해 설명합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[XAudio2 Audio Graphs](https://msdn.microsoft.com/library/windows/desktop/ee415739)</p></td>
<td align="left"><p>클라이언트에서 오디오 스트립 집합을 입력으로 가져와 처리하고, 오디오 디바이스에 최종 결과를 전달하는 XAudio2 오디오 처리 그래프에 대해 설명합니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[XAudio2 Audio Effects](https://msdn.microsoft.com/library/windows/desktop/ee415756)</p></td>
<td align="left"><p>들어오는 오디오 데이터를 가져와 전달하기 전에 반향 효과 같은 특정 작업을 데이터에 수행하는 XAudio2 오디오 효과에 대해 설명합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Streaming Audio Data with XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415821)</p></td>
<td align="left"><p>XAudio2를 사용한 오디오 데이터 스트리밍에 대해 설명합니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[X3DAudio](https://msdn.microsoft.com/library/windows/desktop/ee415714)</p></td>
<td align="left"><p>3D 공간의 지점에서 발생하는 소리의 환상 효과를 만들기 위해 XAudio2와 함께 사용되는 API인 X3DAudio에 대해 설명합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[XAudio2 Programming Reference](https://msdn.microsoft.com/library/windows/desktop/ee415899)</p></td>
<td align="left"><p>XAudio2 API에 대한 전체 참조를 제공합니다.</p></td>
</tr>
</tbody>
</table>

 

### XAudio2 "방법" 리소스

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">항목</th>
<th align="left">설명</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[How to: Initialize XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415779)</p></td>
<td align="left"><p>XAudio2 엔진의 인스턴스를 만들고, 마스터링 음성을 만들어 오디오 재생을 위해 XAudio2를 초기화하는 방법을 알아봅니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[How to: Load Audio Data Files in XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415781)</p></td>
<td align="left"><p>XAudio2에서 오디오 데이터를 재생하는 데 필요한 구조를 채우는 방법을 알아봅니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[How to: Play a Sound with XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415787)</p></td>
<td align="left"><p>이전에 로드한 오디오 데이터를 XAudio2에서 재생하는 방법을 알아봅니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[How to: Use Submix Voices](https://msdn.microsoft.com/library/windows/desktop/ee415794)</p></td>
<td align="left"><p>동일한 서브믹스 음성에 출력을 보내도록 음성 그룹을 설정하는 방법을 알아봅니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[How to: Use Source Voice Callbacks](https://msdn.microsoft.com/library/windows/desktop/ee415769)</p></td>
<td align="left"><p>XAudio2 원본 음성 콜백을 사용하는 방법을 알아봅니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[How to: Use Engine Callbacks](https://msdn.microsoft.com/library/windows/desktop/ee415774)</p></td>
<td align="left"><p>XAudio2 엔진 콜백을 사용하는 방법을 알아봅니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[How to: Build a Basic Audio Processing Graph](https://msdn.microsoft.com/library/windows/desktop/ee415767)</p></td>
<td align="left"><p>단일 마스터링 음성 및 단일 원본 음성에서 구성된 오디오 처리 그래프를 만드는 방법을 알아봅니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[How to: Dynamically Add or Remove Voices From an Audio Graph](https://msdn.microsoft.com/library/windows/desktop/ee415772)</p></td>
<td align="left"><p>[
            How to: Build a Basic Audio Processing Graph](https://msdn.microsoft.com/library/windows/desktop/ee415767)의 각 단계를 수행하여 만든 그래프에서 서브믹스 음성을 추가 또는 제거하는 방법을 알아봅니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[How to: Create an Effect Chain](https://msdn.microsoft.com/library/windows/desktop/ee415789)</p></td>
<td align="left"><p>음성에 효과 체인을 적용하여 해당 음성의 오디오 데이터에 대한 사용자 지정 처리를 허용하는 방법을 알아봅니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[How to: Create an XAPO](https://msdn.microsoft.com/library/windows/desktop/ee415730)</p></td>
<td align="left"><p>[<strong>IXAPO</strong>](https://msdn.microsoft.com/library/windows/desktop/ee415893)를 구현하여 XAPO(XAudio2 오디오 처리 개체)를 만드는 방법을 알아봅니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[How to: Add Run-time Parameter Support to an XAPO](https://msdn.microsoft.com/library/windows/desktop/ee415728)</p></td>
<td align="left"><p>[<strong>IXAPOParameters</strong>](https://msdn.microsoft.com/library/windows/desktop/ee415896) 인터페이스를 구현하여 XAPO에 런타임 매개 변수 지원을 추가하는 방법을 알아봅니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[How to: Use an XAPO in XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415733)</p></td>
<td align="left"><p>XAudio2 효과 체인에서 XAPO로 구현된 효과를 사용하는 방법을 알아봅니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[How to: Use XAPOFX in XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415723)</p></td>
<td align="left"><p>XAudio2 효과 체인에서 XAPOFX에 포함된 효과 중 하나를 사용하는 방법을 알아봅니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[How to: Stream a Sound from Disk](https://msdn.microsoft.com/library/windows/desktop/ee415791)</p></td>
<td align="left"><p>오디오 버퍼를 읽을 별도의 스레드를 만들어 XAudio2에서 오디오 데이터를 스트리밍하고, 이 스레드를 제어하는 콜백을 사용하는 방법을 알아봅니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[How to: Integrate X3DAudio with XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415798)</p></td>
<td align="left"><p>X3DAudio를 사용하여 XAudio2에 대한 볼륨 및 피치 값과 XAudio2 기본 제공 반향 효과에 대한 매개 변수를 제공하는 방법을 알아봅니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[How to: Group Audio Methods as an Operation Set](https://msdn.microsoft.com/library/windows/desktop/ee415783)</p></td>
<td align="left"><p>XAudio2 작업 집합을 사용하여 동시에 적용되는 메서드 호출 집합을 만드는 방법을 알아봅니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Debugging Audio Glitches in XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415765)</p></td>
<td align="left"><p>XAudio2에 대한 디버그 로깅 수준을 설정하는 방법을 알아봅니다.</p></td>
</tr>
</tbody>
</table>

 

### Media Foundation 리소스

MF(Media Foundation)는 오디오 및 비디오 재생을 스트리밍하기 위한 미디어 플랫폼입니다. Media Foundation API를 사용하면 다양한 알고리즘을 통해 인코딩 및 압축된 오디오와 비디오를 스트리밍할 수 있습니다. 실시간 게임 플레이 시나리오에는 적합하지 않지만, 오디오 및 동영상 구성 요소의 선형 캡처와 표현을 위한 강력한 도구와 광범위한 코덱 지원을 제공합니다.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">항목</th>
<th align="left">설명</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[About Media Foundation](https://msdn.microsoft.com/library/windows/desktop/ms696274)</p></td>
<td align="left"><p>Media Foundation API 및 이를 지원하는 데 사용할 수 있는 도구에 대한 일반적인 정보를 제공합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Media Foundation: Essential Concepts](https://msdn.microsoft.com/library/windows/desktop/ee663601)</p></td>
<td align="left"><p>Media Foundation 응용 프로그램을 작성하기 전에 이해해야 할 개념을 소개합니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Media Foundation Architecture](https://msdn.microsoft.com/library/windows/desktop/ms696219)</p></td>
<td align="left"><p>Microsoft 미디어 파운데이션의 일반적인 디자인과, 사용하는 미디어 primitive 및 처리 파이프라인에 대해 설명합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Audio/Video Capture](https://msdn.microsoft.com/library/windows/desktop/dd317910)</p></td>
<td align="left"><p>Microsoft 미디어 파운데이션을 사용하여 오디오 및 비디오 캡처를 수행하는 방법에 대해 설명합니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Audio/Video Playback](https://msdn.microsoft.com/library/windows/desktop/dd317914)</p></td>
<td align="left"><p>앱에서 오디오/비디오 재생을 구현하는 방법에 대해 설명합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Supported Media Formats in Media Foundation](https://msdn.microsoft.com/library/windows/desktop/dd757927)</p></td>
<td align="left"><p>Microsoft 미디어 파운데이션이 기본적으로 지원하는 미디어 형식을 보여 줍니다. (타사에서는 사용자 지정 플러그 인을 작성하여 추가 형식을 지원할 수 있습니다.)</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Encoding and File Authoring](https://msdn.microsoft.com/library/windows/desktop/dd318778)</p></td>
<td align="left"><p>Microsoft 미디어 파운데이션을 사용하여 오디오 및 비디오 인코딩을 수행하고 미디어 파일을 작성하는 방법에 대해 설명합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Windows Media Codecs](https://msdn.microsoft.com/library/windows/desktop/ff819508)</p></td>
<td align="left"><p>Windows Media 오디오 및 비디오 코덱의 기능을 사용하여 압축된 데이터 스트림을 생성 및 사용하는 방법에 대해 설명합니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Media Foundation Programming Reference](https://msdn.microsoft.com/library/windows/desktop/ms704847)</p></td>
<td align="left"><p>Media Foundation API에 대한 참조 정보를 제공합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Media Foundation SDK Samples](https://msdn.microsoft.com/library/windows/desktop/aa371827)</p></td>
<td align="left"><p>Media Foundation 사용 방법을 보여 주는 샘플 앱을 제공합니다.</p></td>
</tr>
</tbody>
</table>

 

### Windows 런타임 XAML 미디어 유형

[DirectX-XAML interop](https://msdn.microsoft.com/library/windows/apps/hh825871)을 사용 중이면 비교적 간단한 게임 시나리오인 경우 DirectX 및 C++를 사용하여 Windows 런타임 XAML 미디어 API를 Windows 스토어 앱에 통합할 수 있습니다.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">항목</th>
<th align="left">설명</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[<strong>Windows.UI.Xaml.Controls.MediaElement</strong>](https://msdn.microsoft.com/library/windows/apps/br242926)</p></td>
<td align="left"><p>오디오, 비디오 또는 둘 다가 포함된 개체를 나타내는 XAML 요소입니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Audio, video, and camera](https://msdn.microsoft.com/library/windows/apps/mt203788)</p></td>
<td align="left"><p>UWP(유니버설 Windows 플랫폼) 앱에 기본 오디오 및 비디오를 포함하는 방법을 알아봅니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[MediaElement](https://msdn.microsoft.com/library/windows/apps/mt187272)</p></td>
<td align="left"><p>UWP 앱에서 로컬로 저장된 미디어 파일을 재생하는 방법을 알아봅니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[MediaElement](https://msdn.microsoft.com/library/windows/apps/mt187272)</p></td>
<td align="left"><p>UWP 앱에서 대기 시간이 짧은 미디어 파일을 스트리밍하는 방법을 알아봅니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Media casting](https://msdn.microsoft.com/library/windows/apps/mt282143)</p></td>
<td align="left"><p>재생 계약을 사용하여 UWP 앱에서 다른 디바이스로 미디어를 스트리밍하는 방법을 알아봅니다.</p></td>
</tr>
</tbody>
</table>

 

## 참조


-   [XAudio2 소개](https://msdn.microsoft.com/library/windows/desktop/ee415813)
-   [XAudio2 프로그래밍 지침](https://msdn.microsoft.com/library/windows/desktop/ee415737)
-   [Microsoft Media Foundation 개요](https://msdn.microsoft.com/library/windows/desktop/ms694197)

> **참고**  
이 문서는 UWP(유니버설 Windows 플랫폼) 앱을 작성하는 Windows 10 개발자용입니다. Windows 8.x 또는 Windows Phone 8.x를 개발하는 경우 [보관된 문서](http://go.microsoft.com/fwlink/p/?linkid=619132)를 참조하세요.

 

## 관련 항목


-   [XAudio2 프로그래밍 지침](https://msdn.microsoft.com/library/windows/desktop/ee415737)

 

 






<!--HONumber=Mar16_HO1-->



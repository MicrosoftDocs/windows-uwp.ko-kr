---
description: UWP 앱을 개발하는 방법을 알아봅니다.
title: UWP 앱 개발
keywords: Uwp 앱 개발 스레딩 비동기 플랫폼 개요 포털 개발 개발자
ms.date: 03/29/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 233666294555c46b5ba8b1e558eb32d6aed84e2a
ms.sourcegitcommit: 08cb5a4ca2e02179ad6b768c841fe3d5216bcae3
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80614960"
---
# <a name="develop-uwp-apps"></a>UWP 앱 개발

Windows 10용 UWP 앱을 만드는 방법 문서와 코드입니다.

:::row:::
    :::column:::
        <a href="/windows/uwp/get-started/universal-application-platform-guide">
            <img src="https://docs.microsoft.com//media/hubs/windows/win_developer-uwp.svg" alt="UWP overview" />
        </a><br/>
        <h3 style="margin-top: 10px; margin-bottom: 0px"><a href="/windows/uwp/get-started/universal-application-platform-guide">유니버설 Windows 플랫폼 개요</a></h3>
        <p style="margin-top: 0px; margin-bottom: 50px">UWP의 정의, 작동 방식 및 UWP에서 제공하는 기능에 대해 설명합니다.</p>
    :::column-end:::
    :::column:::
        <a href="/windows/uwp/porting/index">
            <img src="https://docs.microsoft.com/media/illustrations/teams-fast-track.svg" alt="Porting guide" />
        </a><br/>
        <h3 style="margin-top: 10px; margin-bottom: 0px"><a href="/windows/uwp/porting/index">포팅 가이드</a></h3>
        <p style="margin-top: 0px; margin-bottom: 50px">기존 Windows Forms, WPF, Android 또는 iOS 앱을 UWP에 가져옵니다.</p>
    :::column-end:::
:::row-end:::

<!-- <ul class="panelContent cardsH" style="margin-left: 1px">
    <li>
        <a href="/windows/uwp/get-started/universal-application-platform-guide" style="display:block">
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage" style="background-color: #f2f2f2">                 
                            <img src="https://docs.microsoft.com//media/hubs/windows/win_developer-uwp.svg" alt=" "/>
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>Overview of the Universal Windows Platform</h3>
                        <p>An explanation of what UWP is, how it works, and the features it provides.</p>
                    </div>
                </div>
            </div>
        </div>
        </a>
    </li>
    <li>
        <a href="/windows/uwp/porting/index" style="display:block">
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage" style="background-color: #f2f2f2">                
                            <img src="https://docs.microsoft.com/media/illustrations/teams-fast-track.svg" alt=" " />
                        </div>
                    </div>                
                    <div class="cardText">
                        <h3>Porting guide</h3>
                        <p>Bring your existing Windows Forms, WPF, Android, or iOS app to UWP. </p>
                    </div>
                </div>
            </div>
        </div>
        </a>
    </li>                 
</ul> -->

## <a name="api-reference"></a>API 참조

:::row:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px"><a href="/uwp/api">Windows UWP 네임스페이스</a></h3>
        <p style="margin-top: 0px; margin-bottom: 50px">네임스페이스별로 구성된 Windows 런타임을 구성하는 클래스, 구조체, 인터페이스, 메서드, 속성 및 이벤트입니다.</p>
    :::column-end:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px"><a href="/uwp/schemas/">UWP용 스키마</a></h3>
        <p style="margin-top: 0px; margin-bottom: 50px">UWP(유니버설 Windows 플랫폼) 앱용 파일 및 XML 스키마 사양입니다.</p>
    :::column-end:::
:::row-end:::

<!-- <ul class="panelContent cardsH" style="margin-left: 1px">
    <li>
        <a href="/uwp/api" style="display:block">
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
                        <h3>Windows UWP namespaces</h3>
                        <p>The classes, structures, interfaces, methods, properties, and events that make up the Windows Runtime, organized by namespace.</p>
                    </div>
                </div>
            </div>
        </div>
        </a>
    </li>
    <li>
        <a href="/uwp/schemas/" style="display:block">
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
                        <h3>Schemas for UWP</h3>
                        <p>File and XML schema specifications for Universal Windows Platform (UWP) apps. </p>
                    </div>
                </div>
            </div>
        </div>
        </a>
    </li>                 
</ul> -->

## <a name="articles"></a>문서

:::row:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px">앱 유형</h3>
        <a href="/windows/uwp/apps-for-education/">교육용 앱</a><br/>
        <a href="/windows/uwp/enterprise/">엔터프라이즈 앱</a><br/>
        <a href="/windows/uwp/gaming/">게임 및 DirectX 앱</a><br/>
        <a href="/microsoft-edge/progressive-web-apps">프로그레시브 웹앱</a><br/>
    :::column-end:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px">앱 UI</h3>
        <a href="https://developer.microsoft.com/windows/apps/design">컨트롤, 레이아웃, 입력 체계, 애니메이션, 유용성 및 UI 디자인은 디자인 및 UI 섹션을 참조하세요.</a><br/>
    :::column-end:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px">통신</h3>
        <a style="display:block" href="/windows/uwp/app-to-app/">앱 간 통신</a><br/>
        <a style="display:block" href="/windows/uwp/networking/">네트워킹 및 웹 서비스</a><br/>
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px">데이터 및 파일</h3>
        <a href="/windows/uwp/audio-video-camera/">오디오 비디오 및 카메라</a><br/>
        <a href="/windows/uwp/data-access/" style="display:block" >데이터 액세스</a><br/>
        <a href="/windows/uwp/data-binding/"style="display:block" >데이터 바인딩</a><br/>
        <a href="/windows/uwp/files/" style="display:block" >파일, 폴더 및 라이브러리</a><br/>
        <a href="/windows/uwp/machine-learning/">기계 학습</a><br/>
    :::column-end:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px">배포</h3>
        <a href="/windows/uwp/updates-and-versions/choose-a-uwp-version">UWP 버전 선택</a><br/>
        <a href="/windows/uwp/debug-test-perf/">디버깅, 테스트 및 성능</a><br/>
        <a href="/windows/uwp/monetize/">수익 창출, 참여 및 Store 서비스</a><br/>
        <a href="/windows/uwp/packaging/">앱 패키징</a><br/>
    :::column-end:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px">플랫폼</h3>
        <a href="/windows/uwp/cpp-and-winrt-apis/">C++/WinRT<</a><br/>
        <a href="/windows/uwp/launch-resume/">실행, 다시 시작 및 백그라운드 작업</a><br/>
        <a href="/windows/uwp/security/">보안</a><br/>
        <a href="/windows/uwp/threading-async/">스레딩 및 비동기 프로그래밍</a><br/>
        <a href="/windows/uwp/composition/visual-layer">시각적 계층</a><br/>
        <a href="/windows/uwp/updates-and-versions/application-development-for-windows-as-a-service">WaaS(Windows as a Service)</a><br/>
        <a href="/windows/uwp/winrt-components/">Windows 런타임 구성 요소</a><br/>
        <a href="/windows/uwp/xaml-platform/">XAML 플랫폼</a><br/>
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px">사람 및 장소</h3>
        <a href="/windows/uwp/contacts-and-calendar/">연락처, 내 피플 및 일정</a><br/>
        <a href="/windows/uwp/maps-and-location/">지도 및 위치</a><br/>
    :::column-end:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px">주변 장치, 센서 및 전원</h3>
        <a href="/windows/uwp/contacts-and-calendar/">개요</a><br/>
        <a href="/windows/uwp/devices-sensors/enable-device-capabilities">디바이스 기능 사용</a><br/>
        <a href="/windows/uwp/devices-sensors/pair-devices">디바이스 페어링</a><br/>
        <a href="/windows/uwp/devices-sensors/point-of-service">서비스 지점</a><br/>
        <a href="/windows/uwp/devices-sensors/sensors">센서</a><br/>
        <a href="/windows/uwp/devices-sensors/printing-and-scanning">인쇄</a><br/>
        <a href="/windows/uwp/devices-sensors/3d-printing">3 차원 인쇄<</a><br/>
        <a href="/windows/uwp/devices-sensors/nfc">NFC</a><br/>
        <a href="/windows/uwp/devices-sensors/get-battery-info">배터리 정보</a><br/>
    :::column-end:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px">포팅</h3>
        <a href="/windows/uwp/porting/">개요</a><br/>
        <a href="/windows/uwp/porting/wpsl-to-uwp-root">Windows Phone Silverlight에서 UWP로 이동</a><br/>
        <a href="/windows/uwp/porting/w8x-to-uwp-root">Windows 런타임 8.x에서 UWP로 이동</a><br/>
        <a href="/windows/uwp/porting/desktop-to-uwp-root">데스크톱 브리지</a><br/>
        <a href="/windows/uwp/porting/desktop-to-uwp-migrate">데스크톱 및 UWP 간의 코드 공유</a><br/>
        <a href="/windows/uwp/porting/android-ios-uwp-map">Android 및 iOS 개발자용 개념 매핑</a><br/>
        <a href="/windows/uwp/porting/ios-to-uwp-root">iOS에서 UWP로 이동</a><br/>
        <a href="/microsoft-edge/progressive-web-apps">웹앱을 PWA로 변환</a><br/>
        <a href="/windows/uwp/porting/apps-on-arm">ARM 기반 Windows 10</a><br/>
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px">프로세스 및 스레딩</h3>
        <a href="/windows/uwp/launch-resume/">실행, 다시 시작 및 백그라운드 작업</a><br/>
        <a href="/windows/uwp/threading-async/">스레딩 및 비동기 프로그래밍</a><br/><br/><br/>
    :::column-end:::
:::row-end:::


 ## <a name="samples-and-tools"></a>샘플 및 도구

 :::row:::
    :::column:::
        <a href="https://developer.microsoft.com/windows/samples">
            <img src="https://docs.microsoft.com/media/illustrations/sql-database-develop.svg" alt="Samples" />
        </a><br/>
        <h3 style="margin-top: 10px; margin-bottom: 0px"><a href="https://developer.microsoft.com/windows/samples">샘플</a></h3>
        <p style="margin-top: 0px; margin-bottom: 50px">이러한 샘플을 실험하여 멋진 Windows용 앱을 빌드하는 방법을 알아봅니다. 이러한 샘플은 기능의 작동 방식을 보여 주고, 사용자 고유의 UWP 앱을 바로 시작하는 데 도움이 됩니다.</p>
    :::column-end:::
    :::column:::
        <a href="https://developer.microsoft.com/windows/downloads">
            <img src="https://docs.microsoft.com/media/illustrations/sql-get-started-download.svg" alt="Developer tools" />
        </a><br/>
        <h3 style="margin-top: 10px; margin-bottom: 0px"><a href="https://developer.microsoft.com/windows/downloads">개발자 도구</a></h3>
        <p style="margin-top: 0px; margin-bottom: 50px">Visual Studio 2019, Windows 10 SDK 및 다른 개발자 도구를 가져옵니다.</p>
    :::column-end:::
:::row-end:::

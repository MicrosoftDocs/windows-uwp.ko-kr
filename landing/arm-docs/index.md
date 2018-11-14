---
layout: LandingPage
description: 이 페이지는 ARM64 win32 및 UWP 앱 개발 시작 수에 대 한 정보를 제공 합니다.
title: ARM 기반 Windows 10
author: msatranjr
ms.author: misatran
ms.date: 05/08/2018
ms.localizationpriority: medium
ms.topic: article
keywords: ARM, ARM, ARM64 드라이버 빌드 win32 ARM64 앱 빌드에 Windows 10
ms.openlocfilehash: 83f2a0d03040a682e6965558174294fe27e21bfb
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6454426"
---
# <a name="windows-10-on-arm"></a>ARM 기반 Windows 10
Windows 10 ARM 프로세서로 구동 되는 Pc에서 실행 됩니다. 이 페이지는 앱 개발 시작 및 플랫폼에 대해 알아볼 수에 대 한 정보를 제공 합니다. 또한 페이지의 맨 아래에 있는 링크를 사용 하 여 피드백을 제공 하기 위해 새.

## <a name="introductory-videos"></a>소개 동영상
시청 및 ARM에서 Windows 10을 실행 하는 방법을 알아봅니다.

<ul class="cols cols3">
    <li>
        <a href="https://youtu.be/OZtVBDeVqCE"><img alt="Building ARM64 Win32 C++ apps video" src="./images/Arm64Scaled.png" /></a>
        <h3>ARM64 Win32 c + + 앱 빌드</h3><p>Visual Studio에 대 한 a r m 64 도구를 설치 하는 방법을 알아봅니다. 그런 다음 살펴보겠습니다 하면 만들고 새 ARM 64 프로젝트를 컴파일하는 단계를 합니다.</p>
    </li>
    <li>
        <a href="https://channel9.msdn.com/Events/Build/2018/BRK2438"><img alt="Build 2018 Windows 10 on ARM for developers" src="./images/buildVideoStillScaled.png" /></a>
        <h3>개발자를 위한 arm 빌드 2018 Windows 10</h3><p>알아봅니다 Windows 10에 ARM 장치 어떻게 x86 매직 에뮬레이션 작동 하며 마지막 제출 및 ARM에서 Windows 10 용 앱을 작성 하는 방법. 데스크톱과 UWP에 대 한 a r m 64 앱을 빌드하는 방법을 보여 줄 것입니다.</p>
    </li>
    <li>
        <a href="https://channel9.msdn.com/Events/Ch9Live/Windows-Community-Standup/Kevin-Gallo-January-2018"><img alt="Community standup video featuring Kevin Gallo" src="./images/communityStandupStillScaled.png" /></a>
        <h3>황영순 Gallo를 사용 하 여 스탠드업 Windows 커뮤니티</h3><p>Arm64 기반, Windows 10을 실행 하는 방법에 대 한 깊은 이해를 가져오고 앱이이 플랫폼에서 경험에 대 한 느낌을 확인 합니다.</p>
    </li>
</ul>

## <a name="understanding-windows-10-on-arm"></a>Windows 10 arm 이해
이러한 리소스를 살펴보면 플랫폼을 알아야 가져옵니다.

<ul class="cardsF panelContent cols cols2">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <a class="card" href="/windows/uwp/porting/apps-on-arm" title="시작" data-linktype="absolute-path">
                    <div class="cardImageOuter">
                            <img class="cardImage" role="presentation" alt="Get started icon" src="/media/common/i_get-started.svg" data-linktype="external" />
                    </div>
                </a>
                <div class="cardText">
                    <h3>Arm 기반 Windows 10 시작</h3>
                    <p class="x-hidden-focus">기본 사항을 이해 하려면 설명서를 확인 합니다.</p>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <a class="card" href="/windows/uwp/porting/apps-on-arm-x86-emulation" title="항목 약 x86 에뮬레이션" data-linktype="absolute-path">
                    <div class="cardImageOuter">
                             <img class="cardImage" role="presentation" alt="x86 emulation icon" src="/media/common/i_advanced.svg" data-linktype="external" />
                    </div>
                </a>
                <div class="cardText">
                    <h3>자세한 방법 x86 에뮬레이션 작동</h3>
                    <p class="x-hidden-focus">Arm 기반 Windows 10의 주요이 기능에 대 한 모두에 대해 알아봅니다.</p>
                </div>
            </div>
        </div>
    </li>
    <!--<li>
        <div class="cardSize">
            <div class="cardPadding">
                <a class="card" href="https://blogs.msdn.microsoft.com/harip/" data-linktype="absolute-path">
                    <div class="cardImageOuter">
                            <img class="cardImage" role="presentation" alt="" src="/media/common/i_blog.svg?branch=master" data-linktype="external" />
                            </a>
                    </div>
                </a>
                <div class="cardText">
                    <h3>Read the Kernel blog</h3>
                    <p class="x-hidden-focus">Get a deep understanding of the Windows by reading articles that are written by the creators of the kernel.</p>
                </div>
            </div>
        </div>
    </li>-->
</ul>

## <a name="developing-for-windows-10-on-arm"></a>Arm 기반 Windows 10 용 개발
ARM에서 Windows 10으로 앱을 조정 시작 하 고 사용할 수 있는 기능을 있습니다 활용 합니다.  

<ul class="cardsF panelContent cols cols3">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <a class="card" href="https://blogs.windows.com/buildingapps/?p=52087" title="ARM64 앱 빌드 블로그" data-linktype="absolute-path">
                    <div class="cardImageOuter">
                            <img class="cardImage" role="presentation" alt="Build ARM64 Win32 apps blog icon" src="/media/common/i_build.svg" data-linktype="external" />
                    </div>
                    </a>
                <div class="cardText">
                    <h3>SDK 사용 하 여 ARM64 앱 빌드</h3>
                    <p class="x-hidden-focus">여기서 우리 안내 arm 기반 Windows 10에서 기본적으로 실행 되도록 a r m 64로 앱 컴파일이 블로그 게시물을 확인 합니다.</p>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <a class="card" href="/windows/uwp/porting/apps-on-arm-troubleshooting-arm32" title="Arm32 앱 문제 해결" data-linktype="absolute-path">
                    <div class="cardImageOuter">
                            <img class="cardImage" role="presentation" alt="UWP apps on ARM icon" src="/media/common/i_code-edit.svg" data-linktype="external" />
                    </div>
                </a>
                <div class="cardText">
                    <h3>ARM에서 UWP 앱</h3>
                    <p class="x-hidden-focus">성공에 대해 유니버설 Windows 플랫폼 (UWP) 앱을 설정 하려면이 지침을 따릅니다.</p>                    
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <a class="card" href="/windows-hardware/drivers/debugger/debugging-arm64" title="ARM64 앱 디버깅" data-linktype="absolute-path">
                    <div class="cardImageOuter">
                             <img class="cardImage" role="presentation" alt="Debugging on ARM icon" src="/media/common/i_debug.svg" data-linktype="external" />
                    </div>
                </a>
                <div class="cardText">
                    <h3>Arm 기반 디버깅</h3>
                    <p class="x-hidden-focus">Arm 기반 Windows 10에서 원활 하 게 실행 코드를 가져옵니다.</p>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <a class="card" href="/windows-hardware/drivers/develop/building-arm64-drivers" title="ARM64 드라이버 빌드" data-linktype="absolute-path">
                    <div class="cardImageOuter">
                            <img class="cardImage" role="presentation" alt="Building ARM64 drivers icon" src="/media/common/i_drivers.svg" data-linktype="external" />
                            </a>
                    </div>
                </a>
                <div class="cardText">
                    <h3>WDK를 사용하여 ARM64 드라이버 빌드</h3>
                    <p class="x-hidden-focus">A r m 64에 대 한 드라이버를 다시 컴파일해야 합니다.</p>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <a class="card" href="/windows/uwp/porting/apps-on-arm-troubleshooting-x86" title="X86 문제 해결 앱" data-linktype="absolute-path">
                    <div class="cardImageOuter">
                            <img class="cardImage" role="presentation" alt="x86 apps on ARM icon" src="/media/common/i_code-blocks.svg" data-linktype="external" />
                            </a>
                    </div>
                </a>
                <div class="cardText">
                    <h3>x86 arm 기반 앱</h3>
                    <p class="x-hidden-focus">개발 x86 앱 들은 최선을 다하고 arm 기반 Windows 10에서 수행할 수 있습니다.</p>
                </div>
            </div>
        </div>
    </li>
</ul>

<!--## Other videos
<ul class="cols cols4">
<li>
        <a href="#"><img alt="" src="./images/dummyStillScaled.png" /></a>
            <p>TBD</p>    
    </li>
<li>
        <a href="#"><img alt="" src="./images/dummyStillScaled.png" /></a>
            <p>TBD</p>    
    </li>
<li>
        <a href="#"><img alt="" src="./images/dummyStillScaled.png" /></a>
            <p>TBD</p>    
    </li>
<li>
        <a href="#"><img alt="" src="./images/dummyStillScaled.png" /></a>
            <p>TBD</p>    
    </li>
</ul>-->

## <a name="let-us-know-if-you-have-feedback"></a>알려주세요 피드백이 있는 경우
우리는 기존 고객의 의견을 활용 하 여 제품을 지속적으로 향상 합니다. 이해 문제가에서 멈춤 또는 어떻게 제대로 공유 하려는 경우 환경에는 이러한 링크는 데 도움이 됩니다.

<ul class="cardsM cols cols3">
<li>
        <a class="card" href="feedback-hub://?tabid=2&contextid=803" data-linktype="absolute-path">
            <img class="cardImage" role="presentation" alt="Feedback hub icon" src="/media/common/i_feedback.svg" data-linktype="external" />
            <div class="cardText">
                <h3>피드백 허브를 사용 하 여</h3>
                <p>않은 우리 누락 문제가 있나요? 아이디어가 있으십니까? 피드백 허브에 알려 주십시오.</p>
            </div>
        </a>
    </li>
    <li>
        <a class="card" href="mailto:woafeedback@microsoft.com" data-linktype="absolute-path">
            <img class="cardImage" role="presentation" alt="Report a bug icon" src="/media/common/i_mail.svg" data-linktype="external" />
            <div class="cardText">
                <h3>버그를 보고 합니다.</h3>
                <p>플랫폼에서 버그를 찾을 수 있나요? 세부 정보를 사용 하 여 전자 메일 보내기.</p>
            </div>
        </a>
    </li>
    <li>
        <a class="card" href="https://github.com/MicrosoftDocs/windows-uwp/tree/docs/landing/arm-docs" data-linktype="absolute-path">
            <img class="cardImage" role="presentation" alt="Give doc feedback icon" src="/media/common/i_form.svg" data-linktype="external" />
            <div class="cardText">
                <h3>문서 피드백 제공</h3>
                <p>우리의 문서를 사용 하 여 문제를 발견 했습니다. 명확 하 게 하는 것을 만들기 위해 하 시겠습니까? 우리의 문서 GitHub 리포지토리에서 문제를 만듭니다.</p>
            </div>
        </a>
    </li>
</ul>
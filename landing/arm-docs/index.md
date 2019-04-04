---
layout: LandingPage
description: ARM64 win32 및 UWP 앱 개발을 시작하는 데 필요한 정보를 제공합니다.
title: ARM 기반 Windows 10
author: msatranjr
ms.author: misatran
ms.date: 05/08/2018
ms.localizationpriority: medium
ms.topic: article
keywords: ARM 기반 Windows 10, ARM, win32 ARM64 앱 빌드, ARM64 드라이버 빌드
ms.openlocfilehash: 83f2a0d03040a682e6965558174294fe27e21bfb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57583361"
---
# <a name="windows-10-on-arm"></a>ARM 기반 Windows 10
Windows 10은 ARM 프로세서로 구동되는 PC에서 실행됩니다. 여기서는 플랫폼에 대해 자세히 알아보고 앱 개발을 시작할 수 있는 정보를 제공합니다. 또한 페이지 아래쪽의 링크를 사용하여 피드백을 보내주세요.

## <a name="introductory-videos"></a>소개 비디오
Windows 10이 ARM에서 실행되는 방법을 시청하고 알아봅니다.

<ul class="cols cols3">
    <li>
        <a href="https://youtu.be/OZtVBDeVqCE"><img alt="Building ARM64 Win32 C++ apps video" src="./images/Arm64Scaled.png" /></a>
        <h3>ARM64 Win32 C++ 앱 빌드</h3><p>Visual Studio용 ARM64 도구를 설치하는 방법에 대해 알아봅니다. 그런 다음, 새 ARM64 프로젝트를 만들고 컴파일하는 단계를 안내합니다.</p>
    </li>
    <li>
        <a href="https://channel9.msdn.com/Events/Build/2018/BRK2438"><img alt="Build 2018 Windows 10 on ARM for developers" src="./images/buildVideoStillScaled.png" /></a>
        <h3>개발자용 ARM 기반 Windows 10 빌드 2018</h3><p>ARM 기반 Windows 10 디바이스, x86 에뮬레이션의 마법이 작동하는 방법, 마지막으로 ARM 기반 Windows 10용 앱을 제출하고 빌드하는 방법에 대해 알아봅니다. 데스크톱 및 UWP용 ARM64 앱을 빌드하는 방법을 보여 줍니다.</p>
    </li>
    <li>
        <a href="https://channel9.msdn.com/Events/Ch9Live/Windows-Community-Standup/Kevin-Gallo-January-2018"><img alt="Community standup video featuring Kevin Gallo" src="./images/communityStandupStillScaled.png" /></a>
        <h3>Kevin Gallo의 Windows 커뮤니티 스탠드업</h3><p>Windows 10이 ARM64에서 실행되는 방식에 대해 깊이 살펴보고 이 플랫폼에서 앱과 환경을 체험합니다.</p>
    </li>
</ul>

## <a name="understanding-windows-10-on-arm"></a>ARM 기반 Windows 10 이해
이러한 리소스를 살펴보며 플랫폼을 알아봅니다.

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
                    <h3>ARM 기반 Windows 10 시작</h3>
                    <p class="x-hidden-focus">설명서를 확인하여 기본 사항을 이해합니다.</p>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <a class="card" href="/windows/uwp/porting/apps-on-arm-x86-emulation" title="x86 에뮬레이션 관련 항목" data-linktype="absolute-path">
                    <div class="cardImageOuter">
                             <img class="cardImage" role="presentation" alt="x86 emulation icon" src="/media/common/i_advanced.svg" data-linktype="external" />
                    </div>
                </a>
                <div class="cardText">
                    <h3>x86 에뮬레이션 작동 방식 알아보기</h3>
                    <p class="x-hidden-focus">ARM 기반 Windows 10의 이 주요 기능에 대해 자세히 알아봅니다.</p>
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

## <a name="developing-for-windows-10-on-arm"></a>ARM 기반 Windows 10 개발
ARM 기반 Windows 10에 맞게 앱을 조정하고 사용할 수 있는 기능을 활용합니다.  

<ul class="cardsF panelContent cols cols3">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <a class="card" href="https://blogs.windows.com/buildingapps/?p=52087" title="ARM64 앱 빌드" data-linktype="absolute-path">
                    <div class="cardImageOuter">
                            <img class="cardImage" role="presentation" alt="Build ARM64 Win32 apps blog icon" src="/media/common/i_build.svg" data-linktype="external" />
                    </div>
                    </a>
                <div class="cardText">
                    <h3>SDK를 사용하여 ARM64 앱 빌드</h3>
                    <p class="x-hidden-focus">앱을 ARM64로 컴파일하는 방법을 안내하는 이 블로그 게시물을 확인하여 ARM 기반 Windows 10을 기본적으로 실행합니다.</p>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <a class="card" href="/windows/uwp/porting/apps-on-arm-troubleshooting-arm32" title="ARM32 앱 문제 해결" data-linktype="absolute-path">
                    <div class="cardImageOuter">
                            <img class="cardImage" role="presentation" alt="UWP apps on ARM icon" src="/media/common/i_code-edit.svg" data-linktype="external" />
                    </div>
                </a>
                <div class="cardText">
                    <h3>ARM의 UWP 앱</h3>
                    <p class="x-hidden-focus">성공을 위해 UWP(유니버설 Windows 플랫폼) 앱을 설정하려면 다음 지침을 따릅니다.</p>                    
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
                    <h3>ARM에서 디버깅</h3>
                    <p class="x-hidden-focus">ARM 기반 Windows 10에서 코드가 원활하게 실행되도록 합니다.</p>
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
                    <p class="x-hidden-focus">ARM64용 드라이버를 다시 컴파일합니다.</p>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <a class="card" href="/windows/uwp/porting/apps-on-arm-troubleshooting-x86" title="x86 앱 문제 해결" data-linktype="absolute-path">
                    <div class="cardImageOuter">
                            <img class="cardImage" role="presentation" alt="x86 apps on ARM icon" src="/media/common/i_code-blocks.svg" data-linktype="external" />
                            </a>
                    </div>
                </a>
                <div class="cardText">
                    <h3>ARM의 x86 앱</h3>
                    <p class="x-hidden-focus">ARM 기반 Windows 10에서 최고의 성능을 발휘할 수 있도록 x86 앱을 개발합니다.</p>
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

## <a name="let-us-know-if-you-have-feedback"></a>사용자 피드백
여러분과 기존 고객의 피드백을 활용하여 제품을 지속적으로 개선하고 있습니다. 아이디어가 있거나, 문제가 발생하거나, 멋진 경험을 공유하려는 경우 아래의 링크가 도움이 됩니다.

<ul class="cardsM cols cols3">
<li>
        <a class="card" href="feedback-hub://?tabid=2&contextid=803" data-linktype="absolute-path">
            <img class="cardImage" role="presentation" alt="Feedback hub icon" src="/media/common/i_feedback.svg" data-linktype="external" />
            <div class="cardText">
                <h3>피드백 허브 사용</h3>
                <p>빠뜨린 항목이 있나요? 멋진 아이디어가 있나요? 피드백 허브로 알려주세요.</p>
            </div>
        </a>
    </li>
    <li>
        <a class="card" href="mailto:woafeedback@microsoft.com" data-linktype="absolute-path">
            <img class="cardImage" role="presentation" alt="Report a bug icon" src="/media/common/i_mail.svg" data-linktype="external" />
            <div class="cardText">
                <h3>버그 보고</h3>
                <p>플랫폼에서 발견한 버그가 있나요? 자세한 내용을 이메일로 보내주세요.</p>
            </div>
        </a>
    </li>
    <li>
        <a class="card" href="https://github.com/MicrosoftDocs/windows-uwp/tree/docs/landing/arm-docs" data-linktype="absolute-path">
            <img class="cardImage" role="presentation" alt="Give doc feedback icon" src="/media/common/i_form.svg" data-linktype="external" />
            <div class="cardText">
                <h3>문서 피드백 제공</h3>
                <p>문서에 문제가 있나요? 좀 더 명확하게 설명할 필요가 있나요? GitHub 리포지토리에 문제를 제기하세요.</p>
            </div>
        </a>
    </li>
</ul>
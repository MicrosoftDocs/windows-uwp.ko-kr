---
description: 다양한 장치 및 화면 크기에서 멋지게 보이고 탐색하기 쉬운 UWP 앱을 디자인하고 코딩하는 방법을 알아봅니다.
title: 디자인 기본 사항
author: mijacobs
layout: LandingPage
keywords: UWP 앱 레이아웃, 유니버설 windows 플랫폼, 앱 디자인, 인터페이스
ms.author: mijacobs
ms.date: 3/7/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: e07c49e141841b8ef1eb44c6b421739951bb866d
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/15/2018
ms.locfileid: "1654312"
---
# <a name="design-basics-for-uwp-apps"></a>UWP 앱의 디자인 기본 사항

<ul class="panelContent cardsH" style="margin-left: 1px">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage" style="background-color: #f2f2f2" >
                        <a href="design-and-ui-intro.md">
                            <img src="images/landing-page/reposition-sm.png" alt=" " style="display: block; width: 100%; height: auto;" />
                            </a>
                        </div>
                    </div> 
                    <div class="cardText">
                        <h3><a href="design-and-ui-intro.md">UWP 앱 디자인 소개</a></h3>
                        <p>UI 기능에 대한 소개가 모든 UWP 앱과 문서 개요에 포함되어 있습니다.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage" style="background-color: #f2f2f2">
                        <a href="../fluent-design-system/index.md">
                            <img src="images/landing-page/fluentdesign-app-sm.png" alt=" " style="display: block; width: 100%; height: auto;"/>
                            </a>
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="../fluent-design-system/index.md">흐름 디자인 시스템 개요</a></h3>
                        <p>흐름 디자인 시스템은 혁신적인 UWP 기능이 결집된 구현체로서 모든 유형의 Windows 기반 장치에서 높은 성능을 발휘할 수 있는 앱을 개발하기 위한 모범 사례가 여기에 통합되어 있습니다.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
</ul>


<h2>앱 구조</h2>
<ul class="panelContent cardsH" style="margin-left: 1px">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <!-- <div class="cardImageOuter">
                        <div class="cardImage">
                        <a href="design-and-ui-intro.md">
                            <img src="images/1910808-hig-uap-toolkit-03.png" alt=" " style="display: block; width: 100%; height: auto;" />
                            </a>
                        </div>
                    </div>  -->
                    <div class="cardText">
                        <h3><a href="navigation-basics.md">탐색 기본 사항</a></h3>
                        <p>UWP 앱의 탐색은 탐색 구조, 탐색 요소 및 시스템 수준 기능의 유연한 모델을 기반으로 합니다. 이 문서에서는 이러한 구성 요소를 소개하고 적합한 탐색 환경을 만드는 데 함께 사용하는 방법을 보여 줍니다.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <!-- <div class="cardImageOuter">
                        <div class="cardImage">
                        <a href="design-and-ui-intro.md">
                            <img src="images/1910808-hig-uap-toolkit-03.png" alt=" " style="display: block; width: 100%; height: auto;" />
                            </a>
                        </div>
                    </div>  -->
                    <div class="cardText">
                        <h3><a href="commanding-basics.md">명령 기본 사항</a></h3>
                        <p>명령 요소는 사용자가 메일 보내기, 항목 삭제 또는 양식 제출과 같은 작업을 수행할 수 있게 해주는 대화형 UI 요소입니다. 이 문서에서는 단추 및 확인란과 같은 명령 요소, 이러한 명령 요소에서 지원하는 조작 및 명령 요소를 호스트하기 위한 명령 화면(예: 명령 모음 및 상황에 맞는 메뉴)에 대해 설명합니다.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <!-- <div class="cardImageOuter">
                        <div class="cardImage">
                        <a href="design-and-ui-intro.md">
                            <img src="images/1910808-hig-uap-toolkit-03.png" alt=" " style="display: block; width: 100%; height: auto;" />
                            </a>
                        </div>
                    </div>  -->
                    <div class="cardText">
                        <h3><a href="content-basics.md">콘텐츠 기본 사항</a></h3>
                        <p>모든 앱의 기본 목적은 콘텐츠에 액세스할 수 있도록 하는 것입니다. 사진 편집 앱에서는 사진이 콘텐츠이고, 여행 앱에서는 여행 목적지에 대한 지도와 정보가 콘텐츠인 식입니다. 이 문서에서는 사용, 만들기, 조작의 세 가지 콘텐츠 시나리오에 대한 콘텐츠 디자인 권장 사항을 제공합니다.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
</ul>

<h2>Photo lab 자습서</h2>
<p>XAML 및 C#에서 기본 사진 편집 응용 프로그램을 만드는 방법을 알아보세요.</p>
<img src="images/landing-page/photolab-50.png" style="{height: 339px}" alt=" " />

<ul class="panelContent cardsH" style="margin-left: 1px">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <!-- <div class="cardImageOuter">
                        <div class="cardImage"  >
                        <a href="xaml-basics-ui.md">
                            <img src="images/xaml-basics/detailpage.png" alt=" " style="display: block; width: 100%; height: auto;" />
                            </a>
                        </div>
                    </div>  -->
                    <div class="cardText">
                        <h3><a href="xaml-basics-ui.md">1. 기본 UI 만들기</a></h3>
                        <p>XAML을 사용해 기본 사용자 인터페이스 만들기</p> 
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <!-- <div class="cardImageOuter">
                        <div class="cardImage"  >
                        <a href="xaml-basics-adaptive-layout.md">
                            <img src="images/xaml-basics/detailpage.png" alt=" " style="display: block; width: 100%; height: auto;" />
                            </a>
                        </div>
                    </div>  -->
                    <div class="cardText">
                        <h3><a href="xaml-basics-adaptive-layout.md">2. 적응형 레이아웃 만들기</a></h3>
                        <p>사진 편집 응용 프로그램에 적응형 레이아웃을 제공합니다.</p> 
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <!-- <div class="cardImageOuter">
                        <div class="cardImage"  >
                        <a href="xaml-basics-style.md">
                            <img src="images/xaml-basics/detailpage.png" alt=" " style="display: block; width: 100%; height: auto;" />
                            </a>
                        </div>
                    </div>  -->
                    <div class="cardText">
                        <h3><a href="xaml-basics-style.md">3. 사용자 지정 스타일 만들기</a></h3>
                        <p>사용자 지정 스타일을 만들어 UWP 컨트롤에 고유의 모양과 느낌을 구현해 주세요.</p>  
                    </div>
                </div>
            </div>
        </div>
    </li>    
</ul>





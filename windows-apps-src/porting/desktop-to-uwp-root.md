---
Description: 기존 Windows 사용자 기존 Windows 폼, WPF, Win32 앱이나 게임을 위한 최신 Windows 앱 패키지를 만듭니다. Windows 10 사용자를 위해 최신 환경 추가 하 고 배포 및 수익 화를 간소화 합니다.
Search.Product: eADQiWindows 10XVcnh
title: 데스크톱 응용 프로그램 패키지
ms.date: 09/05/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 74373c24-f948-43bb-aa85-01e2e8e87162
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 70a9b6046e3b7be9ac84678ac21c0c9f89a4a7b2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627828"
---
# <a name="package-desktop-applications-desktop-bridge"></a>패키지 데스크톱 응용 프로그램 (데스크톱 브리지)

기존 데스크톱 응용 프로그램을 Windows 10 사용자를 위해 최신 환경 추가 하십시오. 그런 다음 Microsoft Store를 통해 배포, 글로벌 시장에 더 효과적으로 도달하는 성과를 이루세요. 저장소에 바로 기본 제공 기능을 활용 하 여 훨씬 간단한 방식으로 응용 프로그램 비용 절감 수 있습니다. 물론 Microsoft Store를 이용하지 않아도 됩니다. 기존 채널을 자유롭게 이용하세요.

![데스크톱 브리지](images/desktop-to-uwp/desktop-bridge-4.png)

데스크톱 응용 프로그램에 대 한 패키지를 만들 때 응용 프로그램 id 가져오기 됩니다 하 고 해당 id를 사용 하 여 데스크톱 응용 프로그램에 액세스 하려면 Windows 플랫폼 (UWP (유니버설) Api입니다. 이를 사용해 라이브 타일과 알림 같은 최신 몰입형 환경을 구현할 수 있습니다.  단순 조건부 컴파일을 사용 하 고 응용 프로그램이 Windows 10에서 실행 하는 경우에 UWP 코드를 실행 하려면 런타임 검사 합니다.

Windows 10 환경용 명확 하 게 사용 하는 코드 외에도 응용 프로그램 변경 되지 않습니다 하 고 기존 Windows 7, Windows Vista 또는 Windows XP 사용자를 기반 분산을 계속할 수 있습니다. Windows 10에서 응용 프로그램이 완전 신뢰에서 실행을 계속 처럼 사용자 모드는 지금 수행 합니다.

>[!IMPORTANT]
>(데스크톱 브리지 라고도 함) 데스크톱 응용 프로그램에 대 한 Windows 앱 패키지를 만들 수 있습니다. Windows 10 버전 1607에서에서 도입 되었으며 Windows 10 1 주년 업데이트 (10.0; 대상으로 하는 프로젝트 에서만 사용할 수 있습니다. Build 14393) 또는 Visual Studio의 이후 릴리스 합니다.

> [!NOTE]
> Microsoft Virtual Academy가 게시한 짧은 동영상에서 <a href="https://mva.microsoft.com/en-US/training-courses/developers-guide-to-the-desktop-bridge-17373?l=oZG0B1WhD_8406218965/">이 시리즈</a>를 확인하세요. 이 비디오에서는 데스크톱 응용 프로그램을 Windows 플랫폼 (UWP (유니버설)을 가져오는 전체 과정을 안내 합니다.

## <a name="benefits"></a>장점

여기에는 Windows 데스크톱 응용 프로그램용 Windows 앱 패키지를 만들어야 하는 이유가 몇 가지 나와 있습니다.

:heavy_check_mark: **배포 간소화.** 브리지를 사용하는 앱과 게임은 배포가 매우 편리합니다. 이 환경은 사용자가 안심 하 고 응용 프로그램 설치 및 업데이트는 확인 합니다. 사용자가 앱을 제거하도록 선택한 경우 흔적없이 완전히 제거됩니다. 이렇게 하면 설치 환경을 작성하고 사용자를 최신 상태로 유지하는 데 드는 시간이 줄어듭니다.

:heavy_check_mark: **자동 업데이트 및 라이선스**. 응용 프로그램은 Microsoft Store 기본 제공 라이선스 및 자동 업데이트 기능에 참여할 수 있습니다. 자동 업데이트를 사용할 경우 파일의 변경된 부분만 다운로드되므로 매우 안정적이고 효율적인 메커니즘입니다.

:heavy_check_mark: **도달 범위 증가 및 수익 창출 간소화**. Microsoft Store를 통한 배포를 선택하면 수 백만 명의 Windows 10 사용자(현지 결제 옵션으로 앱 및 게임을 취득하고 앱에서 바로 구매를 수행할 수 있음)에게 쉽게 도달할 수 있습니다.

:heavy_check_mark: **UWP 기능 추가**.  원하는 속도로 XAML 사용자 인터페이스, 라이브 타일 업데이트, UWP 백그라운드 작업, 앱 서비스 등의 UWP 기능을 앱 패키지에 추가할 수 있습니다.

:heavy_check_mark: **디바이스에서 폭넓어진 사용 사례**. 브리지를 사용하면 코드를 유니버설 Windows 플랫폼으로 점차적으로 마이그레이션하여 모든 Windows 10 디바이스(휴대폰, Xbox One 및 HoloLens 포함)에 도달할 수 있습니다.

자세한 혜택 목록은 [데스크톱 브리지](https://developer.microsoft.com/windows/bridges/desktop)를 참조하세요.

## <a name="prepare"></a>준비

먼저 문서를 검토 하 여 응용 프로그램을 준비 [데스크톱 앱 패키지 준비](desktop-to-uwp-prepare.md), 및 다음에 대 한 Windows 앱 패키지를 만들기 전에 응용 프로그램에 적용 되는 문제를 해결 합니다. 변경 하려면 여러 응용 프로그램 패키지를 만들기 전에 없을 수 있습니다. 그러나 패키지를 만들기 전에 응용 프로그램을 조정 해야 할 수도 있습니다는 몇 가지 경우가 있습니다.

<a id="convert" />

## <a name="package"></a>패키지

다음은 앱을 Windows 앱 패키지로 만들 때 사용할 수 있는 몇 가지 도구들입니다.

### <a name="desktop-app-converter"></a>Desktop App Converter

'변환기'가 이 도구의 이름으로 표시되지만, 실제 앱을 변환하지 않습니다. 응용 프로그램 변경 되지 않습니다. 그러나 이 도구는 Windows 앱 패키지를 생성합니다. 응용 프로그램을 사용 하면 다양 한 시스템을 수정 하는 경우 또는 설치 관리자 기능에 대해 모든 불확실성 경우 사례에서 매우 편리 수 있습니다.

Desktop App Converter 가상 파일 및 레지스트리 시스템은 응용 프로그램의 패키지 버전을 사용 하는 설치 관리자의 작업으로 변환 합니다. 또한 Desktop App Converter는 사용자를 위해 몇 가지 추가 작업을 수행합니다. 그 중 몇 가지는 다음과 같습니다.

:heavy_check_mark: 미리 보기 처리기, 미리 보기 이미지 처리기, 특성 처리기, 방화벽 규칙, URL 플래그를 자동으로 등록합니다.

:heavy_check_mark: 자동으로 등록 파일 형식 매핑을 사용 하 여 사용자가 그룹 파일을 사용 하도록 설정 하는 **종류** 파일 탐색기에서 열.

:heavy_check_mark: 공용 COM 서버를 등록합니다.

:heavy_check_mark: 앱을 실행 하는 데 사용할 수 있는 인증서를 생성 합니다.

:heavy_check_mark: 패키지에 포함 된 데스크톱 응용 프로그램 및 Microsoft Store 요구 사항에 대해 응용 프로그램의 유효성을 검사 합니다.

Desktop App Converter 사용 하는 또 다른 좋은 이유는 Visual Studio 이외의 다른 개발 환경을 사용 하 여 응용 프로그램을 유지 하는 경우. 응용 프로그램 설치 관리자 없는 경우에 Desktop App Converter 사용할 수 있습니다.

참조 [Desktop App Converter 사용 하 여 데스크톱 응용 프로그램 패키지](desktop-to-uwp-run-desktop-app-converter.md)

### <a name="visual-studio"></a>Visual Studio

Visual Studio를 사용하여 응용 프로그램을 유지하고 응용 프로그램에 설치 관리자가 없거나 또는 설치 관리자 너무 많은 복잡한 작업을 수행하지 않는 경우 대신 Visual Studio를 사용하는 것이 좋습니다.

Visual Studio로 매우 쉽게 패키지를 만들 수 있습니다. 패키징 프로젝트를 추가하고 데스크톱 프로젝트를 참조한 다음 F5 키를 눌러 앱을 디버깅합니다. 수동 조정이 필요하지 않습니다. 이 새로운 간소화된 환경은 이전 버전의 Visual Studio에서 사용할 수 있는 환경에 대해 상당히 향상되었습니다. 이를 사용하여 수행할 수 있는 몇 가지 다른 작업은 다음과 같습니다.

:heavy_check_mark: 시각적 자산을 자동으로 생성 합니다.

:heavy_check_mark: 프로그램 매니페스트에 비주얼 디자이너를 사용 하 여 변경 내용을 확인 합니다.

:heavy_check_mark: 마법사를 사용 하 여 패키지를 생성 합니다.

:heavy_check_mark: 쉽게 응용 프로그램에서 이미 예약한 이름에서 id를 할당 [파트너 센터](https://partner.microsoft.com/dashboard)합니다.

참조 [Visual Studio를 사용 하 여 데스크톱 응용 프로그램 패키지](desktop-to-uwp-packaging-dot-net.md)

### <a name="third-party-installer"></a>타사 설치 관리자

 몇 가지 인기 있는 타사 제품 및 설치 관리자는 이제 데스크톱 응용 프로그램을 패키지 하는 기능을 지원 합니다. 이를 사용하면 MSI 설치 관리자 또는 앱 패키지를 몇 번의 클릭만으로 생성할 수 있습니다. 이러한 도구를 사용하는 방법에 대한 설명서는 없지만, 자세한 내용은 웹 사이트를 방문해 확인하세요.

#### <a name="advanced-installer"></a>고급 설치 관리자

Caphyon은 몇 번의 클릭만으로 응용 프로그램에 대한 Windows 앱 패키지를 손쉽게 생성할 수 있도록 GUI 기반의 무료 데스크톱 앱 패키징 도구를 제공합니다. 모든 설치 관리자가 사용할 수 있습니다. 자동 모드에서 실행 하 고 유효성 검사를 수행 것 확인 응용 프로그램 패키징 적합 한지 여부를 결정 합니다.
Desktop App Converter는 Hyper-V 및 [VMware](https://www.vmware.com/)에도 통합이 됩니다. 즉, 일치하는 [Docker](https://docs.docker.com/) 이미지(크기가 3GB 이상일 수 있음)를 다운로드할 필요 없이 자체 가상 머신을 사용할 수 있습니다.

<img width="20%" src="images/desktop-to-uwp/Advanced_Installer_Vertical.png">

[고급 설치 관리자](https://www.advancedinstaller.com/)를 사용해 기존 프로젝트에서 MSI와 [Windows 앱 패키지](https://www.advancedinstaller.com/uwp-app-package.html)를 생성할 수 있습니다. 또한 고급 설치 관리자를 사용하여 Microsoft Desktop App Converter를 통해 생성한 Windows 앱 패키지를 가져올 수 있습니다. 가져온 패키지는 UWP 앱을 위해 특별히 설계된 시각적 도구를 사용하여 유지 관리할 수 있습니다.

또한 고급 설치 관리자는 [데스크톱 브리지 앱 빌드 및 디버그](https://www.advancedinstaller.com/debug-desktop-bridge-apps.html)에 사용할 수 있는 Visual Studio 2017 및 2015용 확장을 제공합니다.

빠른 개요는 이 [비디오](https://www.youtube.com/watch?v=cmLKgn04Vfg&feature=youtu.be)를 참조하세요.

> [!TIP]
> 최근에 출시된 [고급 설치 관리자 Express Edition](https://www.advancedinstaller.com/express-edition.html)을 확인하세요.

#### <a name="cloudhouse-compatibility-containers"></a>Cloudhouse 호환성 컨테이너

Windows 10 및 10 S와 호환되지 않는 LOB 응용 프로그램을 보유한 기업 고객의 경우, 소스 코드를 변경하지 않아도 Cloudhouse의 호환성 컨테이너를 사용해 Windows XP 및 7 앱을 Windows 10에서 실행한 다음 비즈니스용 Microsoft Store 또는 Microsoft InTune을 통해 공급하기 위해 UWP(유니버설 Windows 플랫폼)에서 실행되도록 변환할 수 있습니다. [무료 평가판](https://www.cloudhouse.com/free-trial)에 등록하세요.

<img width="20%" src="images/desktop-to-uwp/cloudhouse-container-logo.png">

Cloudhouse 패키징 기간 업무 응용 프로그램에는 자동 Packager 제공 [호환성 컨테이너](https://docs.cloudhouse.com/37613-overview/266723-compatibility-containers-for-applications) 지금 앱에서 실행 되는 운영 체제에서 (예: Windows XP)을 차례로 [변환에 대 한 준비](https://docs.cloudhouse.com/37613-overview/266725-compatibility-containers-for-desktop-bridge?from_search=17883905) uwp입니다. 그런 다음 이 컨테이너가 Microsoft의 Desktop App Converter 도구와 통합되어 새로운 Windows 앱 패키지 형식으로 변환됩니다.

Auto Packager는 설치/캡처, 런타임 분석을 사용, 응용 프로그램 파일, 레지스트리, 런타임, 종속성 Windows 10에서 응용 프로그램이 실행될 수 있도록 하는 데 필요한 호환성 및 리디렉션 엔진이 포함된 응용 프로그램 컨테이너를 생성합니다. 컨테이너는 응용 프로그램 및 런타임에 격리를 제공하므로 사용자 장치에서 실행 중인 다른 응용 프로그램과 충돌하거나 영향을 주지 않습니다.

비즈니스용 Microsoft Store를 통해 비즈니스 응용 프로그램을 제공하는 방법에 대한 자세한 내용은 [릴리스 블로그](https://www.cloudhouse.com/resources/release-solution-to-get-any-line-of-business-app-to-uwp)를 참조하세요.

#### <a name="firegiant"></a>FireGiant

합니다 [FireGiant MSIX 확장](https://www.firegiant.com/products/wix-expansion-pack/msix) 동일한 WiX 소스 코드에서 Windows 앱 패키지 및 MSI 패키지를 동시에 만들 수 있습니다. 를 빌드할 때마다 이전 버전의 MSI 사용 하 여 Windows 및 Windows 앱 패키지를 사용 하 여 Windows 10을 대상 지정할 수 있습니다.

<img width="20%" src="images/desktop-to-uwp/FG3rdPartyLogo.png">

FireGiant MSIX 확장 정적 분석 및 WiX 프로젝트의 지능형 에뮬레이션을 사용 하 여 컨테이너 또는 가상 컴퓨터의 디스크 공간 및 런타임 오버 헤드 없이 Windows 앱 패키지 만들기.

FireGiant MSIX 확장을 실행 하 여 설치 관리자를 변환 하지 않습니다, 때문에 반복 해 서 Windows 앱 패키지를 변환 하지 않고도 WiX 설치 관리자를 유지할 수 있습니다. 여러 다른 버전의 Windows 사용자가 최신 향상을 얻을 수 있습니다. 또 MSI 및 Windows 앱 패키지가 동기화 되지 않는 문제를 걱정할 필요가 없습니다.

이 확인해 [비디오](https://www.youtube.com/watch?v=AFBpdBiAYQE) 어떻게 몇 줄의 코드로 FireGiant CEO Rob Mensching 된 버전을 만듭니다 (Windows 앱 패키지) Appx의 인기 있는 오픈 소스 7-zip 압축 도구 및 Windows 응용 프로그램 모두 어떻게 개선 하는지 그 및 동일한 WiX 소스 코드의 변경 내용으로 MSI 패키지입니다.

#### <a name="installaware"></a>InstallAware

Microsoft의 기술 혁신을 빠르게 지원하는 것으로 [잘 알려진](https://www.installaware.com/press-room.htm) Install**Aware**는 단일 소스로부터 [Windows 앱 패키지(데스크톱 브리지)](https://www.installaware.com/appx-builder.htm), App-V(응용 프로그램 가상화), MSI(Windows Installer), EXE(네이티브 코드) 패키지를 빌드합니다.

<img width="20%" src="images/desktop-to-uwp/installaware.png">

Install**Aware**는 Visual Studio 버전 2012-2017용 무료 Install**Aware** 확장을 제공합니다. 단 한 번의 클릭으로 [Visual Studio 도구 모음](https://www.installaware.com/visual-studio-installer-2015.htm)에서 직접 Windows 앱 패키지를 만들 수 있습니다.

또한 Package **Aware**(스냅숏 없이 설정 캡처)나 데이터베이스 가져오기 마법사(MSI 설치 관리자 및 MSM 병합 모듈용)를 사용, 설정에 대한 소스 코드가 없는 경우에도 모든 설정을 가져올 수 있습니다. 또한 [GUI 도구](https://www.installaware.com/scripting-two-way-integrated-ide.htm)를 사용해 시각적으로 스크립팅 측면에서 가져오기를 유지 및 향상시킬 수 있습니다.

[고급 APPX 생성 옵션](https://www.installaware.com/mhtml5/desktop/appx.htm)은 Microsoft Store 제출 대상 지정이나 최종 사용자에 사이드로드 배포할 서명한 Windows 앱 패키지 바이너리 생성에 도움을 줍니다. 심지어 단일 소스로 **Nano Server**를 대상으로 배포를 지정할 수 있는 **WSA**(Windows Server Applications) Installer 패키지를 빌드 할 수도 있습니다. GUI에 추가해 [명령줄 자동화](https://www.installaware.com/scripting-automation-interface.htm)를 완전하게 지원합니다.

또한 Install**Aware**는 GNU Affero GPL 라이선스 아래 예제 명령줄 applet과 함께 **APPX 빌더 라이브러리**를 [오픈 소스](https://www.installaware.com/gnu.asp)로 지원합니다. WiX 같은 오픈 소스 플랫폼과 함께 사용하도록 디자인되어 있습니다.

#### <a name="installshield"></a>InstallShield

InstallShield는 최소한의 스크립팅과 코딩, 재작업으로 MSI 및 EXE 설치 관리자를 개발하고, UWP(유니버설 Windows 플랫폼)와 WSA(Windows Server 앱) 패키지를 생성하고, 응용 프로그램을 가상화 할 수 있는 단일 솔루션을 제공합니다.

<img width="20%" src="images/desktop-to-uwp/InstallShield-logo.jpg">

InstallShield 프로젝트를 몇 초면 검사할 수 있습니다. 응용 프로그램과 UWP, WSA 패키지 간 잠재적인 호환성을 자동으로 파악해 검사 작업 시간을 절약합니다.

기존 InstallShield 프로젝트에서 UWP 앱 패키지를 빌드, Microsoft Store를 준비하고 Windows 10에서의 소프트웨어 설치 환경을 단순화하세요. 고객이 원하는 배포 시나리오를 모두 지원하는 Windows 설치 관리자 및 UWP 앱 패키지를 모두 빌드합니다. 기존 InstallShield 프로젝트에서 WSA 패키지를 빌드, Nano Server와 Windows Server 2016 배포를 지원하세요.

더 쉽게 배포 및 유지 관리할 수 있도록 모듈에서 설치를 개발하고, 빌드 타임의 구성 요소 및 종속성을 Microsoft Store용 단일 UWP 앱 패키지로 병합하세요. Microsoft Store 외부로 직접 배포하는 경우, UWP 앱 및 다른 종속성을 Suite/고급 UI 설치 관리자와 묶습니다.

자세한 내용은 여기 [eBook](https://na01.safelinks.protection.outlook.com/?url=https%3A%2F%2Fresources.flexerasoftware.com%2Fweb%2Fpdf%2FeBook-IS-Your-Fast-Track-to-Profit.pdf&data=02%7C01%7Cnormesta%40microsoft.com%7C86b9a00bc8e345c2ac6208d4ba464802%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C1%7C636338258409706554&sdata=IAYNp9nFc8B5ayxwrs%2FQTWowUmOda6p%2Fn%2BjdHea257M%3D&reserved=0)을 참조하세요.

#### <a name="pace-suite"></a>PACE Suite

[PACE Suite](https://pacesuite.com/)는 데스크톱 앱을 유니버설 Windows 플랫폼에 표시하는 데 사용할 수 있는 응용 프로그램 패키지 도구입니다.

<img width="20%" src="images/desktop-to-uwp/PACE.png">

PACE Suite를 사용하면 특별한 패키징 환경을 준비하거나 추가 Windows SDK 구성 요소를 설치할 필요가 없습니다. PACE Suite는 Windows 10 또는 Windows Server 2016 하의 표준 패키징 환경에서 독립적으로 Windows 앱 패키지를 빌드할 수 있습니다. PACE Suite로 Windows 앱 패키지에 설치 관리자를 리패키징하는 방식을 알아보려면 이 [일러스트레이션 예제](https://pacesuite.com/convert-exe-to-appx/)를 확인하세요.

Windows 앱 패키지를 만드는 것 외에 PACE Suite를 사용하여 Windows Installer 패키지(MSI), 패치(MSP), 변환(MST) 및 App-V 패키지 만둘 수도 있습니다. MSI를 작성할 때 PACE Suite를 사용하여 업그레이드, 사용 권한 설정, 사용자 지정 작업, 스크립트 등을 관리할 수 있습니다. 또한 응용 프로그램을 바로 System Center Configuration Manager에 게시할 수도 있습니다.

모든 응용 프로그램 패키징 기능을 검토하려면 [PACE Suite 기능](https://pacesuite.com/features/)을 참조하세요.

#### <a name="rad-studio"></a>RAD Studio

[Embarcadero의 RAD Studio](https://www.embarcadero.com/products/rad-studio/windows-10-store-desktop-bridge) 참조

#### <a name="raypack-studio"></a>RayPack Studio

Raynet의 솔루션을 패키징 [RayPack Studio](https://raynet.de/Raynet-Products/RayPackStudio), 효율적이 고 구성에 쉽게 변환의 여러 결과 중 하나로 데스크톱 응용 프로그램 패키지 만들기를 지원 및 프레임 워크를 다시 패키징합니다.

<img width="20%" src="images/desktop-to-uwp/RaynetLogo_v3.png">

긴 환경 설치 없이 자동 대량 변환을 수행하기 위해 기존 가상 환경(VMware 워크스테이션, Hyper-V)을 사용할 수 있습니다. 이 스튜디오의 구성 요소([RayQC Advanced](https://raynet.de/Raynet-Products/RayQCad))는 변환할 수 있는 소프트웨어를 확인하기 위해 사전 변환 검사 및 호환성 테스트를 수행할 수 있습니다. 또한 사용자는 이제 1주년 및 크리에이터스 업데이트를 포함한 다양한 Windows 10 에디션에 대해 포괄적인 충돌 및 호환성 검사를 수행할 수 있습니다.

Windows 10 APPX/UWP 포맷용 소프트웨어 패키지를 만드는 것 이외에, RayPack Studio는 클래식 Windows Installer 패키지(MSI), 패치(MSP), 변환(MST), App-V 패키지를 만드는 데 사용할 수도 있습니다. 또한,이 해결 방법 소프트웨어 제품과 전문가 enterprise 소프트웨어 패키지에 대 한 구성 요소 집합과 함께 제공 됩니다. 소프트웨어 패키징 및 가상화뿐만 아니라 RayPack Studio는 소프트웨어 응용 프로그램 및 패키지 충돌 및 호환성 검사([RayQC Advanced](https://raynet.de/Raynet-Products/RayQCad)), 소프트웨어 평가([RayEval](https://raynet.de/Raynet-Products/RayEval)), 및 품질 보증([RayQC](https://raynet.de/Raynet-Products/RayQC)) 등의 모든 패키징 관련 작업을 고려합니다.

[RayFlow](https://raynet.de/Raynet-Products/RayFlow)와 결합하여 Raynet의 Enterprise Workflow System을 통해 사용자는 전체 엔터프라이즈 응용 프로그램 수명 주기 동안 패키지 주문에서부터 평가, 분석, 패키징, 품질 보증, 사용자 동의 테스트 및 배포에 이르기까지 소프트웨어 관련 작업을 효율적으로 수행할 수 있습니다. 모든 패키지 및 형식은 직접 SCCM 또는 다른 솔루션에 저장하거나 배포할 수 있습니다. 전체 응용 프로그램 수명 주기 프로세스는 RayFlow에서 추적 및 관리됩니다. 또한, ServiceNow 등의 모든 주문 시스템을 통합할 수 있습니다. Raynet은 서비스 공급자에 대한 도구와 함께 전 세계에 소프트웨어 패키징 공장을 빌드합니다.

Raynet의 RayPack Studio 및 RayFlow에 대해 알아보고 [무료 평가판 라이선스](https://raynet.de/contact?init=license)를 다운로드하세요. 자세한 내용은 [www.raynet.de](https://raynet.de/home)를 참조하세요.

**관련 링크**:

* Raynet: [https://raynet.de/home](https://raynet.de/home)
* RayPack Studio: [https://raynet.de/Raynet-Products/RayPackStudio](https://raynet.de/Raynet-Products/RayPackStudio)
* RayFlow: [https://raynet.de/Raynet-Products/RayFlow](https://raynet.de/Raynet-Products/RayFlow)
* RayEval: [https://raynet.de/Raynet-Products/RayEval](https://raynet.de/Raynet-Products/RayEval)
* RayQC: [https://raynet.de/Raynet-Products/RayQC](https://raynet.de/Raynet-Products/RayQC)
* RayQC Advanced: [https://raynet.de/Raynet-Products/RayQCad](https://raynet.de/Raynet-Products/RayQCad)
* 무료 평가판 라이선스: [https://raynet.de/contact?init=license](https://raynet.de/contact?init=license)

### <a name="manual-packaging"></a>수동 패키징

최종 옵션으로 이러한 도구 중 하나를 사용 하지 않고 응용 프로그램을 변환할 수 있습니다. 변환을 세부적으로 제어하고 싶다면 매니페스트 파일을 만든 다음, **MakeAppx.exe** 도구를 실행하여 Windows 앱 패키지를 만들 수 있습니다.

참조 [데스크톱 응용 프로그램을 수동으로 패키지](desktop-to-uwp-manual-conversion.md)합니다.

## <a name="integrate"></a>통합

응용 프로그램이 시스템과 통합 해야 하는 경우 (예: 방화벽 규칙을 설정), 응용 프로그램의 패키지 매니페스트에 해당 항목을 설명 하 고 시스템은 나머지를 수행 합니다. 이런 작업 대부분에서 코드를 작성할 필요가 없습니다. 매니페스트에서 XML의 비트를 사용 하 여 수행할 수 있습니다, 사용자가 로그온 할 때 프로세스를 시작 하 고, 파일 탐색기에 응용 프로그램을 통합 하 고, 응용 프로그램을 추가 하는 등 다른 앱에서 표시 되는 인쇄 대상 목록입니다.

참조 [Windows 10 데스크톱 응용 프로그램 패키지를 통합할](desktop-to-uwp-extensions.md)합니다.

## <a name="enhance"></a>개선

일단 앱을 패키징하고 나면, 라이브 타일, 푸시 알림 같은 기능으로 이를 실행할 수 있습니다. 응용 프로그램의 참여 수준이 크게 향상 시킬 수 있습니다 이러한 기능 중 일부를 및 추가할 거의 시간 비용이 있습니다. 몇 가지 향상 기능에는 약간의 코드가 추가적으로 필요합니다.

[Windows 10용 데스크톱 응용 프로그램 개선](desktop-to-uwp-enhance.md) 참조

## <a name="extend"></a>확장

일부 Windows 10 환경(예, 터치 구현 UI 페이지)은 최신 앱 컨테이너 내부에서 실행해야 합니다. 일반적으로 UWP API로 기존 데스크톱 응용 프로그램을 [향상](desktop-to-uwp-enhance.md)시켜 환경을 추가할지 먼저 결정해야 합니다. 경험을 얻는 UWP 구성 요소를 사용 해야 할 경우 UWP 프로젝트를 솔루션에 추가 및 앱 서비스를 사용 하 여 데스크톱 응용 프로그램 및 UWP 구성 요소 간 통신을 수 있습니다.

[최신 UWP 구성 요소로 데스크톱 응용 프로그램 확장](desktop-to-uwp-extend.md) 참조.

## <a name="migrate"></a>마이그레이션

데스크톱 응용 프로그램을 UWP 앱으로 변환할 수 있는 도구는 없지만, 수많은 기존 코드를 재사용할 수 있으며 이를 통해 빌드 비용을 낮출 수 있습니다. .NET Standard 2.0 라이브러리에 최대한 많은 비즈니스 논리를 이동하여 이를 수행할 수 있습니다.

.NET Standard 2.0에는 즐겨 찾는 NuGet 패키지 및 타사 라이브러리에 대한 호환성 shim과 함께 여러 .NET API를 대폭 늘렸습니다.

코드를 .NET Standard 라이브러리로 마이그레이션하고 모든 Windows 10 장치에 연결할 수 있는 UWP(유니버설 Windows 플랫폼) 앱을 만들 수 있습니다.

[데스크톱 앱과 UWP 앱 간의 코드 공유](desktop-to-uwp-migrate.md) 참조


## <a name="test"></a>테스트

응용 프로그램을 테스트할 현실적인 설정에서 배포를 준비 하는 대로, 응용 프로그램에 로그인 한 후 설치 하는 것이 좋습니다. [앱 테스트](https://docs.microsoft.com/en-us/windows/uwp/porting/desktop-to-uwp-debug#test-your-app)를 참조합니다.

>[!IMPORTANT]
> Microsoft Store 응용 프로그램을 게시 하려는 경우 Windows 10 S 모드로 실행 되는 장치에서 응용 프로그램이 올바르게 작동 하는지 확인 합니다. 이는 Microsoft Store의 요구 사항입니다. [Windows 10 S 모드 Windows 앱 테스트](desktop-to-uwp-test-windows-s.md)를 참조하세요.

## <a name="validate"></a>유효성 검사

응용 프로그램에 게 Microsoft Store 게시 된 또는 받기의 최상의 있는 기회 [Windows 인증](https://go.microsoft.com/fwlink/p/?LinkID=309666), 유효성 검사 및 인증을 위해 제출 하기 전에 로컬로 테스트 합니다.

DAC 응용 프로그램 패키지를 사용 하는 경우 사용할 수 있습니다 새 ``-Verify`` 패키지 데스크톱 응용 프로그램 및 저장소 요구 사항에 대해 패키지의 유효성을 검사 하는 플래그입니다. [앱의 패키징, 서명, Microsoft Store 제출 준비](desktop-to-uwp-run-desktop-app-converter.md#optional-parameters)를 참조하세요.

Visual Studio를 사용 하는 경우에 응용 프로그램을 확인할 수 있습니다 합니다 **앱 패키지 만들기** 마법사. [앱 패키지 업로드 파일 만들기](../packaging/packaging-uwp-apps.md#create-an-app-package-upload-file)를 참조하세요.

도구를 수동으로 실행하려면 [Windows 앱 인증 키트](../debug-test-perf/windows-app-certification-kit.md)를 참조하세요.

앱의 유효성을 검사하기 위해 Windows 앱 인증이 사용하는 테스트 목록은 [Windows 데스크톱 브리지 앱 테스트](../debug-test-perf/windows-desktop-bridge-app-tests.md)를 참조하세요.

## <a name="distribute"></a>배포

Microsoft Store 게시 하거나 사이드 로드 하 여 응용 프로그램을 배포할 수 있습니다 다른 시스템에 놓습니다.

참조 [패키지에 포함 된 데스크톱 앱을 배포할](desktop-to-uwp-distribute.md)합니다.

## <a name="support-and-feedback"></a>지원 및 피드백

**질문에 답변**

질문이 있으세요? Stack Overflow에서 질문해 주세요. 저희 팀은 이러한 [태그](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)를 모니터링합니다. [여기](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)에서 Microsoft에 문의할 수도 있습니다.

**피드백 제공 또는 기능 제안**

[UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)를 참조하세요.

## <a name="in-this-section"></a>이 섹션의 내용

| 항목 | 설명 |
|-------|-------------|
| [앱 패키지 준비](desktop-to-uwp-prepare.md) | 데스크톱 앱을 패키징 하기 전에 검토할 항목 목록을 제공합니다. |
| [Desktop App Converter 사용 하 여 앱 패키지](desktop-to-uwp-run-desktop-app-converter.md) | 데스크톱 앱 변환기를 실행하는 방법을 보여 줍니다. |
| [데스크톱 응용 프로그램을 수동으로 패키지](desktop-to-uwp-manual-conversion.md) | 앱 패키지 및 매니페스트를 직접 만드는 방법을 알아봅니다. |
| [Visual Studio를 사용 하 여 데스크톱 응용 프로그램 패키지](desktop-to-uwp-packaging-dot-net.md)| Visual Studio를 사용 하 여 데스크톱 응용 프로그램을 패키지 하는 방법을 보여 줍니다. |
| [Windows 10 데스크톱 응용 프로그램 통합](desktop-to-uwp-extensions.md) | 패키징 프로젝트의 패키지 매니페스트 파일에서 작업을 설명 하 여 사용 하 여 Windows 10을 사용 하 여 응용 프로그램을 통합 합니다. |
| [Windows 10 용 데스크톱 응용 프로그램을 향상합니다](desktop-to-uwp-enhance.md)| UWP API를 사용, Windows 10 사용자가 만족할 최신 환경을 추가할 수 있습니다. |
| [패키지에 포함 된 데스크톱 응용 프로그램에 사용할 수 있는 UWP Api](desktop-to-uwp-supported-api.md) | 있는지 UWP Api를 사용 하 여 패키지에 포함 된 데스크톱 응용 프로그램에 사용할 수 있습니다. |
| [최신 UWP 구성 요소를 사용 하 여 데스크톱 응용 프로그램 확장](desktop-to-uwp-extend.md)| UWP 앱 컨테이너 내부에서만 실행되는 고급 환경을 추가합니다. 앱 서비스를 사용 하 여 UWP 프로세스를 사용 하 여 데스크톱 응용 프로그램을 연결 합니다.|
| [실행, 디버그 및 패키지 된 데스크톱 응용 프로그램 테스트](desktop-to-uwp-debug.md) | 패키지로 만든 앱 디버깅에 대한 옵션에 대해 설명합니다. |
| [패키지에 포함 된 데스크톱 응용 프로그램 배포 ](desktop-to-uwp-distribute.md) | 사용자에 게 변환 된 응용 프로그램을 배포 하는 방법을 참조 하세요.  |
| [알려진된 Issues(desktop-to-uwp-known-issues.md) | 알려진 데스크톱 응용 프로그램 패키징을 사용 하 여 문제를 나열 합니다. |

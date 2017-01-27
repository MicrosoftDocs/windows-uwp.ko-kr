---
author: awkoren
Description: "데스크톱-UWP 브리지를 시작하여 UWP(유니버설 Windows 플랫폼) 앱으로 Windows 데스크톱 응용 프로그램(예: Win32, WPF 및 Windows Forms)을 변환합니다."
Search.Product: eADQiWindows 10XVcnh
title: "데스크톱 브리지를 사용하여 데스크톱 앱을 UWP(유니버설 Windows 플랫폼)로 가져오기"
translationtype: Human Translation
ms.sourcegitcommit: 462d2b13cefc6abb4d7c6f814ec4ee659e4afde8
ms.openlocfilehash: 1ef54c3c45113e434333058d0f039e213ea8eed2

---

# <a name="bring-your-desktop-app-to-the-universal-windows-platform-uwp-with-the-desktop-bridge"></a>데스크톱 브리지를 사용하여 데스크톱 앱을 UWP(유니버설 Windows 플랫폼)로 가져오기

데스크톱-UWP 브리지를 시작하여 UWP(유니버설 Windows 플랫폼) 앱으로 Windows 데스크톱 응용 프로그램을 변환합니다.

데스크톱 브리지는 Windows 데스크톱 응용 프로그램(예: Win32, Windows Forms, WPF) 또는 게임을 UWP 앱이나 게임으로 변환할 수 있는 브리지입니다. 변환 후에는 Windows 데스크톱 응용 프로그램이 Windows 10 데스크톱을 대상으로 하는 UWP 앱 패키지(.appx 또는 .appxbundle)의 형태로 패키징되고, 서비스되고, 배포됩니다.

데스크톱 앱을 UWP 패키지로 변환할 수 있도록 하는 기술은 두 부분으로 나뉩니다. 첫 번째는 기존 이진 파일을 가져온 후 UWP 패키지로 다시 패키징하는 변환 프로세스입니다. 코드는 여전히 동일하며 다르게 패키징될 뿐입니다. 두 번째 부분은 Windows 연례 업데이트에 포함된 런타임 기술로, UWP 패키지에 앱 컨테이너가 아닌 완전 신뢰로 실행되는 실행 파일이 포함될 수 있도록 합니다. 또한 이 기술은 변환된 앱에 일부 UWP API를 사용하는 데 필요한 패키지 ID를 제공합니다.

## <a name="benefits"></a>혜택

다음은 Windows 데스크톱 응용 프로그램을 변환할 때의 몇 가지 이점입니다. 

**배포 간소화.** 브리지를 사용하는 앱 및 게임은 사용자가 안심하고 설치 및 업데이트할 수 있는 뛰어난 배포 환경을 제공합니다. 사용자가 앱을 제거하도록 선택한 경우 흔적없이 완전히 제거됩니다. 이렇게 하면 설치 환경을 작성하고 사용자를 최신 상태로 유지하는 데 드는 시간이 줄어듭니다.

**자동 업데이트 및 라이선스**. 앱은 Windows 스토어의 기본 제공 라이선스 및 자동 업데이트 기능에 참여할 수 있습니다. 자동 업데이트를 사용할 경우 파일의 변경된 부분만 다운로드되므로 매우 안정적이고 효율적인 메커니즘입니다.

**도달 범위 증가 및 수익 창출 간소화**. Windows 스토어를 통해 배포하도록 선택하면 수백만 명의 Windows 10 사용자(현지 결제 옵션으로 앱 및 게임을 취득하고 앱에서 바로 구매를 수행할 수 있음)에게 쉽게 도달할 수 있습니다.

**UWP 기능 추가**.  원하는 속도로 XAML 사용자 인터페이스, 라이브 타일 업데이트, UWP 백그라운드 작업, 앱 서비스 등의 UWP 기능을 앱 패키지에 추가할 수 있습니다.

**디바이스에서 폭넓어진 사용 사례**. 브리지를 사용하면 코드를 유니버설 Windows 플랫폼으로 점차적으로 마이그레이션하여 모든 Windows 10 디바이스(휴대폰, Xbox One 및 HoloLens 포함)에 도달할 수 있습니다.

## <a name="prepare"></a>준비

데스크톱-UWP 브리지는 간편한 사용을 위해 고안되어 앱의 변환 프로세스를 준비하는 데 많은 작업을 수행하지 않아도 됩니다. 그러나 변환 전에 몇 가지 주의 사항과 고유한 상황이 있습니다. 계속하기 전에 [데스크톱-UWP 브리지용 앱 준비](desktop-to-uwp-prepare.md) 문서를 살펴보고 앱에 적용되는 문제를 해결하세요.

## <a name="convert"></a>변환

앱을 변환하기 위한 몇 가지 다른 옵션이 있습니다.

**DAC(Desktop App Converter)** DAC는 사용자를 위해 앱을 자동으로 변환하고 서명하는 도구입니다. DAC를 사용하면 작업이 편리하고 자동화되며, 앱에서 시스템을 많이 수정하거나 설치 관리자가 수행하는 작업에 대해 잘 알지 못하는 경우 유용합니다. 시작하려면 [데스크톱 앱 변환기](desktop-to-uwp-run-desktop-app-converter.md)의 문서를 참조하세요. 

**수동 변환**. xcopy를 사용하여 앱을 설치하거나 설치 관리자가 시스템을 자주 변경할 경우 수동 변환이 더 능률적인 선택일 수 있습니다. 여기에는 매니페스트 파일 만들기, MakeAppx.exe 도구 실행 및 앱 패키지 서명이 포함됩니다. 수동으로 변환하는 방법에 대한 자세한 내용은 [데스크톱 브리지를 사용하여 수동으로 앱을 UWP로 변환](desktop-to-uwp-manual-conversion.md)을 참조하세요. 

**타사 설치 관리자**. 몇몇 인기 타사 제품 및 설치 관리자는 이제 데스크톱 브리지를 지원하며 몇 번의 클릭만으로 MSI 설치 관리자 또는 변환된 앱 패키지를 생성할 수 있습니다. 옵션은 다음과 같습니다. 

* [Flexera의 InstallShield](http://www.flexerasoftware.com/producer/products/software-installation/installshield-software-installer)
* [FireGiant의 WiX](https://www.firegiant.com/r/appx) 
* [Caphyon의 Advanced Installer](http://www.advancedinstaller.com/uwp-app-package)
* [Embarcadero의 RAD Studio](https://www.embarcadero.com/products/rad-studio/windows-10-store-desktop-bridge) 
* [InstallAware](https://www.installaware.com/appx.htm)

자세한 내용은 각 설치 관리자의 해당 웹 사이트를 참조하세요. 

## <a name="enhance"></a>보정 

라이브 타일, 푸시 알림 등과 같은 기능을 추가하는 광범위한 UWP API를 사용하여 변환된 데스크톱 앱을 향상시킬 수 있습니다. 전체 코드 샘플은 GitHub에서 [UWP에 대한 데스크톱 앱 브리지 샘플](https://github.com/Microsoft/DesktopBridgeToUWP-Samples) 및 [UWP(유니버설 Windows 플랫폼) 앱 샘플](https://github.com/Microsoft/Windows-universal-samples) 리포지토리를 참조하세요. 지원되는 API의 전체 목록을 보려면 [데스크톱 브리지로 변환된 앱에 대해 지원되는 UWP API](desktop-to-uwp-supported-api.md)를 검토하세요. 

UWP API를 호출 하는 작업 외에도 변환된 앱에서만 액세스할 수 있는 기능이 포함된 앱을 확장할 수 있습니다. 이러한 기능에는 사용자 로그온 시 프로세스 시작 및 파일 탐색기 통합과 같은 시나리오가 포함되어 있으며, 원래 데스크톱 앱과 전체 UWP 앱 패키지 간의 전환을 부드럽게 하도록 디자인되었습니다. 자세한 내용은 [데스크톱 브리지 앱 확장](desktop-to-uwp-extensions.md)을 참조하세요. 

## <a name="migrate"></a>마이그레이션

브리지를 사용하면 Windows 데스크톱에서 앱을 실행하고 게시하면서 이전 코드를 UWP로 점차적으로 마이그레이션할 수 있습니다. UWP로 완전히 마이그레이션되고 앱에 WPF/Win32 구성 요소가 더 이상 없으면 휴대폰, Xbox One 및 HoloLens를 비롯한 모든 Windows 장치에 연결할 수 있습니다.

## <a name="debug"></a>디버그

Visual Studio를 사용하여 앱을 디버그할 수 있습니다. 자세한 내용은 [데스크톱 브리지로 변환된 앱 디버그](desktop-to-uwp-debug.md)를 확인하세요. 

데스크톱 브리지가 이면에서 작동하는 방식에 대해 관심이 있는 경우 [데스크톱 브리지의 백그라운드 작업](desktop-to-uwp-behind-the-scenes.md)을 확인하세요. 

## <a name="distribute"></a>배포

Windows 스토어 또는 테스트용으로 로드를 사용하여 앱을 배포할 수 있습니다. 자세한 내용은 [데스크톱 브리지로 변환된 앱 배포](desktop-to-uwp-distribute.md)를 참조하세요. 사용자에게 앱을 배포하기 전에 먼저 앱에 서명해야 합니다. 단계별 지침은 [데스크톱 브리지로 변환된 앱에 서명](desktop-to-uwp-signing.md)을 참조하세요. 

## <a name="support-and-feedback"></a>지원 및 피드백

앱 변환 문제가 발생할 경우 [포럼](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/home?forum=wpdevelop)에서 도움을 요청할 수 있습니다. 

피드백을 제공하거나 기능 제안을 만들려면 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)에 항목을 제출하거나 찬성해 주세요. 

## <a name="in-this-section"></a>이 섹션의 내용

| 항목 | 설명 |
|-------|-------------|
| [데스크톱 앱 변환기](desktop-to-uwp-run-desktop-app-converter.md) | 데스크톱 앱 변환기를 실행하는 방법을 보여 줍니다. |
| [데스크톱 브리지를 사용하여 수동으로 앱을 UWP로 변환](desktop-to-uwp-manual-conversion.md) | 앱 패키지 및 매니페스트를 직접 만드는 방법을 알아봅니다. |
| [데스크톱 브리지 앱 확장](desktop-to-uwp-extensions.md) | 시작 작업 및 파일 탐색기 통합과 같은 기능을 사용하도록 설정하려면 확장을 사용하여 변환된 앱을 향상합니다. |
| [데스크톱 브리지로 변환된 앱에 대해 지원되는 UWP API](desktop-to-uwp-supported-api.md) | 변환된 데스크톱 앱에서 사용할 수 있는 UWP API를 확인합니다. |
| [데스크톱 브리지로 변환된 앱 디버그](desktop-to-uwp-debug.md) | 변환된 앱 디버깅에 대한 옵션에 대해 설명합니다. | 
| [데스크톱 브리지로 변환된 앱에 서명](desktop-to-uwp-signing.md) | 인증서로 변환된 앱 패키지에 서명하는 방법을 알아봅니다. |
| [데스크톱 브리지로 변환된 앱 배포](desktop-to-uwp-distribute.md) | 변환된 앱을 사용자에게 배포하는 방법을 참조하세요.  |
| [데스크톱 브리지의 백그라운드 작업](desktop-to-uwp-behind-the-scenes.md) | 데스크톱 UWP 브리지가 이면에서 작동하는 방식에 대해 깊이 있게 살펴보세요. | 
| [데스크톱 브리지의 알려진 문제](desktop-to-uwp-known-issues.md) | 데스크톱-UWP 브리지의 알려진 문제를 나열합니다. | 
| [UWP에 대한 데스크톱 앱 브리지 코드 샘플](https://github.com/Microsoft/DesktopBridgeToUWP-Samples) | 변환된 앱의 기능을 보여 주는 GitHub의 코드 샘플입니다. |


<!--HONumber=Dec16_HO3-->



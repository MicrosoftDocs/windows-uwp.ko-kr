---
ms.assetid: 4b0c86d3-f05b-450b-bf9c-6ab4d3f07d31
description: 이 로드맵은 Windows 10 및 UWP(유니버설 Windows 플랫폼) 앱의 주요 엔터프라이즈 기능에 대해 간략하게 설명합니다.
title: 엔터프라이즈
ms.date: 08/30/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 152a1bf2a751c2d2c78818b85868bfea3be911ac
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259649"
---
# <a name="enterprise"></a>엔터프라이즈

이 문서에서는 Windows 10 앱용 UWP(유니버설 Windows 플랫폼)에서 제공하는 주요 엔터프라이즈 기능에 대해 간략하게 설명합니다. 이러한 기능 중 일부를 자세히 보여 주는 비디오는 [UWP 및 Visual Studio를 사용하여 LOB 애플리케이션을 빠르게 구성](https://channel9.msdn.com/Events/Build/2018/BRK3502)을 참조하세요.

<a id="template-studio" />

### <a name="windows-template-studio"></a>Windows Template Studio

Windows Template Studio는 마법사 기반 환경을 사용하여 새 UWP(유니버설 Windows 플랫폼) 앱을 신속하게 만들 수 있는 Visual Studio 2017 및 Visual Studio 2019 확장입니다. 그 결과로 얻게 되는 UWP 프로젝트는 검증된 패턴과 모범 사례를 구현하면서도 최신 Windows 10 기능을 통합하는 잘 구성된 읽기 가능한 코드입니다.

![Windows Template Studio](images/windows-template-studio.png)

[Windows Template Studio](https://marketplace.visualstudio.com/items?itemName=WASTeamAccount.WindowsTemplateStudio)를 참조하세요.

<a id="desktop-style-UI" />

### <a name="controls-to-create-desktop-style-uis"></a>데스크톱 스타일 UI를 만드는 컨트롤

기존 데스크톱 애플리케이션 UI와 UWP UI 간의 격차를 줄이는 새 UWP XAML 컨트롤을 출시했습니다.

예를 들어 새 [MenuBar](/windows/uwp/design/controls-and-patterns/menus), [DropDownButton](/windows/uwp/design/controls-and-patterns/buttons#create-a-drop-down-button), [SplitButton](/windows/uwp/design/controls-and-patterns/buttons#create-a-split-button) 및 [CommandBarFlyout](/windows/uwp/design/controls-and-patterns/command-bar-flyout) 컨트롤은 명령을 공개하는 더 유연한 방법을 제공하며, [EditableComboBox](/windows/uwp/design/controls-and-patterns/combo-box#make-a-combo-box-editable)를 사용하면 사용자가 미리 정의된 옵션 목록에 나열되지 않은 값을 입력할 수 있습니다.

![MenuBar](images/menu-bar.png)

<a id="enterprise" />

### <a name="controls-to-support-enterprise-scenarios"></a>엔터프라이즈 시나리오를 지원하는 컨트롤

[DataGridView](https://docs.microsoft.com/en-us/windows/communitytoolkit/controls/datagrid)는 행과 열에 데이터 컬렉션을 표시할 수 있는 유연한 방법을 제공합니다.

[TreeView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/tree-view)는 중첩된 항목이 포함된 노드를 확장 및 축소하는 계층적 목록을 사용할 수 있도록 합니다. 이 컨트롤은 UI의 폴더 구조 또는 중첩된 관계를 나타내는 데 사용할 수 있습니다.

![DataGrid 컨트롤](images/DataGrid.gif)


### <a name="windows-ui-library"></a>Windows UI Library

Windows UI Library는 UWP 앱용 컨트롤 및 기타 사용자 인터페이스 요소를 제공하는 NuGet 패키지 세트입니다. 또한 이전 버전의 Windows 10과의 하위 수준 호환성을 지원하므로 최신 OS가 사용자에게 없더라도 앱이 작동합니다.

![Windows UI Library](images/win-ui.png)

[Windows UI Library(미리 보기 버전)](https://docs.microsoft.com/uwp/toolkits/winui/)를 참조하세요.

<a id="xaml-islands" />

### <a name="uwp-controls-in-desktop-applications"></a>데스크톱 애플리케이션의 UWP 컨트롤

이제 Windows 10을 사용 하면 WPF, Windows Forms 및 C++ Win32 데스크톱 애플리케이션에서 UWP 컨트롤을 사용할 수 있습니다. 즉, Windows Ink 및 Fluent 디자인 시스템을 지원하는 컨트롤과 같은 UWP 컨트롤을 통해서만 사용할 수 있는 최신 Windows 10 UI 기능을 통해 기존 데스크톱 애플리케이션의 모양, 느낌 및 기능을 향상시킬 수 있습니다. 이 기능을 XAML 제도라고 합니다.

[데스크톱 애플리케이션에서 UWP 컨트롤 호스팅](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-host-controls)을 참조하세요.

<a id="standard" />

### <a name="net-standard-20"></a>.NET Standard 2.0

.NET Standard에는 .NET Standard 1.x보다 20,000개가 더 많은 API가 포함되어 있습니다. 따라서 기존 .NET Framework 라이브러리를 훨씬 쉽게 마이그레이션한 다음, UWP 애플리케이션을 포함한 여러 .NET 애플리케이션에서 사용할 수 있습니다.

![net-standard](images/dot-net-standard-project-template.png)

[데스크톱 앱 및 UWP 앱 간의 코드 공유](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-migrate)를 참조하세요.

<a id="sql-server" />

### <a name="sql-server-connectivity"></a>SQL Server 연결

앱에서 [System.Data.SqlClient](https://docs.microsoft.com/dotnet/api/system.data.sqlclient) 네임스페이스를 사용하여 SQL Server 데이터베이스에 직접 연결한 다음, 데이터를 저장하고 검색할 수 있습니다.

[UWP 앱에서 SQL Server 데이터베이스 사용](https://docs.microsoft.com/en-us/windows/uwp/data-access/sql-server-databases)을 참조하세요.

<a id="MSIX" />

### <a name="msix-deployment"></a>MSIX 배포

MSIX는 모든 Windows 앱에 최신 패키징 환경을 제공하는 Windows 앱 패키징 형식입니다. MSIX 패키지 형식은 Win32, WPF 및 Windows Forms 앱에 새로운 최신 패키지 및 배포 기능을 사용할 수 있도록 하며, 기존 앱 패키지와 설치 파일의 기능도 유지합니다.

MSIX는 .msi, .appx, App-V 및 ClickOnce 설치 기술의 조합에 따라 안전하고, 안정적이며, 신뢰할 수 있도록 빌드된 패키지 형식입니다.

![MSIX 아이콘](images/MSIX-App-Package.ico)

[MSIX 설명서](https://docs.microsoft.com/windows/msix/)를 참조하세요.

<a id="distribution" />

## <a name="security"></a>보안

Windows 10에서는 앱 개발자가 사용자의 ID, 회사 네트워크의 보안 및 디바이스에 저장된 비즈니스 데이터를 보호할 수 있는 보안 기능 모음을 제공합니다. Windows 10의 새로운 기능인 Microsoft Passport는 엔터프라이즈급 보안을 제공하고 지문, 얼굴 및 홍채 기반 인식 기능을 지원하는 PIN 또는 Windows Hello를 사용하여 액세스할 수 있는 배포하기 쉬운 2단계 암호 대안입니다.

| 항목 | 설명 |
|-------|-------------|
| [보안 Windows 앱 개발 소개](https://docs.microsoft.com/windows/uwp/security/intro-to-secure-windows-app-development) | 이 기초 문서에서는 인증, 진행 데이터(data-in-flight) 및 저장 데이터(data-at-rest) 단계의 다양한 Windows 보안 기능에 대해 설명합니다. 또한 이러한 단계를 앱에 통합하는 방법을 설명합니다. 광범위한 항목을 다루며, 주로 앱 설계자가 유니버설 Windows 플랫폼 앱을 빠르고 쉽게 만들 수 있는 Windows 기능을 더 잘 이해할 수 있도록 지원하는 데 목적이 있습니다. |
| [인증 및 사용자 ID](https://docs.microsoft.com/windows/uwp/security/authentication-and-user-identity) | UWP 앱에는 이 문서에 요약된 여러 가지 사용자 인증 옵션이 있습니다. 엔터프라이즈에는 새로운 Microsoft Passport 기능을 사용하는 것이 좋습니다. Microsoft Passport는 기존 자격 증명을 확인하고 생체 인식 또는 PIN 기반 사용자 제스처로 보호되는 디바이스별 자격 증명을 만들어 편리하고 안전한 환경을 제공하여 암호를 강력한 2FA(2단계 인증)로 대체합니다. |
| [암호화](https://docs.microsoft.com/windows/uwp/security/cryptography) | 암호화 섹션에서는 UWP 앱이 사용할 수 있는 암호화 기능을 간략하게 설명합니다. 중요한 비즈니스 데이터를 쉽게 암호화하는 방법에 대한 기초 연습에서, 암호화 키 조작, MAC, 해시 및 서명 작업 등의 고급 과정에 이르기까지 다양한 문서가 제공됩니다. |
| [WIP(Windows Information Protection)](wip-hub.md) | WIP(Windows Information Protection)가 파일, 버퍼, 클립보드, 네트워킹, 백그라운드 작업 및 잠금 상태의 데이터 보호와 어떤 관계가 있는지를 보여 주는 전체 개발자 그림을 보여 주는 허브 항목입니다. |

## <a name="data-binding-and-databases"></a>데이터 바인딩 및 데이터베이스

데이터 바인딩은 앱의 UI에서 데이터베이스 등의 외부 원본 데이터를 표시하고 선택적으로 해당 데이터와 동기화된 상태를 유지하는 하나의 방법입니다. 데이터 바인딩은 데이터 문제를 UI 문제와 분리하여 개념 모델을 간소화하고 앱의 가독성, 테스트 용이성 및 유지 관리성을 향상시킬 수 있도록 해줍니다.

| 항목 | 설명 |
|-------|-------------|
| [데이터 바인딩 개요](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-quickstart) | UWP(유니버설 Windows 플랫폼) 앱에서 컨트롤(또는 다른 UI 요소)을 단일 항목에 바인딩하거나 항목 컨트롤을 항목 컬렉션에 바인딩하는 방법을 보여 줍니다. 또한 항목의 렌더링을 제어하고 선택 항목을 기반으로 세부 정보 보기를 구현하고, 표시할 데이터를 변환하는 방법을 보여 줍니다. |
| [UWP용 Entity Framework 7](https://docs.microsoft.com/windows/uwp/data-access/entity-framework-7-with-sqlite-for-csharp-apps) | UWP를 지원하는 Entity Framework 7을 사용하면 큰 데이터 집합에 대해 복잡한 쿼리를 간단하게 실행할 수 있습니다. 이 연습에서는 Entity Framework를 사용하여 로컬 SQLite 데이터베이스에 대한 기본 데이터 액세스를 수행하는 UWP 앱을 빌드합니다. |
| [SQLite 로컬 데이터베이스](https://channel9.msdn.com/Series/A-Developers-Guide-to-Windows-10/10) | 이 동영상은 로컬 앱 데이터베이스에 대한 권장 솔루션인 SQLite 사용에 대한 포괄적인 개발자 가이드입니다. [SQLite](https://www.sqlite.org/download.html)를 방문하여 UWP용 최신 버전을 다운로드하거나 Windows 10 SDK와 함께 제공된 버전을 사용하세요. |

## <a name="networking-and-data-serialization"></a>네트워킹 및 데이터 직렬화

LOB(기간 업무) 앱은 다양한 다른 시스템에 데이터를 저장하거나 통신해야 하는 경우가 많습니다. 일반적으로 이 작업을 위해 REST 또는 SOAP와 같은 프로토콜을 사용하여 네트워크 서비스에 연결한 다음 데이터를 공통 형식으로 직렬화하거나 역직렬화합니다. UWP 앱의 네트워크 및 데이터 직렬화 작업은 WPF, WinForms 및 ASP.NET 응용 프로그램과 유사합니다. 자세한 내용은 다음 문서를 참조하세요.

| 항목 | 설명 |
|-------|-------------|
| [네트워킹 기본 사항](https://docs.microsoft.com/windows/uwp/networking/networking-basics) | 이 연습에서는 사용 중인 통신 프로토콜에 관계없이 모든 UWP 앱과 관련된 기본 네트워킹 개념을 설명합니다.  |
| [네트워킹 기술 선택](https://docs.microsoft.com/windows/uwp/networking/which-networking-technology) | UWP 앱에 사용할 수 있는 네트워킹 기술에 대한 빠른 개요 및 앱에 가장 적합한 기술을 선택하는 방법에 대한 제안 사항입니다. |
| [XML 및 SOAP 직렬화](https://docs.microsoft.com/dotnet/framework/serialization/xml-and-soap-serialization) | XML 직렬화는 개체를 특정 XSD(XML 스키마 정의 언어)를 준수하는 XML 스트림으로 변환합니다. XML과 강력한 형식의 클래스 간에 변환하기 위해 네이티브 [XDocument](https://docs.microsoft.com/dotnet/api/system.xml.linq.xdocument) 클래스 또는 외부 라이브러리를 사용할 수 있습니다. |
| [JSON 직렬화](https://docs.microsoft.com/uwp/api/Windows.Data.Json) | JSON(JavaScript Object Notation) 직렬화는 REST API와 통신하는 데 널리 사용되는 형식입니다. UWP 앱에 대해 완벽하게 지원되는 [Newtonsoft Json.NET](https://www.newtonsoft.com/json). |

## <a name="devices"></a>장치

프린터, 바코드 스캐너 또는 스마트 카드 판독기와 같은 LOB(기간 업무) 도구와 통합하기 위해 외부 디바이스나 센서를 앱에 통합해야 할 수도 있습니다. 이 섹션에 설명된 기술을 사용하여 앱에 추가할 수 있는 기능의 몇 가지 예는 다음과 같습니다.

| 항목  | 설명 |
|--------|-------------|
| [디바이스 열거](https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices) | 이 문서에서는 [Windows.Devices.Enumeration](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration) 네임스페이스를 사용하여 시스템에 내부에서 연결되었거나, 외부에서 연결되었거나, 무선 또는 네트워킹 프로토콜을 통해 검색 가능한 디바이스를 찾는 방법을 설명합니다. 디바이스에서 작동하는 앱을 빌드하는 경우 여기서 시작하세요. |
| [인쇄 및 스캔](https://docs.microsoft.com/windows/uwp/devices-sensors/printing-and-scanning) | POS(Point-Of-Sale) 시스템, 영수증 프린터, 대용량 공급 장치 스캐너와 같은 비즈니스 디바이스에 연결하고 사용하는 방법을 포함하여 앱에서 인쇄 및 스캔하는 방법에 대해 설명합니다. |
| [Bluetooth](https://docs.microsoft.com/windows/uwp/devices-sensors/bluetooth) | 기존 Bluetooth 연결을 사용하여 데이터를 주고받거나 디바이스를 제어하는 것 외에도 Windows 10에서는 BTLE(Bluetooth 저에너지)를 사용하여 백그라운드에서 알림을 보내거나 받을 수 있습니다. 사용자가 특정 위치에 가까워지거나 떠날 때 알림을 표시하거나 기능을 사용하도록 설정하려면 사용합니다. |
| [엔터프라이즈 공유 스토리지](enterprise-shared-storage.md) | 디바이스 잠금 시나리오에서 동일한 앱 내에서, 앱 인스턴스 간에 또는 앱 간에도 데이터를 공유할 수 방법을 알아봅니다. |

## <a name="device-targeting"></a>디바이스 대상 지정

오늘날 대부분의 사용자는 다양한 폼 팩터와 화면 크기의 휴대폰이나 태블릿을 회사로 가져옵니다. UWP(유니버설 Windows 플랫폼)를 사용하면 데스크톱 PC 및 PPI 디스플레이를 비롯한 모든 유형의 디바이스에서 원활하게 실행되는 단일 LOB(기간 업무) 앱을 작성하여 앱의 사용자 기반과 코드의 효율성을 최대화할 수 있습니다.

| 항목 | 설명 |
|-------|-------------|
| [UWP 앱 가이드](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide) | 이 기초 가이드에서는 디바이스 패밀리 정의 및 대상 디바이스 패밀리를 결정하는 방법, 다양한 디바이스 폼 팩터에 맞게 UI를 조정할 수 있는 새 UI 컨트롤 및 패널, 앱에서 사용할 수 있는 API 화면을 이해하고 제어하는 방법을 포함하여 Windows 10 UWP 플랫폼에 대해 알아봅니다. |
| [적응형 XAML UI 코드 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) | 이 코드 예제에서는 디바이스 유형에 관계없이 앱에 사용할 수 있는 모든 레이아웃 옵션 및 컨트롤을 보여 주며, 패널과 상호 작용하여 원하는 레이아웃을 얻는 방법을 보여 줍니다. 각 컨트롤이 다양한 폼 팩터에 어떻게 응답하는지를 보여 주는 것은 물론, 앱 자체도 이에 응답하여 적응형 UI를 얻기 위한 다양한 방법을 보여 줍니다. |
| [Xamarin 항목](/xamarin/) | 전화를 대상으로 하는 Xamarin |

## <a name="deployment"></a>배포

엔터프라이즈 사용자에게 앱을 배포하는 옵션이 있습니다. 비즈니스용 Microsoft Store와 기존 모바일 디바이스 관리를 사용하거나 앱을 디바이스에 사이드로드할 수 있습니다. Microsoft Store에 게시하여 일반 사람들에게 앱을 제공할 수도 있습니다.

| 항목 | 설명 |
|-------|-------------|
| [엔터프라이즈에 LOB 앱 배포](https://docs.microsoft.com/windows/uwp/publish/distribute-lob-apps-to-enterprises) | 일반 사용자에게 광범위하게 LOB(기간 업무) 앱을 제공하지 않고도 비즈니스용 Microsoft Store를 통해 대량 구매할 수 있도록 해당 앱을 엔터프라이즈에 직접 게시할 수 있습니다. |
| [앱 사이드로드](https://docs.microsoft.com/windows/deploy/sideload-apps-in-windows-10) | 앱을 테스트용으로 로드할 때 서명된 앱 패키지를 디바이스에 배포합니다. 이러한 앱의 서명, 호스팅 및 배포를 유지 관리합니다. Windows 10에서는 앱을 테스트용으로 로드하는 프로세스가 간소화되었습니다.             |
| [Microsoft Store에 앱 게시](https://developer.microsoft.com/store/publish-apps) | 통합 Microsoft Store를 사용하면 모든 Windows 디바이스용 앱을 모두 게시하고 관리할 수 있습니다. 시장별 가격 책정, 배포 및 가시성 제어, 기타 옵션을 사용하여 앱의 가용성을 사용자 지정하세요. |

## <a name="enterprise-uwp-samples"></a>엔터프라이즈 UWP 샘플

| 항목 |  설명 |
|------ |--------------|
| [VanArsdel 인벤토리 샘플](https://github.com/Microsoft/InventorySample) | 기간 업무 시나리오를 보여 주는 UWP 샘플 앱입니다. 이 샘플은 가상 회사인 VanArsdel의 고객, 주문 및 제품을 만들고 관리하는 것을 기반으로 합니다. |
| [고객 주문 데이터베이스 샘플](https://github.com/Microsoft/Windows-appsample-customers-orders-database) | AAD(Azure Active Directory) 인증, UI 컨트롤(데이터 그리드 포함), Sqlite 및 SQL Azure 데이터베이스 통합, Entity Framework 및 클라우드 API 서비스와 같은 유용한 기능을 엔터프라이즈 개발자에게 보여 주는 UWP 앱 샘플입니다. 이 샘플은 가상 회사인 Contoso의 고객 계정, 주문 및 제품을 만들고 관리하는 것을 기반으로 합니다. |

## <a name="patterns-and-practices"></a>패턴 및 사례

대규모 엔터프라이즈급 앱의 코드베이스는 제어하기 어려울 수 있습니다. Prism은 WPF, Windows 10 UWP 및 Xamarin Forms에서 느슨하게 결합되고 유지 관리 및 테스트가 가능한 XAML 애플리케이션을 빌드하기 위한 프레임워크입니다. Prism은 MVVM, 종속성 주입, 명령, EventAggregator 등을 포함하여 체계적이고 유지 관리가 가능한 XAML 응용 프로그램을 작성하는 데 도움이 되는 디자인 패턴 컬렉션의 구현을 제공합니다.

Prism에 대한 자세한 내용은 [GitHub 리포지토리](https://github.com/PrismLibrary/Prism)를 참조하세요.

 

 

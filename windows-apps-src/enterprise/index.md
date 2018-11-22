---
ms.assetid: 4b0c86d3-f05b-450b-bf9c-6ab4d3f07d31
description: 이 로드맵은 Windows10 및 유니버설 Windows 플랫폼 (UWP) 앱에 대 한 주요 엔터프라이즈 기능 개요를 제공합니다.
title: Enterprise
author: awkoren
ms.author: alkoren
ms.date: 08/30/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ffdc88449c025ba0a590ccc2bbd3f0c05346630f
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/22/2018
ms.locfileid: "7579078"
---
# <a name="enterprise"></a>Enterprise

이 로드맵은 Windows10Universal Windows 플랫폼 (UWP) 앱에 대 한 주요 엔터프라이즈 기능 개요를 제공합니다.

**참고**이 문서는 개발자를 대상으로 엔터프라이즈 UWP 앱을 작성 합니다. 일반 UWP 개발에 대한 자세한 내용은 [Windows 10 앱 사용 방법 가이드](https://msdn.microsoft.com/library/windows/apps/mt244352)를 참조하세요. WPF, Windows Forms 또는 Win32 개발에 대한 자세한 내용은 [데스크톱 개발자 센터](https://dev.windows.com/desktop)를 참조하세요. Windows 10 배포 또는 엔터프라이즈 보안 기능 관리 등의 IT 전문가 리소스에 대한 자세한 내용은 [TechNet의 Windows 10](https://msdn.microsoft.com/library/dn986868)을 참조하세요.

빌드 시이 프레젠테이션에서 [신속 하 게 구성 LOB 응용 프로그램 UWP 및 Visual Studio를 사용 하 여](https://channel9.msdn.com/Events/Build/2018/BRK3502) 보여 줍니다 된는 발전 중 일부를 보여 주는이 응용 프로그램의 버전이 있나요

앞에 세우고 사항:

## <a name="whats-new-for-enterprise-applications"></a>엔터프라이즈 응용 프로그램에 대 한 새로운 기능

다음 몇 가지 도구, 라이브러리 및 매우 만들어져 있는 접근 권한 값은 최근에 합니다.

> [!div class="checklist"]
> * [Windows Template Studio](#template-studio)
> * [데스크톱 스타일 Ui를 만들 수 있는 컨트롤](#desktop-style-UI)
> * [엔터프라이즈 시나리오를 지 원하는 컨트롤](#enterprise)
> * [Windows UI 라이브러리](#UI-library)
> * [데스크톱 응용 프로그램의 UWP 컨트롤](#xaml-islands)
> * [.NET Standard 2.0](#standard)
> * [SQL Server 연결](#sql-server)
> * [MSIX 배포](#MSIX)

<a id="template-studio" />

### <a name="windows-template-studio"></a>Windows Template Studio

Windows Template Studio 마법사 기반 경험을 사용 하 여 새 유니버설 Windows 플랫폼 (UWP) 앱 만들기를 가속화 하는 Visual Studio 2017 확장 됩니다. 결과 UWP 프로젝트는 입증 된 패턴과 모범 사례를 구현 하는 동안 최신 Windows 10 기능을 통합 하는 올바른 형식을 갖춘, 읽을 수 있는 코드입니다.

![Windows Template Studio](images/windows-template-studio.png)

[Windows Template Studio](https://marketplace.visualstudio.com/items?itemName=WASTeamAccount.WindowsTemplateStudio) 를 참조 하세요.

<a id="desktop-style-UI" />

### <a name="controls-to-create-desktop-style-uis"></a>데스크톱 스타일 Ui를 만들 수 있는 컨트롤

우리 UWP UI와 일반적인 데스크톱 응용 프로그램 UI 간의 격차를 작성 하는 새 UWP XAML 컨트롤 출시 했습니다.

예를 들어 새 [메뉴 모음](https://review.docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/menus?branch=jimwalk%2Frs5-menu-bar), [DropDownButton](https://docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/buttons#create-a-drop-down-button), [분할 단추](https://docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/buttons#create-a-split-button)및 [CommandBarFlyout](https://review.docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/command-bar-flyout?branch=jimwalk%2Frs5-command-bar-flyout) 컨트롤, 명령을 노출 더 유연한 방법을 제공 하 고 [EditableComboBox](https://review.docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/combo-box?branch=rs5#make-a-combo-box-editable) 보겠습니다 사용자 입력 나열 되지 않은 값 미리 정의 된 목록이 옵션입니다.

![메뉴 모음](images/menu-bar.png)

<a id="enterprise" />

### <a name="controls-to-support-enterprise-scenarios"></a>엔터프라이즈 시나리오를 지 원하는 컨트롤

[DataGridView](https://docs.microsoft.com/en-us/windows/communitytoolkit/controls/datagrid) 행과 열에 데이터 컬렉션을 표시 하는 유연한 방법을 제공 합니다.

[TreeView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/tree-view) 는 중첩 된 항목이 포함 된 노드를 확장 및 축소 하는 계층적 목록을 수 있습니다. 이 컨트롤은 UI에 폴더 구조나 중첩된 관계를 나타내는 데 사용할 수 있습니다.

![DataGrid 컨트롤](images/DataGrid.gif)


### <a name="windows-ui-library"></a>Windows UI 라이브러리

Windows UI 라이브러리는 UWP 앱에 대 한 컨트롤 및 기타 사용자 인터페이스 요소를 제공 하는 NuGet 패키지의 집합입니다. 또한 사용자가 최신 OS 없는 경우에 앱이 작동 하므로 하위 수준 이전 버전의 Windows 10 호환이 있습니다.

![Windows UI 라이브러리](images/win-ui.png)

[Windows UI 라이브러리 (미리 보기 버전)를](https://docs.microsoft.com/en-us/uwp/toolkits/winui/)참조 하세요.

<a id="xaml-islands" />

### <a name="uwp-controls-in-desktop-applications"></a>데스크톱 응용 프로그램의 UWP 컨트롤

이제 Windows 10을 사용 하면 WPF, Windows Forms, 및 c + + Win32 데스크톱 응용 프로그램의 UWP 컨트롤을 사용할 수 있습니다. 즉, 모양, 느낌 및 Windows Ink Fluent 디자인 시스템을 지 원하는 컨트롤 등 UWP 컨트롤을 통해만 사용할 수 있는 최신 Windows 10 UI 기능으로 기존 데스크톱 응용 프로그램의 기능을 개선할 수 있습니다. 이 기능은 XAML 제도 이라고 합니다.

[데스크톱 응용 프로그램의 UWP 컨트롤](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-host-controls)을 참조 하세요.

<a id="standard" />

### <a name="net-standard-20"></a>.NET Standard 2.0

.NET 표준.NET Standard 보다 더 많은 Api를 통해 20000 포함 1.x 합니다. 기존.NET Framework 라이브러리 마이그레이션하고 UWP 응용 프로그램을 포함 하 여 다른.NET 응용 프로그램 간에 용도를 훨씬 쉽게 있습니다.

![net 표준](images/dot-net-standard-project-template.png)

[데스크톱 앱과 UWP 앱 간의 공유 코드](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-migrate)를 참조 하세요.

<a id="sql-server" />

### <a name="sql-server-connectivity"></a>SQL Server 연결

앱을 SQL Server 데이터베이스에 직접 연결한 후, [System.Data.SqlClient](https://docs.microsoft.com/dotnet/api/system.data.sqlclient?redirectedfrom=MSDN&view=netframework-4.7.2) 네임스페이스를 사용해 데이터를 저장 및 검색할 수 있습니다.

[UWP 앱에서 SQL Server 데이터베이스 사용](https://docs.microsoft.com/en-us/windows/uwp/data-access/sql-server-databases)을 참조하세요.

<a id="MSIX" />

### <a name="msix-deployment"></a>MSIX 배포

MSIX는 모든 Windows 앱에 최신 패키징 환경을 제공 하는 Windows 앱 패키지 형식입니다. Msix 패키지는 기존 앱 패키지의 기능을 유지 하 고 Win32, WPF 및 Windows Forms 앱에 새, 최신 패키징 및 배포 기능을 사용 하는 것 외에도 파일을 설치 합니다.

MSIX은을 안정적으로 하도록 작성 하는 패키징 형식.msi,.appx, App-v 및 ClickOnce 설치 기술을의 조합을 기반으로 합니다.

![MSIX 아이콘](images/WinUI_MSIX_2col_740x417.png)

[MSIX 문서](https://docs.microsoft.com/windows/msix/)를 참조 하세요.

<a id="distribution" />

## <a name="security"></a>보안

Windows10은 사용자에 게, 회사 네트워크의 보안 및 장치에 저장 된 비즈니스 데이터의 id를 보호 하기 위해 앱 개발자에 대 한 보안 기능 집합을 제공 합니다. 새 Windows10에 대 한이 Microsoft Passport는 PIN을 사용 하 여 액세스할 수 있는 배포를 쉽게 2 단계 암호 대 안으로 또는 Windows Hello, 엔터프라이즈급 보안을 제공 하 고 지문, 얼굴 지원이 홍 채 기반 인식 합니다.

| 항목 | 설명 |
|-------|-------------|
| [보안 Windows 앱 개발 소개](https://msdn.microsoft.com/library/windows/apps/mt622741) | 이 기초 문서에서는 인증, 진행 데이터(data-in-flight) 및 저장 데이터(data-at-rest) 단계의 다양한 Windows 보안 기능에 대해 설명합니다. 또한 이러한 단계를 앱에 통합하는 방법을 설명합니다. 빠르고 쉽게 유니버설 Windows 플랫폼 앱을 만들 수 있게 하는 Windows 기능을 이해 하는 앱 설계자 서 주로 목적은 및 다양 한 주제를 다룹니다. |
| [인증 및 사용자 ID](https://msdn.microsoft.com/library/windows/apps/mt270184) | UWP 앱에는 이 문서에 요약된 여러 가지 사용자 인증 옵션이 있습니다. 엔터프라이즈에는 새로운 Microsoft Passport 기능을 사용하는 것이 좋습니다. Microsoft Passport가 기존 자격 증명을 확인 하 여 강력한 2 단계 인증 (2FA)를 사용 하 여 암호를 대체 하 고 디바이스 별 자격 증명을 만들어 생체 인식 또는 PIN 기반 사용자 제스처로 보호 되는 모두 편리 하 고 높은 안전한 환경을 위해서는 합니다. |
| [Cryptography](https://msdn.microsoft.com/library/windows/apps/mt270191) | 암호화 섹션에서는 UWP 앱이 사용할 수 있는 암호화 기능을 간략하게 설명합니다. 중요한 비즈니스 데이터를 쉽게 암호화하는 방법에 대한 기초 연습에서, 암호화 키 조작, MAC, 해시 및 서명 작업 등의 고급 과정에 이르기까지 다양한 문서가 제공됩니다. |
| [WIP(Windows Information Protection)](wip-hub.md) | WIP(Windows Information Protection)가 파일, 버퍼, 클립보드, 네트워킹, 백그라운드 작업 및 잠금 상태의 데이터 보호와 어떤 관계가 있는지를 보여 주는 전체 개발자 그림을 보여 주는 허브 항목입니다. |

## <a name="data-binding-and-databases"></a>데이터 바인딩 및 데이터베이스

데이터 바인딩은 앱의 UI에서 데이터베이스 등의 외부 원본 데이터를 표시하고 선택적으로 해당 데이터와 동기화된 상태를 유지하는 하나의 방법입니다. 데이터 바인딩은 데이터 문제를 UI 문제와 분리하여 개념 모델을 간소화하고 앱의 가독성, 테스트 용이성 및 유지 관리성을 향상시킬 수 있도록 해줍니다.

| 항목 | 설명 |
|-------|-------------|
| [데이터 바인딩 개요](https://msdn.microsoft.com/library/windows/apps/mt269383) | 이 항목은 유니버설 Windows 플랫폼 (UWP) 앱에 있는 항목의 컬렉션에 항목 컨트롤에 바인딩하거나 컨트롤 (또는 다른 UI 요소)을 단일 항목에 바인딩하는 방법을 보여 줍니다. 또한 항목의 렌더링을 제어하고 선택 항목을 기반으로 세부 정보 보기를 구현하고, 표시할 데이터를 변환하는 방법을 보여 줍니다. |
| [UWP용 Entity Framework 7](https://msdn.microsoft.com/library/windows/apps/mt592863) | UWP를 지원하는 Entity Framework 7을 사용하면 큰 데이터 집합에 대해 복잡한 쿼리를 간단하게 실행할 수 있습니다. 이 연습에서는 Entity Framework를 사용 하 여 로컬 SQLite 데이터베이스에 대해 기본 데이터 액세스를 수행 하는 UWP 앱을 빌드합니다. |
| [SQLite 로컬 데이터베이스](https://channel9.msdn.com/Series/A-Developers-Guide-to-Windows-10/10) | 이 동영상은 로컬 앱 데이터베이스에 대한 권장 솔루션인 SQLite 사용에 대한 포괄적인 개발자 가이드입니다. [SQLite](https://www.sqlite.org/download.html)를 방문하여 UWP용 최신 버전을 다운로드하거나 Windows 10 SDK와 함께 제공된 버전을 사용하세요. |

## <a name="networking-and-data-serialization"></a>네트워킹 및 데이터 직렬화

LOB(기간 업무) 앱은 다양한 다른 시스템에 데이터를 저장하거나 통신해야 하는 경우가 많습니다. 일반적으로 이 작업을 위해 REST 또는 SOAP와 같은 프로토콜을 사용하여 네트워크 서비스에 연결한 다음 데이터를 공통 형식으로 직렬화하거나 역직렬화합니다. UWP 앱의 네트워크 및 데이터 직렬화 작업은 WPF, WinForms 및 ASP.NET 응용 프로그램과 유사합니다. 자세한 내용은 다음 문서를 참조하세요.

| 항목 | 설명 |
|-------|-------------|
| [네트워킹 기본 사항](https://msdn.microsoft.com/library/windows/apps/mt280233) | 이 연습에서는 사용 중인 통신 프로토콜에 관계없이 모든 UWP 앱과 관련된 기본 네트워킹 개념을 설명합니다.  |
| [네트워킹 기술 선택](https://msdn.microsoft.com/library/windows/apps/mt280235) | UWP 앱에 사용할 수 있는 네트워킹 기술에 대한 빠른 개요 및 앱에 가장 적합한 기술을 선택하는 방법에 대한 제안 사항입니다. |
| [XML 및 SOAP 직렬화](https://msdn.microsoft.com/library/90c86ass.aspx) | XML 직렬화는 특정 XML 스키마 정의 언어 (XSD) 준수 하는 XML 스트림으로 개체를 변환 합니다. XML과 강력한 형식의 클래스 간에 변환하기 위해 네이티브 [XDocument](https://msdn.microsoft.com/library/system.xml.linq.xdocument.aspx) 클래스 또는 외부 라이브러리를 사용할 수 있습니다. |
| [JSON 직렬화](https://msdn.microsoft.com/library/windows/apps/br240639) | JSON (JavaScript object notation) 직렬화는 REST Api와의 통신에 대 한 인기 있는 형식. UWP 앱에 대해 완벽하게 지원되는 [Newtonsoft Json.NET](http://www.newtonsoft.com/json). |

## <a name="devices"></a>디바이스

프린터, 바코드 스캐너 또는 스마트 카드 판독기와 같은 LOB(기간 업무) 도구와 통합하기 위해 외부 디바이스나 센서를 앱에 통합해야 할 수도 있습니다. 이 섹션에 설명된 기술을 사용하여 앱에 추가할 수 있는 기능의 몇 가지 예는 다음과 같습니다.

| 항목  | 설명 |
|--------|-------------|
| [디바이스 열거](https://msdn.microsoft.com/library/windows/apps/mt187355) | 이 문서에서는 [Windows.Devices.Enumeration](https://msdn.microsoft.com/library/windows/apps/br225459) 네임스페이스를 사용하여 시스템에 내부에서 연결되었거나, 외부에서 연결되었거나, 무선 또는 네트워킹 프로토콜을 통해 검색 가능한 디바이스를 찾는 방법을 설명합니다. 디바이스에서 작동하는 앱을 빌드하는 경우 여기서 시작하세요. |
| [인쇄 및 스캔](https://msdn.microsoft.com/library/windows/apps/mt204544) | 연결 하 고 POS (point-of-sale) 시스템, 영수증 프린터, 대용량 공급 디바이스 스캐너 등의 비즈니스 디바이스를 사용 하 여 작업을 포함 하 여 앱에서 인쇄 및 스캔 하는 방법에 설명 합니다. |
| [Bluetooth](https://msdn.microsoft.com/library/windows/apps/mt270288) | 기존 Bluetooth 연결을 사용하여 데이터를 주고받거나 디바이스를 제어하는 것 외에도 Windows 10에서는 BTLE(Bluetooth 저에너지)를 사용하여 백그라운드에서 알림을 보내거나 받을 수 있습니다. 사용자가 특정 위치에 가까워지거나 떠날 때 알림을 표시하거나 기능을 사용하도록 설정하려면 사용합니다. |
| [엔터프라이즈 공유 저장소](enterprise-shared-storage.md) | 디바이스 잠금 시나리오에서 동일한 앱 내에서, 앱 인스턴스 간에 또는 앱 간에도 데이터를 공유할 수 방법을 알아봅니다. |

## <a name="device-targeting"></a>디바이스 대상 지정

오늘날 대부분의 사용자는 다양한 폼 팩터와 화면 크기의 휴대폰이나 태블릿을 회사로 가져옵니다. UWP(유니버설 Windows 플랫폼)를 사용하면 데스크톱 PC 및 PPI 디스플레이를 비롯한 모든 유형의 디바이스에서 원활하게 실행되는 단일 LOB(기간 업무) 앱을 작성하여 앱의 사용자 기반과 코드의 효율성을 최대화할 수 있습니다.

| 항목 | 설명 |
|-------|-------------|
| [UWP 앱 가이드](https://msdn.microsoft.com/library/windows/apps/dn894631) | 이 기초 가이드에서는 디바이스 패밀리 정의 및 대상 디바이스 패밀리를 결정하는 방법, 다양한 디바이스 폼 팩터에 맞게 UI를 조정할 수 있는 새 UI 컨트롤 및 패널, 앱에서 사용할 수 있는 API 화면을 이해하고 제어하는 방법을 포함하여 Windows 10 UWP 플랫폼에 대해 알아봅니다. |
| [적응형 XAML UI 코드 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619992) | 이 코드 샘플 모든 레이아웃 옵션 및 디바이스 유형에 관계 없이 앱에 대 한 컨트롤 보여주며 찾고자 하는 모든 레이아웃을 얻는 방법을 보여주는 패널과 상호 작용할 수 있습니다. 각 컨트롤이 다양한 폼 팩터에 어떻게 응답하는지를 보여 주는 것은 물론, 앱 자체도 이에 응답하여 적응형 UI를 얻기 위한 다양한 방법을 보여 줍니다. |
| [Xamarin 항목]() | Xamarin 전화를 대상으로 하는 것에 대 한 |

## <a name="deployment"></a>배포

엔터프라이즈 사용자에게 앱을 배포하는 옵션이 있습니다. Microsoft 스토어를 사용 하 여 기존 모바일 장치 관리 비즈니스용 또는 장치에 앱을 테스트용으로 로드할 수 있습니다. 또한 가능 앱 사용할 수 있는 일반 공개 Microsoft Store에 게시 하 여 합니다.

| 항목 | 설명 |
|-------|-------------|
| [엔터프라이즈에 LOB 앱 배포](https://msdn.microsoft.com/library/windows/apps/mt608995) | 비즈니스 라인 앱을 공개적으로 앱을 광범위 하 게 사용할 수 있도록 하지 않고 비즈니스용 Microsoft 스토어를 통해 대량 구매에 대 한 엔터프라이즈에 직접 게시할 수 있습니다. |
| [앱 사이드로드](https://technet.microsoft.com/library/mt269549) | 앱을 테스트용으로 로드할 때 서명된 앱 패키지를 디바이스에 배포합니다. 이러한 앱의 서명, 호스팅 및 배포를 유지 관리합니다. Windows 10에서는 앱을 테스트용으로 로드하는 프로세스가 간소화되었습니다.             |
| [Microsoft Store에 앱을 게시 합니다.](https://dev.windows.com/publish) | 통합 된 Microsoft Store에 게시 하 고 모든 Windows 장치용 앱을 관리할 수 있습니다. 시장별 가격 책정, 배포 및 가시성 제어, 기타 옵션을 사용하여 앱의 가용성을 사용자 지정하세요. |

## <a name="enterprise-uwp-samples"></a>엔터프라이즈 UWP 샘플

소개 텍스트 여기에 표시 됩니다.

작업-음성 채팅 아니므로 및/또는 Karl 함께 엔터프라이즈에 초점을 맞춘 더 많은 샘플을 가져옵니다.

| 항목 |  설명 |
|------ |--------------|
| [VanArsdel 인벤토리 샘플](https://github.com/Microsoft/InventorySample) | 데스크톱 응용 프로그램에서 최신 Windows 기능을 사용 하는 방법을 보여 주는 업무용 시나리오에 초점을 샘플 Windows 10 응용 프로그램 (유니버설 Windows 플랫폼을 사용 하 여). 샘플은를 기반으로 만들고 VanArsdel 가상 회사에 대 한 고객, 주문 및 제품을 관리 합니다.
MVVM을 강조 표시 SQL 데이터베이스, Entity Framework 합니다. 다른 사용자를 나열 합니다.|

## <a name="patterns-and-practices"></a>패턴 및 사례

대규모 엔터프라이즈급 앱의 코드베이스는 제어하기 어려울 수 있습니다. Prism은 WPF, Windows10 UWP 및 Xamarin Forms에서 느슨하게 결합, 유지 관리 및 테스트가 가능한 XAML 응용 프로그램을 빌드하기 위한 프레임 워크입니다. Prism은 MVVM, 종속성 주입, 명령, EventAggregator 등을 포함하여 체계적이고 유지 관리가 가능한 XAML 응용 프로그램을 작성하는 데 도움이 되는 디자인 패턴 컬렉션의 구현을 제공합니다.

Prism에 대한 자세한 내용은 [GitHub 리포지토리](https://github.com/PrismLibrary/Prism)를 참조하세요.

 

 

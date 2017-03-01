---
ms.assetid: 4b0c86d3-f05b-450b-bf9c-6ab4d3f07d31
description: "이 로드맵은 Windows 10 및 UWP(유니버설 Windows 플랫폼) 앱의 주요 엔터프라이즈 기능을 간략하게 설명합니다."
title: "엔터프라이즈"
author: awkoren
ms.author: alkoren
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: ffbccf88cd00331b622c158a7e46773ae62197e2
ms.lasthandoff: 02/07/2017

---

# <a name="enterprise"></a>엔터프라이즈


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

이 로드맵은 Windows 10 UWP(유니버설 Windows 플랫폼) 앱에 대한 주요 엔터프라이즈 기능을 간략하게 설명합니다. Windows 10에서는 모든 디바이스에 맞게 조정되는 하나의 앱을 만들어 한 번 작성하여 모든 디바이스에 배포할 수 있습니다. 조직에서 필요로 하는 보안, 관리 및 구성에 대한 제어 기능을 제공하는 동시에 사용자가 기대하는 멋진 환경을 빌드할 수 있습니다.

**참고**  이 문서는 엔터프라이즈 UWP 앱을 작성하는 개발자를 대상으로 합니다. 일반 UWP 개발에 대한 자세한 내용은 [Windows 10 앱 사용 방법 가이드](https://msdn.microsoft.com/library/windows/apps/mt244352)를 참조하세요. WPF, Windows Forms 또는 Win32 개발에 대한 자세한 내용은 [데스크톱 개발자 센터](https://dev.windows.com/desktop)를 참조하세요. Windows 10 배포 또는 엔터프라이즈 보안 기능 관리 등의 IT 전문가 리소스에 대한 자세한 내용은 [TechNet의 Windows 10](https://msdn.microsoft.com/library/dn986868)을 참조하세요.

 

## <a name="security"></a>보안


Windows 10에서는 앱 개발자가 사용자의 ID, 회사 네트워크의 보안 및 디바이스에 저장된 비즈니스 데이터를 보호할 수 있도록 보안 기능 집합을 제공합니다. Windows 10의 새로운 기능인 Microsoft Passport는 PIN 또는 Windows Hello를 사용하여 액세스할 수 있으며 엔터프라이즈급 보안을 제공하고 지문, 얼굴 및 홍채 기반 인식 기능을 지원하는 2단계 암호 대안으로, 배포가 용이합니다.

| 항목 | 설명 |
|-------|-------------|
| [보안 Windows 앱 개발 소개](https://msdn.microsoft.com/library/windows/apps/mt622741) | 이 기초 문서에서는 인증, 진행 데이터(data-in-flight) 및 저장 데이터(data-at-rest) 단계의 다양한 Windows 보안 기능에 대해 설명합니다. 또한 이러한 단계를 앱에 통합하는 방법을 설명합니다. 이 문서는 광범위한 항목을 다루며, 빠르고 쉽게 유니버설 Windows 플랫폼 앱을 만들 수 있게 하는 Windows 기능에 대한 앱 설계자의 이해를 돕기 위해 작성되었습니다. |
| [인증 및 사용자 ID](https://msdn.microsoft.com/library/windows/apps/mt270184) | UWP 앱에는 이 문서에 요약된 여러 가지 사용자 인증 옵션이 있습니다. 엔터프라이즈에는 새로운 Microsoft Passport 기능을 사용하는 것이 좋습니다. Microsoft Passport는 기존 자격 증명을 확인하고 생체 인식 또는 PIN 기반 사용자 제스처로 보호되는 장치별 자격 증명을 만들어 암호를 강력한 2FA(2단계 인증)로 대체함으로써 편리하고 안전한 환경을 만듭니다. |
| [암호화](https://msdn.microsoft.com/library/windows/apps/mt270191) | 암호화 섹션에서는 UWP 앱이 사용할 수 있는 암호화 기능을 간략하게 설명합니다. 중요한 비즈니스 데이터를 쉽게 암호화하는 방법에 대한 기초 연습에서, 암호화 키 조작, MAC, 해시 및 서명 작업 등의 고급 과정에 이르기까지 다양한 문서가 제공됩니다. |
| [WIP(Windows Information Protection)](wip-hub.md) | WIP(Windows Information Protection)가 파일, 버퍼, 클립보드, 네트워킹, 백그라운드 작업 및 잠금 상태에서의 데이터 보호와 어떤 관계가 있는지를 보여 주는 전체 개발자 그림을 보여 주는 허브 항목입니다. |

 

## <a name="data-binding-and-databases"></a>데이터 바인딩 및 데이터베이스


데이터 바인딩은 앱의 UI에서 데이터베이스 등의 외부 원본 데이터를 표시하고 선택적으로 해당 데이터와 동기화된 상태를 유지하는 하나의 방법입니다. 데이터 바인딩은 데이터 문제를 UI 문제와 분리하여 개념 모델을 간소화하고 앱의 가독성, 테스트 용이성 및 유지 관리성을 향상시킬 수 있도록 해줍니다.

| 항목 | 설명 |
|-------|-------------|
| [데이터 바인딩 개요](https://msdn.microsoft.com/library/windows/apps/mt269383) | 이 항목에서는 UWP(유니버설 Windows 플랫폼) 앱에서 컨트롤(또는 다른 UI 요소)을 단일 항목에 바인딩하거나 항목 컨트롤을 항목 컬렉션에 바인딩하는 방법을 보여줍니다. 또한 항목의 렌더링을 제어하고 선택 항목을 기반으로 세부 정보 보기를 구현하고, 표시할 데이터를 변환하는 방법을 보여 줍니다. |
| [UWP용 Entity Framework 7](https://msdn.microsoft.com/library/windows/apps/mt592863) | UWP를 지원하는 Entity Framework 7을 사용하면 큰 데이터 집합에 대해 복잡한 쿼리를 간단하게 실행할 수 있습니다. 이 연습에서는 Entity Framework를 사용하여 로컬 SQLite 데이터베이스에 대해 기본 데이터 액세스를 수행하는 UWP 앱을 빌드합니다. |
| [SQLite 로컬 데이터베이스](https://channel9.msdn.com/Series/A-Developers-Guide-to-Windows-10/10) | 이 동영상은 로컬 앱 데이터베이스에 대한 권장 솔루션인 SQLite 사용에 대한 포괄적인 개발자 가이드입니다. [SQLite](https://www.sqlite.org/download.html)를 방문하여 UWP용 최신 버전을 다운로드하거나 Windows 10 SDK와 함께 제공된 버전을 사용하세요. |

 

## <a name="networking-and-data-serialization"></a>네트워킹 및 데이터 직렬화


LOB(기간 업무) 앱은 다양한 다른 시스템에 데이터를 저장하거나 통신해야 하는 경우가 많습니다. 일반적으로 이 작업을 위해 REST 또는 SOAP와 같은 프로토콜을 사용하여 네트워크 서비스에 연결한 다음 데이터를 공통 형식으로 직렬화하거나 역직렬화합니다. UWP 앱의 네트워크 및 데이터 직렬화 작업은 WPF, WinForms 및 ASP.NET 응용 프로그램과 유사합니다. 자세한 내용은 다음 문서를 참조하세요.

| 항목 | 설명 |
|-------|-------------|
| [네트워킹 기본 사항](https://msdn.microsoft.com/library/windows/apps/mt280233) | 이 연습에서는 사용 중인 통신 프로토콜에 관계없이 모든 UWP 앱과 관련된 기본 네트워킹 개념을 설명합니다.  |
| [네트워킹 기술 선택](https://msdn.microsoft.com/library/windows/apps/mt280235) | UWP 앱에 사용할 수 있는 네트워킹 기술에 대한 빠른 개요 및 앱에 가장 적합한 기술을 선택하는 방법에 대한 제안 사항입니다. |
| [XML 및 SOAP 직렬화](https://msdn.microsoft.com/library/90c86ass.aspx) | XML 직렬화는 특정 XSD(XML 스키마 정의 언어)를 준수하는 XML 스트림으로 개체를 변환합니다. XML과 강력한 형식의 클래스 간에 변환하기 위해 네이티브 [XDocument](https://msdn.microsoft.com/library/system.xml.linq.xdocument.aspx) 클래스 또는 외부 라이브러리를 사용할 수 있습니다. |
| [JSON 직렬화](https://msdn.microsoft.com/library/windows/apps/br240639) | JSON(JavaScript Object Notation) 직렬화는 REST API와의 통신에 많이 사용되는 형식입니다. UWP 앱에 대해 완벽하게 지원되는 [Newtonsoft Json.NET](http://www.newtonsoft.com/json). |

 

## <a name="devices"></a>디바이스


프린터, 바코드 스캐너 또는 스마트 카드 판독기와 같은 LOB(기간 업무) 도구와 통합하기 위해 외부 디바이스나 센서를 앱에 통합해야 할 수도 있습니다. 이 섹션에 설명된 기술을 사용하여 앱에 추가할 수 있는 기능의 몇 가지 예는 다음과 같습니다.

| 항목  | 설명 |
|--------|-------------|
| [디바이스 열거](https://msdn.microsoft.com/library/windows/apps/mt187355) | 이 문서에서는 [Windows.Devices.Enumeration](https://msdn.microsoft.com/library/windows/apps/br225459) 네임스페이스를 사용하여 시스템에 내부에서 연결되었거나, 외부에서 연결되었거나, 무선 또는 네트워킹 프로토콜을 통해 검색 가능한 디바이스를 찾는 방법을 설명합니다. 디바이스에서 작동하는 앱을 빌드하는 경우 여기서 시작하세요. |
| [인쇄 및 스캔](https://msdn.microsoft.com/library/windows/apps/mt204544) | POS(Point-Of-Sale) 시스템, 영수증 프린터, 대용량 공급 장치 스캐너 등의 비즈니스 장치에 연결하고 사용하는 방법을 포함하여 앱에서 인쇄 및 스캔하는 방법을 설명합니다. |
| [Bluetooth](https://msdn.microsoft.com/library/windows/apps/mt270288) | 기존 Bluetooth 연결을 사용하여 데이터를 주고받거나 디바이스를 제어하는 것 외에도 Windows 10에서는 BTLE(Bluetooth 저에너지)를 사용하여 백그라운드에서 알림을 보내거나 받을 수 있습니다. 사용자가 특정 위치에 가까워지거나 떠날 때 알림을 표시하거나 기능을 사용하도록 설정하려면 사용합니다. |
| [엔터프라이즈 공유 저장소](enterprise-shared-storage.md) | 디바이스 잠금 시나리오에서 동일한 앱 내에서, 앱 인스턴스 간에 또는 앱 간에도 데이터를 공유할 수 방법을 알아봅니다. |

 

## <a name="device-targeting"></a>디바이스 대상 지정


오늘날 대부분의 사용자는 다양한 폼 팩터와 화면 크기의 휴대폰이나 태블릿을 회사로 가져옵니다. UWP(유니버설 Windows 플랫폼)를 사용하면 데스크톱 PC 및 PPI 디스플레이를 비롯한 모든 유형의 디바이스에서 원활하게 실행되는 단일 LOB(기간 업무) 앱을 작성하여 앱의 사용자 기반과 코드의 효율성을 최대화할 수 있습니다.

| 항목 | 설명 |
|-------|-------------|
| [UWP 앱 가이드](https://msdn.microsoft.com/library/windows/apps/dn894631) | 이 기초 가이드에서는 디바이스 패밀리 정의 및 대상 디바이스 패밀리를 결정하는 방법, 다양한 디바이스 폼 팩터에 맞게 UI를 조정할 수 있는 새 UI 컨트롤 및 패널, 앱에서 사용할 수 있는 API 화면을 이해하고 제어하는 방법을 포함하여 Windows 10 UWP 플랫폼에 대해 알아봅니다. |
| [적응형 XAML UI 코드 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619992) | 이 코드 예제에서는 장치 유형에 관계없이 앱에 사용할 수 있는 모든 레이아웃 옵션 및 컨트롤을 보여주며, 패널을 조작하여 원하는 레이아웃을 얻는 방법을 보여줍니다. 각 컨트롤이 다양한 폼 팩터에 어떻게 응답하는지를 보여 주는 것은 물론, 앱 자체도 이에 응답하여 적응형 UI를 얻기 위한 다양한 방법을 보여 줍니다. |

 

## <a name="deployment"></a>배포


엔터프라이즈 사용자에게 앱을 배포하는 옵션이 있습니다. 비즈니스용 Windows 스토어와 기존 모바일 디바이스 관리를 사용하거나 앱을 디바이스에 테스트용으로 로드할 수 있습니다. Windows 스토어에 게시하여 일반 사람들에게 앱을 제공할 수도 있습니다.

| 항목 | 설명 |
|-------|-------------|
| [엔터프라이즈에 LOB 앱 배포](https://msdn.microsoft.com/library/windows/apps/mt608995) | LOB(기간 업무) 앱을 일반 사용자에게 광범위하게 제공하지 않고 비즈니스용 Windows 스토어를 통해 대량 구매하도록 엔터프라이즈에 직접 해당 앱을 게시할 수 있습니다. |
| [앱 테스트용 로드](https://technet.microsoft.com/library/mt269549) | 앱을 테스트용으로 로드할 때 서명된 앱 패키지를 디바이스에 배포합니다. 이러한 앱의 서명, 호스팅 및 배포를 유지 관리합니다. Windows 10에서는 앱을 테스트용으로 로드하는 프로세스가 간소화되었습니다.             |
| [Windows 스토어에 앱 게시](https://dev.windows.com/publish) | 통합 Windows 스토어에서는 모든 Windows 디바이스용 앱을 모두 게시하고 관리할 수 있습니다. 시장별 가격 책정, 배포 및 가시성 제어, 기타 옵션을 사용하여 앱의 가용성을 사용자 지정하세요. |

 

## <a name="patterns-and-practices"></a>패턴 및 사례


대규모 엔터프라이즈급 앱의 코드베이스는 제어하기 어려울 수 있습니다. Prism은 WPF, Windows 10 UWP 및 Xamarin Forms에서 느슨하게 결합되고 유지 관리 및 테스트가 가능한 XAML 응용 프로그램을 빌드하기 위한 프레임워크입니다. Prism은 MVVM, 종속성 주입, 명령, EventAggregator 등을 포함하여 체계적이고 유지 관리가 가능한 XAML 응용 프로그램을 작성하는 데 도움이 되는 디자인 패턴 컬렉션의 구현을 제공합니다.

Prism에 대한 자세한 내용은 [GitHub 리포지토리](https://github.com/PrismLibrary/Prism)를 참조하세요.

 

 


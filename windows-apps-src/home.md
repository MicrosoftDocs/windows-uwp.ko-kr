---
Description: "다음은 휴대폰, 태블릿, PC 등 Windows 10 기반 디바이스에서만 실행할 수 있는 유니버설 Windows 앱을 만드는 데 필요한 정보입니다."
title: "Windows 10 앱 사용 방법 가이드 - Windows 앱 개발"
ms.assetid: 2A39F3D8-85AD-4315-A69B-2B79242780E3
redirect_url: https://developer.microsoft.com/windows/develop
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: 654eac5df1bd3309714348dcfc511850234bb0ae

---


# Windows 10 앱 사용 방법 가이드

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

다음은 휴대폰, 태블릿, PC 등 Windows 10 기반 디바이스에서만 실행할 수 있는 유니버설 Windows 앱을 만드는 데 필요한 정보입니다. 이 섹션에서는 수행하려는 작업의 종류별로 구성된 지침 및 코드 예제를 제공합니다.

UWP(유니버설 Windows 플랫폼) 및 이를 사용하여 동일한 코드로 여러 Windows 디바이스 유형에 적절히 맞춤화된 환경을 제공하는 방법은 다음 문서를 참조하세요.

-   첫 유니버설 Windows 앱 만들기
-   UWP(유니버설 Windows 플랫폼) 앱 지침
-   유니버설 Windows 앱이란?

| 항목 | 설명 |
|-------|-------------|
| [앱 간 통신](app-to-app/index.md) | 유니버설 Windows 앱(Windows 웹앱 포함)에서 다른 앱을 실행하고 데이터 및 파일을 교환할 수 있는 방법을 알아봅니다. 일반적으로 사용자가 여러 앱을 관리해야 하는 복잡한 작업을 이제 원활하게 처리할 수 있습니다. |
| [오디오, 동영상 및 카메라](audio-video-camera/index.md) | 웹캠과 같은 캡처 디바이스에서 사진 및 동영상을 캡처하고 앱에서 오디오 스트림을 렌더링합니다. |
| [연락처 및 일정](contacts-and-calendar/index.md) | 사용자가 앱에서 자신의 Windows 연락처 및 일정 약속에 액세스할 수 있으므로 Windows 앱 간에 전환하지 않고도 콘텐츠, 메일 및 일정 정보를 공유하거나 메시지를 보낼 수 있습니다.|
| [데이터 액세스](data-access/index.md) | UWP(유니버설 Windows 플랫폼) 앱에서 개체 관계형 매핑을 사용하는 방법과 디바이스의 개인 데이터베이스에 데이터를 저장하는 방법에 대해 알아보세요. |
| [데이터 바인딩](data-binding/index.md) | 유니버설 Windows 앱의 UI 요소를 데이터베이스, 파일, 내부 개체 등 서로 다른 데이터 원본과 동기화하여 데이터 중심 사용자 환경을 제공합니다. |
| [디버깅, 테스트 및 성능](debug-test-perf/index.md) | 테스트 및 디버깅 주기와 Microsoft Visual Studio와 함께 제공되거나 별도의 다운로드로 제공되는 관련 도구의 사용 방법을 알아봅니다. 유니버설 Windows 앱이 의도한 환경을 제공하고 Windows 스토어에 게시할 준비가 되었는지 확인합니다. |
| [디바이스, 센서 및 전원](devices-sensors\index.md) | 프린터, 카메라 및 센서와 같은 다양한 디바이스를 유니버설 Windows 앱에 통합하여 사용자에게 강력하고 유연한 연결된 디바이스 환경을 제공합니다. | 
| [엔터프라이즈](enterprise/index.md) | Windows 10 UWP(유니버설 Windows 플랫폼) 앱에 대한 주요 엔터프라이즈 기능을 알아봅니다. |
| [파일, 폴더 및 라이브러리](files/index.md) | 텍스트 및 다른 데이터 형식을 읽고 파일에 쓰며, 파일 및 폴더를 관리하는 방법을 알아봅니다. 또한 앱 설정 읽기/쓰기, 파일 및 폴더 선택기, 비디오/음악 라이브러리와 같은 특수 "샌드박스"가 적용된 위치에 대해서도 알아봅니다. |
| [게임 및 DirectX](https://msdn.microsoft.com/library/windows/apps/mt228375.aspx) | 새 UWP(유니버설 Windows 플랫폼)에서 게임 만들기의 기본 사항을 알아봅니다. |
| [그래픽 및 애니메이션](graphics/index.md) | 사용자가 사용자 환경에 시각적으로 참여하고 관심을 가지도록 UI 그래픽 및 애니메이션으로 유니버설 Windows 앱을 개선합니다. |
| [실행, 다시 시작 및 백그라운드 작업](launch-resume/index.md) | 백그라운드 작업을 만들고 시스템 생성 이벤트에 등록하여 유니버설 Windows 앱이 일시 중단되거나 실행되지 않는 경우에도 기능을 제공합니다. |
| [지도 및 위치](maps-and-location/index.md) | 유니버설 Windows 앱이 Bing 지도 서비스에 연결하여 3D 위성 이미지 및 거리 뷰가 포함된 정확한 지도 시각적 개체를 생성하는 방법을 알아봅니다. |
| [앱으로 수익 창출](monetize\index.md) | 무료 앱, 평가판(시간 기반 및 기능 기반), 유료 앱 및 앱에서 바로 구매 제품을 만들어 고객에게 무료로 앱을 사용해 보고 앱을 경험하는 동안 구매 의사를 결정할 수 있는 옵션을 제공합니다. |
| [네트워킹 및 웹 서비스](networking\index.md) | 사용 가능한 네트워크 연결을 사용하여 RSS 피드를 가져오거나, 다중 접속 게임에 참여하거나, 근거리 디바이스와 상호 작용하는 등의 작업을 수행할 수 있는 연결된 유니버설 Universal 앱 또는 네트워크 인식 유니버설 Universal 앱을 만듭니다. |
| [앱 패키징](packaging\index.md) | 유니버설 Windows 앱을 구성하는 파일이 포함된 앱 패키지와 이를 사용하여 Windows 스토어를 통해 앱을 배포, 관리, 업데이트하는 방법을 이해합니다. 또한 특정 리소스에 대한 액세스를 위해 앱 패키지 매니페스트에서 선언해야 하는 앱 접근 권한 값에 대해서도 알아봅니다. |
| [Windows 10으로 앱 포팅](porting\index.md) | 기존 앱을 UWP로 가져와 선택한 Windows 기반 디바이스를 대상으로 할 뿐만 아니라 각 디바이스 유형에 고유한 기능 및 사용자 환경에서 수익을 창출하는 단일 앱 패키지를 만들 수 있습니다. |
| [보안](security/index.md) | 사용자 환경을 그대로 유지하면서 중요한 사용자 정보를 관리하고 앱 데이터 및 리소스의 보안을 유지하도록 도와줍니다. 기본 암호 보호, 로밍 자격 증명, Single Sign-On, Microsoft 계정 인증 및 암호화와 같은 기능이 모두 원하는 대로 됩니다. |
| [스레딩 및 비동기 프로그래밍](threading-async/index.md) | 비동기 프로그래밍을 사용하여 시간이 오래 걸릴 수 있는 다른 작업을 완료하는 동안 계속 실행되고 UI에 응답하도록 함으로써 앱의 응답성을 유지할 수 있습니다. |
| [Windows 런타임 구성 요소](winrt-components/index.md) | C#, Visual Basic, JavaScript, C++ 등 모든 언어로 시작하고 사용할 수 있는 이러한 자체 포함된 개체에 대해 자세히 알아봅니다. 예를 들어 타사 라이브러리를 사용하여 계산 비용이 많이 드는 작업을 수행하는 C++로 Windows 런타임 구성 요소를 만들거나, 유니버설 Windows 앱에서 일부 Visual Basic 또는 C#을 다시 사용할 수 있습니다. 
| [XAML 플랫폼](xaml-platform/index.md) | XAML 프로그래밍 언어의 기본 개념을 시작합니다. 또는 이미 XAML에 익숙한 경우 기본 개념을 건너뛰고 Visual Studio를 사용하여 뛰어난 유니버설 Windows 앱을 만들 수 있는 XAML에서 Windows 런타임 기능을 구현하는 방법을 알아봅니다. |



<!--HONumber=Jul16_HO2-->



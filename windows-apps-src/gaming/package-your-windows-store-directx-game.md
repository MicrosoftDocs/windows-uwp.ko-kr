---
title: UWP (유니버설 Windows 플랫폼) DirectX 게임 패키지
description: UWP (큰 유니버설 Windows 플랫폼) 게임, 특히 지역별 자산이 있는 여러 언어를 지원 하거나, 선택 사항인 고화질 자산을 사용 하는 경우에는 큰 크기를 쉽게 파악할 수 있습니다.
ms.assetid: 68254203-c43c-684f-010a-9cfa13a32a77
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, directx, 패키지
ms.localizationpriority: medium
ms.openlocfilehash: 8a1e255a5219e39d866915bce25778dc8f1dfdba
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175247"
---
#  <a name="package-your-universal-windows-platform-uwp-directx-game"></a>UWP (유니버설 Windows 플랫폼) DirectX 게임 패키지

UWP (큰 유니버설 Windows 플랫폼) 게임, 특히 지역별 자산이 있는 여러 언어를 지원 하거나, 선택 사항인 고화질 자산을 사용 하는 경우에는 큰 크기를 쉽게 파악할 수 있습니다. 이 항목에서는 고객이 실제로 필요한 리소스만 받도록 앱 패키지 및 앱 번들을 사용 하 여 앱을 사용자 지정 하는 방법에 대해 알아봅니다.

앱 패키지 모델 외에도 Windows 10은 두 가지 유형의 pack을 그룹화 하는 앱 번들을 지원 합니다.

-   앱 팩에는 플랫폼별 실행 파일 및 라이브러리가 포함 되어 있습니다. 일반적으로 UWP 게임은 x86, x64 및 ARM CPU 아키텍처 각각에 해당 하는 최대 3 개의 앱 팩을 포함할 수 있습니다. 해당 하드웨어 플랫폼과 관련 된 모든 코드와 데이터는 해당 앱 팩에 포함 되어야 합니다. 또한 앱 팩에는 게임에 대 한 모든 핵심 자산이 포함 되어 있으며,이는 기본 수준에서 충실도 및 성능을 사용 하 여 실행 됩니다.
-   리소스 팩에는 게임 자산(텍스처, 메시, 사운드, 텍스트) 등 플랫폼에 종속되지 않는 선택적 또는 확장 데이터가 포함됩니다. UWP 게임에는 고화질 자산이 나 질감, DirectX 기능 수준 11 + 리소스 또는 언어별 자산 및 리소스에 대 한 리소스 팩을 비롯 한 하나 이상의 리소스 팩이 있을 수 있습니다.

앱 번들 및 앱 팩에 대 한 자세한 내용은 [앱 리소스 정의](/previous-versions/windows/apps/hh965321(v=win.10))를 참조 하세요.

앱 팩에 모든 콘텐츠를 저장할 수 있지만이는 비효율적 이며 중복 됩니다. 각 플랫폼에 대해 동일한 대량 질감 파일을 세 번 복제 하는 이유는 무엇 이며,이는 사용 하지 않을 수 있는 ARM 플랫폼의 경우입니다. 좋은 목표는 고객이 다운로드 해야 하는 항목을 최소화 하는 것입니다. 따라서 게임을 일찍 재생 하 고, 장치에 공간을 절약 하 고, 데이터 요금을 절감할 수 있습니다.

UWP 앱 설치 관리자의이 기능을 사용 하려면 게임 개발 초기에 앱 및 리소스 패키징에 대 한 디렉터리 레이아웃 및 파일 명명 규칙을 고려 하는 것이 중요 합니다. 따라서 도구와 소스가 패키징을 간소화 하는 방식으로 정확 하 게 출력할 수 있습니다. 자산 만들기를 개발 또는 구성 하 고 도구와 스크립트를 관리 하 고 리소스를 로드 하거나 참조 하는 코드를 작성할 때이 문서에 설명 된 규칙을 따릅니다.

## <a name="why-create-resource-packs"></a>리소스 팩을 만드는 이유


응용 프로그램, 특히 많은 로캘 또는 광범위 한 UWP 하드웨어 플랫폼에서 판매할 수 있는 게임 앱을 만들 때 이러한 로캘 또는 플랫폼을 지원 하기 위해 여러 버전의 파일을 포함 해야 하는 경우가 많습니다. 예를 들어 미국 및 일본 모두에서 게임을 출시 하는 경우 en-us 로캘에 대 한 음성 파일의 한 집합을 영어로 표시 하 고 다른 하나는 일본어로 표시 해야 할 수 있습니다. 또는 x86 및 x64 플랫폼 뿐만 아니라 ARM 장치에 대해 게임에서 이미지를 사용 하려는 경우 각 CPU 아키텍처에 대해 한 번씩 동일한 이미지 자산을 세 번 업로드 해야 합니다.

또한 더 낮은 DirectX 기능 수준을 사용 하는 플랫폼에 적용 되지 않는 많은 정의 리소스를 게임에 포함 하는 경우, 기본 앱 팩에 이러한 리소스를 포함 하 고 사용자가 장치에서 사용할 수 없는 많은 구성 요소를 다운로드 해야 하는 이유가 무엇 인가요? 이러한 많은 .def 리소스를 선택적 리소스 팩으로 분리 하는 것은 해당 하는 고급 장치를 사용 하는 고객이 비용을 절감할 수 있는 시간 (데이터 요금을 부과 가능)으로 얻을 수 있는 반면, 고급 장치를 사용 하지 않는 사용자는 게임을 더 빠르게 수행 하 고 네트워크 사용량 비용을 절감할 수 있습니다.

게임 리소스 팩의 콘텐츠 후보는 다음과 같습니다.

-   국제 로캘 특정 자산 (지역화 된 텍스트, 오디오 또는 이미지)
-   여러 장치 크기 조정 요인에 대 한 고해상도 자산 (1.0 x, 1.4 x 및 1.8 x)
-   더 높은 DirectX 기능 수준 (9, 10 및 11)에 대 한 높은 정의 자산

이 모든 것은 UWP 프로젝트의 일부인 appxmanifest.xml 및 최종 패키지의 디렉터리 구조에 정의 됩니다. 새 Visual Studio UI로 인해이 문서의 프로세스를 수행 하는 경우 수동으로 편집할 필요가 없습니다.

> **중요**    이러한 리소스의 로드 및 관리는 **Windows ApplicationModel .Resources** api를 통해 처리 됩니다. \* 이러한 앱 모델 리소스 Api를 사용 하 여 로캘, 배율 인수 또는 DirectX 기능 수준에 맞는 파일을 로드 하는 경우 명시적 파일 경로를 사용 하 여 자산을 로드할 필요가 없습니다. 대신, 원하는 자산의 일반화 된 파일 이름으로 리소스 Api를 제공 하 고, 리소스 관리 시스템이 사용자의 현재 플랫폼 및 로캘 구성에 대해 리소스의 올바른 변형 (동일한 Api를 사용 하 여 직접 지정할 수 있음)을 얻을 수 있도록 합니다.

 

리소스 패키징을 위한 리소스는 다음 두 가지 기본 방법 중 하나로 지정 됩니다.

-   자산 파일의 파일 이름은 동일 하며, 리소스 팩 특정 버전은 명명 된 특정 디렉터리에 배치 됩니다. 이러한 디렉터리 이름은 시스템에서 예약 됩니다. 예를 들어 \\ en-us, \\ scale-140, \\ dxfl-dx11입니다.
-   자산 파일은 임의의 이름이 있는 폴더에 저장 되지만 파일은 언어 또는 다른 한정자를 나타내기 위해 시스템에 예약 된 문자열과 함께 추가 되는 일반 레이블로 이름이 지정 됩니다. 특히 한정자 문자열은 밑줄 ("") 뒤에 일반화 된 파일 이름에 붙어 있습니다 \_ . 예를 들어 \\ 자산 \\ 메뉴 \_ 옵션 1 마이그레이션 \_lang-en-us.png, \\ 자산 \\ 메뉴 \_ 옵션 1 마이그레이션 \_scale-140.png, \\ 자산 \\ coolsign \_ dxfl-dx11. 이러한 문자열을 결합할 수도 있습니다. 예를 들어 \\ 자산 \\ 메뉴 \_ 옵션 1 마이그레이션 \_lang-en-us.png를 140 확장 \_ 합니다.
    > **참고**    디렉터리 이름에 단독으로 사용 되는 것이 아니라 파일 이름에 사용 되는 언어 한정자는 <tag> [언어, 크기 조정 및 기타 한정자에 대 한 리소스 조정](../app-resources/tailor-resources-lang-scale-contrast.md)에 설명 된 대로 "lang-" 형식 (예: "lang-en-us")을 사용 해야 합니다.

     

리소스 포장의 추가 특이성 디렉터리 이름을 결합할 수 있습니다. 그러나 중복 될 수는 없습니다. 예를 들어 \\ en-us \\ 메뉴 \_ 옵션 1 마이그레이션 \_lang-en-us.png은 중복 됩니다.

디렉터리 구조가 각 리소스 디렉터리에서 동일 하기만 하면 리소스 디렉터리 아래에 필요한 예약 되지 않은 하위 디렉터리 이름을 지정할 수 있습니다. 예를 들어 \\ dxfl-dx10 \\ 자산 \\ 질감 \\ coolsign. 자산을 로드 하거나 참조할 때 경로 이름을 일반화 하 여 언어, 배율 또는 DirectX 기능 수준에 대 한 한정자 (폴더 노드 또는 파일 이름)를 제거 해야 합니다. 예를 들어 변형 중 하나가 dxfl dx10 자산 질감 coolsign 자산에 대 한 코드를 참조 하려면 \\ \\ \\ \\ \\ 자산 \\ 질감 coolsign를 사용 \\ 합니다. 마찬가지로, variant 이미지가 있는 자산을 참조 하려면 \\ \\ \_scale-140.png \\background.png 이미지를 사용 \\ 합니다.

다음은 예약 된 디렉터리 이름 및 파일 이름 밑줄 접두사입니다.

| 자산 형식                   | 리소스 팩 디렉터리 이름                                                                                                                  | 리소스 팩 파일 이름 접미사                                                                                                    |
|------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------|
| 지역화 된 자산             | Windows 10에 대 한 모든 가능한 언어 또는 언어 및 로캘 조합입니다. 폴더 이름에는 한정자 접두사 "lang-"이 필요하지 않습니다. | " \_ " 뒤에 언어, 로캘 또는 언어 로캘 지정 자가 옵니다. 예를 들면 " \_ en", " \_ us" 또는 " \_ en-us"입니다. |
| 배율 비율 자산        | 100, 소수 자릿수-140, 규모 180의 크기를 조정 합니다. 각각 1.0 x, 1.4 x 및 1.8 x UI 배율 인수에 대 한 것입니다.                                     | " \_ " Scale-100 "," scale 140 "또는" scale-180 "이 뒤에 나옵니다.                                                                    |
| DirectX 기능 수준 자산 | dxfl-9, dxfl-dx10 및 dxfl-dx11. 이는 각각 DirectX 9, 10 및 11 기능 수준에 대 한 것입니다.                                     | " \_ " 다음에 "dxfl-9", "dxfl-dx10" 또는 "dxfl-dx11"가 옵니다.                                                                     |

 

## <a name="defining-localized-language-resource-packs"></a>지역화 된 언어 리소스 팩 정의


로캘별 파일은 해당 언어에 대해 이름이 지정 된 프로젝트 디렉터리 (예: "en")에 배치 됩니다.

여러 언어에 대 한 지역화 된 자산을 지원 하도록 앱을 구성 하는 경우 다음을 수행 해야 합니다.

-   지원 되는 각 언어와 로캘에 대해 앱 하위 디렉터리 (또는 파일 버전)를 만듭니다 (예: en-us, jp-jp, zh-cn, fr-fr 등).
-   개발 하는 동안 언어 또는 로캘에서 모든 자산의 복사본 (예: 지역화 된 오디오 파일, 질감 및 메뉴 그래픽)을 해당 언어 로캘 하위 디렉터리에 저장 합니다. 최상의 사용자 환경을 위해 사용 가능한 로캘 (또는 다운로드 및 설치 후 실수로 삭제 한 경우)에 대해 사용 가능한 언어 리소스 팩을 가져오지 않은 경우 사용자에 게 경고를 표시 합니다.
-   각 자산 또는 문자열 리소스 파일 (. resw)이 각 디렉터리에서 동일한 이름을 사용 하는지 확인 합니다. 예를 들어 \_ \\ \\ 파일의 내용이 다른 언어용 인 경우에도 메뉴option1.png는 en-us 및 jp 디렉터리 모두에서 같은 이름을 가져야 합니다. 이 경우 \\ en-us \\ 메뉴 \_option1.png 및 \\ jp \\ 메뉴 \_option1.png 표시 됩니다.
    > **참고**    필요에 따라 파일 이름에 로캘을 추가 하 고 동일한 디렉터리에 저장할 수 있습니다. 예를 들어 \\ 자산 \\ 메뉴 \_ 옵션 1 마이그레이션 \_lang-en-us.png, \\ 자산 \\ 메뉴 \_ 옵션 1 마이그레이션 \_lang-jp-jp.png.

     

-   응용 프로그램에 대 한 로캘별 리소스를 지정 하 고 [**로드 하려면**](/uwp/api/Windows.ApplicationModel.Resources.Core) [**windows**](/uwp/api/Windows.ApplicationModel.Resources) 응용 프로그램의 api를 사용 합니다. 또한 특정 로캘을 포함 하지 않는 자산 참조를 사용 합니다. 이러한 Api는 사용자의 설정을 기반으로 올바른 로캘을 결정 한 다음 사용자에 대 한 올바른 리소스를 검색 하기 때문입니다.
-   Microsoft Visual Studio 2015에서 **프로젝트->스토어->앱 패키지 만들기** ...를 선택 하 고 패키지를 만듭니다.

## <a name="defining-scaling-factor-resource-packs"></a>크기 조정 요소 리소스 팩 정의


Windows 10은 1.0 x, 1.4 x 및 1.8 x의 세 가지 사용자 인터페이스 배율 인수를 제공 합니다. 각 표시의 크기 조정 값은 화면 크기, 화면 해상도 및 화면에서 사용자의 가정 된 평균 거리를 기준으로 설치 중에 설정 됩니다. 사용자는 가독성을 높이기 위해 배율 요소를 조정할 수도 있습니다. 게임은 가능한 최상의 환경을 위해 DPI 인식 및 배율 인수를 모두 인식 해야 합니다. 이 인식의 일부는 세 가지 배율 인수 각각에 대해 중요 한 시각적 자산의 버전을 만드는 것을 의미 합니다. 여기에는 포인터 상호 작용과 적중 테스트도 포함 됩니다.

여러 UWP 앱 배율 인수에 대 한 리소스 팩을 지원 하도록 앱을 구성 하는 경우 다음을 수행 해야 합니다.

-   지원할 각 배율 인수(scale-100, scale-140 및 scale-180)에 대한 앱 하위 디렉터리(또는 파일 버전)를 만듭니다.
-   개발 하는 동안 크기 조정 요소에서 차이가 없는 경우에도 각 배율 인수 리소스 디렉터리에 모든 자산의 크기를 적절 하 게 복사 합니다.
-   각 각 디렉터리에서 각 자산의 이름이 동일한 지 확인 합니다. 예를 들어 \_ \\ \\ 파일 내용이 다른 경우에도 100 및 180 디렉터리 모두에서 메뉴option1.png의 이름이 동일 해야 합니다. 이 경우 \\option1.png 확장-100 메뉴로 표시 되 \\ \_ 고 \\option1.png는 확장 140 \\ 메뉴가 표시 \_ 됩니다.
    > **참고**    또한 필요에 따라 파일 이름에 배율 인수 접미사를 추가 하 고 동일한 디렉터리에 저장할 수 있습니다. 예를 들어 \\ 자산 \\ 메뉴 \_ 옵션 1 마이그레이션 \_scale-100.png, \\ 자산 \\ 메뉴 \_ 옵션 1 마이그레이션 \_scale-140.png.

     

-   [**Windows**](/uwp/api/Windows.ApplicationModel.Resources.Core) 의 api를 사용 하 여 자산을 로드 합니다. 자산 참조는 일반화 (접미사 없음) 해야 하며, 특정 배율 변화를 유지 해야 합니다. 시스템은 표시 및 사용자 설정에 적합 한 규모의 자산을 검색 합니다.
-   Visual Studio 2015에서 **프로젝트 >스토어->앱 패키지 만들기** ...를 선택 하 고 패키지를 만듭니다.

## <a name="defining-directx-feature-level-resource-packs"></a>DirectX 기능 수준 리소스 팩 정의


DirectX 기능 수준은 DirectX의 이전 버전과 현재 버전 (특히 Direct3D)에 대 한 GPU 기능 집합에 해당 합니다. 여기에는 셰이더 모델 사양과 기능, 셰이더 언어 지원, 질감 압축 지원 및 전체 그래픽 파이프라인 기능이 포함 됩니다.

기준 앱 팩은 BC1, BC2 또는 BC3 기준 질감 압축 형식을 사용 해야 합니다. 이러한 형식은 모든 UWP 장치에서 전용 다중 GPU 워크스테이션 및 미디어 컴퓨터까지 사용 되는 낮은 종단 ARM 플랫폼에서 사용할 수 있습니다.

DirectX 기능 수준 10 이상의 질감 형식 지원은 로컬 디스크 공간 및 다운로드 대역폭을 절약 하기 위해 리소스 팩에 추가 해야 합니다. 이렇게 하면 BC6H 및 BC7와 같은 11에 대 한 고급 압축 체계를 사용할 수 있습니다. 자세한 내용은 [Direct3D 11에서 질감 블록 압축](/windows/desktop/direct3d11/texture-block-compression-in-direct3d-11)을 참조 하세요. 이러한 형식은 최신 Gpu에서 지 원하는 고해상도 질감 자산에 대해 더 효율적 이며,이를 사용 하면 고성능 플랫폼에서 게임의 모양, 성능 및 공간 요구 사항이 향상 됩니다.

| DirectX 기능 수준 | 지원 되는 텍스처 압축 |
|-----------------------|-------------------------------|
| 9                     | BC1, BC2, BC3                 |
| 10                    | BC4, BC5                      |
| 11                    | BC6H, BC7                     |

 

또한 각 DirectX 기능 수준은 다른 셰이더 모델 버전을 지원 합니다. 컴파일된 셰이더 리소스는 기능 수준에 따라 만들 수 있으며 DirectX 기능 수준 리소스 팩에 포함 될 수 있습니다. 또한 일부 이후 버전 셰이더 모델에서는 일반적인 맵과 같은 자산을 사용할 수 있습니다 .이는 이전 셰이더 모델 버전에서 사용할 수 없습니다. 이러한 셰이더 모델 관련 자산은 DirectX 기능 수준 리소스 팩에도 포함 되어야 합니다.

리소스 메커니즘은 주로 자산에 대해 지원 되는 질감 형식에 중점을 둘 수 있으므로 3 개의 전체 기능 수준만 지원 합니다. 9 1 vs 9 3과 같은 하위 수준 (점 버전)에 대 한 별도의 셰이더가 필요한 경우 \_ \_ 자산 관리 및 렌더링 코드에서 명시적으로 처리 해야 합니다.

다른 DirectX 기능 수준에 대 한 리소스 팩을 지원 하도록 앱을 구성 하는 경우 다음을 수행 해야 합니다.

-   지원할 각 DirectX 기능 수준에 대 한 앱 하위 디렉터리 (또는 파일 버전)를 만듭니다 (dxfl-9, dxfl, dx10 및 dxfl).
-   개발 하는 동안 각 기능 수준 리소스 디렉터리에 기능 수준 특정 자산을 추가 합니다. 로캘 및 배율 인수와는 달리, 게임의 각 기능 수준에 대해 서로 다른 렌더링 코드 분기가 있을 수 있으며, 하나 또는 모든 지원 되는 기능 수준의 하위 집합 에서만 사용 되는 질감, 컴파일된 셰이더 또는 기타 자산이 있는 경우 해당 자산을 사용 하는 기능 수준에 대 한 해당 자산을 디렉터리에만 배치 합니다. 모든 기능 수준에서 로드 되는 자산의 경우 각 기능 수준 리소스 디렉터리에 동일한 이름의 버전이 있는지 확인 합니다. 예를 들어 "coolsign" 이라는 기능 수준 독립적인 질감의 경우 dxfl-9 디렉터리에 BC3 압축 된 버전을, BC7 \\ \\ -dxfl 디렉터리에 dx11 압축 된 버전을 추가 합니다.
-   각 자산 (여러 기능 수준에서 사용할 수 있는 경우)이 각 디렉터리에서 동일한 이름을 사용 하는지 확인 합니다. 예를 들어 coolsign는 \\ \\ 파일의 내용이 다른 경우에도 dxfl-9 및 dxfl 디렉터리 모두에서 이름이 동일 해야 합니다. 이 경우 \\ dxfl-9 \\ coolsign 및 \\ dxfl-dx11 coolsign로 표시 \\ 됩니다.
    > **참고**    또한 필요에 따라 기능 수준 접미사를 파일 이름에 추가 하 고 동일한 디렉터리에 저장할 수 있습니다. 예를 들어 \\ 질감이 \\ coolsign \_ dxfl-dx9, \\ 질감 \\ coolsign \_ dxfl-dx11입니다.

     

-   그래픽 리소스를 구성할 때 지원 되는 DirectX 기능 수준을 선언 합니다.
    ```cpp
    D3D_FEATURE_LEVEL featureLevels[] = 
    {
      D3D_FEATURE_LEVEL_11_1,
      D3D_FEATURE_LEVEL_11_0,
      D3D_FEATURE_LEVEL_10_1,
      D3D_FEATURE_LEVEL_10_0,
      D3D_FEATURE_LEVEL_9_3,
      D3D_FEATURE_LEVEL_9_1
    };
    ```

    ```cpp
    ComPtr<ID3D11Device> device;
    ComPtr<ID3D11DeviceContext> context;
    D3D11CreateDevice(
        nullptr,                    // Use the default adapter.
        D3D_DRIVER_TYPE_HARDWARE,
        0,                      // Use 0 unless it is a software device.
        creationFlags,          // defined above
        featureLevels,          // What the app will support.
        ARRAYSIZE(featureLevels),
        D3D11_SDK_VERSION,      // This should always be D3D11_SDK_VERSION.
        &device,                    // created device
        &m_featureLevel,            // The feature level of the device.
        &context                    // The corresponding immediate context.
    );
    ```

-   [**Windows**](/uwp/api/Windows.ApplicationModel.Resources.Core) 의 api를 사용 하 여 리소스를 로드 합니다. 자산 참조는 일반화 (접미사 없음) 하 여 기능 수준을 그대로 유지 해야 합니다. 그러나 언어와 규모와는 달리 시스템은 지정 된 표시에 대해 최적의 기능 수준을 자동으로 결정 하지 않습니다. 코드 논리에 따라 결정 하는 데 사용 됩니다. 이러한 결정을 마쳤으면 Api를 사용 하 여 기본 기능 수준의 OS를 알려 줍니다. 그러면 시스템은 해당 기본 설정에 따라 올바른 자산을 검색할 수 있습니다. 다음은 플랫폼에 대 한 현재 DirectX 기능 수준을 앱에 알리는 방법을 보여 주는 코드 샘플입니다.
    
    ```cpp
    // Set the current UI thread's MRT ResourceContext's DXFeatureLevel with the right DXFL. 

    Platform::String^ dxFeatureLevel;
        switch (m_featureLevel)
        {
        case D3D_FEATURE_LEVEL_9_1:
        case D3D_FEATURE_LEVEL_9_2:
        case D3D_FEATURE_LEVEL_9_3:
            dxFeatureLevel = L"DX9";
            break;
        case D3D_FEATURE_LEVEL_10_0:
        case D3D_FEATURE_LEVEL_10_1:
            dxFeatureLevel = L"DX10";
            break;
        default:
            dxFeatureLevel = L"DX11";
        }

        ResourceContext::SetGlobalQualifierValue(L"DXFeatureLevel", dxFeatureLevel);
    ```

    > **참고**    코드에서 이름 또는 기능 수준 디렉터리 아래의 경로를 기준으로 질감을 직접 로드 합니다. 기능 수준 디렉터리 이름 또는 접미사를 포함 하지 마십시오. 예를 들어 \\ "dxfl-dx11 \\ 질감이 \\ coolsign" 또는 "질감이 \\ coolsign dxfl-dx11"이 아닌 "텍스처 coolsign"를 로드 \_ 합니다.

     

-   이제 [**ResourceManager**](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceManager) 를 사용 하 여 현재 DirectX 기능 수준과 일치 하는 파일을 찾습니다. **ResourceManager** 는 Resourcemap [**: GetValue**](/uwp/api/windows.applicationmodel.resources.core.resourcemap.getvalue) (또는 [**resourcemap::**](/uwp/api/windows.applicationmodel.resources.core.resourcemap.trygetvalue)) 및 제공 된 [**resourcemap**](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceContext)로 쿼리 하는 [**resourcemap**](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceMap)반환 합니다. 이는 [**SetGlobalQualifierValue**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue)를 호출 하 여 지정 된 DirectX 기능 수준과 가장 일치 하는 [**ResourceCandidate**](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceCandidate) 를 반환 합니다.
    
    ```cpp
    // An explicit ResourceContext is needed to match the DirectX feature level for the display on which the current view is presented.

    auto resourceContext = ResourceContext::GetForCurrentView();
    auto mainResourceMap = ResourceManager::Current->MainResourceMap;

    // For this code example, loader is a custom ref class used to load resources.
    // You can use the BasicLoader class from any of the 8.1 DirectX samples similarly.


    auto possibleResource = mainResourceMap->GetValue(
        L"Files/BumpPixelShader.cso",
        resourceContext
    );
    Platform::String^ resourceName = possibleResource->ValueAsString;
    ```

-   Visual Studio 2015에서 **프로젝트 >스토어->앱 패키지 만들기** ...를 선택 하 고 패키지를 만듭니다.
-   Appxmanifest.xml 매니페스트 설정에서 앱 번들을 사용 하도록 설정 했는지 확인 합니다.

## <a name="related-topics"></a>관련 항목


* [앱 리소스 정의](/previous-versions/windows/apps/hh965321(v=win.10))
* [앱 패키징](../packaging/index.md)
* [앱 패키지 작성 (MakeAppx.exe)](/windows/desktop/appxpkg/make-appx-package--makeappx-exe-)

 

 
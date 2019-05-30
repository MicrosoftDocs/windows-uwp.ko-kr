---
title: UWP(유니버설 Windows 플랫폼) DirectX 게임 패키지
description: 대규모 UWP(유니버설 Windows 플랫폼) 게임, 특히 지역별 자산이나 선택적 기능 HD 자산을 사용하여 여러 언어를 지원하는 게임은 크기가 쉽게 급증할 수 있습니다.
ms.assetid: 68254203-c43c-684f-010a-9cfa13a32a77
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, 게임, directx, 패키지
ms.localizationpriority: medium
ms.openlocfilehash: 27ea422982ce991de20e67649bc0925a60547cd8
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368312"
---
#  <a name="package-your-universal-windows-platform-uwp-directx-game"></a>UWP(유니버설 Windows 플랫폼) DirectX 게임 패키지

대규모 UWP(유니버설 Windows 플랫폼) 게임, 특히 지역별 자산이나 선택적 기능 HD 자산을 사용하여 여러 언어를 지원하는 게임은 크기가 쉽게 급증할 수 있습니다. 이 항목에서는 고객이 실제로 필요한 리소스만 받을 수 있도록 앱 패키지와 앱 번들을 사용하여 앱을 사용자 지정하는 방법을 알아봅니다.

앱 패키지 모델 외에도 Windows 10에서는 두 가지 유형의 팩을 함께 그룹화 하는 앱 번들을 지원 합니다.

-   앱 팩에는 플랫폼별 실행 파일과 라이브러리가 포함됩니다. 일반적으로 UWP 게임에는 x86, x64 및 ARM CPU 아키텍처에 대해 각각 하나씩, 최대 3개의 앱 팩이 포함될 수 있습니다. 해당 하드웨어 플랫폼과 관련된 모든 코드 및 데이터가 앱 팩에 포함되어야 합니다. 또한 앱 팩에는 기준 수준의 충실도와 성능으로 게임을 실행하기 위한 모든 핵심 자산이 포함되어야 합니다.
-   리소스 팩에는 게임 자산(텍스처, 메시, 사운드, 텍스트) 등 플랫폼에 종속되지 않는 선택적 또는 확장 데이터가 포함됩니다. UWP 게임에는 HD 자산 또는 텍스처를 위한 리소스 팩, DirectX 기능 수준 11+ 리소스 또는 언어별 자산 및 리소스를 포함하여 리소스 팩이 하나 이상 있을 수 있습니다.

앱 번들과 앱 팩에 대한 자세한 내용은 [앱 리소스 정의](https://docs.microsoft.com/previous-versions/windows/apps/hh965321(v=win.10))를 참조하세요.

모든 콘텐츠를 앱 팩에 포함할 수 있지만 이것은 비효율적이고 중복됩니다. 동일한 큰 텍스처 파일을 각 플랫폼, 특히 이 파일이 사용되지 않는 ARM 플랫폼을 위해 3번 중복해서 포함할 이유가 있을까요? 바람직한 목표는 고객이 더 빨리 게임 플레이를 시작하고, 장치의 공간을 절약하고, 발생 가능한 요금제 대역폭 비용을 피할 수 있도록 다운로드해야 하는 콘텐츠를 최소화하는 것입니다.

UWP 앱 설치 관리자의 이 기능을 사용하려면 도구 및 소스가 패키징을 간소화하는 방식으로 올바르게 출력할 수 있도록 게임 개발의 초기 단계에서 앱 및 리소스 패키징을 위한 디렉터리 레이아웃 및 파일 명명 규칙을 고려하는 것이 중요합니다. 자산 생성을 개발 또는 구성하고 도구 및 스크립트를 관리할 때 및 리소스를 로드하거나 참조하는 코드를 작성할 때는 이 문서에 설명된 규칙을 따르세요.

## <a name="why-create-resource-packs"></a>리소스 팩을 만드는 것은 무엇 때문인가요?


앱, 특히 여러 로캘이나 다양한 UWP 하드웨어 플랫폼에서 판매될 수 있는 게임 앱을 만드는 경우 해당 로캘 또는 플랫폼을 지원하기 위해 여러 버전의 많은 파일을 포함해야 합니다. 예를 들어 미국과 일본에서 게임을 릴리스하는 경우 en-us 로캘을 위한 영어 음성 파일 집합과 jp-jp 로캘을 위한 일본어 음성 파일 집합이 필요할 수 있습니다. 또는 게임에서 x86 및 x64 플랫폼뿐 아니라 ARM 장치용 이미지를 사용하려는 경우 각 CPU 아키텍처에 대해 한 번씩, 동일한 이미지 자산을 3번 업로드해야 합니다.

또한 게임에 하위 DirectX 기능 수준의 플랫폼에 적용되지 않는 많은 HD 리소스가 있는 경우 이러한 리소스를 기준 앱 팩에 포함하여 사용자가 장치에서 사용할 수 없는 많은 구성 요소를 다운로드하도록 만들 이유가 없습니다. HD 리소스를 선택적 리소스 팩으로 분리하면 HD 리소스를 지원하는 장치를 가진 고객은 (요금제) 대역폭 비용을 지불하고 다운로드하는 반면, 고급 장치가 없는 고객은 더 저렴한 네트워크 사용 비용으로, 더 빨리 게임을 다운로드할 수 있습니다.

게임 리소스 팩에 포함할 수 있는 콘텐츠는 다음과 같습니다.

-   국가별 자산(지역화된 텍스트, 오디오 또는 이미지)
-   각 장치 배율 인수(1.0x, 1.4x 및 1.8x)를 위한 HD 자산
-   높은 DirectX 기능 수준(9, 10 및 11)을 위한 HD 자산

이러한 모든 콘텐츠는 UWP 프로젝트에 포함된 package.appxmanifest와 최종 패키지의 디렉터리 구조에서 정의됩니다. 새 Visual Studio UI 때문에 이 문서의 프로세스를 따를 경우 수동으로 편집할 필요가 없습니다.

> **중요**    로드 하 고 이러한 리소스의 관리를 통해 처리 되는 **Windows.ApplicationModel.Resources** \* Api. 이러한 앱 모델 리소스 API를 사용하여 로캘, 배율 인수 또는 DirectX 기능 수준에 맞는 파일을 로드하는 경우 명시적 파일 경로를 사용하여 자산을 로드할 필요가 없습니다. 대신 원하는 자산의 범용 파일 이름만 리소스 API에 제공하고 리소스 관리 시스템이 사용자의 현재 플랫폼 및 로캘 구성에 맞는 변형의 리소스를 가져오도록 합니다. 이러한 구성도 동일한 API를 사용하여 직접 지정할 수 있습니다.

 

리소스 패키징을 위한 리소스는 다음 두 가지 기본 방법 중 하나로 지정됩니다.

-   자산 파일은 동일한 파일 이름을 사용하며, 리소스 팩 특정 버전이 명명된 특정 디렉터리에 배치됩니다. 디렉터리 이름은 시스템에 의해 예약되어 있습니다. 예를 들어 \\en-우리 \\크기 조정-140, \\dxfl dx11 합니다.
-   자산 파일은 임의 이름의 폴더에 저장되지만 시스템에서 언어나 다른 한정자를 나타내기 위해 예약한 문자열이 끝에 추가된 일반 레이블로 파일 이름이 지정됩니다. 일반화 된 파일 이름에 밑줄 후 한정자 문자열은 부착 하는 특히 ("\_"). 예를 들어 \\자산\\메뉴\_option1\_lang-en-us.png \\자산\\메뉴\_option1\_확장-140.png \\자산\\coolsign\_dxfl dx11.dds 합니다. 이러한 문자열을 결합할 수도 있습니다. 예를 들어 \\자산\\메뉴\_option1\_배율 140\_lang-en-us.png 합니다.
    > **참고**    언어 한정자를 형식 이어야 디렉터리 이름에는 파일 이름 대신 단독으로 사용 하는 경우 "lang-<tag>", 예: "lang--us"에 설명 된 대로 [언어를 눈금에 대 한 리소스를 조정 하 고 다른 한정자](../app-resources/tailor-resources-lang-scale-contrast.md)합니다.

     

리소스 패키징의 구체성을 향상시키기 위해 디렉터리 이름을 결합할 수 있습니다. 그러나 이름이 중복될 수는 없습니다. 예를 들어 \\en-우리\\메뉴\_option1\_lang-en-us.png 중복 됩니다.

각 리소스 디렉터리에서 디렉터리 구조가 같기만 하면 예약되지 않은 필요한 모든 하위 디렉터리 이름을 리소스 디렉터리 아래에 지정할 수 있습니다. 예를 들어 \\dxfl dx10\\자산\\질감\\coolsign.dds 합니다. 자산을 로드하거나 참조하는 경우 폴더 노드 또는 파일 이름에 있든 관계없이 언어, 배율 또는 DirectX 기능 수준에 대한 한정자를 모두 제거하여 경로 이름을 일반화해야 합니다. 예를 들어, 자산 변형 중 하나에 대 한 코드에서 참조 하는 \\dxfl dx10\\자산\\질감\\coolsign.dds를 사용 하 여 \\자산\\질감\\ coolsign.dds 합니다. 마찬가지로, variant 사용 하 여 자산을 가리키도록 \\이미지\\백그라운드\_확장-140.png를 사용 하 여 \\이미지\\background.png 합니다.

다음은 예약된 디렉터리 이름 및 파일 이름 밑줄 접두사입니다.

| 자산 유형                   | 리소스 팩 디렉터리 이름                                                                                                                  | 리소스 팩 파일 이름 접미사                                                                                                    |
|------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------|
| 지역화된 자산             | 가능한 모든 언어 또는 Windows 10에 대 한 언어 및 로캘 조합 합니다. 폴더 이름에는 한정자 접두사 "lang-"이 필요하지 않습니다. | "\_" 뒤에 언어, 로캘 또는 언어 로캘 지정자입니다. 예를 들어, "\_en","\_주세요", 또는 "\_en-우리", 각각. |
| 배율 인수 자산        | scale-100, scale-140, scale-180입니다. 각각 1.0x, 1.4x 및 1.8x UI 배율 인수에 사용됩니다.                                     | "\_" 뒤에 "배율 100", "크기 조정-140" 또는 "확장-180"입니다.                                                                    |
| DirectX 기능 수준 자산 | dxfl-dx9, dxfl-dx10, dxfl-dx11입니다. 각각 DirectX 9, 10 및 11 기능 수준에 사용됩니다.                                     | "\_" 뒤에 "dxfl dx9", "dxfl-dx10" 또는 "dxfl dx11"입니다.                                                                     |

 

## <a name="defining-localized-language-resource-packs"></a>지역화된 언어 리소스 팩 정의


로캘별 파일은 언어에 따라 명명된 프로젝트 디렉터리에 저장됩니다 (예: "en").

여러 언어에 대해 지역화된 자산을 지원하도록 앱을 구성하는 경우 다음 작업을 해야 합니다.

-   지원할 각 언어 및 로캘에 대한 앱 하위 디렉터리(또는 파일 버전)를 만듭니다(예: en-us, jp-jp, zh-cn, fr-fr 등).
-   개발하는 동안 언어나 로캘별로 다르지 않더라도 모든 자산(지역화된 오디오 파일, 텍스처 및 메뉴 그래픽)의 복사본을 해당 언어 로캘 하위 디렉터리에 저장합니다. 최상의 사용자 환경을 구현하려면 로캘에 맞는 언어 리소스 팩을 사용할 수 있는데 사용자가 다운로드하지 않은 경우 또는 다운로드하여 설치한 후 실수로 삭제한 경우 사용자에게 경고해야 합니다.
-   각 디렉터리에서 각 자산 또는 문자열 리소스 파일(.resw)의 이름이 같아야 합니다. 예를 들어 메뉴\_option1.png 둘 다에 이름이 있어야 합니다 \\en-우리 및 \\jp jp 디렉터리 파일의 내용을 다른 언어에 대 한 경우에 마찬가지입니다. 이 경우 표시 되는 그대로 \\en-우리\\메뉴\_option1.png 및 \\jp jp\\메뉴\_option1.png 합니다.
    > **참고**    필요에 따라 파일 이름에 로캘을 추가 하 고 동일한 디렉터리에 저장할 수 있습니다 예를 들어 \\자산\\메뉴\_option1\_lang-en-us.png, \\자산\\메뉴\_option1\_lang-jp-jp.png 합니다.

     

-   [  **Windows.ApplicationModel.Resources**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources) 및 [**Windows.ApplicationModel.Resources.Core**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources.Core)의 API를 사용하여 앱의 로캘별 리소스를 지정하고 로드할 수 있습니다. 또한 이러한 API는 사용자 설정에 따라 올바른 로캘을 결정한 다음 사용자에 맞는 리소스를 검색하기 때문에 특정 로캘을 포함하지 않는 자산 참조를 사용합니다.
-   Microsoft Visual Studio 2015에서 **프로젝트 저장소-> 응용 프로그램 패키지 만들기->...**  패키지를 만듭니다.

## <a name="defining-scaling-factor-resource-packs"></a>배율 인수 리소스 팩 정의


Windows 10에서는 세 가지 사용자 인터페이스 확장 요소를 제공합니다. 1.0 x, 1.4 x 및 1.8 x입니다. 각 디스플레이의 배율 값은 화면 크기, 화면 해상도 및 화면과 사용자 간의 예상 평균 거리 등 다양한 요인에 따라 설치 중에 설정됩니다. 사용자는 가독성을 높이기 위해 배율 인수를 조정할 수도 있습니다. 최상의 환경을 구현하려면 게임이 DPI 인식 및 배율 인수 인식이어야 합니다. 이러한 인식은 부분적으로 세 가지 배율 인수에 대해 각각 중요한 시각적 자산 버전을 만들어야 함을 의미합니다. 포인터 조작과 적중 테스트도 포함됩니다.

각 UWP 앱 배율 인수에 대한 리소스 팩을 지원하도록 앱을 구성하는 경우 다음 작업을 해야 합니다.

-   지원할 각 배율 인수(scale-100, scale-140 및 scale-180)에 대한 앱 하위 디렉터리(또는 파일 버전)를 만듭니다.
-   개발하는 동안 배율 인수별로 다르지 않더라도 배율 인수에 적합한 모든 자산의 복사본을 각 배율 인수 리소스 디렉터리에 저장합니다.
-   각 디렉터리에서 각 자산의 이름이 같아야 합니다. 예를 들어 메뉴\_option1.png 둘 다에 이름이 있어야 합니다 \\확장-100 및 \\배율 180 디렉터리 파일의 내용을 다른 경우에 마찬가지입니다. 이 경우 표시 되는 그대로 \\확장-100\\메뉴\_option1.png 및 \\배율 140\\메뉴\_option1.png 합니다.
    > **참고**    다시 필요에 따라 파일 이름에 크기 조정 비율 접미사를 추가 하는 동일한 디렉터리에 저장 예를 들어 \\자산\\메뉴\_option1\_ 확장-100.png \\자산\\메뉴\_option1\_140.png 확장 합니다.

     

-   [  **Windows.ApplicationModel.Resources.Core**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources.Core)의 API를 사용하여 자산을 로드할 수 있습니다. 특정 배율 변형을 제외하여 자산 참조를 일반화(접미사 없음)해야 합니다. 시스템이 디스플레이 및 사용자 설정에 적합한 배율 자산을 검색합니다.
-   Visual Studio 2015에서 **프로젝트 저장소-> 응용 프로그램 패키지 만들기->...**  패키지를 만듭니다.

## <a name="defining-directx-feature-level-resource-packs"></a>DirectX 기능 수준 리소스 팩 정의


DirectX 기능 수준은 이전 및 현재 버전의 DirectX(특히 Direct3D)에 대한 GPU 기능 집합에 해당합니다. 여기에는 셰이더 모델 사양 및 기능, 셰이더 언어 지원, 텍스처 압축 지원 및 전반적인 그래픽 파이프라인 기능이 포함됩니다.

사용자 기준 앱 팩 기준 질감 압축 형식을 사용 해야 합니다. BC1, BC2 또는 BC3 합니다. 이러한 형식은 저급 ARM 플랫폼에서 전용 다중 GPU 워크스테이션 및 미디어 컴퓨터에 이르기까지 모든 UWP 장치에서 사용할 수 있습니다.

로컬 디스크 공간 및 다운로드 대역폭을 절약하려면 DirectX 기능 수준 10 이상의 텍스처 형식 지원을 리소스 팩에 추가해야 합니다. 그러면 BC6H 및 BC7과 같은 11용 고급 압축 구성표를 사용할 수 있습니다. (자세한 내용은 참조 하세요. [Direct3D 11에서 질감 블록 압축](https://docs.microsoft.com/windows/desktop/direct3d11/texture-block-compression-in-direct3d-11).) 이러한 형식은 최신 Gpu에서 지 원하는 고해상도 질감 자산에 대 한 효율적이 고, 성능 및 고급 플랫폼에서 게임의 공간 요구 사항을 개선 사용 합니다.

| DirectX 기능 수준 | 지원되는 텍스처 압축 |
|-----------------------|-------------------------------|
| 9                     | BC1, BC2, BC3                 |
| 10                    | BC4, BC5                      |
| 11                    | BC6H, BC7                     |

 

또한 각 DirectX 기능 수준은 서로 다른 셰이더 모델 버전을 지원합니다. 기능 수준별로 컴파일된 셰이더 리소스를 만들 수 있으며, DirectX 기능 수준 리소스 팩에 포함할 수 있습니다. 또한 이후 버전의 일부 셰이더 모델은 이전 셰이더 모델 버전에서 사용할 수 없는 일반 맵 등의 자산을 사용할 수 있습니다. 이러한 셰이더 모델별 자산도 DirectX 기능 수준 리소스 팩에 포함해야 합니다.

리소스 메커니즘은 주로 자산에 대해 지원되는 텍스처 형식에 중점을 두기 때문에 3개의 전체 기능 수준만 지원합니다. DX9 같은 하위 수준 (점 버전)에 대 한 별도 셰이더 해야 하는 경우\_1 vs DX9\_3, 자산 관리 및 렌더링 코드에서 처리 해야 하 명시적으로 합니다.

각 DirectX 기능 수준에 대한 리소스 팩을 지원하도록 앱을 구성하는 경우 다음 작업을 해야 합니다.

-   지원할 각 DirectX 기능 수준(dxfl-dx9, dxfl-dx10 및 dxfl-dx11)에 대한 앱 하위 디렉터리(또는 파일 버전)를 만듭니다.
-   개발하는 동안 기능 수준별 자산을 각 기능 수준 리소스 디렉터리에 저장합니다. 로캘 및 배율 인수와 달리, 게임의 각 기능 수준에 대해 다른 렌더링 코드 분기를 사용할 수 있으며, 특정 기능 수준이나 지원되는 모든 기능 수준의 하위 집합에서만 사용되는 텍스처, 컴파일된 셰이더 또는 기타 자산이 있는 경우 해당 자산을 사용하는 기능 수준의 디렉터리에만 자산을 저장합니다. 모든 기능 수준에서 로드되는 자산의 경우 각 기능 수준 리소스 디렉터리에 동일한 이름의 해당 버전이 있어야 합니다. 예를 들어 텍스처를 위한 기능 수준 독립 "coolsign.dds" 라는, BC3 압축 버전을 배치 합니다 \\dxfl dx9 디렉터리 및 BC7 압축 버전은 \\dxfl dx11 디렉터리입니다.
-   여러 기능 수준에서 사용 가능한 경우 각 디렉터리에서 각 자산의 이름이 같아야 합니다. 예를 들어 coolsign.dds 해야 이름이 동일한 둘 다에 \\dxfl dx9 및 \\dxfl dx11 디렉터리 파일의 내용을 다른 경우에 마찬가지입니다. 이 경우 표시 되는 그대로 \\dxfl dx9\\coolsign.dds 및 \\dxfl dx11\\coolsign.dds 합니다.
    > **참고**    다시 필요에 따라 파일 이름에는 기능 수준 접미사를 추가 하는 동일한 디렉터리에 저장 예를 들어 \\질감\\coolsign\_dxfl-dx9.dds \\ 질감\\coolsign\_dxfl dx11.dds 합니다.

     

-   그래픽 리소스를 구성할 때 지원되는 DirectX 기능 수준을 선언합니다.
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

-   [  **Windows.ApplicationModel.Resources.Core**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources.Core)의 API를 사용하여 리소스를 로드할 수 있습니다. 기능 수준을 제외하여 자산 참조를 일반화(접미사 없음)해야 합니다. 그러나 언어 및 배율과 달리 시스템이 지정된 디스플레이에 적합한 기능 수준을 자동으로 결정하지 않으며, 사용자가 코드 논리에 따라 결정해야 합니다. 결정한 후 API를 사용하여 OS에 기본 기능 수준을 알립니다. 그러면 시스템이 해당 기본 설정에 따라 올바른 자산을 검색할 수 있습니다. 다음은 플랫폼의 현재 DirectX 기능 수준을 앱에 알리는 방법을 보여 주는 코드 샘플입니다.
    
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

    > **참고**  코드에서 이름 (또는 기능 수준 디렉터리 아래의 경로)에서 직접 질감을 로드 합니다. 기능 수준 디렉터리 이름이나 접미사는 포함하지 마세요. 예를 들어 로드 "질감\\coolsign.dds"가 아니라 "dxfl dx11\\질감\\coolsign.dds" 또는 "질감\\coolsign\_dxfl dx11.dds"입니다.

     

-   이제 [**ResourceManager**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceManager)를 사용하여 현재 DirectX 기능 수준과 일치하는 파일을 찾습니다. **ResourceManager**는 [**ResourceMap**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceMap)을 반환하며, [**ResourceMap::GetValue**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.resources.core.resourcemap.getvalue)(또는 [**ResourceMap::TryGetValue**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.resources.core.resourcemap.trygetvalue)) 및 제공된 [**ResourceContext**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceContext)로 쿼리합니다. 그러면 [**SetGlobalQualifierValue**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue)를 호출하여 지정된 DirectX 기능 수준과 가장 일치하는 [**ResourceCandidate**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceCandidate)가 반환됩니다.
    
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

-   Visual Studio 2015에서 **프로젝트 저장소-> 응용 프로그램 패키지 만들기->...**  패키지를 만듭니다.
-   package.appxmanifest 매니페스트 설정에서 앱 번들을 사용하도록 설정해야 합니다.

## <a name="related-topics"></a>관련 항목


* [앱 리소스 정의](https://docs.microsoft.com/previous-versions/windows/apps/hh965321(v=win.10))
* [앱 패키징](https://docs.microsoft.com/windows/uwp/packaging/index)
* [앱 패키지 (MakeAppx.exe)](https://docs.microsoft.com/windows/desktop/appxpkg/make-appx-package--makeappx-exe-)

 

 





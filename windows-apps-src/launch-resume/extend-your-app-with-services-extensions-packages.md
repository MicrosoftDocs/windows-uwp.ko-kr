---
author: TylerMSFT
title: 서비스, 확장 및 패키지로 앱 확장
description: UWP(유니버설 Windows 플랫폼) 스토어 앱 업데이트 시 실행되는 백그라운드 작업을 만드는 방법을 알아봅니다.
ms.author: twhitney
ms.date: 05/7/2018
ms.topic: article
keywords: windows 10, uwp, 확장하기, 구성 요소화, 앱 서비스, 패키지, 확장
ms.localizationpriority: medium
ms.openlocfilehash: 624d52ff96fb2537afa3affb2d842aa29e1e6667
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5564775"
---
# <a name="extend-your-app-with-services-extensions-and-packages"></a>서비스, 확장 및 패키지로 앱 확장

Windows 10은 앱 확장 및 구성 요소화에 도움이 되는 여러 기술을 제공합니다. 이 표는 시나리오에 어떤 기술을 사용해야 하는지를 결정하는 데 도움이 됩니다. 표 다음에 시나리오에 대한 간략한 설명이 이어집니다.

| 시나리오                           | 리소스 패키지   | 자산 패키지      | 선택적 패키지   | 플랫 번들        | 앱 확장      | 앱 서비스        | 스트리밍 설치  |
|------------------------------------|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|
| 타사 코드 플러그 인            |                    |                    |                    |                    | :heavy_check_mark: |                    |                    |
| In-proc 코드 플러그 인              |                    |                    | :heavy_check_mark: |                    |                    |                    |                    |
| UX 자산(문자열/이미지)         | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |
| 주문형 콘텐츠 <br/> (예: 추가 수준) |      |                    | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |
| 개별 라이선싱 및 취득 |                    |                    | :heavy_check_mark: |                    | :heavy_check_mark: | :heavy_check_mark: |                    |
| 앱에서 바로 취득                 |                    |                    | :heavy_check_mark: |                    | :heavy_check_mark: |                    |                    |
| 설치 시간 최적화              | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |
| 디스크 사용 공간 감소              | :heavy_check_mark: |                    | :heavy_check_mark: |                    |                    |                    |                    |
| 패키지 최적화                 |                    | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    |                    |                    |
| 게시 시간 줄이기             | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    |                    |                    |

## <a name="scenario-descriptions-the-rows-in-the-table-above"></a>시나리오 설명(위의 표의 행)

**타사 플러그 인**  

Microsoft Store에서 다운로드하고 앱에서 실행할 수 있는 코드입니다. Microsoft Edge 브라우저용 확장을 예로 들 수 있습니다.

**In-proc 코드 플러그 인**  

앱과 함께 In-process로 실행되는 코드입니다. C++만 지원됩니다. 콘텐츠도 포함할 수 있습니다. 코드가 In-process로 실행되기 때문에 신뢰 수준이 높습니다. 타사에 이러한 종류의 확장성을 제공하지 않기로 선택할 수 있습니다.

**UX 자산(문자열/이미지)**  

지역화된 문자열, 이미지 및 로캘을 기준으로 또는 다른 이유로 팩터링하려는 기타 UI 콘텐츠 등의 사용자 인터페이스 자산입니다.

**주문형 콘텐츠**  

나중에 다운로드하려는 콘텐츠입니다. 새 수준, 스킨 또는 기능을 다운로드할 수 있는 앱에서 바로 구매를 예로 들 수 있습니다.

**개별 라이선싱 및 취득**  

앱과는 별개로 콘텐츠 라이선스를 허여하고 콘텐츠를 취득하는 기능입니다.

**앱에서 바로 취득**  

앱 내에서 콘텐츠 취득을 위한 프로그래밍 지원이 있는지 여부를 나타냅니다.

**설치 시간 최적화**

Microsoft Store에서 앱을 취득하고 실행하기 시작하는 데 걸리는 시간을 줄이는 기능을 제공합니다.

**디스크 사용 공간 줄이기** 필요한 앱이나 리소스만 포함하여 앱의 크기를 줄입니다.

**패키지 최적화** 대규모 또는 복잡한 앱에 대한 앱 패키지 과정을 최적화합니다.

**게시 시간 단축** Microsoft Store, 로컬 공유, 또는 웹 서버에 앱을 게시하는 데 걸리는 시간을 최소화합니다.

## <a name="technology-descriptions-the-columns-in-the-table-above"></a>기술 설명(위의 표의 열)

**리소스 패키지**

리소스 패키지는 여러 표시 크기와 시스템 언어에 맞게 앱을 조정하는 데 사용할 수 있는 자산 전용 패키지입니다. 리소스 패키지는 사용자 언어, 시스템 규모 및 DirectX 기능을 대상으로 하여 다양한 사용자 시나리오에 맞춘 앱을 제공할 수 있게 합니다. 앱 패키지에는 여러 리소스가 들어 있을 수 있지만 OS는 사용자 장치의 관련 리소스만 다운로드하여 대역폭과 디스크 공간을 절약합니다.

**자산 패키지** 자산 패키지는 앱에서 사용되는 일반적이 중앙 집중식 실행 가능한 소스 또는 실행 불가능한 파일입니다. 일반적으로 비 프로세서 또는 특정 언어 파일입니다. 예를 들어, 한 자산 패키지에서 사진 컬렉션을, 다른 자산 패키지에서 동영상을 포함할 수 있으며 앱에서 이 둘 모두를 사용합니다. 예를 들어, 한 자산 패키지에서 사진 컬렉션을, 다른 자산 패키지에서 동영상을 포함할 수 있습니다. 앱이 여러 아키텍처와 여러 언어를 지원하는 경우 이러한 자산은 아키텍처 패키지 또는 리소스 패키지에 포함될 수 있지만 이러한 경우 자산이 다른 아키텍처 패키지에 여러 번 복제되며 디스크 공간을 차지하게 됩니다. 자산 패키지를 사용하는 경우 전체 앱 패키지에 한 번만 포함되면 됩니다. 자세한 내용은 [자산 패키지 소개](../packaging/asset-packages.md)를 참조하세요.

**선택적 패키지**

선택적 패키지는 앱 패키지의 원래 기능을 보충하거나 확장하는 데 사용됩니다. 앱을 먼저 게시하고 나중에 선택적 패키지를 게시하거나 앱과 선택적 패키지를 동시에 둘 다 게시할 수 있습니다. 선택적 패키지를 통해 앱을 확장하여 콘텐츠를 별도의 앱 패키지로 배포하고 수익화할 수 있는 이점이 있습니다. 선택적 패키지는 앱 확장과 달리 메인 앱의 ID로 실행되므로 대개 원래 앱 개발자가 개발합니다. 선택적 패키지를 어떻게 정의하느냐에 따라 선택적 패키지에서 메인 앱으로 코드나 자산 또는 코드와 자산을 로드할 수 있습니다. 별도로 수익화하고 라이선스를 허용하고 배포할 수 있는 콘텐츠로 앱을 향상시키려면 선택적 패키지가 올바른 선택입니다. 구현 세부 사항은 [Optional packages and related set authoring(선택적 패키지 및 관련 집합 제작)](https://docs.microsoft.com/windows/uwp/packaging/optional-packages)을 참조하세요.

**플랫 번들**
[플랫 번들 앱 패키지](../packaging/flat-bundles.md)는 일반 앱 번들과 비슷하지만 폴더 내에 모든 앱 패키지를 포함하는 대신 해당 앱 패키지에 대한 *참조*만 포함합니다. 파일 자체 대신 앱 패키지에 대한 참조를 포함하여 플랫 번들은 앱을 패키지하고 다운로드하는 데 걸리는 시간을 줄입니다.

**앱 확장**

[앱 확장](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions)을 사용하면 UWP 앱이 다른 UWP 앱에서 제공하는 콘텐츠를 호스트할 수 있습니다. 해당 앱의 읽기 전용 콘텐츠를 검색, 열거 및 액세스합니다.

앱에서 확장을 지원하는 경우 개발자가 앱에 대한 확장을 제출할 수 있습니다. 따라서 사전 테스트를 거치지 않은 확장을 로드할 때 호스트 앱이 강력해야 합니다. 확장을 신뢰할 수 없는 것으로 간주해야 합니다.

응용 프로그램은 확장에서 코드를 로드할 수 없습니다. 코드 실행이 필요할 경우 앱 서비스를 고려하세요.

**앱 서비스**

Windows 앱 서비스를 사용하면 UWP 앱이 다른 유니버설 Windows 앱에 서비스를 제공할 수 있어 앱 간 통신이 가능합니다. Windows 10, 버전 1607부터 앱 서비스를 통해 앱이 동일한 장치에서 호출할 수 있는 UI 없는 서비스를 원격 장치에 만들 수 있습니다. 자세한 내용은 [앱 서비스 만들기 및 사용](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)을 참조하세요.

앱 서비스는 다른 UWP 앱에 서비스를 제공하는 UWP 앱입니다. 장치에서 앱 서비스는 웹 서비스와 비슷합니다. 앱 서비스는 호스트 앱에서 배경 작업으로 실행되고 다른 앱에 서비스를 제공할 수 있습니다. 예를 들어, 앱 서비스는 다른 앱에서 사용할 수 있는 바코드 스캐너 서비스를 제공할 수 있습니다. 또는 앱의 Enterprise Suite에 해당 제품군의 다른 앱에서 사용할 수 있는 일반 맞춤법 검사 앱 서비스가 있습니다.

**UWP 앱 스트리밍 설치**

스트리밍 설치는 앱이 사용자에게 배달되는 방식을 최적화하는 방법입니다. 전체 앱이 다운로드되기를 기다렸다가 사용하는 대신 필요한 부분이 다운로드되면 바로 앱을 실행할 수 있습니다. 기본 활성화 및 실행에 필요한 섹션과 앱의 나머지 부분을 위한 추가 콘텐츠로 앱을 나누는 것은 개발자에게 달려 있습니다. 자세한 내용과 구현 사항은 [UWP App Streaming Install(UWP 앱 스트리밍 설치)](https://docs.microsoft.com/windows/uwp/packaging/streaming-install)을 참조하세요.

## <a name="see-also"></a>참고 항목

[앱 서비스 만들기 및 사용](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)  
[자산 패키지 소개](../packaging/asset-packages.md)  
[패키징 레이아웃으로 패키지 만들기](../packaging/packaging-layout.md)  
[선택적 패키지 및 관련 집합 제작](https://docs.microsoft.com/windows/uwp/packaging/optional-packages)  
[자산 패키지 및 패키지 접기를 사용하여 개발](../packaging/package-folding.md)  
[UWP 앱 스트리밍 설치](https://docs.microsoft.com/windows/uwp/packaging/streaming-install)  
[플랫 번들 앱 패키지](../packaging/flat-bundles.md)  
[Windows.ApplicationModel.AppService 네임스페이스](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.AppService)  
[Windows.ApplicationModel.Extensions 네임스페이스](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions)  
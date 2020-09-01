---
title: 서비스, 확장 및 패키지로 앱 확장
description: UWP (유니버설 Windows 플랫폼) 스토어 앱을 업데이트할 때 실행 되는 백그라운드 작업을 만드는 방법을 설명 합니다.
ms.date: 05/07/2018
ms.topic: article
keywords: windows 10, uwp, 확장, componentize, app service, 패키지, 확장
ms.localizationpriority: medium
ms.openlocfilehash: 9239178701082012f2878e996ede2f3f56a9a94b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155877"
---
# <a name="extend-your-app-with-services-extensions-and-packages"></a>서비스, 확장 및 패키지로 앱 확장

Windows 10에는 앱을 확장 하 고 componentizing 하는 여러 기술이 있습니다. 이 표는 요구 사항에 따라 사용 해야 하는 기술을 결정 하는 데 도움이 됩니다. 그리고 시나리오와 기술에 대 한 간략 한 설명이 나옵니다.

| 시나리오                           | 리소스 패키지   | 자산 패키지      | 선택적 패키지   | 플랫 번들        | 앱 확장      | App Service        | 스트리밍 설치  |
|------------------------------------|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|
| 타사 코드 플러그 인            |                    |                    |                    |                    | :heavy_check_mark: |                    |                    |
| In-proc 코드 플러그 인              |                    |                    | :heavy_check_mark: |                    |                    |                    |                    |
| UX 자산 (문자열/이미지)         | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |
| 주문형 콘텐츠 <br/> (예: 추가 수준) |      |                    | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |
| 별도 라이선스 및 취득 |                    |                    | :heavy_check_mark: |                    | :heavy_check_mark: | :heavy_check_mark: |                    |
| 앱에서 가져오기                 |                    |                    | :heavy_check_mark: |                    | :heavy_check_mark: |                    |                    |
| 설치 시간 최적화              | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |
| 디스크 공간 줄이기              | :heavy_check_mark: |                    | :heavy_check_mark: |                    |                    |                    |                    |
| 패키징 최적화                 |                    | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    |                    |                    |
| 게시 시간 단축             | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    |                    |                    |

## <a name="scenario-descriptions-the-rows-in-the-table-above"></a>시나리오 설명 (위 표의 행)

**타사 플러그 인**  

스토어에서 다운로드 하 여 앱에서 실행할 수 있는 코드입니다. 예를 들어 Microsoft Edge 브라우저의 확장입니다.

**In-proc 코드 플러그 인**  

앱과 함께 in-process로 실행 되는 코드입니다. 콘텐츠를 포함할 수도 있습니다. 코드가 in-process로 실행 되기 때문에 더 높은 신뢰 수준이 가정 됩니다. 타사에 이러한 종류의 확장성을 노출 하지 않도록 선택할 수 있습니다.

**UX 자산 (문자열/이미지)**  

지역화 된 문자열, 이미지 및 로캘 또는 기타 원인에 따라 요인을 고려 하려는 기타 UI 콘텐츠와 같은 사용자 인터페이스 자산

**주문형 콘텐츠**  

나중에 다운로드 하려는 콘텐츠입니다. 예를 들어 앱에서 바로 구매 기능을 사용 하 여 새로운 수준, 스킨 또는 기능을 다운로드할 수 있습니다.

**별도 라이선스 및 취득**  

앱에 독립적으로 콘텐츠를 사용 하 고 권한을 부여할 수 있습니다.

**앱에서 가져오기**  

앱 내에서 콘텐츠를 얻기 위한 프로그래밍 지원이 있는지 여부를 나타냅니다.

**설치 시간 최적화**

스토어에서 앱을 확보 하 고 실행을 시작 하는 데 걸리는 시간을 줄일 수 있는 기능을 제공 합니다.

**디스크 공간 줄이기** 필요한 앱 또는 리소스만 포함 하 여 앱의 크기를 줄입니다.

**패키징 최적화** 대규모 또는 복잡 한 앱에 대 한 앱 패키징 프로세스를 최적화 합니다.

**게시 시간 단축** 스토어, 로컬 공유 또는 웹 서버에 앱을 게시 하는 데 걸리는 시간을 최소화 합니다.

## <a name="technology-descriptions-the-columns-in-the-table-above"></a>기술 설명 (위 표의 열)

**리소스 패키지**

리소스 패키지는 앱이 여러 표시 크기 및 시스템 언어에 맞게 조정 될 수 있도록 하는 자산 전용 패키지입니다. 리소스 패키지는 응용 프로그램을 다양 한 사용자 시나리오에 맞게 조정할 수 있도록 사용자 언어, 시스템 크기 조정 및 DirectX 기능을 대상으로 합니다. 앱 패키지는 여러 리소스를 포함할 수 있지만, OS는 사용자 장치당 관련 리소스를 다운로드 하 여 대역폭과 디스크 공간을 절약 합니다.

**자산 패키지** 자산 패키지는 응용 프로그램에서 사용할 수 있는 일반적인 실행 파일 또는 실행 불가능 파일의 일반적인 원본입니다. 이러한 파일은 일반적으로 비 프로세서 또는 언어 관련 파일입니다. 예를 들어이 항목에는 하나의 자산 패키지에 있는 그림 컬렉션과 다른 자산 패키지의 비디오가 포함 될 수 있으며, 둘 다 앱에서 사용 됩니다. 앱에서 여러 아키텍처와 여러 언어를 지 원하는 경우 이러한 자산은 아키텍처 패키지 또는 리소스 패키지에 포함 될 수 있지만 여러 아키텍처 패키지에서 자산이 여러 번 중복 되어 디스크 공간을 차지 함을 의미 하기도 합니다. 자산 패키지를 사용 하는 경우 전체 앱 패키지에 한 번만 포함 해야 합니다. 자세히 알아보려면 [asset 패키지 소개](/windows/msix/package/asset-packages) 를 참조 하세요.

**선택적 패키지**

선택적 패키지는 앱 패키지의 원래 기능을 보완 하거나 확장 하는 데 사용 됩니다. 앱을 게시 한 후 나중에 선택적 패키지를 게시 하거나 앱과 선택적 패키지를 동시에 게시할 수 있습니다. 선택적 패키지를 통해 앱을 확장 하면 콘텐츠를 별도의 앱 패키지로 배포 하 고 수익 화 하는 이점이 있습니다. 선택적 패키지는 일반적으로 주 앱 (앱 확장과 달리)의 id로 실행 되기 때문에 원래 앱 개발자가 개발 하려고 합니다. 선택적 패키지를 정의 하는 방법에 따라 선택적 패키지에서 주 앱으로 코드, 자산 또는 코드 및 자산을 로드할 수 있습니다. 개별적으로 수익 화할, 사용이 허가 되 고 배포 될 수 있는 콘텐츠를 사용 하 여 앱을 개선 해야 하는 경우 선택적 패키지가 적합 한 선택이 될 수 있습니다. 구현에 대 한 자세한 내용은 [선택적 패키지 및 관련 집합 제작](/windows/msix/package/optional-packages)을 참조 하세요.

**플랫 번들** 
 [플랫 번들 앱 패키지](/windows/msix/package/flat-bundles) 는 폴더 내의 모든 앱 패키지를 포함 하는 대신 해당 앱 패키지에 대 한 *참조* 만 포함 하는 것을 제외 하 고 일반 앱 번들과 비슷합니다. 플랫 번들은 파일 자체 대신 앱 패키지에 대 한 참조를 포함 하 여 앱을 패키지 하 고 다운로드 하는 데 걸리는 시간을 줄입니다.

**앱 확장**

[앱 확장](/uwp/api/windows.applicationmodel.appextensions) 을 사용 하면 uwp 앱이 다른 uwp 앱에서 제공 하는 콘텐츠를 호스트할 수 있습니다. 해당 앱의 읽기 전용 콘텐츠를 검색, 열거 및 액세스합니다.

앱에서 확장을 지 원하는 경우 개발자가 앱에 대 한 확장을 제출할 수 있습니다. 따라서 호스트 앱은 미리 테스트 되지 않은 확장을 로드할 때 강력 해야 합니다. 확장은 신뢰할 수 없는 것으로 간주 해야 합니다.

응용 프로그램은 확장에서 코드를 로드할 수 없습니다. 코드를 실행 해야 하는 경우 App Services 고려 하세요.

**App Service**

Windows 앱 서비스는 UWP 앱이 다른 유니버설 Windows 앱에 서비스를 제공할 수 있도록 하 여 앱 간 통신을 가능 하 게 합니다. 앱 서비스를 사용 하면 앱이 동일한 장치에서 호출할 수 있는 UI 없는 서비스를 만들 수 있으며, 원격 장치에서 Windows 10, 버전 1607부터 시작할 수 있습니다. 자세한 내용은 [app Service 만들기 및 사용](./how-to-create-and-consume-an-app-service.md) 을 참조 하세요.

App services는 다른 UWP 앱에 서비스를 제공 하는 UWP 앱입니다. 장치에서 웹 서비스와 유사 합니다. App service는 호스트 앱에서 백그라운드 작업으로 실행 되며 다른 앱에 서비스를 제공할 수 있습니다. 예를 들어 app service는 다른 앱이 사용할 수 있는 바코드 스캐너 서비스를 제공할 수 있습니다. 또는 응용 프로그램의 엔터프라이즈 제품군에는 제품군의 다른 앱에서 사용할 수 있는 일반적인 맞춤법 검사 app service가 있습니다.

**UWP 앱 스트리밍 설치**

스트리밍 설치는 앱이 사용자에 게 전달 되는 방식을 최적화 하는 방법입니다. 앱을 사용 하기 전에 전체 앱이 다운로드 될 때까지 기다리는 대신 사용자는 필요한 부분이 다운로드 되는 즉시 앱을 사용할 수 있습니다. 개발자는 응용 프로그램을 기본 활성화 및 시작을 위해 필수 섹션으로 분할 하 고 나머지 앱에 대 한 추가 콘텐츠를 사용할 수 있습니다. 자세한 내용과 구현에 대 한 자세한 내용은 [UWP 앱 스트리밍 설치](/windows/msix/package/streaming-install) 를 참조 하세요.

## <a name="see-also"></a>관련 항목

[앱 서비스 만들기 및 사용](./how-to-create-and-consume-an-app-service.md)  
[자산 패키지 소개](/windows/msix/package/asset-packages)  
[패키징 레이아웃으로 패키지 만들기](/windows/msix/package/packaging-layout)  
[선택형 패키지 및 관련 세트 제작](/windows/msix/package/optional-packages)  
[자산 패키지 및 패키지 접기를 사용하여 개발](/windows/msix/package/package-folding)  
[UWP 앱 스트리밍 설치](/windows/msix/package/streaming-install)  
[플랫 번들 앱 패키지](/windows/msix/package/flat-bundles)  
[AppService 네임 스페이스](/uwp/api/Windows.ApplicationModel.AppService)  
[Windows ApplicationModel. Extensions 네임 스페이스](/uwp/api/windows.applicationmodel.appextensions)
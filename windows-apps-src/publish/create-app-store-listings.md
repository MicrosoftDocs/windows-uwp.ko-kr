---
author: jnHs
Description: The Store listings section of the app submission process is where you provide the text and images that customers will see when viewing your app's listing in the Microsoft Store.
title: 앱 Store 목록 만들기
ms.assetid: 50D67219-B6C6-4EF0-B76A-926A5F24997D
ms.author: wdg-dev-content
ms.date: 06/18/2018
ms.topic: article
keywords: Windows 10, UWP, 목록, 설명, Microsoft Store 페이지, 릴리스 노트, 제목
ms.localizationpriority: medium
ms.openlocfilehash: 237642897beb51c9b685068ee714182fa1fe1bb5
ms.sourcegitcommit: d0e836dfc937ebf7dfa9c424620f93f3c8e0a7e8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5638248"
---
# <a name="create-app-store-listings"></a>앱 Store 목록 만들기


[앱 제출 프로세스](app-submissions.md)의 **Store 목록** 섹션에서는 Microsoft Store의 앱 목록을 확인할 때 고객에게 표시되는 텍스트 및 [이미지](app-screenshots-and-images.md)를 제공합니다.

**Store 목록**의 많은 필드가 옵션이지만 목록이 돋보이도록 다양한 이미지와 가능한 많은 정보를 제공하는 것이 좋습니다. **Store 목록** 단계가 완료된 것으로 간주되는 데 필요한 최소 요구 사항은 텍스트 설명과 하나 이상의 [스크린샷](app-screenshots-and-images.md#screenshots)입니다. 일부 제출에서는 [개인 정보 취급 방침](#privacy-policy) 및 [지원 연락처 정보](#support-contact-info) 필드가 필수입니다. 

> [!TIP]
> 또한 대시보드에 직접 정보를 제공하고 파일을 업로드하는 것이 아니라 .csv 파일에서 오프라인으로 목록 정보를 입력하고 싶은 경우에 [Store 목록 가져오기 및 내보내기](import-and-export-store-listings.md)를 선택적으로 수행할 수 있습니다. 가져오기 및 내보내기 옵션을 사용할 수 있는 기능은 한 번에 여러 번 업데이트를 수행할 수 있기 때문에 목록이 다양한 언어로 되어 있는 경우에 특히 편리할 수 있습니다. 

기본적으로 모든 대상 운영 체제에 동일한 Store 목록(언어별)을 사용합니다. 제출이 지원하는 특정 운영 체제에 대한 사용자 지정된 Store 목록을 사용하려는 경우 [플랫폼별 Store 목록 만들기](create-platform-specific-store-listings.md)가 가능합니다. Windows 10 고객에게는 항상 기본 목록이 표시됩니다.

## <a name="store-listing-languages"></a>Microsoft Store 목록 언어

하나 이상의 언어에 대한 **Microsoft Store 목록** 페이지를 완료해야 합니다. 패키지가 지원하는 각 언어로 Microsoft Store 목록을 제공하는 것이 좋지만 Microsoft Store 목록을 제공하지 않을 언어는 유연성 있게 제거할 수 있습니다. 패키지에서 지원되지 않는 추가 언어로 Microsoft Store 목록을 만들 수도 있습니다.

> [!NOTE]
> 제출에 이미 패키지가 포함된 경우, 제출 개요 페이지에서 패키지에 지원되는 [언어](supported-languages.md)를 표시합니다(지원되는 언어를 삭제하지 않은 경우).

Microsoft Store 목록에 대한 언어를 추가 또는 제거하려면 제출 개요 페이지에서 **언어 추가/제거**를 클릭합니다. 패키지를 이미 업로드한 경우 **패키지에서 지원되는 언어** 섹션에 나열된 해당 언어를 확인합니다. 이러한 언어 중 하나 이상을 제거하려면 **제거**를 클릭합니다. 나중에 이 섹션에서 이전에 제거한 언어를 포함하도록 결정하는 경우 **추가**를 클릭하면 됩니다.

**추가 Microsoft Store 목록 언어** 섹션에서 **추가 언어 관리**를 클릭하여 패키지에 포함되지 *않은* 언어를 추가하거나 제거할 수 있습니다. 추가할 언어의 확인란을 선택한 다음 **업데이트**를 클릭합니다. 선택한 언어는 **추가 Microsoft Store 목록 언어** 섹션에 표시됩니다. 이러한 언어 중 하나 이상을 제거하려면 **제거**를 클릭하거나 **추가 언어 관리**를 클릭하고 제거할 언어의 확인란을 선택 취소합니다.

선택을 완료하면 **저장**을 클릭하여 제출 개요 페이지로 돌아갑니다.

## <a name="add-and-edit-store-listing-info"></a>추가 하 고 스토어 목록 정보를 편집 합니다.

스토어 목록을 편집 하려면 제출 개요 페이지에서 언어 이름을 선택 합니다.

**Microsoft Store 목록** 페이지의 맨 위에 선택 언어에 대한 기본 Microsoft Store 목록과 관련된 필드가 있습니다. 이러한 필드는 모든 고객에게 표시되지만, 이전 OS 버전(Windows 8.x 이하, Windows Phone 8.x 이하)을 대상으로 하는 패키지가 있고 지정된 OS 버전에서 고객에게 표시될 다른 스크린샷 또는 정보를 포함하도록 플랫폼별 Microsoft Store 목록을 만든 경우는 표시되지 않습니다. 자세한 내용은 [플랫폼별 Microsoft Store 목록 만들기](create-platform-specific-store-listings.md)를 참조하세요.

## <a name="product-name"></a>제품 이름

이 드롭다운 상자는 이름 (앱에 대 한 이름을 여러 개 예약 않은) 하는 경우 스토어 목록에 사용 해야를 지정할 수 있습니다.

스토어 목록에서 작업할 때와 동일한 언어로 패키지를 업로드 하는 경우 해당 패키지에 사용한 이름이 선택 됩니다. 이미 게시 된 후 필요한 [앱](manage-app-names.md#rename-an-app-that-has-already-been-published) 이름을 변경 하는 경우 새 이름을 사용 하는 패키지를 사용 하 여 새 제출을 만들 때 다음 다른 예약 된 이름을 선택할 수 있습니다.

언어에 대 한 패키지를 업로드 하지 않은 작업 중인 및 둘 이상의 이름을 예약한, 이름을 나타내야 해당 언어에 연결된 된 패키지가 없으므로 예약 된 앱 이름 중 하나를 선택 해야 합니다.

> [!NOTE]
> 스토어 언어에서 목록에서 작업할 때에 선택 되는 **제품 이름** 적용 됩니다. 고객이; 앱을 설치 하는 경우 표시 되는 이름 영향을 주지 설치 패키지의 매니페스트에에서 해당 이름을 제공 됩니다. 혼동을 피하기 위해 각 언어 패키지 및 스토어 목록에는 동일한 이름의 사용 하는 것이 좋습니다.

## <a name="description"></a>설명

설명 필드에서는 고객에게 앱이 수행하는 작업을 알려 줄 수 있습니다. 이 필드는 필수 필드이며, 일반 텍스트를 최대 10,000자 입력할 수 있습니다.

설명을 돋보이게 하는 몇 가지 팁은 [호소력 있는 앱 설명 쓰기](write-a-great-app-description.md)를 참조하세요.

<span id="release-notes" />

## <a name="whats-new-in-this-version"></a>이 버전의 새로운 기능

앱을 처음으로 제출하려는 경우에는 이 필드를 비워둡니다. 기존 앱의 업데이트를 제출할 경우 이 필드를 통해 고객에게 최신 릴리스에서 변경된 내용을 알릴 수 있습니다. 이 필드는 1,500자로 제한됩니다 (전에는 이 필드가 **릴리스 정보**라고 불림).

## <a name="app-features"></a>앱 기능

앱의 주요 기능에 대한 간단한 요약입니다. 기능은 **설명** 외에도 앱 Store 목록의 **기능** 섹션에 글머리 기호 목록으로 고객에게 표시됩니다. 기능당 몇 단어 또는 200자 이하만 사용하여 간단하게 유지합니다. 최대 20개 기능을 포함할 수 있습니다.

> [!NOTE]
> Store 목록에 글머리 기호로 앱 기능이 표시되므로 별도의 글머리 기호를 추가하지 마세요.

## <a name="screenshots"></a>스크린샷

앱 제출에 한 개의 스크린샷이 필요합니다. 고객이 자신의 장치 유형에서 어떻게 앱이 표시되는지 알 수 있도록 앱에서 지원하는 각 장치 유형에 최소 4개의 스크린샷을 제공할 것을 권장합니다.

자세한 내용은 [앱 스크린샷 및 이미지](app-screenshots-and-images.md#screenshots)를 참조하세요.


## <a name="store-logos"></a>Microsoft Store 로고 

Microsoft Store 로고는 앱이 고객에게 더 효과적으로 표시될 수 있도록 업로드 할 수 있는 옵션 이미지입니다. Store가 앱 패키지의 로고 이미지를 사용하도록 허용하는 대신, Windows 10(Xbox 포함) 고객을 대상으로 한 앱 Store 목록에서 여기에 업로드 한 이미지만 사용하도록 선택적으로 지정할 수 있습니다.

> [!IMPORTANT]
> 앱이 Xbox나 Windows Phone 8.1 이전 버전을 지원하는 경우, Microsoft Store에 목록이 정확히 표시될 수 있는 특정 이미지를 제공해야 합니다. 

자세한 내용은 [Microsoft Store 로고](app-screenshots-and-images.md#store-logos)를 참조하세요.


## <a name="additional-art-assets"></a>추가 아트 자산

예고편과 홍보 이미지 등 제품에 대한 추가 자산을 제출할 수 있습니다. 모두 선택 사항이지만, 가능한 자산을 많이 업로드 하는 것이 좋습니다. 이런 이미지는 고객의 제품에 대한 이해를 높이고, 더욱 소구력 높은 목록을 만드는 데 도움을 줍니다.

자세한 내용은 [추가 아트 자산](app-screenshots-and-images.md#additional-art-assets)을 참조하세요.

<a id="supplemental-information" />

## <a name="supplemental-fields"></a>보조 필드

이 섹션의 필드는 모두 선택 사항입니다. 아래의 정보를 검토하여 이러한 정보를 제공하는 것이 제출에 도움이 되는지 판단하세요. 특히 대부분의 제출에 **간단한 설명**을 제공하는 것이 좋습니다. 다른 필드는 다른 시나리오에서 제품에 최적의 경험을 제공하는 데 도움이 될 수 있습니다.

### <a name="short-title"></a>짧은 제목

제품 이름의 짧은 버전입니다. 이러한 짧은 이름은 제품의 정식 제목 대신에 Xbox One이 다양한 위치에 표시될 수 있습니다(설치 동안 도전 과제 등에 표시).

이 필드는 50자로 제한됩니다


### <a name="sort-title"></a>정렬 제목

제품을 사전 순서로 정렬할 수 있거나 다양한 방식으로 철자를 입력할 수 있는 경우에는 여기에 다른 버전을 입력할 수 있습니다. 이렇게 하면 고객이 검색 동안 해당 버전을 입력할 경우 더 빠르게 제품을 찾을 수 있습니다. 

이 필드는 255자로 제한됩니다.


### <a name="voice-title"></a>음성 제목

제품의 대체 이름이 있는 경우에는 Kinect 또는 헤드셋을 사용할 때 Xbox One의 오디오 환경에서 사용할 수 있습니다.

이 필드는 255자로 제한됩니다.


### <a name="short-description"></a>간단한 설명

제품의 Microsoft Store 목록 위쪽에서 사용할 수 있는 눈에 띄는 간단한 설명. 제공하지 않는 경우 긴 [설명](#description)의 첫 번째 단락(최대 500자)이 대신 사용됩니다. 설명은 이 텍스트 아래에도 표시되기 때문에 Microsoft Store 목록이 반복되지 않도록 다른 텍스트를 사용하여 간단한 설명을 제공하는 것이 좋습니다.

게임의 경우 Xbox One의 게임 허브의 정보 섹션에도 간단한 설명이 나타날 수 있습니다.

최상의 결과 위해 아래에서 270 문자 간단한 설명을 유지 합니다. 필드는 500 자로 제한, 하지만 일부 보기에서는 먼저 270 문자만 짧은 설명의 나머지 부분을 볼 수 있는 링크) (함께 표시 됩니다.


### <a name="additional-system-requirements"></a>추가 시스템 요구 사항

필요한 경우 앱이 제대로 작동하는 데 필요한 하드웨어 구성을 [앱 속성](enter-app-properties.md#system-requirements)의 **시스템 요구 사항** 섹션에서 제공한 정보 이상으로 설명할 수 있습니다. 이 정보는 일부 컴퓨터에서 사용할 수 없는 하드웨어가 앱에 필요한 경우에 특히 중요합니다. 예를 들어 앱이 3D 프린터나 마이크로 컨트롤러 같은 외부 USB 하드웨어에서만 제대로 작동하는 경우 여기에 입력하는 것이 좋습니다. 여기에 입력하는 정보는 Windows 10, 버전 1607 이상(Xbox 포함)에서 앱의 Store 목록을 조회하는 고객에게 제품의 속성 페이지에 지정한 요구 사항과 함께 표시됩니다. 

**최소 하드웨어** 및 **권장 하드웨어** 모두에 대해 최대 11개 항목까지 입력할 수 있습니다. 이 항목들은 Store 목록에 글머리 기호 목록으로 고객에게 표시됩니다. 항목당 200자 이하의 몇 단어만 사용하여 간단하게 유지합니다.

> [!NOTE]
> 추가 시스템 요구 사항이 Store 목록에 글 머리 기호로 표시되므로 별도의 글머리 기호를 추가하지 마세요.


<span id="shared-fields" />

## <a name="additional-information"></a>추가 정보

아래 설명한 항목은 고객들의 제품 발견과 이해에 도움을 줍니다. 여기 입력한 정보는 [플랫폼별 Microsoft Store 목록](create-platform-specific-store-listings.md)을 만든 경우에도 운영 체제와 상관 없이 지정 언어로 Microsoft Store 목록에 표시됩니다. (이 섹션은 이전에 **공유 필드**로 불림).

### <a name="search-terms"></a>검색어

검색어(이전에는 키워드로 불림)는 고객에게 표시되지 않지만 고객이 검색어를 사용하여 검색을 할 때 Store에서 앱을 검색할 수 있도록 도와주는 한 단어나 간단한 구입니다. 각각 최대 30자의 검색어를 최대 7개까지 포함할 수 있으며, 모든 검색어에 21개 이상의 단어를 사용할 수 없습니다.

검색어를 추가할 때, 특히 검색어가 앱 이름에 포함이 되지 않을 경우에는 고객이 개발자의 앱과 유사한 앱을 검색할 때 사용할 수 있는 단어를 고려해 보세요. 실제로 앱과 관련이 없는 검색어는 사용하지 마세요.

### <a name="copyright-and-trademark-info"></a>저작권 및 상표 정보

추가 저작권 및/또는 상표 정보를 제공하려는 경우 여기에 입력합니다. 이 필드는 200자로 제한됩니다.


### <a name="additional-license-terms"></a>추가 사용 조건

[앱 개발자 계약](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement)에서 연결된 **표준 응용 프로그램 사용 조건**에 따라 고객에게 앱 사용을 허가하려면 이 필드를 비워 둡니다.

사용 조건이 **표준 응용 프로그램 사용 조건**과 다른 경우 여기에 입력하세요.

이 필드에 단일 URL을 입력하면 이 URL이 고객이 추가 사용 조건을 읽기 위해 클릭할 수 있는 링크로 고객에게 표시됩니다. 이는 추가 사용 조건이 매우 길거나 추가 사용 조건에 클릭 가능한 링크 또는 서식을 포함하려는 경우에 유용합니다.

이 필드에는 텍스트를 최대 10,000자 입력할 수 있습니다. 이렇게 하면 고객에게 이러한 추가 사용 조건이 일반 텍스트로 표시됩니다.


### <a name="developed-by"></a>개발자

앱의 Microsoft Store 목록에 **개발자** 필드를 포함시킬 경우 여기에 텍스트를 입력합니다. (**게시자** 필드는 **개발자** 필드에 값을 제공하는지 여부에 관계 없이 계정에 연결된 게시자 표시 이름을 나열합니다.)

이 필드는 255자로 제한됩니다.
 

<span id="privacy-policy" />

> [!NOTE]
> **개인 정보 취급 방침**, **웹 사이트** 및 **지원 연락처 정보** 필드가 이제는 [속성](enter-app-properties.md) 페이지에 위치합니다.


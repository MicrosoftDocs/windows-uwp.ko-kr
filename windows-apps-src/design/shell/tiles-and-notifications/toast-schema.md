---
Description: 다음 문서에서는 알림 콘텐츠에 있는 모든 속성 및 요소에 대해 설명 합니다.
title: 알림 콘텐츠 스키마
ms.assetid: 7CBC3BD5-D9C3-4781-8BD0-1F28039E1FA8
label: Toast content schema
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c095e48e24a06caf9e31066b21f9e2b023ed51cf
ms.sourcegitcommit: 5d84d8fe60e83647fa363b710916cf8b92c6e331
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91878476"
---
# <a name="toast-content-schema"></a>알림 콘텐츠 스키마

 

다음은 알림 콘텐츠에 있는 모든 속성 및 요소에 대 한 설명입니다.

[알림 라이브러리](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)대신 원시 xml을 사용 하는 경우 [xml 스키마]()를 참조 하세요.

[Toa 내용](#toastcontent)
* [Toa 시각적 개체](#toastvisual)
  * [To Bindinggeneric](#toastbindinggeneric)
    * [Ito Bindinggenericchild](#itoastbindinggenericchild)
    * [To Genericogo](#toastgenericapplogo)
    * [ToastGenericHeroImage](#toastgenericheroimage)
    * [ToastGenericAttributionText](#toastgenericattributiontext)
* [Ito Actions](#itoastactions)
* [To... 오디오](#toastaudio)
* [ToastHeader](#toastheader)


## <a name="toastcontent"></a>Toa 내용
To Content는 시각적 개체, 작업 및 오디오를 비롯 한 알림의 콘텐츠를 설명 하는 최상위 수준 개체입니다.

| 속성 | 유형 | 필수 | 설명 |
|---|---|---|---|
| **Launch**| 문자열 | false | 알림 메시지를 활성화할 때 응용 프로그램에 전달 되는 문자열입니다. 이 문자열의 형식과 내용은 앱에서 자체 사용을 위해 정의 됩니다. 사용자가 연결 된 앱을 시작 하는 알림 메시지를 탭 하거나 클릭 하면 시작 문자열은 앱에 컨텍스트를 제공 하 여 기본 방식으로 시작 하는 대신 알림 콘텐츠와 관련 된 보기를 사용자에 게 표시할 수 있도록 합니다. |
| **시각적 개체** | [Toa 시각적 개체](#toastvisual) | true | 알림 메시지의 시각적 부분을 설명 합니다. |
| **actions** | [Ito Actions](#itoastactions) | false | 필요에 따라 단추 및 입력을 사용 하 여 사용자 지정 작업을 만듭니다. |
| **오디오** | [To... 오디오](#toastaudio) | false | 알림 메시지의 오디오 부분에 대해 설명 합니다. |
| **ActivationType** | [ToastActivationType](#toastactivationtype) | false | 사용자가이 알림 메시지의 본문을 클릭할 때 사용 되는 정품 인증 유형을 지정 합니다. |
| **ActivationOptions** | [ToastActivationOptions](#toastactivationoptions) | false | 작성자의 새로운 업데이트: 알림 활성화와 관련 된 추가 옵션입니다. |
| **시나리오** | [To 시나리오](#toastscenario) | false | 경보가 나 미리 알림과 같이 알림이 사용 되는 시나리오를 선언 합니다. |
| **DisplayTimestamp** | DateTimeOffset? | false | 작성자의 새로운 항목 업데이트: Windows 플랫폼에서 알림을 받은 시간이 아니라 알림 콘텐츠가 실제로 전달 된 시기를 나타내는 사용자 지정 타임 스탬프를 사용 하 여 기본 타임 스탬프를 재정의 합니다. |
| **Header** | [ToastHeader](#toastheader) | false | 작성자의 새로운 기능 업데이트: 알림에 사용자 지정 헤더를 추가 하 여 알림 센터 내에서 여러 알림을 그룹화 합니다. |


### <a name="toastscenario"></a>To 시나리오
알림이 나타내는 시나리오를 지정 합니다.

| 값 | 의미 |
|---|---|
| **기본값** | 정상적인 알림 동작입니다. |
| **미리 알림** | 미리 알림 알림입니다. 이는 사용자가 해제 될 때까지 미리 확장 된 상태로 표시 되 고 사용자 화면에 표시 됩니다. |
| **경보** | 경보 알림입니다. 이는 사용자가 해제 될 때까지 미리 확장 된 상태로 표시 되 고 사용자 화면에 표시 됩니다. 오디오는 기본적으로 반복 되며 알람 오디오를 사용 합니다. |
| **IncomingCall** | 들어오는 호출 알림입니다. 이는 특수 한 호출 형식으로 미리 확장 되어 표시 되며, 사용자의 화면을 닫을 때까지 유지 됩니다. 오디오는 기본적으로 반복 되며 벨 소리 오디오를 사용 합니다. |


## <a name="toastvisual"></a>Toa 시각적 개체
알림을의 시각적 개체에는 텍스트, 이미지, 적응 콘텐츠 등을 포함 하는 바인딩이 포함 되어 있습니다.

| 속성 | 유형 | 필수 | 설명 |
|---|---|---|---|
| **BindingGeneric** | [To Bindinggeneric](#toastbindinggeneric) | true | 모든 장치에서 렌더링할 수 있는 일반 알림 바인딩입니다. 이 바인딩은 필수 이며 null 일 수 없습니다. |
| **BaseUri** | URI | false | 이미지 원본 특성의 상대 Url과 결합 된 기본 URL입니다. |
| **AddImageQuery** | bool? | false | Windows가 알림 알림에서 제공 된 이미지 URL에 쿼리 문자열을 추가할 수 있게 하려면 "true"로 설정 합니다. 서버에서 이미지를 호스팅하고 쿼리 문자열을 기반으로 하는 이미지 변형을 검색 하거나 쿼리 문자열을 무시 하 고 쿼리 문자열 없이 지정 된 대로 이미지를 반환 하 여 쿼리 문자열을 처리할 수 있는 경우이 특성을 사용 합니다. 이 쿼리 문자열은 크기 조정, 대비 설정 및 언어를 지정 합니다. 예를 들어, 알림에 지정 된 "www.website.com/images/hello.png" 값은 "www.website.com/images/hello.png? ms-scale = 100&&m a s a s s-a s a-s"가 됩니다. |
| **언어**| 문자열 | false | 지역화 된 리소스를 사용 하는 경우 "en-us" 또는 "fr-fr"와 같은 BCP-47 언어 태그로 지정 된 시각적 페이로드의 대상 로캘입니다. 이 로캘은 바인딩 또는 텍스트에 지정 된 로캘에 의해 재정의 됩니다. 지정 하지 않으면 시스템 로캘이 대신 사용 됩니다. |


## <a name="toastbindinggeneric"></a>To Bindinggeneric
제네릭 바인딩은 알림을의 기본 바인딩이 며 텍스트, 이미지, 적응 콘텐츠 등을 지정 합니다.

| 속성 | 유형 | 필수 | 설명 |
|---|---|---|---|
| **Children** | IList<[Ito Bindinggenericchild](#itoastbindinggenericchild)> | false | 텍스트, 이미지 및 그룹 (기념일 업데이트에 추가)을 포함할 수 있는 알림 본문의 내용입니다. 텍스트 요소는 다른 요소 앞에와 야 하며 텍스트 요소 3 개만 지원 됩니다. 텍스트 요소가 다른 요소 뒤에 배치 되는 경우에는 맨 위에 배치 되거나 삭제 됩니다. 마지막으로 HintStyle 등의 특정 텍스트 속성은 루트 자식 텍스트 요소에서 지원 되지 않으며 AdaptiveSubgroup 내부 에서만 작동 합니다. 기념일 업데이트 없이 장치에서 AdaptiveGroup를 사용 하는 경우 그룹 콘텐츠는 삭제 됩니다. |
| **AppLogoOverride** | [To Genericogo](#toastgenericapplogo) | false | 앱 로고를 재정의 하는 선택적 로고입니다. |
| **HeroImage** | [ToastGenericHeroImage](#toastgenericheroimage) | false | 알림 및 작업 센터 내에 표시 되는 선택적 추천 "주인공" 이미지입니다. |
| **특성이** | [ToastGenericAttributionText](#toastgenericattributiontext) | false | 알림 메시지의 아래쪽에 표시 되는 선택적 특성 텍스트입니다. |
| **BaseUri** | URI | false | 이미지 원본 특성의 상대 Url과 결합 된 기본 URL입니다. |
| **AddImageQuery** | bool? | false | Windows가 알림 알림에서 제공 된 이미지 URL에 쿼리 문자열을 추가할 수 있게 하려면 "true"로 설정 합니다. 서버에서 이미지를 호스팅하고 쿼리 문자열을 기반으로 하는 이미지 변형을 검색 하거나 쿼리 문자열을 무시 하 고 쿼리 문자열 없이 지정 된 대로 이미지를 반환 하 여 쿼리 문자열을 처리할 수 있는 경우이 특성을 사용 합니다. 이 쿼리 문자열은 크기 조정, 대비 설정 및 언어를 지정 합니다. 예를 들어, 알림에 지정 된 "www.website.com/images/hello.png" 값은 "www.website.com/images/hello.png? ms-scale = 100&&m a s a s s-a s a-s"가 됩니다. |
| **언어**| 문자열 | false | 지역화 된 리소스를 사용 하는 경우 "en-us" 또는 "fr-fr"와 같은 BCP-47 언어 태그로 지정 된 시각적 페이로드의 대상 로캘입니다. 이 로캘은 바인딩 또는 텍스트에 지정 된 로캘에 의해 재정의 됩니다. 지정 하지 않으면 시스템 로캘이 대신 사용 됩니다. |


## <a name="itoastbindinggenericchild"></a>Ito Bindinggenericchild
텍스트, 이미지, 그룹 등을 포함 하는 알림 자식 요소에 대 한 표식 인터페이스입니다.

| 구현 |
| --- |
| [AdaptiveText](#adaptivetext) |
| [AdaptiveImage](#adaptiveimage) |
| [AdaptiveGroup](#adaptivegroup) |
| [AdaptiveProgressBar](#adaptiveprogressbar) |


## <a name="adaptivetext"></a>AdaptiveText
적응 텍스트 요소입니다. 최상위 수준에 배치 되 면 HintMaxLines만 적용 됩니다. 그러나이를 그룹/하위 그룹의 자식으로 배치 하면 전체 텍스트 스타일이 지원 됩니다.

| 속성 | 유형 | 필수 |설명 |
|---|---|---|---|
| **Text** | string 또는 [Bindablestring](#bindablestring) | false | 표시할 텍스트입니다. 데이터 바인딩 지원은 작성자 업데이트에 추가 되지만 최상위 텍스트 요소에 대해서만 작동 합니다. |
| **HintStyle** | [AdaptiveTextStyle](#adaptivetextstyle) | false | 스타일은 텍스트의 글꼴 크기, 두께 및 불투명도를 제어 합니다. 그룹/하위 그룹 내의 텍스트 요소에 대해서만 작동 합니다. |
| **HintWrap** | bool? | false | 텍스트 줄 바꿈을 사용 하려면 true로 설정 합니다. 최상위 텍스트 요소는이 속성을 무시 하 고 항상 래핑합니다. (HintMaxLines = 1을 사용 하 여 최상위 텍스트 요소에 대해 래핑을 사용 하지 않도록 설정할 수 있음) 그룹/하위 그룹 내의 텍스트 요소는 래핑에 대해 기본적으로 false로 설정 됩니다. |
| **HintMaxLines** | int? | false | 텍스트 요소에 표시할 수 있는 최대 줄 수입니다. |
| **HintMinLines** | int? | false | 텍스트 요소에서 표시 해야 하는 최소 줄 수입니다. 그룹/하위 그룹 내의 텍스트 요소에 대해서만 작동 합니다. |
| **HintAlign** | [AdaptiveTextAlign](#adaptivetextalign) | false | 텍스트의 가로 맞춤입니다. 그룹/하위 그룹 내의 텍스트 요소에 대해서만 작동 합니다. |
| **언어** | 문자열 | false | "En-us" 또는 "fr-fr"와 같은 BCP-47 언어 태그로 지정 된 XML 페이로드의 대상 로캘입니다. 여기에 지정 된 로캘은 바인딩 또는 시각적 개체와 같이 지정 된 다른 모든 로캘을 재정의 합니다. 이 값이 리터럴 문자열이 면이 특성은 기본적으로 사용자의 UI 언어로 설정 됩니다. 이 값이 문자열 참조 인 경우이 특성은 문자열을 확인 하는 Windows 런타임에서 선택한 로캘로 기본값을 설정 합니다. |


### <a name="bindablestring"></a>BindableString
문자열에 대 한 바인딩 값입니다.

| 속성 | 유형 | 필수 | 설명 |
|---|---|---|---|
| **BindingName** | 문자열 | true | 바인딩 데이터 값에 매핑되는 이름을 가져오거나 설정 합니다. |


### <a name="adaptivetextstyle"></a>AdaptiveTextStyle
텍스트 스타일은 글꼴 크기, 두께 및 불투명도를 제어 합니다. 미묘한 불투명도는 60% 불투명 합니다.

| 값 | 의미 |
|---|---|
| **기본값** | 기본값입니다. 스타일은 렌더러에 의해 결정 됩니다. |
| **캡션** | 단락 글꼴 크기 보다 작습니다. |
| **CaptionSubtle** | 캡션과 동일 하지만 미세한 불투명도가 있습니다. |
| **본문** | 단락 글꼴 크기입니다. |
| **BodySubtle** | 본문과 같지만 미묘한 불투명도가 있습니다. |
| **하단** | 단락 글꼴 크기, 굵은 두께입니다. 기본적으로 본문의 굵은 버전이 표시 됩니다. |
| **BaseSubtle** | 밑과 같지만 미묘한 불투명도가 있습니다. |
| **제목** | H4 글꼴 크기입니다. |
| **SubtitleSubtle** | 부제와 동일 하지만 미세한 불투명도가 있습니다. |
| **제목** | H3 글꼴 크기입니다. |
| **TitleSubtle** | 제목과 같지만 미묘한 불투명도가 있습니다. |
| **TitleNumeral** | 제목과 같지만 위쪽/아래쪽 안쪽 여백이 제거 되었습니다. |
| **Subheader.aboutdocs** | H2 글꼴 크기입니다. |
| **SubheaderSubtle** | Subheader와 동일 하지만 미묘한 불투명도가 있습니다. |
| **SubheaderNumeral** | 하위 헤더와 동일 하지만 위쪽/아래쪽 안쪽 여백이 제거 되었습니다. |
| **Header** | H1 글꼴 크기입니다. |
| **HeaderSubtle** | 헤더와 동일 하지만 미묘한 불투명도가 있습니다. |
| **HeaderNumeral** | 헤더와 동일 하지만 위쪽/아래쪽 안쪽 여백이 제거 되었습니다. |


### <a name="adaptivetextalign"></a>AdaptiveTextAlign
텍스트의 가로 alignmen를 제어 합니다.

| 값 | 의미 |
|---|---|
| **기본값** | 기본값입니다. 렌더러에 의해 맞춤이 자동으로 결정 됩니다. |
| **자동** | 현재 언어 및 문화권에 따라 결정 되는 맞춤입니다. |
| **비어** | 텍스트를 왼쪽에 가로로 맞춥니다. |
| **중심과** | 가운데에 텍스트를 가로로 맞춥니다. |
| **오른쪽** | 텍스트를 오른쪽에 가로로 맞춥니다. |


## <a name="adaptiveimage"></a>AdaptiveImage
인라인 이미지입니다.

| 속성 | 유형 | 필수 |설명 |
|---|---|---|---|
| **원본** | 문자열 | true | 이미지에 대 한 URL입니다. appdata 및 http가 지원 됩니다. 가 중 작성자 업데이트를 기준으로 웹 이미지는 일반 연결의 경우 최대 3mb, 요금제 연결의 경우 1mb가 될 수 있습니다. 아직가 중 작성자 업데이트를 실행 하지 않는 장치에서는 웹 이미지가 200 KB 보다 크지 않아야 합니다. |
| **HintCrop** | [AdaptiveImageCrop](#adaptiveimagecrop) | false | 기념일 업데이트: 원하는 이미지 자르기를 제어 합니다. |
| **HintRemoveMargin** | bool? | false | 기본적으로 그룹/하위 그룹 내의 이미지 주위에는 8px 여백이 있습니다. 이 속성을 true로 설정 하 여이 여백을 제거할 수 있습니다. |
| **HintAlign** | [AdaptiveImageAlign](#adaptiveimagealign) | false | 이미지의 가로 맞춤입니다. 그룹/하위 그룹 내의 이미지에 대해서만 작동 합니다. |
| **AlternateText** | 문자열 | false | 내게 필요한 옵션을 위해 사용 되는 이미지를 설명 하는 대체 텍스트입니다. |
| **AddImageQuery** | bool? | false | Windows가 알림 알림에서 제공 된 이미지 URL에 쿼리 문자열을 추가할 수 있게 하려면 "true"로 설정 합니다. 서버에서 이미지를 호스팅하고 쿼리 문자열을 기반으로 하는 이미지 변형을 검색 하거나 쿼리 문자열을 무시 하 고 쿼리 문자열 없이 지정 된 대로 이미지를 반환 하 여 쿼리 문자열을 처리할 수 있는 경우이 특성을 사용 합니다. 이 쿼리 문자열은 크기 조정, 대비 설정 및 언어를 지정 합니다. 예를 들어, 알림에 지정 된 "www.website.com/images/hello.png" 값은 "www.website.com/images/hello.png? ms-scale = 100&&m a s a s s-a s a-s"가 됩니다. |


### <a name="adaptiveimagecrop"></a>AdaptiveImageCrop
원하는 이미지를 자르는 방법을 지정 합니다.

| 값 | 의미 |
|---|---|
| **기본값** | 기본값입니다. 렌더러에 의해 결정 되는 자르기 동작입니다. |
| **없음** | 이미지가 잘리지 않습니다. |
| **Circle** | 이미지가 원 모양으로 잘립니다. |


### <a name="adaptiveimagealign"></a>AdaptiveImageAlign
이미지의 가로 맞춤을 지정 합니다.

| 값 | 의미 |
|---|---|
| **기본값** | 기본값입니다. 렌더러에 의해 결정 되는 맞춤 동작입니다. |
| **Stretch** | 이미지가 배치 된 위치에 따라 사용 가능한 너비를 채우도록 이미지를 확장 하 고 잠재적으로 사용 가능한 높이도 늘립니다. |
| **비어** | 이미지를 왼쪽에 맞추고 이미지를 네이티브 해상도로 표시 합니다. |
| **중심과** | 이미지를 가로로 가운데에 맞추고 이미지를 네이티브 해상도로 displayign 합니다. |
| **오른쪽** | 이미지를 오른쪽에 맞추고 이미지를 네이티브 해상도로 표시 합니다. |


## <a name="adaptivegroup"></a>AdaptiveGroup
기념일 업데이트: 그룹 의미 체계는 그룹의 콘텐츠가 전체로 표시 되어야 하는지 아니면 표시 되지 않을 지 여부를 확인 합니다. 그룹을 사용 하 여 여러 열을 만들 수도 있습니다.

| 속성 | 유형 | 필수 |설명 |
|---|---|---|---|
| **Children** | IList<[AdaptiveSubgroup](#adaptivesubgroup)> | false | 하위 그룹은 세로 열로 표시 됩니다. 하위 그룹을 사용 하 여 AdaptiveGroup 내에 콘텐츠를 제공 해야 합니다. |


## <a name="adaptivesubgroup"></a>AdaptiveSubgroup
기념일 업데이트: 하위 그룹은 텍스트와 이미지를 포함할 수 있는 세로 세로 막대입니다.

| 속성 | 유형 | 필수 |설명 |
|---|---|---|---|
| **Children** | IList<[IAdaptiveSubgroupChild](#iadaptivesubgroupchild)> | false | [AdaptiveText](#adaptivetext) 및 [AdaptiveImage](#adaptiveimage) 는 하위 그룹의 유효한 하위 그룹입니다. |
| **HintWeight** | int? | false | 다른 하위 그룹을 기준으로 가중치를 지정 하 여이 하위 그룹 열의 너비를 제어 합니다. |
| **HintTextStacking** | [AdaptiveSubgroupTextStacking](#adaptivesubgrouptextstacking) | false | 이 하위 그룹 콘텐츠의 세로 맞춤을 제어 합니다. |


### <a name="iadaptivesubgroupchild"></a>IAdaptiveSubgroupChild
하위 그룹 자식의 마커 인터페이스입니다.

| 구현 |
| --- |
| [AdaptiveText](#adaptivetext) |
| [AdaptiveImage](#adaptiveimage) |


### <a name="adaptivesubgrouptextstacking"></a>AdaptiveSubgroupTextStacking
TextStacking 콘텐츠의 세로 맞춤을 지정 합니다.

| 값 | 의미 |
|---|---|
| **기본값** | 기본값입니다. 렌더러는 기본 세로 맞춤을 자동으로 선택 합니다. |
| **상위** | 세로를 위쪽에 맞춥니다. |
| **중심과** | 세로를 가운데에 맞춥니다. |
| **아래쪽** | 세로를 아래쪽에 맞춥니다. |


## <a name="adaptiveprogressbar"></a>AdaptiveProgressBar
크리에이터 업데이트의 새로운 작업: 진행률 표시줄입니다. 데스크톱 알림을, 빌드 15063 이상 에서만 지원 됩니다.

| 속성 | 유형 | 필수 | 설명 |
|---|---|---|---|
| **제목** | string 또는 [Bindablestring](#bindablestring) | false | 선택적 제목 문자열을 가져오거나 설정 합니다. 데이터 바인딩을 지원 합니다. |
| **값** | double 또는 [AdaptiveProgressBarValue](#adaptiveprogressbarvalue) 또는 [BindableProgressBarValue](#bindableprogressbarvalue) | false | 진행률 표시줄의 값을 가져오거나 설정 합니다. 데이터 바인딩을 지원 합니다. 기본값은 0입니다. |
| **ValueStringOverride** | string 또는 [Bindablestring](#bindablestring) | false | 기본 백분율 문자열 대신 표시할 선택적 문자열을 가져오거나 설정 합니다. 제공 되지 않은 경우 "70%"와 같은 항목이 표시 됩니다. |
| **상태** | string 또는 [Bindablestring](#bindablestring) | true | 왼쪽의 진행률 표시줄 아래에 표시 되는 상태 문자열 (필수)을 가져오거나 설정 합니다. 이 문자열은 "다운로드 중 ..."과 같은 작업의 상태를 반영 해야 합니다. 또는 "설치 중 ..." |


### <a name="adaptiveprogressbarvalue"></a>AdaptiveProgressBarValue
진행률 표시줄의 값을 나타내는 클래스입니다.

| 속성 | 유형 | 필수 | 설명 |
|---|---|---|---|
| **값** | double | false | 완료율을 나타내는 값 (0.0-1.0)을 가져오거나 설정 합니다. |
| **IsIndeterminate 안 됨** | bool | false | 진행률 표시줄이 비활성화 되어 있는지 여부를 나타내는 값을 가져오거나 설정 합니다. True 이면 **값** 이 무시 됩니다. |


### <a name="bindableprogressbarvalue"></a>BindableProgressBarValue
바인딩 가능한 진행률 표시줄 값입니다.

| 속성 | 유형 | 필수 | 설명 |
|---|---|---|---|
| **BindingName** | 문자열 | true | 바인딩 데이터 값에 매핑되는 이름을 가져오거나 설정 합니다. |


## <a name="toastgenericapplogo"></a>To Genericogo
앱 로고 대신 표시 될 로고입니다.

| 속성 | 유형 | 필수 |설명 |
|---|---|---|---|
| **원본** | 문자열 | true | 이미지에 대 한 URL입니다. appdata 및 http가 지원 됩니다. Http 이미지는 200 KB이 하 여야 합니다. |
| **HintCrop** | [ToastGenericAppLogoCrop](#toastgenericapplogocrop) | false | 이미지를 잘라내는 방법을 지정 합니다. |
| **AlternateText** | 문자열 | false | 내게 필요한 옵션을 위해 사용 되는 이미지를 설명 하는 대체 텍스트입니다. |
| **AddImageQuery** | bool? | false | Windows가 알림 알림에서 제공 된 이미지 URL에 쿼리 문자열을 추가할 수 있게 하려면 "true"로 설정 합니다. 서버에서 이미지를 호스팅하고 쿼리 문자열을 기반으로 하는 이미지 변형을 검색 하거나 쿼리 문자열을 무시 하 고 쿼리 문자열 없이 지정 된 대로 이미지를 반환 하 여 쿼리 문자열을 처리할 수 있는 경우이 특성을 사용 합니다. 이 쿼리 문자열은 크기 조정, 대비 설정 및 언어를 지정 합니다. 예를 들어, 알림에 지정 된 "www.website.com/images/hello.png" 값은 "www.website.com/images/hello.png? ms-scale = 100&&m a s a s s-a s a-s"가 됩니다. |


### <a name="toastgenericapplogocrop"></a>ToastGenericAppLogoCrop
앱 로고 이미지의 자르기를 제어 합니다.

| 값 | 의미 |
|---|---|
| **기본값** | 자르기는 렌더러의 기본 동작을 사용 합니다. |
| **없음** | 이미지가 잘리지 않고 사각형으로 표시 됩니다. |
| **Circle** | 이미지가 원으로 잘립니다. |


## <a name="toastgenericheroimage"></a>ToastGenericHeroImage
알림 및 작업 센터 내에 표시 되는 추천 "주인공" 이미지입니다.

| 속성 | 유형 | 필수 |설명 |
|---|---|---|---|
| **원본** | 문자열 | true | 이미지에 대 한 URL입니다. appdata 및 http가 지원 됩니다. Http 이미지는 200 KB이 하 여야 합니다. |
| **AlternateText** | 문자열 | false | 내게 필요한 옵션을 위해 사용 되는 이미지를 설명 하는 대체 텍스트입니다. |
| **AddImageQuery** | bool? | false | Windows가 알림 알림에서 제공 된 이미지 URL에 쿼리 문자열을 추가할 수 있게 하려면 "true"로 설정 합니다. 서버에서 이미지를 호스팅하고 쿼리 문자열을 기반으로 하는 이미지 변형을 검색 하거나 쿼리 문자열을 무시 하 고 쿼리 문자열 없이 지정 된 대로 이미지를 반환 하 여 쿼리 문자열을 처리할 수 있는 경우이 특성을 사용 합니다. 이 쿼리 문자열은 크기 조정, 대비 설정 및 언어를 지정 합니다. 예를 들어, 알림에 지정 된 "www.website.com/images/hello.png" 값은 "www.website.com/images/hello.png? ms-scale = 100&&m a s a s s-a s a-s"가 됩니다. |


## <a name="toastgenericattributiontext"></a>ToastGenericAttributionText
알림 메시지의 아래쪽에 표시 되는 특성 텍스트입니다.

| 속성 | 유형 | 필수 | 설명 |
|---|---|---|---|
| **Text** | 문자열 | true | 표시할 텍스트입니다. |
| **언어** | 문자열 | false | 지역화 된 리소스를 사용 하는 경우 "en-us" 또는 "fr-fr"와 같은 BCP-47 언어 태그로 지정 된 시각적 페이로드의 대상 로캘입니다. 지정 하지 않으면 시스템 로캘이 대신 사용 됩니다. |


## <a name="itoastactions"></a>Ito Actions
알림 작업/입력에 대 한 마커 인터페이스입니다.

| 구현 |
| --- |
| [ToastActionsCustom](#toastactionscustom) |
| [ToastActionsSnoozeAndDismiss](#toastactionssnoozeanddismiss) |


## <a name="toastactionscustom"></a>ToastActionsCustom
*[Ito actions](#itoastactions) 를 구현 합니다.*

단추, 텍스트 상자 및 선택 입력과 같은 컨트롤을 사용 하 여 사용자 지정 작업 및 입력을 만듭니다.

| 속성 | 유형 | 필수 | 설명 |
|---|---|---|---|
| **입력** | IList<[Ito input](#itoastinput)> | false | 텍스트 상자 및 선택 입력과 같은 입력 최대 5 개의 입력만 허용 됩니다. |
| **단추** | IList<[Itoo 단추](#itoastbutton)> | false | 단추는 모든 입력 후 (또는 단추가 빠른 회신 단추로 사용 되는 경우 입력 옆에 표시 됨)에 표시 됩니다. 최대 5 개의 단추만 허용 됩니다 (또는 상황에 맞는 메뉴 항목이 있는 경우 더 적음). |
| **ContextMenuItems** | IList<[To Contextmenuitem](#toastcontextmenuitem)> | false | 기념일 업데이트: 사용자가 알림을 마우스 오른쪽 단추로 클릭 하는 경우 추가 작업을 제공 하는 사용자 지정 상황에 맞는 메뉴 항목입니다. 최대 5 개의 단추와 상황에 맞는 메뉴 항목만 *결합할*수 있습니다. |


## <a name="itoastinput"></a>Ito Input
알림 입력에 대 한 마커 인터페이스입니다.

| 구현 |
| --- |
| [To Textbox](#toasttextbox) |
| [To Selectionbox](#toastselectionbox) |


## <a name="toasttextbox"></a>To Textbox
*[Ito input](#itoastinput) 을 구현 합니다.*

사용자가 텍스트를 입력할 수 있는 텍스트 상자 컨트롤입니다.

| 속성 | 유형 | 필수 | 설명 |
|---|---|---|---|
| **ID** | 문자열 | true | Id는 필수 이며, 나중에 앱이 사용 하는 id/값의 키-값 쌍으로 사용자 입력 텍스트를 매핑하는 데 사용 됩니다. |
| **제목** | 문자열 | false | 텍스트 상자 위에 표시할 제목 텍스트입니다. |
| **PlaceholderContent** | 문자열 | false | 사용자가 텍스트를 아직 입력 하지 않은 경우 텍스트 상자에 표시할 자리 표시자 텍스트입니다. |
| **DefaultInput** | 문자열 | false | 텍스트 상자에 삽입할 초기 텍스트입니다. 빈 텍스트 상자에 대해서는이 값을 비워 둡니다. |


## <a name="toastselectionbox"></a>To Selectionbox
*[Ito input](#itoastinput) 을 구현 합니다.*

사용자가 옵션 드롭다운 목록에서 선택할 수 있는 선택 상자 컨트롤입니다.

| 속성 | 유형 | 필수 | 설명 |
|---|---|---|---|
| **ID** | 문자열 | true | Id는 필수입니다. 사용자가이 항목을 선택 하면이 Id가 선택한 선택 항목을 나타내는 앱의 코드로 다시 전달 됩니다. |
| **콘텐츠** | 문자열 | true | 콘텐츠는 필수 이며 선택 항목에 표시 되는 문자열입니다. |


### <a name="toastselectionboxitem"></a>To Selectionboxitem
선택 상자 항목 (사용자가 드롭다운 목록에서 선택할 수 있는 항목)입니다.

| 속성 | 유형 | 필수 | 설명 |
|---|---|---|---|
| **ID** | 문자열 | true | Id는 필수 이며, 나중에 앱이 사용 하는 id/값의 키-값 쌍으로 사용자 입력 텍스트를 매핑하는 데 사용 됩니다. |
| **제목** | 문자열 | false | 선택 상자 위에 표시할 제목 텍스트입니다. |
| **DefaultSelectionBoxItemId** | 문자열 | false | 이는 기본적으로 선택 되는 항목을 제어 하 고 [Toastselectionboxitem](#toastselectionboxitem)의 Id 속성을 참조 합니다. 이 기능을 제공 하지 않으면 기본 선택 항목이 비어 있습니다 (사용자에 게 표시 되지 않음). |
| **항목** | IList<[To Selectionboxitem](#toastselectionboxitem)> | false | 사용자가이 SelectionBox 상자에서 선택할 수 있는 선택 항목입니다. 5 개의 항목만 추가할 수 있습니다. |


## <a name="itoastbutton"></a>Itoo 단추
알림 단추에 대 한 마커 인터페이스입니다.

| 구현 |
| --- |
| [To...](#toastbutton) |
| [Toa 단추 다시 알림](#toastbuttonsnooze) |
| [To Button해제](#toastbuttondismiss) |


## <a name="toastbutton"></a>To...
*[Itoo 단추](#itoastbutton) 를 구현 합니다.*

사용자가 클릭할 수 있는 단추입니다.

| 속성 | 유형 | 필수 | 설명 |
|---|---|---|---|
| **콘텐츠** | 문자열 | true | 필수 사항입니다. 단추에 표시할 텍스트입니다. |
| **인수** | 문자열 | true | 필수 사항입니다. 사용자가이 단추를 클릭 하면 나중에 앱에서 받을 수 있는 응용 프로그램 정의 인수의 문자열입니다. |
| **ActivationType** | [ToastActivationType](#toastactivationtype) | false | 이 단추가 클릭 될 때 사용할 활성화 유형을 제어 합니다. 기본값은 전경입니다. |
| **ActivationOptions** | [ToastActivationOptions](#toastactivationoptions) | false | 작성자의 새로운 작업 업데이트: 알림 단추 활성화와 관련 된 추가 옵션을 가져오거나 설정 합니다. |


### <a name="toastactivationtype"></a>ToastActivationType
사용자가 특정 작업과 상호 작용할 때 사용 되는 활성화 유형을 결정 합니다.

| 값 | 의미 |
|---|---|
| **전경** | 기본값입니다. 포그라운드 앱이 시작 됩니다. |
| **배경** | 해당 하는 백그라운드 작업 (모든 항목을 설정 했다고 가정)이 트리거되고 사용자를 방해 하지 않고 백그라운드에서 코드를 실행할 수 있습니다 (예: 사용자의 빠른 회신 메시지 보내기). |
| **프로토콜** | 프로토콜 활성화를 사용 하 여 다른 앱을 시작 합니다. |


### <a name="toastactivationoptions"></a>ToastActivationOptions
크리에이터 업데이트의 새로운 옵션: 활성화와 관련 된 추가 옵션입니다.

| 속성 | 유형 | 필수 | 설명 |
|---|---|---|---|
| **AfterActivationBehavior** | [ToastAfterActivationBehavior](#toastafteractivationbehavior) | false | 새 사용자가 다음의 새 기능 작성자 업데이트: 사용자가이 작업을 호출할 때 toast에서 사용 해야 하는 동작을 가져오거나 설정 합니다. 이 기능은 바탕 화면, 즉 [Toa 단추](#toastbutton) 및 [To contextmenuitem](#toastcontextmenuitem)에 대해서만 작동 합니다. |
| **ProtocolActivationTargetApplicationPfn** | 문자열 | false | *ToastActivationType*를 사용 하는 경우 대상 pfn을 선택적으로 지정할 수 있습니다. 따라서 동일한 프로토콜 uri를 처리 하도록 여러 앱을 등록 했는지 여부에 관계 없이 원하는 앱이 항상 시작 됩니다. |


### <a name="toastafteractivationbehavior"></a>ToastAfterActivationBehavior
사용자가 알림 작업을 수행할 때 toast에서 사용 해야 하는 동작을 지정 합니다.

| 값 | 의미 |
|---|---|
| **기본값** | 기본 동작입니다. 사용자가 알림 메시지에 대 한 작업을 수행 하면 알림이 해제 됩니다. |
| **PendingUpdate** | 사용자가 알림 단추를 클릭 하면 알림이 "보류 중인 업데이트" 시각적 상태에 그대로 유지 됩니다. 사용자가이 "보류 중인 업데이트" 시각적 상태를 너무 오랫동안 표시 하지 않도록 백그라운드 작업에서 알림 메시지를 즉시 업데이트 해야 합니다. |


## <a name="toastbuttonsnooze"></a>Toa 단추 다시 알림
*[Itoo 단추](#itoastbutton) 를 구현 합니다.*

알림의 snoozing을 자동으로 처리 하는 시스템 처리 다시 알림 단추입니다.

| 속성 | 유형 | 필수 | 설명 |
|---|---|---|---|
| **CustomContent** | 문자열 | false | 지역화 된 기본 "다시 알림" 텍스트를 재정의 하는 단추에 표시 되는 선택적 사용자 지정 텍스트입니다. |


## <a name="toastbuttondismiss"></a>To Button해제
*[Itoo 단추](#itoastbutton) 를 구현 합니다.*

클릭 하면 알림을 해제 하는 시스템 처리 해제 단추입니다.

| 속성 | 유형 | 필수 | 설명 |
|---|---|---|---|
| **CustomContent** | 문자열 | false | 지역화 된 기본 "해제" 텍스트를 재정의 하는 단추에 표시 되는 선택적 사용자 지정 텍스트입니다. |


## <a name="toastactionssnoozeanddismiss"></a>ToastActionsSnoozeAndDismiss
* [Ito actions](#itoastactions) 구현

자동으로 다시 알림 간격에 대 한 선택 상자를 자동으로 생성 하 고, 자동으로 지역화 된 모든 자동으로 지역화 된 snoozing 논리를 시스템에서 자동으로 처리 합니다.

| 속성 | 유형 | 필수 | 설명 |
|---|---|---|---|
| **ContextMenuItems** | IList<[To Contextmenuitem](#toastcontextmenuitem)> | false | 기념일 업데이트: 사용자가 알림을 마우스 오른쪽 단추로 클릭 하는 경우 추가 작업을 제공 하는 사용자 지정 상황에 맞는 메뉴 항목입니다. 최대 5 개의 항목만 포함할 수 있습니다. |


## <a name="toastcontextmenuitem"></a>To Contextmenuitem
상황에 맞는 메뉴 항목 항목입니다.

| 속성 | 유형 | 필수 | 설명 |
|---|---|---|---|
| **콘텐츠** | 문자열 | true | 필수 사항입니다. 표시할 텍스트입니다. |
| **인수** | 문자열 | true | 필수 사항입니다. 사용자가 메뉴 항목을 클릭할 때 활성화 되 면 앱에서 나중에 검색할 수 있는 응용 프로그램 정의 인수의 문자열입니다. |
| **ActivationType** | [ToastActivationType](#toastactivationtype) | false | 이 메뉴 항목을 클릭할 때 사용할 활성화 유형을 제어 합니다. 기본값은 전경입니다. |
| **ActivationOptions** | [ToastActivationOptions](#toastactivationoptions) | false | 크리에이터 업데이트의 새로운 항목: 알림 상황에 맞는 메뉴 항목 활성화와 관련 된 추가 옵션입니다. |


## <a name="toastaudio"></a>To... 오디오
알림 메시지를 받을 때 재생할 오디오를 지정 합니다.

| 속성 | 유형 | 필수 | 설명 |
|---|---|---|---|
| **소스** | uri | false | 기본 사운드 대신 재생할 미디어 파일입니다. Ms-appx 및 ms appdata만 지원 됩니다. |
| **실행** | boolean | false | 알림이 표시 되는 동안 소리가 반복 되 면 true로 설정 합니다. 한 번만 재생 하려면 false (기본값)입니다. |
| **무음** | boolean | false | 소리를 음소거 하려면 True로 설정 합니다. false 이면 알림 소리를 재생할 수 있습니다 (기본값). |


## <a name="toastheader"></a>ToastHeader
크리에이터 업데이트의 새로운 기능: 작업 센터 내에서 여러 알림을 그룹화 하는 사용자 지정 헤더입니다.

| 속성 | 유형 | 필수 | 설명 |
|---|---|---|---|
| **ID** | 문자열 | true | 이 헤더를 고유 하 게 식별 하는 개발자가 만든 식별자입니다. 두 알림이 동일한 헤더 id를 가진 경우에는 Action Center의 동일한 헤더 아래에 표시 됩니다. |
| **제목** | 문자열 | true | 헤더의 제목입니다. |
| **인수**| 문자열 | true | 사용자가이 헤더를 클릭할 때 앱에 반환 되는 개발자 정의 인수의 문자열을 가져오거나 설정 합니다. null일 수 없습니다. |
| **ActivationType** | [ToastActivationType](#toastactivationtype) | false | 이 헤더가 클릭 될 때 사용할 활성화 형식을 가져오거나 설정 합니다. 기본값은 전경입니다. 포그라운드 및 프로토콜만 지원 됩니다. |
| **ActivationOptions** | [ToastActivationOptions](#toastactivationoptions) | false | 알림 헤더 활성화와 관련 된 추가 옵션을 가져오거나 설정 합니다. |


## <a name="related-topics"></a>관련 항목

* [빠른 시작: 로컬 알림 보내기 및 활성화 처리](/archive/blogs/tiles_and_toasts/quickstart-sending-a-local-toast-notification-and-handling-activations-from-it-windows-10)
* [GitHub의 알림 라이브러리](https://github.com/windows-toolkit/WindowsCommunityToolkit/tree/dev/Notifications)
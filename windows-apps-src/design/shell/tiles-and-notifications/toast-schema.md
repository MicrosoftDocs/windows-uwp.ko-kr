---
author: andrewleader
Description: The following article describes all of the properties and elements within toast content.
title: 알림 콘텐츠 스키마
ms.assetid: 7CBC3BD5-D9C3-4781-8BD0-1F28039E1FA8
label: Toast content schema
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ad1700d58ab3568aa3aefa46b5950d0a8bf3c320
ms.sourcegitcommit: a160b91a554f8352de963d9fa37f7df89f8a0e23
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/21/2018
ms.locfileid: "4129486"
---
# <a name="toast-content-schema"></a>알림 콘텐츠 스키마

 

다음은 알림 콘텐츠 내의 모든 속성과 요소에 대한 설명입니다.

[Notifications library(알림 라이브러리)](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) 대신 원시 XML을 사용하려면 [XML schema(XML 스키)마](toast-xml-schema.md)를 참조하세요.

[ToastContent](#toastcontent)
* [ToastVisual](#toastvisual)
  * [ToastBindingGeneric](#toastbindinggeneric)
    * [IToastBindingGenericChild](#itoastbindinggenericchild)
    * [ToastGenericAppLogo](#toastgenericapplogo)
    * [ToastGenericHeroImage](#toastgenericheroimage)
    * [ToastGenericAttributionText](#toastgenericattributiontext)
* [IToastActions](#itoastactions)
* [ToastAudio](#toastaudio)
* [ToastHeader](#toastheader)


## <a name="toastcontent"></a>ToastContent
ToastContent는 시각적 개체, 작업 및 오디오를 포함한 알림의 콘텐츠를 설명하는 최상위 개체입니다.

| 속성 | 형식 | 필수 | 설명 |
|---|---|---|---|
| **Launch**| string | false | 알림에 의해 활성화될 때 응용 프로그램에 전달되는 문자열입니다. 이 문자열의 형식과 콘텐츠는 앱에서 앱 용도에 맞게 정의됩니다. 사용자가 알림을 탭하거나 클릭하여 연결된 앱을 시작하면 시작 문자열은 기본 방법으로 앱을 시작하는 대신 사용자에게 알림 콘텐츠에 관련된 뷰를 표시할 수 있도록 하는 앱 컨텍스트를 제공합니다. |
| **Visual** | [ToastVisual](#toastvisual) | true | 알림 메시지의 시각적 개체 부분을 설명합니다. |
| **Actions** | [IToastActions](#itoastactions) | false | 필요에 따라 단추와 입력으로 사용자 지정 작업을 만듭니다. |
| **Audio** | [ToastAudio](#toastaudio) | false | 알림 메시지의 오디오 부분을 설명합니다. |
| **ActivationType** | [ToastActivationType](#toastactivationtype) | false | 사용자가 이 알림의 본문을 클릭할 때 사용할 활성화 유형을 지정합니다. |
| **ActivationOptions** | [ToastActivationOptions](#toastactivationoptions) | false | 크리에이터 업데이트의 새로운 기능: 알림 메시지의 활성화와 관련된 추가 옵션입니다. |
| **Scenario** | [ToastScenario](#toastscenario) | false | 알람 또는 미리 알림과 같이 알림을 사용할 시나리오를 선언합니다. |
| **DisplayTimestamp** | DateTimeOffset? | false | 크리에이터 업데이트의 새로운 기능: Windows 플랫폼에서 메시지를 받은 시간이 아닌 알림 내용이 실제로 배달된 시간을 나타내는 사용자 지정 타임스탬프로 기본 타임스탬프를 재정의합니다. |
| **Header** | [ToastHeader](#toastheader) | false | 크리에이터 업데이트의 새로운 기능: 알림에 사용자 지정 헤더를 추가합니다. |


### <a name="toastscenario"></a>ToastScenario
알림이 나타내는 시나리오를 지정합니다.

| 값 | 의미 |
|---|---|
| **Default** | 일반 알림 동작입니다. |
| **Reminder** | 미리 알림입니다. 미리 확장되어 표시되고 해제할 때까지 사용자의 화면에 남아 있습니다. |
| **Alarm** | 알람 알림입니다. 미리 확장되어 표시되고 해제할 때까지 사용자의 화면에 남아 있습니다. 오디오가 기본적으로 반복되고 알람 오디오를 사용합니다. |
| **IncomingCall** | 수신 전화 알림입니다. 특별한 호출 형식으로 미리 확장되어 표시되고 해제할 때까지 사용자의 화면에 남아 있습니다. 오디오가 기본적으로 반복되고 벨소리 오디오를 사용합니다. |


## <a name="toastvisual"></a>ToastVisual
알림의 시각적 개체 부분에는 텍스트, 이미지, 적응형 콘텐츠 등이 들어 있는 바인딩이 포함됩니다.

| 속성 | 형식 | 필수 | 설명 |
|---|---|---|---|
| **BindingGeneric** | [ToastBindingGeneric](#toastbindinggeneric) | true | 모든 장치에서 렌더링할 수 있는 일반 알림 바인딩입니다. 이 바인딩은 필수이며 null일 수 없습니다. |
| **BaseUri** | URI | false | 이미지 소스 특성에 상대 URL과 결합되는 기본 기준 URL입니다. |
| **AddImageQuery** | bool? | false | Windows에서 알림 메시지에 제공된 이미지 URL에 쿼리 문자열을 추가할 수 있게 하려면 "true"로 설정합니다. 서버에서 이미지를 호스트하고, 쿼리 문자열을 기반으로 이미지 변형을 검색하거나 쿼리 문자열을 무시하고 쿼리 문자열 없이 지정된 이미지를 반환하여 쿼리 문자열을 처리할 수 있는 경우 이 특성을 사용합니다. 이 쿼리 문자열은 배율, 대비 설정 및 언어를 지정합니다. 예를 들어, 알림에 지정된 "www.website.com/images/hello.png"의 값은 "www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us"가 됩니다. |
| **Language**| string | false | "en-US" 또는 "fr-FR"과 같이 BCP-47 언어 태그로 지정된 지역화된 리소스를 사용할 때 시각적 페이로드의 대상 로캘입니다. 이 로캘은 바인딩 또는 텍스트에 지정된 로캘로 재정의됩니다. 제공되지 않을 경우 시스템 로캘이 대신 사용됩니다. |


## <a name="toastbindinggeneric"></a>ToastBindingGeneric
일반 바인딩이 알림의 기본 바인딩이며 여기에 텍스트, 이미지, 적응형 콘텐츠 등을 지정합니다.

| 속성 | 형식 | 필수 | 설명 |
|---|---|---|---|
| **Children** | IList<[IToastBindingGenericChild](#itoastbindinggenericchild)> | false | 텍스트, 이미지 및 그룹을 포함할 수 있는 알림의 본문 내용입니다(1주년 업데이트에 추가됨). 텍스트 요소가 다른 요소 앞에 와야 하며 3개의 텍스트 요소만 지원됩니다. 텍스트 요소가 다른 요소 뒤에 위치할 경우 맨 위로 빠지거나 삭제됩니다. 끝으로 HintStyle과 같은 특성 텍스트 속성은 루트 자식 텍스트 요소에서 지원하지 않으며 AdaptiveSubgroup 내에서만 작동합니다. 1주년 업데이트 없이 장치에 AdaptiveGroup을 사용하는 경우 그룹 콘텐츠가 삭제되기만 합니다. |
| **AppLogoOverride** | [ToastGenericAppLogo](#toastgenericapplogo) | false | 앱 로고를 재정의하기 위한 선택적 로고입니다. |
| **HeroImage** | [ToastGenericHeroImage](#toastgenericheroimage) | false | 알림 센터 내와 알림에 표시되는 선택적 추천 "영웅" 이미지입니다. |
| **Attribution** | [ToastGenericAttributionText](#toastgenericattributiontext) | false | 알림 메시지 아래쪽에 표시되는 선택적 특성 텍스트입니다. |
| **BaseUri** | URI | false | 이미지 소스 특성에 상대 URL과 결합되는 기본 기준 URL입니다. |
| **AddImageQuery** | bool? | false | Windows에서 알림 메시지에 제공된 이미지 URL에 쿼리 문자열을 추가할 수 있게 하려면 "true"로 설정합니다. 서버에서 이미지를 호스트하고, 쿼리 문자열을 기반으로 이미지 변형을 검색하거나 쿼리 문자열을 무시하고 쿼리 문자열 없이 지정된 이미지를 반환하여 쿼리 문자열을 처리할 수 있는 경우 이 특성을 사용합니다. 이 쿼리 문자열은 배율, 대비 설정 및 언어를 지정합니다. 예를 들어, 알림에 지정된 "www.website.com/images/hello.png"의 값은 "www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us"가 됩니다. |
| **Language**| string | false | "en-US" 또는 "fr-FR"과 같이 BCP-47 언어 태그로 지정된 지역화된 리소스를 사용할 때 시각적 페이로드의 대상 로캘입니다. 이 로캘은 바인딩 또는 텍스트에 지정된 로캘로 재정의됩니다. 제공되지 않을 경우 시스템 로캘이 대신 사용됩니다. |


## <a name="itoastbindinggenericchild"></a>IToastBindingGenericChild
텍스트, 이미지, 그룹 등을 포함하는 알림 자식 요소의 마커 인터페이스입니다.

| 구현 |
| --- |
| [AdaptiveText](#adaptivetext) |
| [AdaptiveImage](#adaptiveimage) |
| [AdaptiveGroup](#adaptivegroup) |
| [AdaptiveProgressBar](#adaptiveprogressbar) |


## <a name="adaptivetext"></a>AdaptiveText
적응형 텍스트 요소입니다. 최상위 ToastBindingGeneric.Children에 배치할 경우 HintMaxLines만 적용됩니다. 그러나 그룹/하위 그룹의 자식 요소로 배치될 경우 전체 텍스트 스타일 지정이 지원됩니다.

| 속성 | 형식 | 필수 |설명 |
|---|---|---|---|
| **Text** | 문자열 또는 [BindableString](#bindablestring) | false | 표시할 텍스트입니다. 데이터 바인딩 지원은 크리에이터 업데이트에 추기되었지만 최상위 텍스트 요소에 대해서만 작동합니다. |
| **HintStyle** | [AdaptiveTextStyle](#adaptivetextstyle) | false | 스타일로 텍스트의 글꼴 크기, 두께 및 불투명도가 제어됩니다. 그룹/하위 그룹 내의 텍스트 요소에 대해서만 작동합니다. |
| **HintWrap** | bool? | false | 텍스트 배치를 활성화하려면 true로 설정합니다. 최상위 텍스트 요소가 이 속성을 무시하고 항상 줄 바꿈됩니다. 최상위 텍스트 요소에 대해 줄 바꿈을 비활성화하려면 HintMaxLines = 1을 사용합니다. 그룹/하위 그룹 내의 텍스트 요소는 줄 바꿈에 대해 기본적으로 false로 설정됩니다. |
| **HintMaxLines** | int? | false | 텍스트 요소가 표시할 수 있는 최대 줄 수입니다. |
| **HintMinLines** | int? | false | 텍스트 요소가 표시해야 하는 최소 줄 수입니다. 그룹/하위 그룹 내의 텍스트 요소에 대해서만 작동합니다. |
| **HintAlign** | [AdaptiveTextAlign](#adaptivetextalign) | false | 텍스트의 가로 맞춤입니다. 그룹/하위 그룹 내의 텍스트 요소에 대해서만 작동합니다. |
| **Language** | string | false | "en-US" 또는 "fr-FR"과 같이 BCP-47 언어 태그로 지정된 XML 페이로드의 대상 로캘입니다. 여기에 지정된 로캘이 바인딩이나 시각적 개체의 로캘을 비롯하여 지정된 다른 로캘을 재정의합니다. 이 값이 리터럴 문자열인 경우 이 특성은 기본적으로 사용자의 UI 언어로 설정됩니다. 이 값이 문자열 참조인 경우 문자열 확인 시 이 특성은 기본적으로 Windows 런타임에서 선택한 로캘로 설정됩니다. |


### <a name="bindablestring"></a>BindableString
문자열의 바인딩 값입니다.

| 속성 | 형식 | 필수 | 설명 |
|---|---|---|---|
| **BrandingName** | 문자열 | true | 바인딩 데이터 값에 매핑할 이름을 가져오거나 설정합니다. |


### <a name="adaptivetextstyle"></a>AdaptiveTextStyle
텍스트 스타일로 글꼴 크기, 두께 및 불투명도가 제어됩니다. 약한 불투명도는 60% 불투명입니다.

| 값 | 의미 |
|---|---|
| **Default** | 기본값입니다. 스타일은 렌더러로 결정됩니다. |
| **Caption** | 단락 글꼴 크기보다 작습니다. |
| **CaptionSubtle** | Caption과 동일하지만 불투명도가 약합니다. |
| **Body** | 단락 글꼴 크기입니다. |
| **BodySubtle** | Body와 동일하지만 불투명도가 약합니다. |
| **기본** | 단락 글꼴 크기이며 두께는 굵게입니다. 기본적으로 Body의 굵게 버전입니다. |
| **BaseSubtle** | Base와 동일하지만 불투명도가 약합니다. |
| **Subtitle** | H4 글꼴 크기입니다. |
| **SubtitleSubtle** | Subtitle과 동일하지만 불투명도가 약합니다. |
| **Title** | H3 글꼴 크기입니다. |
| **TitleSubtle** | Title과 동일하지만 불투명도가 약합니다. |
| **TitleNumeral** | Title과 동일하지만 안쪽 여백(위쪽/아래쪽)이 제거되었습니다. |
| **Subheader** | H2 글꼴 크기입니다. |
| **SubheaderSubtle** | Subheader와 동일하지만 불투명도가 약합니다. |
| **SubheaderNumeral** | Subheader와 동일하지만 안쪽 여백(위쪽/아래쪽)이 제거되었습니다. |
| **헤더** | H1 글꼴 크기입니다. |
| **HeaderSubtle** | Header와 동일하지만 불투명도가 약합니다. |
| **HeaderNumeral** | Header와 동일하지만 안쪽 여백(위쪽/아래쪽)이 제거되었습니다. |


### <a name="adaptivetextalign"></a>AdaptiveTextAlign
텍스트의 가로 맞춤을 제어합니다.

| 값 | 의미 |
|---|---|
| **Default** | 기본값입니다. 맞춤은 렌더러로 자동 결정됩니다. |
| **Auto** | 현재 언어와 문화로 결정된 맞춤입니다. |
| **Left** | 텍스트를 가로로 왼쪽에 맞춥니다. |
| **Center** | 텍스트를 가로로 가운데에 맞춥니다. |
| **Right** | 텍스트를 가로로 오른쪽에 맞춥니다. |


## <a name="adaptiveimage"></a>AdaptiveImage
인라인 이미지입니다.

| 속성 | 형식 | 필수 |설명 |
|---|---|---|---|
| **Source** | string | true | 이미지에 대한 URL입니다. ms-appx, ms-appdata 및 http가 지원됩니다. Fall Creators Update에서 일반 연결의 경우 웹 이미지는 최대 3MB이고 데이터 통신 연결의 경우 1MB입니다. Fall Creators Update를 아직 실행하지 않는 디바이스에서 웹 이미지는 200KB 이하여야 합니다. |
| **HintCrop** | [AdaptiveImageCrop](#adaptiveimagecrop) | false | 1주년 업데이트의 새로운 기능: 이미지의 원하는 자르기를 제어합니다. |
| **HintRemoveMargin** | bool? | false | 기본적으로 그룹/하위 그룹 내 이미지의 주변 여백은 8px입니다. 이 속성을 true로 설정하여 이 여백을 제거할 수 있습니다. |
| **HintAlign** | [AdaptiveImageAlign](#adaptiveimagealign) | false | 이미지의 가로 맞춤입니다. 그룹/하위 그룹 내의 이미지에 대해서만 작동합니다. |
| **AlternateText** | string | false | 이미지를 설명하는 대체 텍스트로서 접근성 용도로 사용됩니다. |
| **AddImageQuery** | bool? | false | Windows에서 알림 메시지에 제공된 이미지 URL에 쿼리 문자열을 추가할 수 있게 하려면 "true"로 설정합니다. 서버에서 이미지를 호스트하고, 쿼리 문자열을 기반으로 이미지 변형을 검색하거나 쿼리 문자열을 무시하고 쿼리 문자열 없이 지정된 이미지를 반환하여 쿼리 문자열을 처리할 수 있는 경우 이 특성을 사용합니다. 이 쿼리 문자열은 배율, 대비 설정 및 언어를 지정합니다. 예를 들어, 알림에 지정된 "www.website.com/images/hello.png"의 값은 "www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us"가 됩니다. |


### <a name="adaptiveimagecrop"></a>AdaptiveImageCrop
이미지의 원하는 자르기를 지정합니다.

| 값 | 의미 |
|---|---|
| **Default** | 기본값입니다. 렌더러로 결정되는 자르기 동작입니다. |
| **None** | 이미지가 잘리지 않습니다. |
| **Circle** | 이미지가 원 모양으로 잘립니다. |


### <a name="adaptiveimagealign"></a>AdaptiveImageAlign
이미지에 대한 가로 맞춤을 지정합니다.

| 값 | 의미 |
|---|---|
| **Default** | 기본값입니다. 렌더러로 결정되는 대체 동작입니다. |
| **Stretch** | 이미지가 사용 가능한 너비(및 이미지가 어디에 배치되었는지에 따라 사용 가능한 높이)를 채우도록 늘어납니다. |
| **Left** | 이미지를 왼쪽에 맞추고 기본 해상도로 표시합니다. |
| **Center** | 이미지를 가로로 가운데에 맞추고 기본 해상도로 표시합니다. |
| **Right** | 이미지를 오른쪽에 맞추고 기본 해상도로 표시합니다. |


## <a name="adaptivegroup"></a>AdaptiveGroup
1주년 업데이트의 새로운 기능: 그룹이 의미상 그룹의 콘텐츠를 전체로 표시하거나 맞지 않을 경우 표시하지 않음을 확인합니다. 그룹은 또한 여러 열을 만들 수 있게 합니다.

| 속성 | 형식 | 필수 |설명 |
|---|---|---|---|
| **Children** | IList<[AdaptiveSubgroup](#adaptivesubgroup)> | false | 하위 그룹은 세로 열로 표시됩니다. 하위 그룹을 사용하여 AdaptiveGroup 내의 모든 콘텐츠를 제공합니다. |


## <a name="adaptivesubgroup"></a>AdaptiveSubgroup
1주년 업데이트의 새로운 기능: 하위 그룹은 텍스트와 이미지를 포함할 수 있는 세로 열입니다.

| 속성 | 형식 | 필수 |설명 |
|---|---|---|---|
| **Children** | IList<[IAdaptiveSubgroupChild](#iadaptivesubgroupchild)> | false | [AdaptiveText](#adaptivetext)와 [AdaptiveImage](#adaptiveimage)는 하위 그룹의 유효한 자식입니다. |
| **HintWeight** | int? | false | 다른 하위 그룹을 기준으로 두께를 지정하여 이 하위 그룹 열의 너비를 제어합니다. |
| **HintTextStacking** | [AdaptiveSubgroupTextStacking](#adaptivesubgrouptextstacking) | false | 이 하위 그룹의 콘텐츠의 세로 맞춤을 제어합니다. |


### <a name="iadaptivesubgroupchild"></a>IAdaptiveSubgroupChild
하위 그룹 자식의 마커 인터페이스입니다.

| 구현 |
| --- |
| [AdaptiveText](#adaptivetext) |
| [AdaptiveImage](#adaptiveimage) |


### <a name="adaptivesubgrouptextstacking"></a>AdaptiveSubgroupTextStacking
TextStacking은 콘텐츠의 세로 맞춤을 지정합니다.

| 값 | 의미 |
|---|---|
| **Default** | 기본값입니다. 렌더러가 자동으로 기본 세로 맞춤을 선택합니다. |
| **Top** | 위쪽에 세로로 맞춥니다. |
| **Center** | 가운데에 세로로 맞춥니다. |
| **Bottom** | 아래쪽에 세로로 맞춥니다. |


## <a name="adaptiveprogressbar"></a>AdaptiveProgressBar
Creators Update의 새로운 기능: 진행률 표시줄. 데스크톱 빌드 15063 이상의 알림에서만 지원됩니다.

| 속성 | 형식 | 필수 | 설명 |
|---|---|---|---|
| **제목** | 문자열 또는 [BindableString](#bindablestring) | false | 선택적 제목 문자열을 가져오거나 설정합니다. 데이터 바인딩을 지원합니다. |
| **값** | 이중 또는 [AdaptiveProgressBarValue](#adaptiveprogressbarvalue) 또는 [BindableProgressBarValue](#bindableprogressbarvalue) | false | 진행률 표시줄의 값을 가져오거나 설정합니다. 데이터 바인딩을 지원합니다. 기본값은 0입니다. |
| **ValueStringOverride** | 문자열 또는 [BindableString](#bindablestring) | false | 기본 백분율 문자열 대신 표시할 선택적 문자열을 가져오거나 설정합니다. 이것을 제공하지 않는 경우 '70%'와 같은 내용이 표시됩니다. |
| **Status** | 문자열 또는 [BindableString](#bindablestring) | true | 왼쪽의 진행률 표시줄 아래에 표시되는 상태 문자열(필수)을 가져오거나 설정합니다. 이 문자열은 '다운로드 중...' 또는 '설치 중...'과 같은 작업의 상태를 반영해야 합니다. |


### <a name="adaptiveprogressbarvalue"></a>AdaptiveProgressBarValue
진행률 표시줄의 값을 나타내는 클래스입니다.

| 속성 | 형식 | 필수 | 설명 |
|---|---|---|---|
| **값** | double | false | 완료 비율을 나타내는 (0.0-1.0) 값을 가져오거나 설정합니다. |
| **IsIndeterminate** | 부울 | false | 진행률 표시줄의 미확정 여부를 나타내는 값을 가져오거나 설정합니다. 이 값이 true이면 **Value**가 무시됩니다. |


### <a name="bindableprogressbarvalue"></a>BindableProgressBarValue
바인딩 가능 진행률 표시줄 값입니다.

| 속성 | 형식 | 필수 | 설명 |
|---|---|---|---|
| **BrandingName** | 문자열 | true | 바인딩 데이터 값에 매핑할 이름을 가져오거나 설정합니다. |


## <a name="toastgenericapplogo"></a>ToastGenericAppLogo
앱 로고 대신 표시할 로고입니다.

| 속성 | 형식 | 필수 |설명 |
|---|---|---|---|
| **Source** | string | true | 이미지에 대한 URL입니다. ms-appx, ms-appdata 및 http가 지원됩니다. Http 이미지는 크기가 200KB 이하여야 합니다. |
| **HintCrop** | [ToastGenericAppLogoCrop](#toastgenericapplogocrop) | false | 이미지를 어떻게 자를지를 지정합니다. |
| **AlternateText** | string | false | 이미지를 설명하는 대체 텍스트로서 접근성 용도로 사용됩니다. |
| **AddImageQuery** | bool? | false | Windows에서 알림 메시지에 제공된 이미지 URL에 쿼리 문자열을 추가할 수 있게 하려면 "true"로 설정합니다. 서버에서 이미지를 호스트하고, 쿼리 문자열을 기반으로 이미지 변형을 검색하거나 쿼리 문자열을 무시하고 쿼리 문자열 없이 지정된 이미지를 반환하여 쿼리 문자열을 처리할 수 있는 경우 이 특성을 사용합니다. 이 쿼리 문자열은 배율, 대비 설정 및 언어를 지정합니다. 예를 들어, 알림에 지정된 "www.website.com/images/hello.png"의 값은 "www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us"가 됩니다. |


### <a name="toastgenericapplogocrop"></a>ToastGenericAppLogoCrop
앱 로고 이미지의 자르기를 제어합니다.

| 값 | 의미 |
|---|---|
| **Default** | 자르기는 렌더러의 기본 동작을 사용합니다. |
| **None** | 이미지가 잘리지 않으며 사각형이 표시됩니다. |
| **Circle** | 이미지가 원으로 잘립니다. |


## <a name="toastgenericheroimage"></a>ToastGenericHeroImage
알림 센터 내와 알림에 표시되는 추천 "영웅" 이미지입니다.

| 속성 | 형식 | 필수 |설명 |
|---|---|---|---|
| **Source** | string | true | 이미지에 대한 URL입니다. ms-appx, ms-appdata 및 http가 지원됩니다. Http 이미지는 크기가 200KB 이하여야 합니다. |
| **AlternateText** | string | false | 이미지를 설명하는 대체 텍스트로서 접근성 용도로 사용됩니다. |
| **AddImageQuery** | bool? | false | Windows에서 알림 메시지에 제공된 이미지 URL에 쿼리 문자열을 추가할 수 있게 하려면 "true"로 설정합니다. 서버에서 이미지를 호스트하고, 쿼리 문자열을 기반으로 이미지 변형을 검색하거나 쿼리 문자열을 무시하고 쿼리 문자열 없이 지정된 이미지를 반환하여 쿼리 문자열을 처리할 수 있는 경우 이 특성을 사용합니다. 이 쿼리 문자열은 배율, 대비 설정 및 언어를 지정합니다. 예를 들어, 알림에 지정된 "www.website.com/images/hello.png"의 값은 "www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us"가 됩니다. |


## <a name="toastgenericattributiontext"></a>ToastGenericAttributionText
알림 메시지 아래쪽에 표시되는 특성 텍스트입니다.

| 속성 | 형식 | 필수 | 설명 |
|---|---|---|---|
| **Text** | string | true | 표시할 텍스트입니다. |
| **Language** | string | false | "en-US" 또는 "fr-FR"과 같이 BCP-47 언어 태그로 지정된 지역화된 리소스를 사용할 때 시각적 페이로드의 대상 로캘입니다. 제공되지 않을 경우 시스템 로캘이 대신 사용됩니다. |


## <a name="itoastactions"></a>IToastActions
알림 작업/입력의 마커 인터페이스입니다.

| 구현 |
| --- |
| [ToastActionsCustom](#toastactionscustom) |
| [ToastActionsSnoozeAndDismiss](#toastactionssnoozeanddismiss) |


## <a name="toastactionscustom"></a>ToastActionsCustom
*[IToastActions](#itoastactions) 구현*

단추, 텍스트 상자 및 선택 입력과 같은 컨트롤을 사용하여 사용자 지정 작업과 입력을 직접 만듭니다.

| 속성 | 형식 | 필수 | 설명 |
|---|---|---|---|
| **Inputs** | IList<[IToastInput](#itoastinput)> | false | 텍스트 상자 및 선택 입력과 같은 입력입니다. 최대 5개의 입력만 허용됩니다. |
| **Buttons** | IList<[IToastButton](#itoastbutton)> | false | 모든 입력 다음에 단추가 표시됩니다. 또는 단추가 빠른 회신 단추로 사용될 경우 입력 주위에 단추가 표시됩니다. 최대 5개 또는 그보다 적은 수(상황에 맞는 메뉴 항목도 있는 경우)의 단추만 허용됩니다. |
| **ContextMenuItems** | IList<[ToastContextMenuItem](#toastcontextmenuitem)> | false | 1주년 업데이트의 새로운 기능: 사용자가 마우스 오른쪽 단추로 알림을 클릭할 경우 추가 작업을 제공하는 상황에 맞는 사용자 지정 메뉴 항목입니다. 최대 5개의 단추와 상황에 맞는 메뉴 항목만 *결합*할 수 있습니다. |


## <a name="itoastinput"></a>IToastInput
알림 입력의 마커 인터페이스입니다.

| 구현 |
| --- |
| [ToastTextBox](#toasttextbox) |
| [ToastSelectionBox](#toastselectionbox) |


## <a name="toasttextbox"></a>ToastTextBox
*[IToastInput](#itoastinput) 구현*

사용자가 텍스트를 입력할 수 있는 텍스트 상자입니다.

| 속성 | 형식 | 필수 | 설명 |
|---|---|---|---|
| **Id** | string | true | ID는 필수이며 나중에 앱에서 사용하는 ID/값의 키-값 쌍에 사용자가 입력한 텍스트를 매핑하는 데 사용됩니다. |
| **Title** | string | false | 텍스트 상자 위에 표시할 제목 텍스트입니다. |
| **PlaceholderContent** | string | false | 사용자가 아직 아무 텍스트도 입력하지 않았을 때 텍스트 상자에 표시할 자리 표시자 텍스트입니다. |
| **DefaultInput** | string | false | 텍스트 상자에 넣을 초기 텍스트입니다. 빈 텍스트 상자의 경우 null로 둡니다. |


## <a name="toastselectionbox"></a>ToastSelectionBox
*[IToastInput](#itoastinput) 구현*

사용자가 옵션의 드롭다운 목록에서 선택할 수 있게 하는 선택 상자 컨트롤입니다.

| 속성 | 형식 | 필수 | 설명 |
|---|---|---|---|
| **Id** | string | true | Id는 필수입니다. 사용자가 이 항목을 선택한 경우 이 ID가 앱의 코드로 다시 전달되어 사용자의 선택 항목을 나타냅니다. |
| **Content** | string | true | Content는 필수이며 선택 항목에 표시되는 문자열입니다. |


### <a name="toastselectionboxitem"></a>ToastSelectionBoxItem
선택 상자 항목(사용자가 드롭다운 목록에서 선택할 수 있는 항목)입니다.

| 속성 | 형식 | 필수 | 설명 |
|---|---|---|---|
| **Id** | string | true | ID는 필수이며 나중에 앱에서 사용하는 ID/값의 키-값 쌍에 사용자가 입력한 텍스트를 매핑하는 데 사용됩니다. |
| **Title** | string | false | 선택 상자 위에 표시할 제목 텍스트입니다. |
| **DefaultSelectionBoxItemId** | string | false | 이 컨트롤은 기본적으로 선택되는 항목을 제어하며 [ToastSelectionBoxItem](#toastselectionboxitem)의 ID 속성을 가리킵니다. 이를 제공하지 않을 경우 기본 선택이 비어 있게 됩니다(사용자에게 아무 것도 표시되지 않음). |
| **Items** | IList<[ToastSelectionBoxItem](#toastselectionboxitem)> | false | 사용자가 이 SelectionBox에서 선택할 수 있는 선택 항목입니다. 5개의 항목만 추가할 수 있습니다. |


## <a name="itoastbutton"></a>IToastButton
알림 단추의 마커 인터페이스입니다.

| 구현 |
| --- |
| [ToastButton](#toastbutton) |
| [ToastButtonSnooze](#toastbuttonsnooze) |
| [ToastButtonDismiss](#toastbuttondismiss) |


## <a name="toastbutton"></a>ToastButton
*[IToastButton](#itoastbutton) 구현*

사용자가 클릭할 수 있는 단추입니다.

| 속성 | 형식 | 필수 | 설명 |
|---|---|---|---|
| **Content** | string | true | 필수입니다. 단추에 표시할 텍스트입니다. |
| **Arguments** | string | true | 필수입니다. 사용자가 이 단추를 클릭할 경우 나중에 앱에서 받을 앱에서 정의한 인수 문자열입니다. |
| **ActivationType** | [ToastActivationType](#toastactivationtype) | false | 클릭 시 이 단추에서 사용할 활성화 유형을 제어합니다. 기본값은 Foreground입니다. |
| **ActivationOptions** | [ToastActivationOptions](#toastactivationoptions) | false | Creators Update의 새로운 기능: 알림 단추의 활성화와 관련된 추가 옵션을 가져오거나 설정합니다. |


### <a name="toastactivationtype"></a>ToastActivationType
사용자가 특정 작업과 상호 작용할 때 사용할 활성화 유형을 결정합니다.

| 값 | 의미 |
|---|---|
| **Foreground** | 기본값입니다. 포그라운드 앱이 시작됩니다. |
| **Background** | 모든 것이 설정되었다는 가정하에 해당 배경 작업이 트리거되며, 사용자를 방해하지 않고 사용자의 빠른 응답 메시지를 보내는 것처럼 배경에서 코드를 실행할 수 있습니다. |
| **Protocol** | 프로토콜 활성화를 사용하여 다른 앱을 시작합니다. |


### <a name="toastactivationoptions"></a>ToastActivationOptions
크리에이터 업데이트의 새로운 기능: 활성화와 관련된 추가 옵션입니다.

| 속성 | 형식 | 필수 | 설명 |
|---|---|---|---|
| **AfterActivationBehavior** | [ToastAfterActivationBehavior](#toastafteractivationbehavior) | false | Fall Creators Update의 새로운 기능: 사용자가 이 작업을 호출할 때 알림이 사용해야 하는 동작을 가져오거나 설정합니다. 이 기능은 데스크톱 [ToastButton](#toastbutton) 및 [ToastContextMenuItem](#toastcontextmenuitem)에서만 작동합니다. |
| **ProtocolActivationTargetApplicationPfn** | string | false | *ToastActivationType.Protocol*을 사용할 경우 필요에 따라 대상 PFN을 지정할 수 있으므로 동일한 프로토콜 URI 처리를 위해 여러 앱이 등록되었는지 여부에 관계없이 원하는 앱이 항상 실행됩니다. |


### <a name="toastafteractivationbehavior"></a>ToastAfterActivationBehavior
알림에서 사용자가 작업을 수행할 때 알림이 사용해야 하는 동작을 지정합니다.

| 값 | 의미 |
|---|---|
| **Default** | 기본 동작입니다. 알림은 사용자가 알림에 대해 조치를 취하면 해제됩니다. |
| **PendingUpdate** | 사용자가 알림에서 단추를 클릭하면 알림은 '업데이트 보류 중' 시각적 상태로 남습니다. 백그라운드 작업에서 알림을 즉시 업데이트하여 사용자가 이 "업데이트 보류 중"시각적 상태를 너무 오랫동안 보지 않도록 해야 합니다. |


## <a name="toastbuttonsnooze"></a>ToastButtonSnooze
*[IToastButton](#itoastbutton) 구현*

다시 알림을 자동으로 처리하는 시스템 처리 다시 알림 단추입니다.

| 속성 | 형식 | 필수 | 설명 |
|---|---|---|---|
| **CustomContent** | string | false | 단추에 표시되는 선택적 사용자 지정 텍스트로서 지역화된 기본 "다시 알림" 텍스트를 재정의합니다. |


## <a name="toastbuttondismiss"></a>ToastButtonDismiss
*[IToastButton](#itoastbutton) 구현*

클릭 시 알림을 해제하는 시스템 처리 해제 단추입니다.

| 속성 | 형식 | 필수 | 설명 |
|---|---|---|---|
| **CustomContent** | string | false | 단추에 표시되는 선택적 사용자 지정 텍스트로서 지역화된 기본 "해제" 텍스트를 재정의합니다. |


## <a name="toastactionssnoozeanddismiss"></a>ToastActionsSnoozeAndDismiss
*[IToastActions](#itoastactions) 구현

다시 알림 간격과 다시 알림/해제 단추에 대한 선택 상자를 자동으로 생성합니다. 모두 자동으로 지역화되며 다시 알림 논리는 시스템에 의해 자동으로 처리됩니다.

| 속성 | 형식 | 필수 | 설명 |
|---|---|---|---|
| **ContextMenuItems** | IList<[ToastContextMenuItem](#toastcontextmenuitem)> | false | 1주년 업데이트의 새로운 기능: 사용자가 마우스 오른쪽 단추로 알림을 클릭할 경우 추가 작업을 제공하는 상황에 맞는 사용자 지정 메뉴 항목입니다. 최대 5개 항목만 가질 수 있습니다. |


## <a name="toastcontextmenuitem"></a>ToastContextMenuItem
상황에 맞는 메뉴 항목입니다.

| 속성 | 형식 | 필수 | 설명 |
|---|---|---|---|
| **Content** | string | true | 필수입니다. 표시할 텍스트입니다. |
| **Arguments** | string | true | 필수입니다. 사용자가 메뉴 항목을 클릭할 때 활성화 후 앱에서 나중에 검색할 수 있는 앱에서 정의한 인수 문자열입니다. |
| **ActivationType** | [ToastActivationType](#toastactivationtype) | false | 클릭 시 이 메뉴 항목에서 사용할 활성화 유형을 제어합니다. 기본값은 Foreground입니다. |
| **ActivationOptions** | [ToastActivationOptions](#toastactivationoptions) | false | 크리에이터 업데이트의 새로운 기능: 알림 상황에 맞는 메뉴 항목의 활성화와 관련된 추가 옵션입니다. |


## <a name="toastaudio"></a>ToastAudio
알림 메시지를 받을 때 재생할 오디오를 지정합니다.

| 속성 | 형식 | 필수 | 설명 |
|---|---|---|---|
| **Src** | uri | false | 기본 소리 대신 재생할 미디어 파일입니다. ms-appx와 ms-appdata만 지원됩니다. |
| **반복** | 부울 | false | 알림이 표시된 동안 소리를 반복해야 하는 경우 true로 설정하고, 한 번만 재생하려면 false로 설정합니다(기본값). |
| **자동** | 부울 | false | 음소거하려면 true로 설정하고, 알림 메시지 소리가 재생되게 하려면 false로 설정합니다(기본값). |


## <a name="toastheader"></a>ToastHeader
Creators Update의 새로운 기능: 알림 센터 내에 여러 알림을 함께 그룹화하는 사용자 지정 헤더입니다.

| 속성 | 형식 | 필수 | 설명 |
|---|---|---|---|
| **Id** | string | true | 개발자가 생성하는 식별자로서 이 헤더를 고유하게 식별합니다. 두 알림의 헤더 ID가 동일할 경우 알림 센터에서 동일한 헤더 아래에 해당 알림이 표시됩니다. |
| **제목** | string | true | 헤더의 제목입니다. |
| **인수**| string | true | 사용자가 이 머리글을 클릭할 때 응용 프로그램에 반환되는 개발자 정의 문자열 인수를 가져오거나 설정합니다. null일 수 없습니다. |
| **ActivationType** | [ToastActivationType](#toastactivationtype) | false | 클릭 시 이 헤더가 사용할 활성화 유형을 제어합니다. 기본값은 Foreground입니다. 전경 및 프로토콜만이 지원됩니다. |
| **ActivationOptions** | [ToastActivationOptions](#toastactivationoptions) | false | 알림 헤더의 활성화와 관련된 추가 옵션을 가져오거나 설정합니다. |


## <a name="related-topics"></a>관련 항목

* [빠른 시작: 로컬 알림 보내기 및 활성화 처리](http://blogs.msdn.com/b/tiles_and_toasts/archive/2015/07/08/quickstart-sending-a-local-toast-notification-and-handling-activations-from-it-windows-10.aspx)
* [GitHub의 알림 라이브러리](https://github.com/Microsoft/UWPCommunityToolkit/tree/dev/Notifications)
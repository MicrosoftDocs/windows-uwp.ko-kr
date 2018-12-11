---
Description: The following article describes all of the properties and elements within tile content.
title: 타일 콘텐츠 스키마
ms.assetid: 7CBC3BD5-D9C3-4781-8BD0-1F28039E1FA8
label: Tile content schema
template: detail.hbs
ms.date: 07/28/2017
ms.topic: article
keywords: windows 10, uwp, 타일, 타일 알림, 타일 콘텐츠, 스키마, 타일 페이로드
ms.localizationpriority: medium
ms.openlocfilehash: 02ac975ae3893b1d3d591133862d0ff3733cca6b
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8925141"
---
# <a name="tile-content-schema"></a>타일 콘텐츠 스키마

 

다음은 타일 콘텐츠 내의 모든 속성과 요소에 대한 설명입니다.

[Notifications library(알림 라이브러리)](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) 대신 원시 XML을 사용하려면 [XML schema(XML 스키)마](../tiles-and-notifications/adaptive-tiles-schema.md)를 참조하세요.

[TileContent](#tilecontent)
* [TileVisual](#tilevisual)
  * [TileBinding](#tilebinding)
    * [TileBindingContentAdaptive](#TileBindingContentAdaptive)
    * [TileBindingContentIconic](#TileBindingContentIconic)
    * [TileBindingContentContact](#TileBindingContentContact)
    * [TileBindingContentPeople](#TileBindingContentPeople)
    * [TileBindingContentPhotos](#TileBindingContentPhotos)


## <a name="tilecontent"></a>TileContent
TileContent는 시각적 개체를 포함한 타일 알림의 콘텐츠를 설명하는 최상위 개체입니다.

| 속성 | 형식 | 필수 | 설명 |
|---|---|---|---|
| **시각적 개체** | [ToastVisual](#tilevisual) | true | 타일 메시지의 시각적 개체 부분을 설명합니다. |


## <a name="tilevisual"></a>TileVisual
타일의 시각적 부분에는 모든 타일 크기에 대한 시각적 사양과 시각과 관련된 기타 속성이 포함됩니다.

| 속성 | 형식 | 필수 | 설명 |
|---|---|---|---|
| **TileSmall** | [TileBinding](#tilebinding) | false | 작은 바인딩 옵션을 제공하여 작은 타일 크기에 대한 콘텐츠를 지정합니다. |
| **TileMedium** | [TileBinding](#tilebinding) | false | 중간 바인딩 옵션을 제공하여 중간 타일 크기에 대한 콘텐츠를 지정합니다. |
| **TileWide** | [TileBinding](#tilebinding) | false | 와이드 바인딩 옵션을 제공하여 넓은 타일 크기에 대한 콘텐츠를 지정합니다. |
| **TileLarge** | [TileBinding](#tilebinding) | false | 큰 바인딩 옵션을 제공하여 큰 타일 크기에 대한 콘텐츠를 지정합니다. |
| **브랜딩** | [TileBranding](#tilebranding) | false | 타일이 앱의 브랜드를 표시하는 데 사용해야 하는 형식입니다. 기본적으로 브랜딩은 기본 타일에서 상속됩니다. |
| **DisplayName** | 문자열 | false | 이 알림을 표시하는 동안 타일의 표시 이름을 재정의하기 위한 선택적 문자열입니다. |
| **인수** | string | false | 1주년 업데이트의 새로운 기능: 사용자가 Live Tile에서 앱을 시작할 경우 LaunchActivatedEventArgs의 TileActivatedInfo 속성을 통해 앱에 다시 전달되는 앱 정의 데이터입니다. 이를 통해 사용자가 Live Tile을 탭했을 때 사용자에게 어떤 타일 알림이 표시되었는지 알 수 있습니다. 기념일 업데이트를 하지 않은 디바이스에서는 이 기능이 무시됩니다. |
| **LockDetailedStatus1** | 문자열 | false | 이와 같이 지정하면 TileWide 바인딩도 제공해야 합니다. 사용자가 타일을 상세한 상태 앱으로 선택한 경우 잠금 화면에 표시되는 첫 번째 텍스트 줄입니다. |
| **LockDetailedStatus2** | 문자열 | false | 이와 같이 지정하면 TileWide 바인딩도 제공해야 합니다. 사용자가 타일을 상세한 상태 앱으로 선택한 경우 잠금 화면에 표시되는 두 번째 텍스트 줄입니다. |
| **LockDetailedStatus3** | 문자열 | false | 이와 같이 지정하면 TileWide 바인딩도 제공해야 합니다. 사용자가 타일을 상세한 상태 앱으로 선택한 경우 잠금 화면에 표시되는 세 번째 텍스트 줄입니다. |
| **BaseUri** | URI | false | 이미지 소스 특성에 상대 URL과 결합되는 기본 기준 URL입니다. |
| **AddImageQuery** | bool? | false | Windows에서 알림 메시지에 제공된 이미지 URL에 쿼리 문자열을 추가할 수 있게 하려면 "true"로 설정합니다. 서버에서 이미지를 호스트하고, 쿼리 문자열을 기반으로 이미지 변형을 검색하거나 쿼리 문자열을 무시하고 쿼리 문자열 없이 지정된 이미지를 반환하여 쿼리 문자열을 처리할 수 있는 경우 이 특성을 사용합니다. 이 쿼리 문자열은 배율, 대비 설정 및 언어를 지정합니다. 예를 들어, 알림에 지정된 "www.website.com/images/hello.png"의 값은 "www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us"가 됩니다. |
| **Language**| string | false | "en-US" 또는 "fr-FR"과 같이 BCP-47 언어 태그로 지정된 지역화된 리소스를 사용할 때 시각적 페이로드의 대상 로캘입니다. 이 로캘은 바인딩 또는 텍스트에 지정된 로캘로 재정의됩니다. 제공되지 않을 경우 시스템 로캘이 대신 사용됩니다. |


## <a name="tilebinding"></a>TileBinding
바인딩 개체에는 특정 타일 크기에 대한 시각적 콘텐츠가 포함됩니다.

| 속성 | 형식 | 필수 | 설명 |
|---|---|---|---|
| **콘텐츠** | [ITileBindingContent](#itilebindingcontent) | false | 타일에 표시할 시각적 콘텐츠입니다. [TileBindingContentAdaptive](#tilebindingcontentadaptive), [TileBindingContentIconic](#TileBindingContentIconic), [TileBindingContentContact](#TileBindingContentContact), [TileBindingContentPeople](#TileBindingContentPeople) 또는 [TileBindingContentPhotos](#TileBindingContentPhotos) 중 하나입니다. |
| **브랜딩** | [TileBranding](#tilebranding) | false | 타일이 앱의 브랜드를 표시하는 데 사용해야 하는 형식입니다. 기본적으로 브랜딩은 기본 타일에서 상속됩니다. |
| **DisplayName** | 문자열 | false | 이 타일 크기에 대한 타일의 표시 이름을 재정의하기 위한 선택적 문자열입니다. |
| **인수** | string | false | 1주년 업데이트의 새로운 기능: 사용자가 Live Tile에서 앱을 시작할 경우 LaunchActivatedEventArgs의 TileActivatedInfo 속성을 통해 앱에 다시 전달되는 앱 정의 데이터입니다. 이를 통해 사용자가 Live Tile을 탭했을 때 사용자에게 어떤 타일 알림이 표시되었는지 알 수 있습니다. 기념일 업데이트를 하지 않은 디바이스에서는 이 기능이 무시됩니다. |
| **BaseUri** | URI | false | 이미지 소스 특성에 상대 URL과 결합되는 기본 기준 URL입니다. |
| **AddImageQuery** | bool? | false | Windows에서 알림 메시지에 제공된 이미지 URL에 쿼리 문자열을 추가할 수 있게 하려면 "true"로 설정합니다. 서버에서 이미지를 호스트하고, 쿼리 문자열을 기반으로 이미지 변형을 검색하거나 쿼리 문자열을 무시하고 쿼리 문자열 없이 지정된 이미지를 반환하여 쿼리 문자열을 처리할 수 있는 경우 이 특성을 사용합니다. 이 쿼리 문자열은 배율, 대비 설정 및 언어를 지정합니다. 예를 들어, 알림에 지정된 "www.website.com/images/hello.png"의 값은 "www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us"가 됩니다. |
| **Language**| string | false | "en-US" 또는 "fr-FR"과 같이 BCP-47 언어 태그로 지정된 지역화된 리소스를 사용할 때 시각적 페이로드의 대상 로캘입니다. 이 로캘은 바인딩 또는 텍스트에 지정된 로캘로 재정의됩니다. 제공되지 않을 경우 시스템 로캘이 대신 사용됩니다. |


## <a name="itilebindingcontent"></a>ITileBindingContent
타일 바인딩 콘텐츠의 마커 인터페이스입니다. 이를 통해 적응형 또는 특수 템플릿 중 하나에서 타일 시각 항목 지정할 대상을 선택할 수 있습니다.

| 구현 |
| --- |
| [TileBindingContentAdaptive](#TileBindingContentAdaptive) |
| [TileBindingContentIconic](#TileBindingContentIconic) |
| [TileBindingContentContact](#TileBindingContentContact) |
| [TileBindingContentPeople](#TileBindingContentPeople) |
| [TileBindingContentPhotos](#TileBindingContentPhotos) |


## <a name="tilebindingcontentadaptive"></a>TileBindingContentAdaptive
모든 크기에 지원됩니다. 타일 콘텐츠를 지정하는 방법으로 권장되는 방법입니다. Windows 10에 새롭게 선보인 적응형 타일 템플릿에서는 적응 기능을 통해 다양한 사용자 지정 타일을 만들 수 있습니다.

| 속성 | 형식 | 필수 | 설명 |
|---|---|---|---|
| **Children** | IList<[ITileBindingContentAdaptiveChild](#ITileBindingContentAdaptiveChild)> | false | 인라인 시각 요소입니다. [AdaptiveText](#adaptivetext), [AdaptiveImage](#adaptiveimage), [AdaptiveGroup](#adaptivegroup) 개체를 추가할 수 있습니다. 하위 항목은 수직 StackPanel 방식으로 표시됩니다. |
| **BackgroundImage** | [TileBackgroundImage](#tilebackgroundimage) | false | 모든 타일 콘텐츠 뒤에 전체적으로 표시되는 배경 이미지 옵션입니다. |
| **PeekImage** | [TilePeekImage](#tilepeekimage) | false | 타일 맨 위에서부터 애니메이션으로 표현되는 미리 보기 이미지 옵션입니다. |
| **TextStacking** | [TileTextStacking](#tiletextstacking) | false | 하위 항목 콘텐츠 전체의 텍스트 스택(수직 정렬)를 제어합니다. |


## <a name="adaptivetext"></a>AdaptiveText
적응형 텍스트 요소입니다.

| 속성 | 형식 | 필수 |설명 |
|---|---|---|---|
| **Text** | string | false | 표시할 텍스트입니다. |
| **HintStyle** | [AdaptiveTextStyle](#adaptivetextstyle) | false | 스타일로 텍스트의 글꼴 크기, 두께 및 불투명도가 제어됩니다. |
| **HintWrap** | bool? | false | 텍스트 배치를 활성화하려면 true로 설정합니다. 기본값은 false입니다. |
| **HintMaxLines** | int? | false | 텍스트 요소가 표시할 수 있는 최대 줄 수입니다. |
| **HintMinLines** | int? | false | 텍스트 요소가 표시해야 하는 최소 줄 수입니다. |
| **HintAlign** | [AdaptiveTextAlign](#adaptivetextalign) | false | 텍스트의 가로 맞춤입니다. |
| **언어** | string | false | "en-US" 또는 "fr-FR"과 같이 BCP-47 언어 태그로 지정된 XML 페이로드의 대상 로캘입니다. 여기에 지정된 로캘이 바인딩이나 시각적 개체의 로캘을 비롯하여 지정된 다른 로캘을 재정의합니다. 이 값이 리터럴 문자열인 경우 이 특성은 기본적으로 사용자의 UI 언어로 설정됩니다. 이 값이 문자열 참조인 경우 문자열 확인 시 이 특성은 기본적으로 Windows 런타임에서 선택한 로캘로 설정됩니다. |


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
| **HintCrop** | [AdaptiveImageCrop](#adaptiveimagecrop) | false | 이미지의 원하는 자르기를 제어합니다. |
| **HintRemoveMargin** | bool? | false | 기본적으로 그룹/하위 그룹 내 이미지의 주변 여백은 8px입니다. 이 속성을 true로 설정하여 이 여백을 제거할 수 있습니다. |
| **HintAlign** | [AdaptiveImageAlign](#adaptiveimagealign) | false | 이미지의 가로 맞춤입니다. |
| **AlternateText** | string | false | 이미지를 설명하는 대체 텍스트로서 접근성 용도로 사용됩니다. |
| **AddImageQuery** | bool? | false | "true"로 설정하면 Windows가 타일 알림에 제공된 이미지 URL에 쿼리 문자열을 추가할 수 있습니다. 서버에서 이미지를 호스트하고, 쿼리 문자열을 기반으로 이미지 변형을 검색하거나 쿼리 문자열을 무시하고 쿼리 문자열 없이 지정된 이미지를 반환하여 쿼리 문자열을 처리할 수 있는 경우 이 특성을 사용합니다. 이 쿼리 문자열은 배율, 대비 설정 및 언어를 지정합니다. 예를 들어, 알림에 지정된 "www.website.com/images/hello.png"의 값은 "www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us"가 됩니다. |


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
그룹이 의미상 그룹의 콘텐츠를 전체로 표시하거나 맞지 않을 경우 표시하지 않음을 확인합니다. 그룹은 또한 여러 열을 만들 수 있게 합니다.

| 속성 | 형식 | 필수 |설명 |
|---|---|---|---|
| **Children** | IList<[AdaptiveSubgroup](#adaptivesubgroup)> | false | 하위 그룹은 세로 열로 표시됩니다. 하위 그룹을 사용하여 AdaptiveGroup 내의 모든 콘텐츠를 제공합니다. |


## <a name="adaptivesubgroup"></a>AdaptiveSubgroup
하위 그룹은 텍스트와 이미지를 포함할 수 있는 세로 열입니다.

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


## <a name="tilebackgroundimage"></a>TileBackgroundImage
타일 전체에 표시되는 배경 이미지입니다.

| 속성 | 형식 | 필수 |설명 |
|---|---|---|---|
| **Source** | string | true | 이미지에 대한 URL입니다. ms-appx, ms-appdata, http(s)가 지원됩니다. Http 이미지는 크기가 200KB 이하여야 합니다. |
| **HintOverlay** | int? | false | 배경 이미지상의 검은색 오버레이입니다. 이 값은 검은색 오버레이의 불투명도를 제어합니다. 0으로 설정하면 오버레이가 없고 100은 완전히 검은 색입니다. 기본값은 20입니다. |
| **HintCrop** | [TileBackgroundImageCrop](#tilebackgroundimagecrop) | false | 1511 버전의 새로운 기능: 이미지를 어떻게 자를지를 지정합니다. 1511 이전 버전에서는 이 설정이 무시되며 배경 이미지가 잘리지 않고 표시됩니다. |
| **AlternateText** | string | false | 이미지를 설명하는 대체 텍스트로서 접근성 용도로 사용됩니다. |
| **AddImageQuery** | bool? | false | "true"로 설정하면 Windows가 타일 알림에 제공된 이미지 URL에 쿼리 문자열을 추가할 수 있습니다. 서버에서 이미지를 호스트하고, 쿼리 문자열을 기반으로 이미지 변형을 검색하거나 쿼리 문자열을 무시하고 쿼리 문자열 없이 지정된 이미지를 반환하여 쿼리 문자열을 처리할 수 있는 경우 이 특성을 사용합니다. 이 쿼리 문자열은 배율, 대비 설정 및 언어를 지정합니다. 예를 들어, 알림에 지정된 "www.website.com/images/hello.png"의 값은 "www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us"가 됩니다. |


### <a name="tilebackgroundimagecrop"></a>TileBackgroundImageCrop
배경 이미지의 자르기를 제어합니다.

| 값 | 의미 |
|---|---|
| **Default** | 자르기는 렌더러의 기본 동작을 사용합니다. |
| **None** | 이미지가 잘리지 않으며 사각형이 표시됩니다. |
| **Circle** | 이미지가 원으로 잘립니다. |


## <a name="tilepeekimage"></a>TilePeekImage
타일 맨 위에서부터 애니메이션으로 표현되는 미리 보기 이미지입니다.

| 속성 | 형식 | 필수 |설명 |
|---|---|---|---|
| **Source** | string | true | 이미지에 대한 URL입니다. ms-appx, ms-appdata, http(s)가 지원됩니다. Http 이미지는 크기가 200KB 이하여야 합니다. |
| **HintOverlay** | int? | false | 1511의 새로운 기능: 미리 보기 이미지의 검은색 오버레이. 이 값은 검은색 오버레이의 불투명도를 제어합니다. 0으로 설정하면 오버레이가 없고 100은 완전히 검은 색입니다. 기본값은 20입니다. 이전 버전에서는 이 값이 무시되고 미리 보기 이미지가 0 오버레이로 표시됩니다. |
| **HintCrop** | [TilePeekImageCrop](#tilepeekimagecrop) | false | 1511 버전의 새로운 기능: 이미지를 어떻게 자를지를 지정합니다. 1511 이전 버전에서는 이 설정이 무시되며 미리 보기 이미지가 잘리지 않고 표시됩니다. |
| **AlternateText** | string | false | 이미지를 설명하는 대체 텍스트로서 접근성 용도로 사용됩니다. |
| **AddImageQuery** | bool? | false | "true"로 설정하면 Windows가 타일 알림에 제공된 이미지 URL에 쿼리 문자열을 추가할 수 있습니다. 서버에서 이미지를 호스트하고, 쿼리 문자열을 기반으로 이미지 변형을 검색하거나 쿼리 문자열을 무시하고 쿼리 문자열 없이 지정된 이미지를 반환하여 쿼리 문자열을 처리할 수 있는 경우 이 특성을 사용합니다. 이 쿼리 문자열은 배율, 대비 설정 및 언어를 지정합니다. 예를 들어, 알림에 지정된 "www.website.com/images/hello.png"의 값은 "www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us"가 됩니다. |


### <a name="tilepeekimagecrop"></a>TilePeekImageCrop
미리 보기 이미지의 자르기를 제어합니다.

| 값 | 의미 |
|---|---|
| **Default** | 자르기는 렌더러의 기본 동작을 사용합니다. |
| **None** | 이미지가 잘리지 않으며 사각형이 표시됩니다. |
| **Circle** | 이미지가 원으로 잘립니다. |


### <a name="tiletextstacking"></a>TileTextStacking
텍스트 스택은 콘텐츠의 세로 맞춤을 지정합니다.

| 값 | 의미 |
|---|---|
| **Default** | 기본값입니다. 렌더러가 자동으로 기본 세로 맞춤을 선택합니다. |
| **Top** | 위쪽에 세로로 맞춥니다. |
| **Center** | 가운데에 세로로 맞춥니다. |
| **Bottom** | 아래쪽에 세로로 맞춥니다. |


## <a name="tilebindingcontenticonic"></a>TileBindingContentIconic
작은 크기 및 중간 크기에 지원됩니다. 대표적인 타일 템플릿을 활성화하여, 타일의 아이콘과 배지 디스플레이가 클래식한 Windows Phone 스타일로 나란히 배치되도록 할 수 있습니다. 별도의 배지 알림을 통해 아이콘 옆의 번호를 알 수 있습니다.

| 속성 | 형식 | 필수 |설명 |
|---|---|---|---|
| **아이콘** | [TileBasicImage](#tilebasicimage) | true | 최소한 데스크톱 및 모바일, 작은 크기와 중간 크기 타일을 모두 지원하려면 투명 및 흰색만 허용하는 PNG 형식의 정사각형 가로세로 비율 이미지를 200x200 해상도로 제공합니다. 자세한 내용은 [특수 타일 템플릿](../tiles-and-notifications/special-tile-templates-catalog.md)을 참조하세요. |


## <a name="tilebindingcontentcontact"></a>TileBindingContentContact
모바일만 해당합니다. 작은 크기 및 중간 크기, 와이드 타일에 지원됩니다.

| 속성 | 형식 | 필수 |설명 |
|---|---|---|---|
| **이미지** | [TileBasicImage](#tilebasicimage) | true | 표시할 이미지입니다. |
| **텍스트** | [TileBasicText](#tilebasictext) | false | 표시되는 텍스트 줄입니다. 작은 타일에는 표시되지 않습니다. |


## <a name="tilebindingcontentpeople"></a>TileBindingContentPeople
1511의 새로운 기능: 중간 크기, 와이드 타일, 큰 타일에 지원됩니다(데스크톱 및 모바일). 이전에는 모바일 및 중간 크기, 와이드 타일 전용이었습니다.

| 속성 | 형식 | 필수 |설명 |
|---|---|---|---|
| **이미지** | IList<[TileBasicImage](#tilebasicimage)> | true | 원으로 롤링할 이미지입니다. |


## <a name="tilebindingcontentphotos"></a>TileBindingContentPhotos
사진 슬라이드 쇼를 통해 애니메이션을 재생합니다. 모든 크기에 지원됩니다.

| 속성 | 형식 | 필수 |설명 |
|---|---|---|---|
| **이미지** | IList<[TileBasicImage](#tilebasicimage)> | true | 최대 12개 이미지가 제공될 수 있으며(모바일은 최대 9개까지만 표시) 슬라이드 쇼에 사용됩니다. 12개가 넘는 이미지를 추가하면 예외가 throw됩니다. |


### <a name="tilebasicimage"></a>TileBasicImage
다양한 특수 템플릿에 사용되는 이미지입니다.

| 속성 | 형식 | 필수 |설명 |
|---|---|---|---|
| **Source** | string | true | 이미지에 대한 URL입니다. ms-appx, ms-appdata, http(s)가 지원됩니다. Http 이미지는 크기가 200KB 이하여야 합니다. |
| **AlternateText** | string | false | 이미지를 설명하는 대체 텍스트로서 접근성 용도로 사용됩니다. |
| **AddImageQuery** | bool? | false | "true"로 설정하면 Windows가 타일 알림에 제공된 이미지 URL에 쿼리 문자열을 추가할 수 있습니다. 서버에서 이미지를 호스트하고, 쿼리 문자열을 기반으로 이미지 변형을 검색하거나 쿼리 문자열을 무시하고 쿼리 문자열 없이 지정된 이미지를 반환하여 쿼리 문자열을 처리할 수 있는 경우 이 특성을 사용합니다. 이 쿼리 문자열은 배율, 대비 설정 및 언어를 지정합니다. 예를 들어, 알림에 지정된 "www.website.com/images/hello.png"의 값은 "www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us"가 됩니다. |


### <a name="tilebasictext"></a>TileBasicText
다양한 특수 템플릿에 사용되는 기본 텍스트 요소입니다.

| 속성 | 형식 | 필수 |설명 |
|---|---|---|---|
| **Text** | string | false | 표시할 텍스트입니다. |
| **Language** | string | false | "en-US" 또는 "fr-FR"과 같이 BCP-47 언어 태그로 지정된 XML 페이로드의 대상 로캘입니다. 여기에 지정된 로캘이 바인딩이나 시각적 개체의 로캘을 비롯하여 지정된 다른 로캘을 재정의합니다. 이 값이 리터럴 문자열인 경우 이 특성은 기본적으로 사용자의 UI 언어로 설정됩니다. 이 값이 문자열 참조인 경우 문자열 확인 시 이 특성은 기본적으로 Windows 런타임에서 선택한 로캘로 설정됩니다. |


## <a name="related-topics"></a>관련 항목

* [빠른 시작: 로컬 타일 알림 보내기](../tiles-and-notifications/sending-a-local-tile-notification.md)
* [GitHub의 알림 라이브러리](https://github.com/Microsoft/UWPCommunityToolkit/tree/dev/Notifications)
---
description: 다음 문서에서는 타일 콘텐츠 내의 모든 속성과 요소에 대해 설명 합니다.
title: 타일 콘텐츠 스키마
ms.assetid: 7CBC3BD5-D9C3-4781-8BD0-1F28039E1FA8
label: Tile content schema
template: detail.hbs
ms.date: 07/28/2017
ms.topic: article
keywords: windows 10, uwp, 타일, 타일 알림, 타일 콘텐츠, 스키마, 타일 페이로드
ms.localizationpriority: medium
ms.openlocfilehash: 4d1953e6d745a41c3bdd85d5f4dd3c6c9df8b900
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034416"
---
# <a name="tile-content-schema"></a>타일 콘텐츠 스키마

 

다음은 타일 콘텐츠 내의 모든 속성 및 요소에 대 한 설명입니다.

[알림 라이브러리](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)대신 원시 xml을 사용 하는 경우 [xml 스키마](../tiles-and-notifications/adaptive-tiles-schema.md)를 참조 하세요.

[TileContent](#tilecontent)
* [TileVisual](#tilevisual)
  * [TileBinding](#tilebinding)
    * [TileBindingContentAdaptive](#tilebindingcontentadaptive)
    * [TileBindingContentIconic](#tilebindingcontenticonic)
    * [TileBindingContentContact](#tilebindingcontentcontact)
    * [TileBindingContentPeople](#tilebindingcontentpeople)
    * [TileBindingContentPhotos](#tilebindingcontentphotos)


## <a name="tilecontent"></a>TileContent
TileContent는 시각적 개체를 포함 하 여 타일 알림의 콘텐츠를 설명 하는 최상위 개체입니다.

| 속성 | Type | 필수 | 설명 |
|---|---|---|---|
| **시각 효과** | [Toa 시각적 개체](#tilevisual) | true | 타일 알림의 시각적 부분을 설명 합니다. |


## <a name="tilevisual"></a>TileVisual
타일의 시각적 부분은 모든 타일 크기에 대 한 시각적 사양과 더 많은 시각적 개체 관련 속성을 포함 합니다.

| 속성 | Type | 필수 | 설명 |
|---|---|---|---|
| **TileSmall** | [TileBinding](#tilebinding) | false | 작은 타일 크기에 대 한 콘텐츠를 지정 하는 선택적 작은 바인딩을 제공 합니다. |
| **TileMedium** | [TileBinding](#tilebinding) | false | 미디어 타일 크기의 콘텐츠를 지정 하는 선택적 중간 바인딩을 제공 합니다. |
| **TileWide** | [TileBinding](#tilebinding) | false | 넓은 타일 크기의 콘텐츠를 지정 하는 선택적 와이드 바인딩을 제공 합니다. |
| **TileLarge** | [TileBinding](#tilebinding) | false | 큰 타일 크기에 대 한 콘텐츠를 지정 하는 선택적 큰 바인딩을 제공 합니다. |
| **브랜딩** | TileBranding | false | 타일에서 앱의 브랜드를 표시 하는 데 사용 해야 하는 폼입니다. 기본적으로는 기본 타일에서 브랜딩을 상속 합니다. |
| **표시 이름** | 문자열 | false | 이 알림을 표시 하는 동안 타일의 표시 이름을 재정의 하는 선택적 문자열입니다. |
| **인수** | 문자열 | false | 기념일 업데이트의 새로운 기능: 사용자가 라이브 타일에서 앱을 시작할 때 LaunchActivatedEventArgs의 TileActivatedInfo 속성을 통해 앱에 다시 전달 되는 앱 정의 데이터입니다. 그러면 사용자가 라이브 타일을 누를 때 표시 되는 타일 알림을 알 수 있습니다. 기념일 업데이트가 없는 장치에서는 무시 됩니다. |
| **LockDetailedStatus1** | 문자열 | false | 이를 지정 하는 경우에는 TileWide binding도 제공 해야 합니다. 사용자가 타일을 상세 상태 앱으로 선택한 경우 잠금 화면에 표시 되는 첫 번째 텍스트 줄입니다. |
| **LockDetailedStatus2** | 문자열 | false | 이를 지정 하는 경우에는 TileWide binding도 제공 해야 합니다. 사용자가 타일을 상세 상태 앱으로 선택한 경우 잠금 화면에 표시 되는 두 번째 텍스트 줄입니다. |
| **LockDetailedStatus3** | 문자열 | false | 이를 지정 하는 경우에는 TileWide binding도 제공 해야 합니다. 사용자가 타일을 상세 상태 앱으로 선택한 경우 잠금 화면에 표시 되는 세 번째 텍스트 줄입니다. |
| **BaseUri** | URI | false | 이미지 원본 특성의 상대 Url과 결합 된 기본 URL입니다. |
| **AddImageQuery** | bool? | false | Windows가 알림 알림에서 제공 된 이미지 URL에 쿼리 문자열을 추가할 수 있게 하려면 "true"로 설정 합니다. 서버에서 이미지를 호스팅하고 쿼리 문자열을 기반으로 하는 이미지 변형을 검색 하거나 쿼리 문자열을 무시 하 고 쿼리 문자열 없이 지정 된 대로 이미지를 반환 하 여 쿼리 문자열을 처리할 수 있는 경우이 특성을 사용 합니다. 이 쿼리 문자열은 크기 조정, 대비 설정 및 언어를 지정 합니다. 예를 들어, 알림에 지정 된 "www.website.com/images/hello.png" 값은 "www.website.com/images/hello.png? ms-scale = 100&&m a s a s s-a s a-s"가 됩니다. |
| **언어**| 문자열 | false | 지역화 된 리소스를 사용 하는 경우 "en-us" 또는 "fr-fr"와 같은 BCP-47 언어 태그로 지정 된 시각적 페이로드의 대상 로캘입니다. 이 로캘은 바인딩 또는 텍스트에 지정 된 로캘에 의해 재정의 됩니다. 지정 하지 않으면 시스템 로캘이 대신 사용 됩니다. |


## <a name="tilebinding"></a>TileBinding
Binding 개체는 특정 타일 크기의 시각적 콘텐츠를 포함 합니다.

| 속성 | Type | 필수 | 설명 |
|---|---|---|---|
| **콘텐츠** | [ITileBindingContent](#itilebindingcontent) | false | 타일에 표시할 시각적 콘텐츠입니다. [TileBindingContentAdaptive](#tilebindingcontentadaptive), [TileBindingContentIconic](#tilebindingcontenticonic), [TileBindingContentContact](#tilebindingcontentcontact), [TileBindingContentPeople](#tilebindingcontentpeople)또는 [TileBindingContentPhotos](#tilebindingcontentphotos)중 하나입니다. |
| **브랜딩** | TileBranding | false | 타일에서 앱의 브랜드를 표시 하는 데 사용 해야 하는 폼입니다. 기본적으로는 기본 타일에서 브랜딩을 상속 합니다. |
| **표시 이름** | 문자열 | false | 이 타일 크기의 타일 표시 이름을 재정의 하는 선택적 문자열입니다. |
| **인수** | 문자열 | false | 기념일 업데이트의 새로운 기능: 사용자가 라이브 타일에서 앱을 시작할 때 LaunchActivatedEventArgs의 TileActivatedInfo 속성을 통해 앱에 다시 전달 되는 앱 정의 데이터입니다. 그러면 사용자가 라이브 타일을 누를 때 표시 되는 타일 알림을 알 수 있습니다. 기념일 업데이트가 없는 장치에서는 무시 됩니다. |
| **BaseUri** | URI | false | 이미지 원본 특성의 상대 Url과 결합 된 기본 URL입니다. |
| **AddImageQuery** | bool? | false | Windows가 알림 알림에서 제공 된 이미지 URL에 쿼리 문자열을 추가할 수 있게 하려면 "true"로 설정 합니다. 서버에서 이미지를 호스팅하고 쿼리 문자열을 기반으로 하는 이미지 변형을 검색 하거나 쿼리 문자열을 무시 하 고 쿼리 문자열 없이 지정 된 대로 이미지를 반환 하 여 쿼리 문자열을 처리할 수 있는 경우이 특성을 사용 합니다. 이 쿼리 문자열은 크기 조정, 대비 설정 및 언어를 지정 합니다. 예를 들어, 알림에 지정 된 "www.website.com/images/hello.png" 값은 "www.website.com/images/hello.png? ms-scale = 100&&m a s a s s-a s a-s"가 됩니다. |
| **언어**| 문자열 | false | 지역화 된 리소스를 사용 하는 경우 "en-us" 또는 "fr-fr"와 같은 BCP-47 언어 태그로 지정 된 시각적 페이로드의 대상 로캘입니다. 이 로캘은 바인딩 또는 텍스트에 지정 된 로캘에 의해 재정의 됩니다. 지정 하지 않으면 시스템 로캘이 대신 사용 됩니다. |


## <a name="itilebindingcontent"></a>ITileBindingContent
타일 바인딩 콘텐츠에 대 한 표식 인터페이스입니다. 이러한 기능을 사용 하 여 적응에서 타일 시각적 개체를 지정 하거나 특수 한 템플릿 중 하나를 선택할 수 있습니다.

| 구현 |
| --- |
| [TileBindingContentAdaptive](#tilebindingcontentadaptive) |
| [TileBindingContentIconic](#tilebindingcontenticonic) |
| [TileBindingContentContact](#tilebindingcontentcontact) |
| [TileBindingContentPeople](#tilebindingcontentpeople) |
| [TileBindingContentPhotos](#tilebindingcontentphotos) |


## <a name="tilebindingcontentadaptive"></a>TileBindingContentAdaptive
모든 크기에서 지원 됩니다. 타일 콘텐츠를 지정 하는 권장 되는 방법입니다. 적응 타일 템플릿 Windows 10의 새로운 기능과 적응을 통해 다양 한 사용자 지정 타일을 만들 수 있습니다.

| 속성 | Type | 필수 | 설명 |
|---|---|---|---|
| **Children** | IList<ITileBindingContentAdaptiveChild> | false | 인라인 시각적 요소입니다. [AdaptiveText](#adaptivetext), [AdaptiveImage](#adaptiveimage)및 [AdaptiveGroup](#adaptivegroup) 개체를 추가할 수 있습니다. 자식은 세로 StackPanel 방식으로 표시 됩니다. |
| **BackgroundImage** | [TileBackgroundImage](#tilebackgroundimage) | false | 모든 타일 콘텐츠 뒤에 표시 되는 (전체 도련) 배경 이미지 (옵션)입니다. |
| **PeekImage** | [TilePeekImage](#tilepeekimage) | false | 타일 위쪽에서에 애니메이션 효과를 주는 선택적 피킹 (peeking) 이미지입니다. |
| **TextStacking** | [TileTextStacking](#tiletextstacking) | false | 전체 자식 콘텐츠의 텍스트 쌓기 (세로 맞춤)를 제어 합니다. |


## <a name="adaptivetext"></a>AdaptiveText
적응 텍스트 요소입니다.

| 속성 | Type | 필수 |설명 |
|---|---|---|---|
| **Text** | 문자열 | false | 표시할 텍스트입니다. |
| **HintStyle** | [AdaptiveTextStyle](#adaptivetextstyle) | false | 스타일은 텍스트의 글꼴 크기, 두께 및 불투명도를 제어 합니다. |
| **HintWrap** | bool? | false | 텍스트 줄 바꿈을 사용 하려면 true로 설정 합니다. 기본값은 false입니다. |
| **HintMaxLines** | int? | false | 텍스트 요소에 표시할 수 있는 최대 줄 수입니다. |
| **HintMinLines** | int? | false | 텍스트 요소에서 표시 해야 하는 최소 줄 수입니다. |
| **HintAlign** | [AdaptiveTextAlign](#adaptivetextalign) | false | 텍스트의 가로 맞춤입니다. |
| **언어** | 문자열 | false | "En-us" 또는 "fr-fr"와 같은 BCP-47 언어 태그로 지정 된 XML 페이로드의 대상 로캘입니다. 여기에 지정 된 로캘은 바인딩 또는 시각적 개체와 같이 지정 된 다른 모든 로캘을 재정의 합니다. 이 값이 리터럴 문자열이 면이 특성은 기본적으로 사용자의 UI 언어로 설정 됩니다. 이 값이 문자열 참조 인 경우이 특성은 문자열을 확인 하는 Windows 런타임에서 선택한 로캘로 기본값을 설정 합니다. |


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
| **부제** | H4 글꼴 크기입니다. |
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

| 속성 | Type | 필수 |설명 |
|---|---|---|---|
| **원본** | 문자열 | true | 이미지에 대 한 URL입니다. appdata 및 http가 지원 됩니다. 가 중 작성자 업데이트를 기준으로 웹 이미지는 일반 연결의 경우 최대 3mb, 요금제 연결의 경우 1mb가 될 수 있습니다. 아직가 중 작성자 업데이트를 실행 하지 않는 장치에서는 웹 이미지가 200 KB 보다 크지 않아야 합니다. |
| **HintCrop** | [AdaptiveImageCrop](#adaptiveimagecrop) | false | 원하는 이미지 자르기를 제어 합니다. |
| **HintRemoveMargin** | bool? | false | 기본적으로 그룹/하위 그룹 내의 이미지 주위에는 8px 여백이 있습니다. 이 속성을 true로 설정 하 여이 여백을 제거할 수 있습니다. |
| **HintAlign** | [AdaptiveImageAlign](#adaptiveimagealign) | false | 이미지의 가로 맞춤입니다. |
| **AlternateText** | 문자열 | false | 내게 필요한 옵션을 위해 사용 되는 이미지를 설명 하는 대체 텍스트입니다. |
| **AddImageQuery** | bool? | false | Windows에서 타일 알림에 제공 된 이미지 URL에 쿼리 문자열을 추가할 수 있게 하려면 "true"로 설정 합니다. 서버에서 이미지를 호스팅하고 쿼리 문자열을 기반으로 하는 이미지 변형을 검색 하거나 쿼리 문자열을 무시 하 고 쿼리 문자열 없이 지정 된 대로 이미지를 반환 하 여 쿼리 문자열을 처리할 수 있는 경우이 특성을 사용 합니다. 이 쿼리 문자열은 크기 조정, 대비 설정 및 언어를 지정 합니다. 예를 들어, 알림에 지정 된 "www.website.com/images/hello.png" 값은 "www.website.com/images/hello.png? ms-scale = 100&&m a s a s s-a s a-s"가 됩니다. |


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
그룹 의미 체계는 그룹의 콘텐츠가 전체로 표시 되어야 하거나, 해당 콘텐츠가 들어가지 않는 경우 표시 되지 않아야 함을 나타냅니다. 그룹을 사용 하 여 여러 열을 만들 수도 있습니다.

| 속성 | Type | 필수 |설명 |
|---|---|---|---|
| **Children** | IList<[AdaptiveSubgroup](#adaptivesubgroup)> | false | 하위 그룹은 세로 열로 표시 됩니다. 하위 그룹을 사용 하 여 AdaptiveGroup 내에 콘텐츠를 제공 해야 합니다. |


## <a name="adaptivesubgroup"></a>AdaptiveSubgroup
하위 그룹은 텍스트와 이미지를 포함할 수 있는 세로 세로 막대입니다.

| 속성 | Type | 필수 |설명 |
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


## <a name="tilebackgroundimage"></a>TileBackgroundImage
타일에 전체 도련으로 표시 된 배경 이미지입니다.

| 속성 | Type | 필수 |설명 |
|---|---|---|---|
| **원본** | 문자열 | true | 이미지에 대 한 URL입니다. appdata 및 http (s)가 지원 됩니다. Http 이미지는 200 KB이 하 여야 합니다. |
| **HintOverlay** | int? | false | 배경 이미지의 검정 오버레이입니다. 이 값은 검정 오버레이의 불투명도를 제어 합니다. 0은 오버레이가 없고 100는 완전히 검정색입니다. 기본값은 20입니다. |
| **HintCrop** | [TileBackgroundImageCrop](#tilebackgroundimagecrop) | false | 1511의 새로운 방법: 이미지를 잘라내는 방법을 지정 합니다. 1511 이전 버전에서는 무시 되며 배경 이미지가 자르기 없이 표시 됩니다. |
| **AlternateText** | 문자열 | false | 내게 필요한 옵션을 위해 사용 되는 이미지를 설명 하는 대체 텍스트입니다. |
| **AddImageQuery** | bool? | false | Windows에서 타일 알림에 제공 된 이미지 URL에 쿼리 문자열을 추가할 수 있게 하려면 "true"로 설정 합니다. 서버에서 이미지를 호스팅하고 쿼리 문자열을 기반으로 하는 이미지 변형을 검색 하거나 쿼리 문자열을 무시 하 고 쿼리 문자열 없이 지정 된 대로 이미지를 반환 하 여 쿼리 문자열을 처리할 수 있는 경우이 특성을 사용 합니다. 이 쿼리 문자열은 크기 조정, 대비 설정 및 언어를 지정 합니다. 예를 들어, 알림에 지정 된 "www.website.com/images/hello.png" 값은 "www.website.com/images/hello.png? ms-scale = 100&&m a s a s s-a s a-s"가 됩니다. |


### <a name="tilebackgroundimagecrop"></a>TileBackgroundImageCrop
배경 이미지의 자르기를 제어 합니다.

| 값 | 의미 |
|---|---|
| **기본값** | 자르기는 렌더러의 기본 동작을 사용 합니다. |
| **없음** | 이미지가 잘리지 않고 사각형으로 표시 됩니다. |
| **Circle** | 이미지가 원으로 잘립니다. |


## <a name="tilepeekimage"></a>TilePeekImage
타일의 위쪽에서에 애니메이션 효과를 주는 피킹 (peeking) 이미지입니다.

| 속성 | Type | 필수 |설명 |
|---|---|---|---|
| **원본** | 문자열 | true | 이미지에 대 한 URL입니다. appdata 및 http (s)가 지원 됩니다. Http 이미지는 200 KB이 하 여야 합니다. |
| **HintOverlay** | int? | false | 1511의 새로운 방법: 피킹 (peeking) 이미지에 검정색 오버레이가 있습니다. 이 값은 검정 오버레이의 불투명도를 제어 합니다. 0은 오버레이가 없고 100는 완전히 검정색입니다. 기본값은 20입니다. 이전 버전에서는이 값이 무시 되 고 peek 이미지가 0 오버레이로 표시 됩니다. |
| **HintCrop** | [TilePeekImageCrop](#tilepeekimagecrop) | false | 1511의 새로운 방법: 이미지를 잘라내는 방법을 지정 합니다. 1511 이전 버전에서는 무시 되며 peek 이미지는 자르기 없이 표시 됩니다. |
| **AlternateText** | 문자열 | false | 내게 필요한 옵션을 위해 사용 되는 이미지를 설명 하는 대체 텍스트입니다. |
| **AddImageQuery** | bool? | false | Windows에서 타일 알림에 제공 된 이미지 URL에 쿼리 문자열을 추가할 수 있게 하려면 "true"로 설정 합니다. 서버에서 이미지를 호스팅하고 쿼리 문자열을 기반으로 하는 이미지 변형을 검색 하거나 쿼리 문자열을 무시 하 고 쿼리 문자열 없이 지정 된 대로 이미지를 반환 하 여 쿼리 문자열을 처리할 수 있는 경우이 특성을 사용 합니다. 이 쿼리 문자열은 크기 조정, 대비 설정 및 언어를 지정 합니다. 예를 들어, 알림에 지정 된 "www.website.com/images/hello.png" 값은 "www.website.com/images/hello.png? ms-scale = 100&&m a s a s s-a s a-s"가 됩니다. |


### <a name="tilepeekimagecrop"></a>TilePeekImageCrop
피킹 (peeking) 이미지의 자르기를 제어 합니다.

| 값 | 의미 |
|---|---|
| **기본값** | 자르기는 렌더러의 기본 동작을 사용 합니다. |
| **없음** | 이미지가 잘리지 않고 사각형으로 표시 됩니다. |
| **Circle** | 이미지가 원으로 잘립니다. |


### <a name="tiletextstacking"></a>TileTextStacking
텍스트 겹침은 콘텐츠의 세로 맞춤을 지정 합니다.

| 값 | 의미 |
|---|---|
| **기본값** | 기본값입니다. 렌더러는 기본 세로 맞춤을 자동으로 선택 합니다. |
| **상위** | 세로를 위쪽에 맞춥니다. |
| **중심과** | 세로를 가운데에 맞춥니다. |
| **아래쪽** | 세로를 아래쪽에 맞춥니다. |


## <a name="tilebindingcontenticonic"></a>TileBindingContentIconic
Small 및 Medium에서 지원 됩니다. 아이콘 타일 템플릿을 사용 하도록 설정 합니다 .이 템플릿을 사용 하면 타일의 옆에 있는 아이콘 및 배지를 true 클래식 Windows Phone 스타일로 표시할 수 있습니다. 아이콘 옆의 숫자는 별도의 배지 알림을 통해 달성 됩니다.

| 속성 | Type | 필수 |설명 |
|---|---|---|---|
| **아이콘** | [TileBasicImage](#tilebasicimage) | true | 데스크톱과 모바일, 소형 및 중간 타일을 모두 지원 하기 위해 200 x 200, PNG 형식의 해상도를 가진 사각형의 가로 세로 비율 이미지를 제공 하 고 투명도와 흰색 이외의 색은 제공 하지 않습니다. 자세한 내용은 [특수 타일 템플릿](../tiles-and-notifications/special-tile-templates-catalog.md)을 참조 하세요. |


## <a name="tilebindingcontentcontact"></a>TileBindingContentContact
모바일 전용. Small, Medium 및 Wide에서 지원 됩니다.

| 속성 | Type | 필수 |Description |
|---|---|---|---|
| **이미지** | [TileBasicImage](#tilebasicimage) | true | 표시할 이미지입니다. |
| **Text** | [TileBasicText](#tilebasictext) | false | 표시 되는 텍스트 줄입니다. 작은 타일에는 표시 되지 않습니다. |


## <a name="tilebindingcontentpeople"></a>TileBindingContentPeople
1511의 새로운 기능은 Medium, Wide 및 Large (데스크톱 및 모바일)에서 지원 됩니다. 이전에는이 기능이 모바일 전용 이며 중간 및 전체에 불과합니다.

| 속성 | Type | 필수 |설명 |
|---|---|---|---|
| **이미지** | IList<[TileBasicImage](#tilebasicimage)> | true | 원으로 롤업되는 이미지입니다. |


## <a name="tilebindingcontentphotos"></a>TileBindingContentPhotos
사진 슬라이드 쇼를 통해 애니메이션 효과를 적용 합니다. 모든 크기에서 지원 됩니다.

| 속성 | Type | 필수 |설명 |
|---|---|---|---|
| **이미지** | IList<[TileBasicImage](#tilebasicimage)> | true | 최대 12 개의 이미지를 제공할 수 있습니다 (모바일은 최대 9 개 까지만 표시 됨), 슬라이드쇼에 사용 됩니다. 12 개 이상을 추가 하면 예외가 throw 됩니다. |


### <a name="tilebasicimage"></a>TileBasicImage
다양 한 특수 템플릿에 사용 되는 이미지입니다.

| 속성 | Type | 필수 |설명 |
|---|---|---|---|
| **원본** | 문자열 | true | 이미지에 대 한 URL입니다. appdata 및 http (s)가 지원 됩니다. Http 이미지는 200 KB이 하 여야 합니다. |
| **AlternateText** | 문자열 | false | 내게 필요한 옵션을 위해 사용 되는 이미지를 설명 하는 대체 텍스트입니다. |
| **AddImageQuery** | bool? | false | Windows에서 타일 알림에 제공 된 이미지 URL에 쿼리 문자열을 추가할 수 있게 하려면 "true"로 설정 합니다. 서버에서 이미지를 호스팅하고 쿼리 문자열을 기반으로 하는 이미지 변형을 검색 하거나 쿼리 문자열을 무시 하 고 쿼리 문자열 없이 지정 된 대로 이미지를 반환 하 여 쿼리 문자열을 처리할 수 있는 경우이 특성을 사용 합니다. 이 쿼리 문자열은 크기 조정, 대비 설정 및 언어를 지정 합니다. 예를 들어, 알림에 지정 된 "www.website.com/images/hello.png" 값은 "www.website.com/images/hello.png? ms-scale = 100&&m a s a s s-a s a-s"가 됩니다. |


### <a name="tilebasictext"></a>TileBasicText
다양 한 특수 템플릿에 사용 되는 기본 텍스트 요소입니다.

| 속성 | Type | 필수 |설명 |
|---|---|---|---|
| **Text** | 문자열 | false | 표시할 텍스트입니다. |
| **언어** | 문자열 | false | "En-us" 또는 "fr-fr"와 같은 BCP-47 언어 태그로 지정 된 XML 페이로드의 대상 로캘입니다. 여기에 지정 된 로캘은 바인딩 또는 시각적 개체와 같이 지정 된 다른 모든 로캘을 재정의 합니다. 이 값이 리터럴 문자열이 면이 특성은 기본적으로 사용자의 UI 언어로 설정 됩니다. 이 값이 문자열 참조 인 경우이 특성은 문자열을 확인 하는 Windows 런타임에서 선택한 로캘로 기본값을 설정 합니다. |


## <a name="related-topics"></a>관련 항목

* [빠른 시작: 로컬 타일 알림 보내기](../tiles-and-notifications/sending-a-local-tile-notification.md)
* [GitHub의 알림 라이브러리](https://github.com/windows-toolkit/WindowsCommunityToolkit/tree/dev/Notifications)

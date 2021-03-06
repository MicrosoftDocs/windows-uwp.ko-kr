---
description: 사용자가 콘텐츠 및 서비스에 대한 만족도를 반영하는 평점을 확인 및 설정할 수 있도록 해줍니다.
title: 평점 컨트롤
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: abarlow
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 5835993984bd0e080a1a049d20f19c96020f80f9
ms.sourcegitcommit: 39fb8c0dff1b98ededca2f12e8ea7977c2eddbce
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/06/2020
ms.locfileid: "91749669"
---
# <a name="rating-control"></a>평점 컨트롤

평점 컨트롤은 사용자가 콘텐츠 및 서비스에 대한 만족도를 반영하는 평점을 확인 및 설정할 수 있도록 해줍니다. 사용자는 터치, 펜, 마우스, 게임 패드 또는 키보드에서 평점 컨트롤을 사용할 수 있습니다. 다음 지침은 평점 컨트롤의 기능을 사용하여 유연성과 사용자 지정을 제공하는 방법을 보여줍니다.

![평점 컨트롤의 예](images/rating_rs2_doc_ratings_intro.png)

**Windows UI 라이브러리 가져오기**

:::row:::
   :::column:::
      ![WinUI 로고](images/winui-logo-64x64.png)
   :::column-end:::
   :::column span="3":::
      **RatingControl** 컨트롤은 Windows 앱용 새 컨트롤과 UI 기능을 포함하는 NuGet 패키지인 Windows UI 라이브러리의 일부로 포함되어 있습니다. 설치 지침을 비롯한 자세한 내용은 [Windows UI 라이브러리](/uwp/toolkits/winui/)를 참조하세요.
   :::column-end:::
   :::column:::

   :::column-end:::
:::row-end:::

> **Windows UI 라이브러리 API:** [RatingControl 클래스](/uwp/api/microsoft.ui.xaml.controls.ratingcontrol)
>
> **플랫폼 API:** [RatingControl 클래스](/uwp/api/windows.ui.xaml.controls.ratingcontrol)

## <a name="examples"></a>예

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우 여기를 클릭하여 <a href="xamlcontrolsgallery:/item/RatingControl">앱을 열고 작동 중인 RatingControl을 확인</a>합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML Controls Gallery 앱 가져오기(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">소스 코드 가져오기(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

### <a name="editable-rating-with-placeholder-value"></a>자리 표시자 값이 있는 편집 가능한 평점

평점 컨트롤을 사용하는 가장 일반적인 방법은 사용자가 자체 평점 값을 입력하게 하면서 평균 평점을 표시하는 것입니다. 이 시나리오에서 평점 컨트롤은 처음에는 특정 서비스나 콘텐츠 유형(음악, 비디오, 책 등)의 모든 사용자의 평균 만족 평점을 나타내도록 설정됩니다. 사용자가 개별적으로 항목 평점을 매기기 위해 컨트롤을 조작할 때까지 이 상태로 남아 있습니다. 이러한 조작을 통해 평점의 상태가 사용자의 개인 만족 평점을 나타내도록 변경됩니다.

#### <a name="initial-average-rating-state"></a>처음 평균 평점 상태
![처음 평균 평점 상태](images/rating_rs2_doc_movie_aggregate.png)

#### <a name="representation-of-user-rating-once-set"></a>설정 후 사용자 평점 표현

![설정 후 사용자 평점 표현](images/rating_rs2_doc_movie_user.png)

```XAML
<RatingControl x:Name="MyRating" ValueChanged="RatingChanged"/>
```

```csharp
private void RatingChanged(RatingControl sender, object args)
{
    if (sender.Value == null)
    {
        MyRating.Caption = "(" + SomeWebService.HowManyPreviousRatings() + ")";
    }

    else
    {
        MyRating.Caption = "Your rating";
    }
}
```

### <a name="read-only-rating-mode"></a>읽기 전용 평점 모드

댓글 목록과 해당 평점을 표시할 때나 권장 콘텐츠에 표시되는 때와 같이 보조 콘텐츠의 평점을 표시해야 하는 경우가 있습니다. 이 경우에는 사용자가 평점을 편집할 수 없어야 하므로 컨트롤을 읽기 전용으로 설정할 수 있습니다.
UI 설계와 성능의 이유로 매우 큰 가상화된 콘텐츠 목록에 사용될 때 읽기 전용 모드로 평점 컨트롤을 사용하는 것이 좋습니다.

![읽기 전용의 긴 목록](images/rating_rs2_doc_reviews.png)

이렇게 하려면 다음 작업을 수행합니다.

```XAML
<RatingControl IsReadOnly="True"/>
```

## <a name="additional-functionality"></a>추가 기능

평점 컨트롤에는 사용할 수 있는 추가 기능이 많습니다. 이러한 기능 사용에 대한 자세한 내용은 참조 설명서에 나와 있습니다.
다음은 일부 추가 기능의 목록입니다.
-   뛰어난 긴 목록 성능
-   공간이 협소한 UI 시나리오를 위한 세밀한 크기 조정
-   연속 값 채우기 및 평점
-   간격 사용자 지정
-   성장 애니메이션 비활성화
-   별 수 사용자 지정

## <a name="get-the-sample-code"></a>샘플 코드 가져오기

- [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Xaml-Controls-Gallery) - 대화형 형식으로 모든 XAML 컨트롤을 보여줍니다.

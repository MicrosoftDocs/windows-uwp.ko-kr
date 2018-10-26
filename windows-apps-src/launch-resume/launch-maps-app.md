---
author: TylerMSFT
title: Windows 지도 앱 실행
description: 앱에서 Windows 지도 앱을 실행하는 방법을 알아봅니다.
ms.assetid: E363490A-C886-4D92-9A64-52E3C24F1D98
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6fd7377294e0d460720f6a16e71981ab0924ac9a
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "5549283"
---
# <a name="launch-the-windows-maps-app"></a>Windows 지도 앱 실행




앱에서 Windows 지도 앱을 실행하는 방법을 알아봅니다. 이 항목에서는 **bingmaps:, *ms-drive-to:, ms-walk-to:** 및 **ms-settings:** URI(Uniform Resource Identifier) 체계에 대해 설명합니다. 이러한 URI 체계로 Windows 지도 앱을 실행하여 특정 지도, 길 찾기 및 검색 결과를 표시하거나 설정 앱에서 Windows 지도 오프라인 지도를 다운로드할 수 있습니다.

**팁** 앱에서 Windows 지도 앱을 실행하는 방법을 알아보려면 GitHub의 [Windows-universal-samples repo](http://go.microsoft.com/fwlink/p/?LinkId=619979)에서 [UWP(유니버설 Windows 플랫폼) 지도 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619977)을 다운로드하세요.

## <a name="introducing-uris"></a>URI 소개

URI 체계를 사용하면 하이퍼링크를 클릭하거나 앱에서 프로그래밍 방식으로 앱을 열 수 있습니다. **mailto:** 를 사용하여 새 메일을 시작하거나 **http:** 를 사용하여 웹 브라우저를 열 수 있는 것처럼 **bingmaps:**, **ms-drive-to:** 및 **ms-walk-to:** 를 사용하여 Windows 지도 앱을 열 수 있습니다.

-   **bingmaps:** URI는 위치, 검색 결과, 방향 및 교통 지도 제공합니다.
-   **ms-drive-to:** URI는 현재 위치에서 턴바이턴 운전 경로를 제공합니다.
-   **ms-walk-to:** URI는 현재 위치에서 턴바이턴 도보 길 찾기를 제공합니다.

예를 들어 다음 URI는 Windows 지도 앱을 열고 뉴욕시를 중심으로 지도를 표시합니다.

```xml
<bingmaps:?cp=40.726966~-74.006076>
```

![뉴욕시를 중심으로 하는 지도](images/mapnyc.png)

URI 체계에 대한 설명은 다음과 같습니다.

**bingmaps:?query**

이 URI 체계에서 *query*는 일련의 매개 변수 이름/값 쌍입니다.

**&amp;param1=value1&amp;param2=value2 …**

사용 가능한 매개 변수의 전체 목록은 [bingmaps:](#bingmaps-param-reference), [ms-drive-to:](#ms-drive-to-param-reference) 및 [ms-walk-to:](#ms-walk-to-param-reference) 매개 변수 참조를 참조하세요. 이 항목의 뒷부분에 예제도 있습니다.

## <a name="launch-a-uri-from-your-app"></a>앱에서 URI 실행


앱에서 Windows 지도 앱을 시작하려면 **bingmaps:**, **ms-drive-to:** 또는 **ms-walk-to:** URI를 사용하여 [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) 메서드를 호출합니다. 다음 예제에서는 이전 예제와 동일한 URI를 실행합니다. URI를 통해 앱을 실행하는 방법에 대한 자세한 내용은 [URI에 대한 기본 앱 실행](launch-default-app.md)을 참조하세요.

```cs
// Center on New York City
var uriNewYork = new Uri(@"bingmaps:?cp=40.726966~-74.006076");

// Launch the Windows Maps app
var launcherOptions = new Windows.System.LauncherOptions();
launcherOptions.TargetApplicationPackageFamilyName = "Microsoft.WindowsMaps_8wekyb3d8bbwe";
var success = await Windows.System.Launcher.LaunchUriAsync(uriNewYork, launcherOptions);
```

이 예제에서는 Windows 지도 앱을 실행하기 위해 [**LauncherOptions**](https://msdn.microsoft.com/library/windows/apps/hh701435) 클래스를 사용합니다.

## <a name="display-known-locations"></a>알려진 위치 표시

지도의 어느 부분을 표시할 것인지 제어하는 여러 가지 옵션이 있습니다. *cp*(중심점) 매개 변수를 *rad*(반경) 또는 *lvl*(확대/축소 수준) 매개 변수와 함께 사용하여 위치를 표시하고 얼마나 가까이 확대할 것인지 선택할 수 있습니다. *cp* 매개 변수를 사용할 때 *hdg*(방향) 및 *pit*(피치)를 지정하여 어느 방향을 바라볼 것인지 제어할 수 있습니다. 또 다른 방법은 *bb*(경계 상자) 매개 변수를 사용하여 표시하려는 영역의 최대 동서남북 좌표를 제공하는 것입니다.

뷰 유형을 제어하려면 *sty*(스타일) 및 *ss*(Streetside) 매개 변수를 사용합니다. *sty* 매개 변수를 사용하여 도로 뷰와 위성 뷰 간에 전환할 수 있습니다. *ss* 매개 변수는 지도를 Streetside 뷰로 표시합니다. 이러한 매개 변수 및 기타 매개 변수에 대한 자세한 내용은 [bingmaps: 매개 변수 참조](#bingmaps-param-reference)를 참조하세요.


| 샘플 URI                                                                 | 결과                                                                                                                                                                                        |
|----------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| bingmaps:?                                                                 | 지도 앱을 엽니다.                                                                                                                                                                            |
| bingmaps:?cp=40.726966~-74.006076                                          | 뉴욕시를 중심으로 하는 지도를 표시합니다.                                                                                                                                                    |
| bingmaps:?cp=40.726966~-74.006076&amp;lvl=10                                   | 확대/축소 수준을 10으로 하여 뉴욕시를 중심으로 하는 지도를 표시합니다.                                                                                                                            |
| bingmaps:?bb=39.719\_-74.52~41.71\_-73.5                                   | **bb** 인수에 지정된 영역인 뉴욕시의 지도를 표시합니다.                                                                                                           |
| bingmaps:?bb=39.719\_-74.52~41.71\_-73.5&cp=47~-122                        | 경계 상자 인수에 지정된 영역인 뉴욕시의 지도를 표시합니다. *bb*가 지정되었기 때문에 **cp** 인수에 지정된 시애틀의 중심점이 무시됩니다. |
| bingmaps:?collection=point.36.116584\_-115.176753\_Caesars%20Palace&lvl=16 | Caesar's Palace(라스베이거스)라는 지점이 포함된 지도를 표시하고 확대/축소 수준을 16으로 설정합니다.                                                                                                 |
| bingmaps:?collection=point.40.726966\_-74.006076\_Some%255FBusiness        | Some\_Business(라스베가스)라는 지점이 포함된 지도를 표시합니다.                                                                                                                               |
| bingmaps:?cp=40.726966~-74.006076&trfc=1&sty=a                             | 교통량을 켜고 항공 지도 스타일로 뉴욕시 지도를 표시합니다.                                                                                                                          |
| bingmaps:?cp=47.6204~-122.3491&sty=3d                                      | Space Needle의 3D 뷰를 표시합니다.                                                                                                                                                        |
| bingmaps:?cp=47.6204~-122.3491&amp;sty=3d&amp;rad=200&amp;pit=75&amp;hdg=165               | 200m 반경, 75도 피치 및 165도 방향으로 Space Needle의 3D 뷰를 표시합니다.                                                                             |
| bingmaps:?cp=47.6204~-122.3491&ss=1                                        | Space Needle의 Streetside 뷰를 표시합니다.                                                                                                                                                |


## <a name="display-search-results"></a>검색 결과 표시

*q* 매개 변수를 사용하여 장소를 검색할 때에는 조건을 최대한 구체적으로 지정하고 *cp*, *bb* 또는 *where* 매개 변수를 사용하여 검색 위치를 지정하는 것이 좋습니다. 검색 위치를 지정하지 않아서 사용자의 현재 위치를 사용할 수 없으면 검색 결과에 의미 있는 결과가 반환되지 않을 수 있습니다. 검색 결과는 가장 적합한 지도 뷰에 표시됩니다. 이러한 매개 변수 및 기타 매개 변수에 대한 자세한 내용은 [bingmaps: 매개 변수 참조](#bingmaps-param-reference)를 참조하세요.


| 샘플 URI                                                    | 결과                                                                            |
|---------------------------------------------------------------|------------------------------------------------------------------------------------|
| bingmaps:?q=1600%20Pennsylvania%20Ave,%20Washington,%20DC     | 지도를 표시하고 워싱턴 D.C.의 백악관 주소를 검색합니다. |
| bingmaps:?q=coffee&where=Seattle                              | 시애틀에서 커피를 검색합니다.                                                    |
| bingmaps:?cp=40.726966~-74.006076&where=New%20York            | 지정된 중심점 근처의 뉴욕을 검색합니다.                             |
| bingmaps:?bb=39.719\_-74.52~41.71\_-73.5&q=pizza              | 지정된 경계 상자(즉, 뉴욕시) 내부의 피자를 검색합니다.      |

 
## <a name="display-multiple-points"></a>여러 지점 표시


*collection* 매개 변수를 사용하여 지도에 사용자 지정 점 집합을 표시할 수 있습니다. 지점이 여러 개 있는 경우, 지점 목록이 표시됩니다. 한 컬렉션에서 최대 25개 지점이 있을 수 있으며 제공된 순서대로 나열됩니다. 컬렉션은 검색 및 길 찾기를 요청보다 우선합니다. 이 매개 변수 및 기타 매개 변수에 대한 자세한 내용은 [bingmaps: 매개 변수 참조](#bingmaps-param-reference)를 참조하세요.

| 샘플 URI | 결과                                                                                                                   |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------|
| bingmaps:?collection=point.36.116584\_-115.176753\_Caesars%20Palace                                                                                                | Caesar's Palace(라스베이거스)를 검색하고 결과를 지도에 최상의 지도 보기로 표시합니다.                         |
| bingmaps:?collection=point.36.116584\_-115.176753\_Caesars%20Palace&amp;lvl=16                                                                                         | 라스베이거스에 있는 Caesars Palace라는 이름의 고정핀을 표시하고 16 수준으로 확대/축소합니다.                                               |
| bingmaps:?collection=point.36.116584\_-115.176753\_Caesars%20Palace~point.36.113126\_-115.175188\_The%20Bellagio&amp;lvl=16&amp;cp=36.114902~-115.176669                   | 라스베이거스에 있는 Caesars Palace라는 이름의 고정핀과 The Bellagio라는 이름의 고정핀을 표시하고 16 수준으로 확대/축소합니다.*/              |
| bingmaps:?collection=point.40.726966\_-74.006076\_Fake%255FBusiness%255Fwith%255FUnderscore                                                                        | Fake\_Business\_with\_Underscore라는 이름의 고정핀이 포함된 뉴욕을 표시합니다.                                                  |
| bingmaps:?collection=name.Hotel%20List~point.36.116584\_-115.176753\_Caesars%20Palace~point.36.113126\_-115.175188\_The%20Bellagio&amp;lvl=16&amp;cp=36.114902~-115.176669 | Hotel List라는 이름의 목록과 라스베이거스에 있는 Caesars Palace 및 The Bellagio에 대한 두 개의 고정핀을 표시하고 16 수준으로 확대/축소합니다. |

 

## <a name="display-directions-and-traffic"></a>길 찾기 및 교통량 표시


*rtp* 매개 변수를 사용하여 두 지점 간의 길 찾기를 표시할 수 있습니다. 이러한 지점은 주소일 수도 있고 위도 및 경도 좌표일 수도 있습니다. *trfc* 매개 변수를 사용하여 교통 정보를 표시할 수 있습니다. 길 찾기 유형(운전, 도보 또는 대중교통)을 지정하려면 *mode* 매개 변수를 사용합니다. *mode*를 지정하지 않으면 사용자의 기본 교통 설정 모드를 사용하여 길 찾기가 제공됩니다. 이러한 매개 변수 및 기타 매개 변수에 대한 자세한 내용은 [bingmaps: 매개 변수 참조](#bingmaps-param-reference)를 참조하세요.

![길 찾기의 예](images/windowsmapgcdirections.png)

| 샘플 URI                                                                                                              | 결과                                                                                                                                                         |
|-------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| bingmaps:?rtp=pos.44.9160\_-110.4158~pos.45.0475\_-109.4187                                                             | 지점 간 길 찾기가 포함된 지도를 표시합니다. *mode*를 지정하지 않았으므로 사용자의 교통 기본 설정 모드를 사용하여 길 찾기가 제공됩니다. |
| bingmaps:?cp=43.0332~-87.9167&amp;trfc=1                                                                                    | 위스콘신주 밀워키를 중심으로 하는 지도에 교통 정보를 표시합니다.                                                                                                        |
| bingmaps:?rtp=adr.One Microsoft Way, Redmond, WA 98052~pos.39.0731\_-108.7238                                           | 지정된 주소에서 지정된 위치로의 길 찾기를 포함하는 지도를 표시합니다.                                                                            |
| bingmaps:?rtp=adr.1%20Microsoft%20Way,%20Redmond,%20WA,%2098052~pos.36.1223\_-111.9495\_Grand%20Canyon%20northern%20rim | 마이크로소프트 웨이(1 Microsoft Way, Redmond, WA, 98052)에서 그랜드 캐니언 북쪽 경계까지의 길 찾기를 표시합니다.                                                                |
| bingmaps:?rtp=adr.Davenport, CA~adr.Yosemite Village                                                                    | 지정된 위치에서 지정된 랜드마크로의 운전 길 찾기를 포함하는 지도를 표시합니다.                                                                   |
| bingmaps:?rtp=adr.Mountain%20View,%20CA~adr.San%20Francisco%20International%20Airport,%20CA&amp;mode=d                      | 캘리포니아주 마운틴뷰에서 샌프란시스코 국제공항까지의 운전 길 찾기를 표시합니다.                                                                  |
| bingmaps:?rtp=adr.Mountain%20View,%20CA~adr.San%20Francisco%20International%20Airport,%20CA&amp;mode=w                      | 캘리포니아주 마운틴뷰에서 샌프란시스코 국제공항까지의 도보 길 찾기를 표시합니다.                                                                  |
| bingmaps:?rtp=adr.Mountain%20View,%20CA~adr.San%20Francisco%20International%20Airport,%20CA&amp;mode=t                      | 캘리포니아주 마운틴뷰에서 샌프란시스코 국제공항까지의 대중교통 길 찾기를 표시합니다.                                                                  |

## <a name="display-turn-by-turn-directions"></a>턴바이턴 길 찾기 표시


**ms-drive-to:** 및 **ms-walk-to:** URI 체계를 사용하면 경로의 턴바이턴 뷰를 바로 실행할 수 있습니다. 이러한 URI 체계는 사용자의 현재 위치에서의 길 찾기만 제공할 수 있습니다. 사용자의 현재 위치가 포함되지 않은 지점 간 길 찾기를 제공해야 하는 경우 이전 섹션에 설명된 대로 **bingmaps:** URI 체계를 사용하세요. 이러한 URI 체계에 대한 자세한 내용은 [ms-drive-to:](#ms-drive-to-param-reference) 및 [ms-walk-to:](#ms-walk-to-param-reference) 매개 변수 참조를 참조하세요.

> **중요**  **ms-drive-to:** 또는 **ms-walk-to:** URI 체계가 시작되면 지도 앱은 장치에 GPS 위치 수정이 적용된 적이 있는지 확인합니다. 이 기능이 적용된 경우 지도 앱은 턴바이턴 길 찾기를 진행합니다. 이 기능이 적용되지 않은 경우 앱은 [길 찾기와 교통량 표시](#display-directions-and-traffic)의 설명대로 경로 개요를 표시합니다.

![턴바이턴 길 찾기의 예](images/windowsmapsappdirections.png)

| 샘플 URI                                                                                                | 결과                                                                                       |
|-----------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------|
| ms-drive-to:?destination.latitude=47.680504&amp;destination.longitude=-122.328262&amp;destination.name=Green Lake | 사용자의 현재 위치에 그린 레이크까지의 턴바이턴 운전 길 찾기가 포함된 지도를 표시합니다. |
| ms-walk-to:?destination.latitude=47.680504&amp;destination.longitude=-122.328262&amp;destination.name=Green Lake  | 사용자의 현재 위치에 그린 레이크까지의 턴바이턴 도보 길 찾기가 포함된 지도를 표시합니다. |


## <a name="download-offline-maps"></a>오프라인 지도 다운로드

**ms-settings:** URI 체계를 사용하면 설정 앱의 특정 페이지를 바로 시작할 수 있습니다. **ms-settings:** URI 체계는 지도 앱을 실행하지 않지만 설정 앱의 오프라인 지도 페이지를 바로 시작할 수 있게 하며, 지도 앱에서 사용하는 오프라인 지도를 다운로드하기 위한 확인 대화 상자를 표시합니다. URI 체계는 위도 및 경도로 지정된 지점을 사용하고 해당 지점을 포함하는 지역에 사용할 수 있는 오프라인 지도가 있는지 여부를 자동으로 확인합니다.  전달된 위도와 경도가 여러 다운로드 지역 내에 해당하는 경우 확인 대화 상자를 통해 사용자가 다운로드할 지역을 선택할 수 있습니다. 해당 지점을 포함하는 지역에 오프라인 지도를 사용할 수 없는 경우 설정 앱의 오프라인 지도 페이지가 오류 대화 상자와 함께 표시됩니다.

| 샘플 URI  | 결과 |
|-------------|---------|
| ms-settings:maps-downloadmaps?latlong=47.6,-122.3 | 설정 앱에서 오프라인 지도 페이지를 열고 지정된 위도-경도 지점을 포함하는 지역의 지도를 다운로드하기 위한 확인 대화 상자가 표시됩니다. |

<span id="bingmaps-param-reference"/>

## <a name="bingmaps-parameter-reference"></a>bingmaps: 매개 변수 참조

이 표의 각 매개 변수에 대한 구문이 증강 Backus–Naur 양식(ABNF)을 사용하여 표시됩니다.

<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">매개 변수</th>
<th align="left">정의</th>
<th align="left">ABNF 정의 및 예제</th>
<th align="left">세부 정보</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><b>cp</b></p></td>
<td align="left"><p>중심점</p></td>
<td align="left"><p>cp = "cp=" cpval</p>
<p>cpval = degreeslat "~" degreeslon</p>
<p>degreeslat = ["-"] 1*3DIGIT ["." 1*7DIGIT]</p>
<p>degreeslon = ["-"] 1*2DIGIT ["." 1*7DIGIT]</p>
<p>예제:</p>
<p>cp=40.726966~-74.006076</p></td>
<td align="left"><p>두 값 모두 10진수 각도로 표시되고 물결표(<b>~</b>)로 구분되어야 합니다.</p>
<p>유효한 경도 값은 -180과 180(포함) 사이입니다.</p>
<p>유효한 위도 값은 -90과 90(포함) 사이입니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><b>bb</b></p></td>
<td align="left"><p>경계 상자</p></td>
<td align="left"><p>bb = "bb=" southlatitude "_" westlongitude "~" northlatitude "_" eastlongitude</p>
<p>southlatitude = degreeslat</p>
<p>northlatitude = degreeslat</p>
<p>westlongitude = degreeslon</p>
<p>eastlongitude = degreeslon</p>
<p>degreeslat = ["-"] 13DIGIT ["." 17DIGIT]</p>
<p>degreeslon = ["-"] 12DIGIT ["." 17DIGIT]</p>
<p>예제:</p>
<p>bb=39.719_-74.52~41.71_-73.5</p></td>
<td align="left"><p>물결표(<b>~</b>)를 사용해 오른쪽 위 모서리에서 왼쪽 아래 모서리를 분리하여 경계 상자를 지정하는 직사각형 영역이 10진수 각도로 표현되었습니다. 각 위도 및 경도가 밑줄(<b>_</b>)로 구분됩니다.</p>
<p>유효한 경도 값은 -180과 180(포함) 사이입니다.</p>
<p>유효한 위도 값은 -90과 90(포함) 사이입니다.</p><p>cp 및 lvl 매개 변수는 경계 상자가 제공된 경우 무시됩니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><b>각 항목은 다음을 의미합니다.</b></p></td>
<td align="left"><p>위치</p></td>
<td align="left"><p>where = "where=" whereval</p>
<p>whereval = 1 *( ALPHA / DIGIT / "-" / "." / "_" / pct-encoded / "!" / "$" / "'" / "(" / ")" / "*" / "+" / "," / ";" / ":" / "@" / "/" / "?")</p>
<p>예제:</p>
<p>where=1600%20Pennsylvania%20Ave,%20Washington,%20DC</p></td>
<td align="left"><p>특정 위치, 랜드마크 또는 장소에 대한 검색 용어입니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><b>q</b></p></td>
<td align="left"><p>쿼리 용어</p></td>
<td align="left"><p>q = "q="</p>
<p>whereval</p>
<p>예제:</p>
<p>q=mexican%20restaurants</p></td>
<td align="left"><p>로컬 비즈니스 또는 비즈니스의 범주에 대한 검색 용어입니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><b>lvl</b></p></td>
<td align="left"><p>확대/축소 수준</p></td>
<td align="left"><p>lvl = "lvl=" 1<i>2DIGIT ["." 1</i>2DIGIT]</p>
<p>예제:</p>
<p>lvl=10.50</p></td>
<td align="left"><p>지도 보기의 확대/축소 수준을 정의합니다. 유효한 값은 1-20이며 여기서 1이 축소됩니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><b>sty</b></p></td>
<td align="left"><p>스타일</p></td>
<td align="left"><p>sty = "sty=" ("a" / "r"/"3d")</p>
<p>예제:</p>
<p>sty=a</p></td>
<td align="left"><p>지도 스타일을 정의합니다. 이 매개 변수의 유효한 값은 다음과 같습니다.</p>
<ul>
<li>**a**: 지도의 위성뷰를 표시합니다.</li>
<li>**r**: 지도의 도로 보기를 표시합니다.</li>
<li>**3d**: 지도의 3D 보기를 표시합니다. **cp** 매개 변수 및 **rad** 매개 변수(옵션)와 함께 사용합니다.</li>
</ul>
<p>Windows 10에서는 위성뷰 및 3D 보기 스타일이 같습니다.</p>
<div class="alert">
**참고**sty과 동일한 결과가 생성 **sty** 매개 변수를 생략 하면 = r.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p><b>rad</b></p></td>
<td align="left"><p>반경</p></td>
<td align="left"><p>rad = "rad=" 1*8DIGIT</p>
<p>예제:</p>
<p>rad=1000</p></td>
<td align="left"><p>원하는 지도 보기를 지정하는 원형 영역입니다. 반경 값은 미터 단위로 측정됩니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><b>pit</b></p></td>
<td align="left"><p>피치</p></td>
<td align="left"><p>pit = "pit=" pitch</p>
<p>예제:</p>
<p>pit=60</p></td>
<td align="left"><p>지도를 바라보는 각도를 나타냅니다. 90(최대값)은 수평으로 바라본 모습이고, 0(최소값)은 수직으로 아래를 바라본 모습입니다.</p><p>유효한 피치 값은 0과 90(포함) 사이입니다.</td>
</tr>
<tr class="odd">
<td align="left"><p><b>hdg</b></p></td>
<td align="left"><p>제목</p></td>
<td align="left"><p>hdg = "hdg=" heading</p>
<p>예제:</p>
<p>hdg=180</p></td>
<td align="left"><p>지도가 향하는 방향(각도)을 나타냅니다. 0 또는 360은 북쪽, 90은 동쪽, 180은 남쪽, 270은 서쪽입니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><b>ss</b></p></td>
<td align="left"><p>Streetside</p></td>
<td align="left"><p>ss = "ss=" BIT</p>
<p>예제:</p>
<p>ss=1</p></td>
<td align="left"><p><code>ss=1</code>이면 거리 수준 이미지가 표시됩니다. <b>ss</b> 매개 변수를 생략하면 <code>ss=0</code>과 동일한 결과가 생성됩니다. <b>cp</b> 매개 변수와 함께 사용하여 거리 수준 보기의 위치를 지정합니다.</p>
<div class="alert">
**참고**일부 지역에서는 거리 수준 이미지를 사용할 수 없습니다.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p><b>trfc</b></p></td>
<td align="left"><p>교통량</p></td>
<td align="left"><p>trfc = "trfc=" BIT</p>
<p>예제:</p>
<p>trfc=1</p></td>
<td align="left"><p>지도에 교통 정보가 포함되는지 여부를 지정합니다. trfc 매개 변수를 생략하면 <code>trfc=0</code>과 동일한 결과가 생성됩니다.</p>
<div class="alert">
**참고**교통량 데이터는 일부 지역에서는 사용할 수 없습니다.
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left"><p><b>rtp</b></p></td>
<td align="left"><p>경로</p></td>
<td align="left"><p>rtp = "rtp=" (waypoint "~" [waypoint]) / ("~" waypoint)</p>
<p>waypoint = ("pos." point ) / ("adr." whereval)</p>
<p>point = "point." pointval ["_" title]</p>
<p>pointval = degreeslat "" degreeslon</p>
<p>degreeslat = ["-"] 13DIGIT ["." 17DIGIT]</p>
<p>degreeslon = ["-"] 12DIGIT ["." 17DIGIT]</p>
<p>title = whereval</p>
<p>whereval = 1( ALPHA / DIGIT / "-" / "." / "_" / pct-encoded / "!" / "$" / "'" / "(" / ")" / "" / "+" / "," / ";" / ":" / "@" / "/" / "?")</p>


<p>예제:</p>
<p>rtp=adr.Mountain%20View,%20CA~adr.SFO</p>
<p>rtp=adr.One%20Microsoft%20Way,%20Redmond,%20WA~pos.45.23423_-122.1232 _My%20Picnic%20Spot</p></td>
<td align="left"><p>경로의 시작 및 종료를 정의하여 지도에 그립니다. 이때 물결표(<b>~</b>)로 구분합니다. 각 웨이포인트는 위도, 경도 및 선택적 제목을 사용한 위치 또는 주소 식별자로 정의됩니다.</p>
<p>전체 경로는 정확히 두 웨이포인트를 포함합니다. 예를 들어 두 개의 웨이포인트가 있는 경로는 <code>rtp="A"~"B"</code>와 같이 정의됩니다.</p>
<p>불완전한 경로를 지정할 수도 있습니다. 예를 들어 <code>rtp="A"~</code>를 사용하여 경로 시작만을 정의할 수 있습니다. 이 경우, 길 찾기 입력은 포커스가 있는 **시작** 필드와 **끝** 필드에 제공된 웨이포인트로 표시됩니다.</p>
<p><code>rtp=~"B"</code>와 마찬가지로 경로의 끝만 지정된 경우 길 찾기 패널은 **끝** 필드로 제공된 웨이포인트로 표시됩니다. 정확한 현재 위치를 사용할 수 있는 경우 현재 위치는 포커스가 있는 **시작:** 필드에 미리 채워집니다.</p>
<p>불완전한 경로를 지정하면 경로 선이 그려지지 않습니다.</p>
<p>**mode** 매개 변수와 함께 사용하여 교통 모드(운전, 대중교통 또는 도보)를 지정할 수 있습니다. **mode**를 지정하지 않으면 사용자의 교통 기본 설정 모드를 사용하여 길 찾기가 제공됩니다.</p>
<div class="alert">
**참고** **pos** 매개 변수 값으로 위치를 지정한 경우 위치에 제목을 사용할 수 있습니다. 위도 및 경도를 표시하는 대신 제목이 표시됩니다.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p><b>모드</b></p></td>
<td align="left"><p>교통 모드</p></td>
<td align="left"><p>mode = "mode=" ("d" / "t" / "w")</p>
<p>예제:</p>
<p>mode=d</p></td>
<td align="left"><p>교통 모드를 정의합니다. 이 매개 변수의 유효한 값은 다음과 같습니다.</p>
<ul>
<li>**d**: 운전 길 찾기에 대한 경로 개요 표시</li>
<li>**t**: 대중교통 길 찾기에 대한 개요 표시</li>
<li>**w**: 도보 길 찾기에 대한 개요 표시</li>
</ul>
<p>교통 길 찾기에 **rtp** 매개 변수와 함께 사용합니다. **mode**를 지정하지 않으면 사용자의 교통 기본 설정 모드를 사용하여 길 찾기가 제공됩니다. 경로 매개 변수 없이 **mode**를 제공하여 현재 위치에서 해당 모드에 대한 길 찾기 입력을 제공할 수 있습니다.</p></td>
</tr>

<tr class="even">
<td align="left"><p><b>collection</b></p></td>
<td align="left"><p>컬렉션</p></td>
<td align="left"><p>collection = "collection="(name"~"/)point["~"point]</p>
<p>name = "name." whereval </p>
<p>whereval = 1( ALPHA / DIGIT / "-" / "." / "_" / pct-encoded / "!" / "$" / "'" / "(" / ")" / "" / "+" / "," / ";" / ":" / "@" / "/" / "?") </p>
<p>point = "point." pointval ["_" title] </p>
<p>pointval = degreeslat "" degreeslon </p>
<p>degreeslat = ["-"] 13DIGIT ["." 17DIGIT] </p>
<p>degreeslon = ["-"] 12DIGIT ["." 17DIGIT] </p>
<p>title = whereval</p>


<p>예제:</p>
<p>collection=name.My%20Trip%20Stops~point.36.116584_-115.176753_Las%20Vegas~point.37.8268_-122.4798_Golden%20Gate%20Bridge</p></td>
<td align="left"><p>지도 및 목록에 추가되는 지점의 컬렉션입니다. name 매개 변수를 사용하여 지점 컬렉션의 이름을 지정할 수 있습니다. 지점은 위도, 경도 및 제목(옵션)을 사용하여 지정됩니다.</p>
<p>이름과 여러 지점을 물결표(**~**)로 구분합니다.</p>
<p>지정한 항목에 물결표가 있는 경우 물결표가 <code>%7E</code>로 인코딩되어 있는지 확인합니다. 중심점 및 확대/축소 수준 매개 변수가 함께 제공되지 않는 경우, 컬렉션에서는 최상의 지도 보기가 제공됩니다.</p>

<p>**중요** 지정한 항목에 밑줄이 있는 경우, 밑줄이 %255F로 이중 인코드되어 있는지 확인합니다.</p></td>
</tr>
</tbody>
</table>

  
<span id="ms-drive-to-param-reference"/>

## <a name="ms-drive-to-parameter-reference"></a>ms-drive-to: 매개 변수 참조


턴바이턴 운전 길 찾기 요청을 실행하는 URI는 인코드할 필요가 없으며 다음과 같은 형식을 사용합니다.

> **참고**  이 URI 체계에서는 시작점을 지정하지 않습니다. 항상 현재 위치가 시작점으로 간주됩니다. 현재 위치와 다른 시작점을 지정해야 하는 경우 [길 찾기 및 교통량 표시](#display-directions-and-traffic)를 참조하세요.

 

| 매개 변수 | 정의 | 예제 | 세부 정보 |
|------------|-----------|---------|---------|
| **destination.latitude** | 목적지 위도 | 예: destination.latitude=47.6451413797194 | 목적지의 위도입니다. 유효한 위도 값은 -90과 90(포함) 사이입니다. |
| **destination.longitude** | 목적지 경도 | 예: destination.longitude=-122.141964733601 | 목적지의 경도입니다. 유효한 경도 값은 -180과 180(포함) 사이입니다. |
| **destination.name** | 목적지 이름 | 예: destination.name=Redmond, WA | 목적지의 이름입니다. **destination.name** 값을 인코드할 필요가 없습니다. |

 
<span id="ms-walk-to-param-reference"/>

## <a name="ms-walk-to-parameter-reference"></a>ms-walk-to: 매개 변수 참조


턴바이턴 도보 길 찾기 요청을 실행하는 URI는 인코드할 필요가 없으며 다음과 같은 형식을 사용합니다.

> **참고**  이 URI 체계에서는 시작점을 지정하지 않습니다. 항상 현재 위치가 시작점으로 간주됩니다. 현재 위치와 다른 시작점을 지정해야 하는 경우 [길 찾기 및 교통량 표시](#display-directions-and-traffic)를 참조하세요.
 

| 매개 변수 | 정의 | 예제 | 세부 정보 |
|-----------|------------|---------|----------|
| **destination.latitude** | 목적지 위도 | 예: destination.latitude=47.6451413797194 | 목적지의 위도입니다. 유효한 위도 값은 -90과 90(포함) 사이입니다. |
| **destination.longitude** | 목적지 경도 | 예: destination.longitude=-122.141964733601 | 목적지의 경도입니다. 유효한 경도 값은 -180과 180(포함) 사이입니다. |
| **destination.name** | 목적지 이름 | 예: destination.name=Redmond, WA | 목적지의 이름입니다. **destination.name** 값을 인코드할 필요가 없습니다. |

## <a name="ms-settings-parameter-reference"></a>ms-settings: 매개 변수 참조

**ms-settings:** URI 체계에 대한 지도 앱 특정 매개 변수의 구문은 아래에 정의되어 있습니다. **maps-downloadmaps**는 **ms-settings:maps-downloadmaps?** 형태로 **ms-settings:** URI와 함께 지정되어 오프라인 지도 설정 페이지를 나타냅니다. 

| 매개 변수 | 정의 | 예제 | 세부 정보 |
|-----------|------------|---------|----------|
| **latlong** | 오프라인 지도 지역을 정의하는 지점입니다. | 예: latlong=47.6,-122.3 | geopoint는 쉼표로 구분된 위도 및 경도로 지정됩니다. 유효한 위도 값은 -90과 90(포함) 사이입니다. 유효한 경도 값은 -180과 180(포함) 사이입니다. |

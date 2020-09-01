---
title: Windows 지도 앱 실행
description: Bingmaps, ms-드라이브 간 및 ms-설정 URI 체계를 사용 하 여 앱에서 Windows Maps 앱을 시작 하는 방법을 알아봅니다.
ms.assetid: E363490A-C886-4D92-9A64-52E3C24F1D98
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e020972a8dff0b0721fd2c5726999a7896d359c4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167907"
---
# <a name="launch-the-windows-maps-app"></a>Windows 지도 앱 실행




앱에서 Windows Maps 앱을 시작 하는 방법을 알아봅니다. 이 항목에서는 **bingmaps**에 대해 설명 합니다., **ms 드라이브-대상**:, **ms-연습:** 및 **Ms 설정:** URI (Uniform resource Identifier) 체계입니다. 이러한 URI 체계를 사용 하 여 Windows Maps 앱을 특정 지도, 방향 및 검색 결과에 시작 하거나 설정 앱에서 Windows Maps 오프 라인 맵을 다운로드 합니다.

**팁** 앱에서 Windows Maps 앱을 시작 하는 방법에 대해 자세히 알아보려면 GitHub의 [windows 유니버설 샘플](https://github.com/Microsoft/Windows-universal-samples) 리포지토리에서 [UWP (유니버설 Windows 플랫폼) 맵 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl) 을 다운로드 하세요.

## <a name="introducing-uris"></a>Uri 소개

URI 체계를 사용 하면 응용 프로그램에서 하이퍼링크를 클릭 하거나 프로그래밍 방식으로 앱을 열 수 있습니다. **Mailto** 를 사용 하 여 새 전자 메일을 시작 하거나 **http:** 를 사용 하 여 웹 브라우저를 열 **때와 같이** **Bingmaps** **를 사용**하 여 Windows maps 앱을 열 수 있습니다.

-   **Bingmaps:** URI는 위치, 검색 결과, 방향 및 트래픽에 대 한 맵을 제공 합니다.
-   **Ms-drive to:** URI는 현재 위치에서의 단계별 안내를 제공 합니다.
-   **Ms-to:** URI는 현재 위치에서의 단계별 탐색 방향을 제공 합니다.

예를 들어 다음 URI는 Windows Maps 앱을 열고 뉴욕 도시를 중심으로 지도를 표시 합니다.

```xml
<bingmaps:?cp=40.726966~-74.006076>
```

![뉴욕 도시를 중심으로 하는 지도입니다.](images/mapnyc.png)

URI 체계에 대 한 설명은 다음과 같습니다.

**bingmaps:? 쿼리**

이 URI 체계에서 *쿼리* 는 일련의 매개 변수 이름/값 쌍입니다.

**&param1 = value1&param2 = value2 ...**

사용 가능한 매개 변수의 전체 목록은 [bingmaps:](#bingmaps-param-reference), [ms-drive-to:](#ms-drive-to-param-reference)및 [ms 연습:](#ms-walk-to-param-reference) 매개 변수 참조를 참조 하세요. 이 항목의 뒷부분에도 예제가 있습니다.

## <a name="launch-a-uri-from-your-app"></a>앱에서 URI 시작


앱에서 Windows Maps 앱을 시작 하려면 **bingmaps:**, LaunchUriAsync **: 또는** **ms-to:** URI를 사용 하 여 [**LaunchUriAsync**](/uwp/api/windows.system.launcher.launchuriasync) 메서드를 호출 합니다. 다음 예제에서는 이전 예제와 동일한 URI를 실행합니다. URI를 통해 앱을 시작 하는 방법에 대 한 자세한 내용은 [uri에 대 한 기본 앱 시작](launch-default-app.md)을 참조 하세요.

```cs
// Center on New York City
var uriNewYork = new Uri(@"bingmaps:?cp=40.726966~-74.006076");

// Launch the Windows Maps app
var launcherOptions = new Windows.System.LauncherOptions();
launcherOptions.TargetApplicationPackageFamilyName = "Microsoft.WindowsMaps_8wekyb3d8bbwe";
var success = await Windows.System.Launcher.LaunchUriAsync(uriNewYork, launcherOptions);
```

이 예제에서는 [**Launcheroptions**](/uwp/api/Windows.System.LauncherOptions) 클래스를 사용 하 여 Windows Maps 앱이 시작 되도록 합니다.

## <a name="display-known-locations"></a>알려진 위치 표시

지도에서 표시할 부분을 제어 하는 다양 한 옵션이 있습니다. *Cp* (중심점) 매개 변수를 *rad* (radius) 또는 *lvl* (확대/축소 수준) 매개 변수와 함께 사용 하 여 위치를 표시 하 고 축소 하는 방법을 선택할 수 있습니다. *Cp* 매개 변수를 사용 하는 경우 *hdg* (제목) 및 *pit* (피치)를 지정 하 여 찾을 방향을 제어할 수도 있습니다. 또 다른 방법은 *bb* (경계 상자) 매개 변수를 사용 하 여 표시 하려는 영역에 대 한 최대 남부, 동부, 북부 및 서쪽 좌표를 제공 하는 것입니다.

뷰의 유형을 제어 하려면 *sty* (style) 및 *ss* (Streetside) 매개 변수를 사용 합니다. *Sty* 매개 변수를 사용 하 여도로 보기와 항공 뷰 간을 전환할 수 있습니다. *Ss* 매개 변수는 지도를 Streetside 뷰에 넣습니다. 이러한 매개 변수와 기타 매개 변수에 대 한 자세한 내용은 [bingmaps: 매개 변수 참조](#bingmaps-param-reference)를 참조 하세요.


| 샘플 URI                                                                 | 결과                                                                                                                                                                                        |
|----------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| bingmaps:?                                                                 | Maps 앱을 엽니다.                                                                                                                                                                            |
| bingmaps:? cp = 40.726966 ~-74.006076                                          | 뉴욕 도시를 중심으로 지도를 표시 합니다.                                                                                                                                                    |
| bingmaps:? cp = 40.726966 ~-74.006076&lvl = 10                                   | 새 도시를 중심으로 하는 지도를 확대/축소 수준 10으로 표시 합니다.                                                                                                                            |
| bingmaps:? bb = 39.719 \_ -74.52 ~ 41.71 \_ -73.5                                   | **Bb** 인수에 지정 된 영역인 뉴욕 도시의 지도를 표시 합니다.                                                                                                           |
| bingmaps:? bb = 39.719 \_ -74.52 ~ 41.71 \_ -73.5&cp = 47 ~-122                        | 경계 상자 인수에 지정 된 영역인 뉴욕 도시의 지도를 표시 합니다. **Cp** 인수에 지정 된 시애틀의 중심점은 *bb* 가 지정 되었기 때문에 무시 됩니다. |
| bingmaps:? collection = 36.116584 \_ -115.176753 \_ Caesars% 20palace&lvl = 16 | Caesars 왕궁 (Las Vegas) 라는 점이 있는 지도를 표시 하 고 확대/축소 수준을 16으로 설정 합니다.                                                                                                 |
| bingmaps:? collection = 40.726966 \_ -74.006076 \_ Some% 255FBusiness        | Las Vegas에서 이름이 Business 인 지도를 표시 \_ 합니다.                                                                                                                               |
| bingmaps:? cp = 40.726966 ~-74.006076&trfc = 1&sty = a                             | 트래픽이 있는 뉴욕 도시 맵과 항공 지도 스타일을 표시 합니다.                                                                                                                          |
| bingmaps:? cp = 47.6204 ~-122.3491&sty = 3d                                      | 우주 니 들의 3D 뷰를 표시 합니다.                                                                                                                                                        |
| bingmaps:? cp = 47.6204 ~-122.3491&sty = 3d&rad = 200&pit = 75&hdg = 165               | 반지름이 200m이 고, 75도 이며, 165도의 머리글로 공간 니 들의 3D 뷰를 표시 합니다.                                                                             |
| bingmaps:? cp = 47.6204 ~-122.3491&ss = 1                                        | 우주 니 들의 Streetside 뷰를 표시 합니다.                                                                                                                                                |


## <a name="display-search-results"></a>검색 결과 표시

*Q* 매개 변수를 사용 하 여 위치를 검색할 때 용어를 가능한 한 구체적으로 설정 하 고 *cp*, *bb*또는 *where* 매개 변수를 사용 하 여 검색 위치를 지정 하는 것이 좋습니다. 검색 위치를 지정 하지 않고 사용자의 현재 위치를 사용할 수 없는 경우 검색에서 의미 있는 결과를 반환 하지 않을 수 있습니다. 검색 결과가 가장 적절 한 지도 보기에 표시 됩니다. 이러한 매개 변수와 기타 매개 변수에 대 한 자세한 내용은 [bingmaps: 매개 변수 참조](#bingmaps-param-reference)를 참조 하세요.


| 샘플 URI                                                    | 결과                                                                            |
|---------------------------------------------------------------|------------------------------------------------------------------------------------|
| bingmaps:? q = 1600% 20Pennsylvania% 20Ave,% 20Ave,% 20AVE     | 지도를 표시 하 고 워싱턴, D.C에서 백색 집의 주소를 검색 합니다. |
| bingmaps:? q = 커피&(여기서 = 시애틀                              | 시애틀에서 커피를 검색 합니다.                                                    |
| bingmaps:? cp = 40.726966 ~-74.006076&(여기서 = New% 20York            | 지정 된 중심점 근처의 뉴욕을 검색 합니다.                             |
| bingmaps:? bb = 39.719 \_ -74.52 ~ 41.71 \_ -73.5&q = 피자              | 지정 된 경계 상자 (뉴욕 도시)에서 피자를 검색 합니다.      |

 
## <a name="display-multiple-points"></a>여러 개의 요소 표시


*컬렉션* 매개 변수를 사용 하 여 지도의 사용자 지정 요소 집합을 표시 합니다. 점이 둘 이상인 경우에는 점 목록이 표시 됩니다. 컬렉션에는 최대 25 개의 점이 있을 수 있으며,이는 제공 된 순서 대로 나열 됩니다. 컬렉션은 검색 및 지침 요청 보다 우선 합니다. 이 매개 변수 및 기타 매개 변수에 대 한 자세한 내용은 [bingmaps: 매개 변수 참조](#bingmaps-param-reference)를 참조 하세요.

| 샘플 URI | 결과                                                                                                                   |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------|
| bingmaps:? collection = 36.116584 \_ -115.176753 \_ Caesars% 20palace                                                                                                | Las Vegas에서 Caesar의 왕궁를 검색 하 고 가장 적합 한 지도 보기에서 지도에 결과를 표시 합니다.                         |
| bingmaps:? collection = 36.116584 \_ -115.176753 \_ Caesars% 20palace&lvl = 16                                                                                         | Las Vegas에 Caesars 왕궁 라는 압정을 표시 하 고 수준 16으로 확대/축소 합니다.                                               |
| bingmaps:? collection = 36.116584 \_ -115.176753 \_ Caesars% 20palace ~ 36.113126 \_ -115.175188 the \_ % 20Bellagio&lvl = 16&cp = 36.114902 ~-115.176669                   | Caesars 왕궁 라는 압정을 표시 하 고 Bellagio를 Las Vegas에 표시 하며 수준 16으로 확대/축소 합니다.              |
| bingmaps:? collection = 40.726966 \_ -74.006076 \_ N% 255FBusiness% 255Fwith% 255FUnderscore                                                                        | 밑줄을 사용 하 여 가짜 라는 압정으로 뉴욕을 표시 \_ \_ \_ 합니다.                                                  |
| bingmaps:? collection = name. 호텔% 20List ~ 36.116584 \_ -115.176753 \_ Caesars% 20list ~ 36.113126 \_ -115.175188 the \_ % 20Bellagio&lvl = 16&cp = 36.114902 ~-115.176669 | 호텔 목록 이라는 목록과 Caesars 왕궁 압정 두 개를 표시 하 고 Las Vegas에 Bellagio를 표시 한 후 수준 16으로 확대/축소 합니다. |

 

## <a name="display-directions-and-traffic"></a>방향 및 트래픽 표시


*Rtp* 매개 변수를 사용 하 여 두 요소 사이에 방향을 표시할 수 있습니다. 이러한 점은 주소 또는 위도 및 경도 좌표가 될 수 있습니다. 트래픽 정보를 표시 하려면 *trfc* 매개 변수를 사용 합니다. 방향 유형 (구동, 탐색 또는 전송)을 지정 하려면 *mode* 매개 변수를 사용 합니다. *Mode* 가 지정 되지 않은 경우 사용자의 기본 설정 모드를 사용 하 여 방향이 제공 됩니다. 이러한 매개 변수 및 기타 매개 변수에 대 한 자세한 내용은 [bingmaps: 매개 변수 참조](#bingmaps-param-reference)를 참조 하세요.

![지침의 예](images/windowsmapgcdirections.png)

| 샘플 URI                                                                                                              | 결과                                                                                                                                                         |
|-------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| bingmaps:? rtp = 44.9160 \_ -110.4158 ~ pos. 45.0475 \_ -109.4187                                                             | 지점 간 방향이 있는 지도를 표시 합니다. *Mode* 를 지정 하지 않았으므로 사용자의 교통 기본 설정 모드를 사용 하 여 방향이 제공 됩니다. |
| bingmaps:? cp = 43.0332 ~-87.9167&trfc = 1                                                                                    | Milwaukee, WI-FI를 사용 하 여 중앙에서 트래픽을 표시 하는 지도를 표시 합니다.                                                                                                        |
| bingmaps:? rtp = adr. 한 가지 Microsoft 방법, Redmond, WA 98052 ~ 39.0731 \_ -108.7238                                           | 지정 된 주소에서 지정 된 위치로 방향이 있는 지도를 표시 합니다.                                                                            |
| bingmaps:? rtp = adr% 20Microsoft% 20Microsoft,% 20Microsoft,% 20MICROSOFT, %2098052 ~ pos. 36.1223 \_ -111.9495% \_ 20microsoft% 20microsoft% 20microsoft | 1 가지 Microsoft 방식, Redmond, WA, 98052에서 그랜드 협곡 북부 테두리 까지의 방향을 표시 합니다.                                                                |
| bingmaps:? rtp = adr. Davenport, CA ~ adr. Yosemite 마을                                                                    | 지정 된 위치에서 지정 된 랜드마크로의 주행 방향이 있는 지도를 표시 합니다.                                                                   |
| bingmaps:? rtp = adr. 산지% 20View,% 20VIEW ~ adr. San% 20Francisco% 20Francisco% 20Francisco,% 20FRANCISCO&모드 = d                      | 산지 보기, CA에서 San 샌프란시스코 국제 공항, CA의 주행 안내를 표시 합니다.                                                                  |
| bingmaps:? rtp = adr. 산지% 20View,% 20VIEW ~ adr. San% 20Francisco% 20Francisco% 20Francisco,% 20FRANCISCO&모드 = w                      | 산지 보기, CA에서 San 샌프란시스코 국제 공항, CA의 탐색 방향을 표시 합니다.                                                                  |
| bingmaps:? rtp = adr. 산지% 20View,% 20VIEW ~ adr. San% 20Francisco% 20Francisco% 20Francisco,% 20FRANCISCO&mode = t                      | 산지 보기, CA에서 San 샌프란시스코 국제 공항, CA로의 전송 방향을 표시 합니다.                                                                  |

## <a name="display-turn-by-turn-directions"></a>턴 턴 방향 표시


**Ms-drive to:** 및 **ms 단계별:** URI 체계를 사용 하 여 경로의 턴 턴 뷰로 직접 시작할 수 있습니다. 이러한 URI 체계는 사용자의 현재 위치에서의 방향 으로만 제공할 수 있습니다. 사용자의 현재 위치를 포함 하지 않는 요소 사이에 방향을 지정 해야 하는 경우 이전 섹션에 설명 된 대로 **bingmaps:** URI 체계를 사용 합니다. 이러한 URI 체계에 대 한 자세한 내용은 [ms-drive to:](#ms-drive-to-param-reference) 및 [ms 연습:](#ms-walk-to-param-reference) 매개 변수 참조를 참조 하세요.

> **중요**  **Ms-drive to:** 또는 **ms-to:** URI 체계가 시작 될 때 Maps 앱은 장치에 GPS 위치 수정이 있는지 확인 합니다. 있는 경우 Maps 앱이 계속 해 서 턴 방향으로 진행 됩니다. 그렇지 않은 경우 [지침 및 트래픽 표시](#display-directions-and-traffic)에 설명 된 대로 앱에 경로 개요가 표시 됩니다.

![턴 턴 방향에 대 한 예제](images/windowsmapsappdirections.png)

| 샘플 URI                                                                                                | 결과                                                                                       |
|-----------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------|
| ms-drive to:? destination. 위도 = 47.680504&destination. 경도 =-122.328262&destination. 이름 = 녹색 Lake | 현재 위치에서 녹색 Lake에 대 한 턴 턴 방향이 있는 지도를 표시 합니다. |
| ms-to:? destination. 위도 = 47.680504&destination. 경도 =-122.328262&destination. 이름 = 녹색 Lake  | 현재 위치의 녹색 Lake를 턴 턴 방향으로 이동 하는 지도를 표시 합니다. |


## <a name="download-offline-maps"></a>오프 라인 맵 다운로드

**Ms 설정:** URI 체계를 사용 하면 설정 앱에서 특정 페이지로 직접 시작할 수 있습니다. **Ms 설정:** URI 체계가 맵 앱에 시작 되지 않지만, 설정 앱에서 오프 라인 맵 페이지를 직접 시작 하 고 맵 앱에서 사용 하는 오프 라인 맵을 다운로드 하는 확인 대화 상자를 표시할 수 있습니다. URI 체계는 위도 및 경도로 지정 된 지점을 수락 하 고 해당 지점을 포함 하는 영역에 사용할 수 있는 오프 라인 맵이 있는지 자동으로 결정 합니다.  전달 된 위도 및 경도가 여러 다운로드 지역에 속하는 경우 확인 대화 상자에서 다운로드할 지역을 선택할 수 있습니다. 해당 지점을 포함 하는 지역에서 오프 라인 맵을 사용할 수 없는 경우 설정 앱의 오프 라인 맵 페이지가 오류 대화 상자와 함께 표시 됩니다.

| 샘플 URI  | 결과 |
|-------------|---------|
| ms 설정: maps-downloadmaps? latlong = 47.6,-122.3 | 지정 된 위도-경도 지점을 포함 하는 지역에 대 한 지도를 다운로드 하는 확인 대화 상자와 함께 오프 라인 맵 페이지에 설정 앱을 엽니다. |

<span id="bingmaps-param-reference"/>

## <a name="bingmaps-parameter-reference"></a>bingmaps: 매개 변수 참조

이 테이블의 각 매개 변수에 대 한 구문은 Backus-naur Form (ABNF)을 사용 하 여 표시 됩니다.

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
<td align="left"><p>cp = "cp =" cpval</p>
<p>cpval = degreeslat "~" degreeslon</p>
<p>degreeslat = ["-"] 1*3Digit ["." 1*7digit]</p>
<p>degreeslon = ["-"] 1*2Digit ["." 1*7digit]</p>
<p>예제:</p>
<p>cp = 40.726966 ~-74.006076</p></td>
<td align="left"><p>두 값은 모두 10 진수 각도로 표시 되 고 물결표 ()로 구분 되어야 합니다 <b>~</b> .</p>
<p>유효한 경도 값은-180에서 + 180 포함 사이입니다.</p>
<p>유효한 위도 값은-90에서 + 90 포함 사이입니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><b>게시판</b></p></td>
<td align="left"><p>경계 상자</p></td>
<td align="left"><p>bb = "bb =" southlatitude "_" westlongitude "~" northlatitude "_" e/경도</p>
<p>southlatitude = degreeslat</p>
<p>northlatitude = degreeslat</p>
<p>westlongitude = degreeslon</p>
<p>eastlongitude = degreeslon</p>
<p>degreeslat = ["-"] 13DIGIT ["." 17DIGIT]</p>
<p>degreeslon = ["-"] 12DIGIT ["." 17DIGIT]</p>
<p>예제:</p>
<p>bb = 39.719 _-74.52 ~ 41.71 _-73.5</p></td>
<td align="left"><p>물결표 ()를 사용 하 여 오른쪽 위 모퉁이에서 왼쪽 아래 모퉁이를 분리 하 고 10 진수도로 표현 된 경계 상자를 지정 하는 사각형 영역 <b>~</b> 입니다. 각에 대 한 위도 및 경도는 밑줄 (<b>_</b>)로 구분 됩니다.</p>
<p>유효한 경도 값은-180에서 + 180 포함 사이입니다.</p>
<p>유효한 위도 값은-90에서 + 90 포함 사이입니다.</p><p>Cp 및 lvl 매개 변수는 경계 상자를 제공 하면 무시 됩니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><b>where</b></p></td>
<td align="left"><p>위치</p></td>
<td align="left"><p>여기서 = "where =" whereval</p>
<p>whereval = 1 *(ALPHA/DIGIT/"-"/"."/"_"/pct 인코딩된/"!"/"$"/"'"/"("/")"/"*"/"+"/","/";"/":"/"/"/" @" / " ?")</p>
<p>예제:</p>
<p>여기서 = 1600% 20Pennsylvania% 20Ave,% 20Ave,% 20AVE</p></td>
<td align="left"><p>특정 위치, 랜드마크 또는 장소에 대 한 용어를 검색 합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><b>대답</b></p></td>
<td align="left"><p>쿼리 용어</p></td>
<td align="left"><p>q = "q ="</p>
<p>whereval</p>
<p>예제:</p>
<p>q = 멕시코% 20restaurants</p></td>
<td align="left"><p>지역 비즈니스 또는 비즈니스 범주에 대 한 용어를 검색 합니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><b>lvl</b></p></td>
<td align="left"><p>확대/축소 수준</p></td>
<td align="left"><p>lvl = "lvl =" 1<i>2Digit ["." 1</i>2digit]</p>
<p>예제:</p>
<p>lvl = 10.50</p></td>
<td align="left"><p>지도 보기의 확대/축소 수준을 정의 합니다. 유효한 값은 1-20입니다. 여기서 1은 모든 방식으로 축소 됩니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><b>sty</b></p></td>
<td align="left"><p>스타일</p></td>
<td align="left"><p>sty = "sty =" ("a"/"r"/"3d")</p>
<p>예제:</p>
<p>sty = a</p></td>
<td align="left"><p>지도 스타일을 정의 합니다. 이 매개 변수에 유효한 값은 다음과 같습니다.</p>
<ul>
<li><b>a</b>: 지도의 항공 보기를 표시 합니다.</li>
<li><b>r</b>: 지도의도로 보기를 표시 합니다.</li>
<li><b>3d</b>: 지도의 3d 뷰를 표시 합니다. <b>Cp</b> 매개 변수와 함께 사용 하 고 선택적으로 <b>rad</b> 매개 변수와 함께 사용 합니다.</li>
</ul>
<p>Windows 10에서 항공 view 및 3D 보기 스타일은 동일 합니다.</p>
<div class="alert">
<b>참고</b>    <b>Sty</b> 매개 변수를 생략 하면 sty = r과 동일한 결과가 생성 됩니다.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p><b>rad</b></p></td>
<td align="left"><p>반지름</p></td>
<td align="left"><p>rad = "rad =" 1 * 8 자리</p>
<p>예제:</p>
<p>rad = 1000</p></td>
<td align="left"><p>원하는 지도 보기를 지정 하는 원형 영역입니다. 반지름 값은 미터 단위로 측정 됩니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p><b>pit</b></p></td>
<td align="left"><p>피치</p></td>
<td align="left"><p>pit = "pit =" 피치</p>
<p>예제:</p>
<p>pit = 60</p></td>
<td align="left"><p>지도를 표시 하는 각도를 나타냅니다. 여기서 90는 가로 (최대값)를 확인 하 고 0은 아래로 (최소) 조회 합니다.</p><p>유효한 피치 값은 0과 90(포함) 사이입니다.</td>
</tr>
<tr class="odd">
<td align="left"><p><b>hdg</b></p></td>
<td align="left"><p>제목</p></td>
<td align="left"><p>hdg = "hdg =" 제목</p>
<p>예제:</p>
<p>hdg = 180</p></td>
<td align="left"><p>지도의 방향 (도)을 나타냅니다 (0 또는 360 = 북부, 90 = 동부, 180 = 남부 및 270 = 서쪽).</p></td>
</tr>
<tr class="even">
<td align="left"><p><b>ss</b></p></td>
<td align="left"><p>Streetside</p></td>
<td align="left"><p>ss = "ss =" 비트</p>
<p>예제:</p>
<p>ss = 1</p></td>
<td align="left"><p>인 경우 거리 수준 이미지를 표시 함을 나타냅니다 <code>ss=1</code> . <b>Ss</b> 매개 변수를 생략 하면와 같은 결과가 생성 <code>ss=0</code> 됩니다. 을 <b>cp</b> 매개 변수와 함께 사용 하 여 주소 수준 보기의 위치를 지정 합니다.</p>
<div class="alert">
<b>참고</b>    모든 지역에서 거리 수준 이미지를 사용할 수 없습니다.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p><b>trfc</b></p></td>
<td align="left"><p>트래픽</p></td>
<td align="left"><p>trfc = "trfc =" BIT</p>
<p>예제:</p>
<p>trfc = 1</p></td>
<td align="left"><p>지도에 트래픽 정보를 포함할지 여부를 지정 합니다. Trfc 매개 변수를 생략 하면와 같은 결과가 생성 <code>trfc=0</code> 됩니다.</p>
<div class="alert">
<b>참고</b>    일부 지역에서는 트래픽 데이터를 사용할 수 없습니다.
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left"><p><b>rtp</b></p></td>
<td align="left"><p>경로</p></td>
<td align="left"><p>rtp = "rtp =" (이동 경로 "~" [이동 경로])/("~" 이동 경로)</p>
<p>중간 경로 = ("pos" point)/("adr"를 지정 합니다. whereval)</p>
<p>point = "point" pointval ["_" title]</p>
<p>pointval = degreeslat "" degreeslon</p>
<p>degreeslat = ["-"] 13DIGIT ["." 17DIGIT]</p>
<p>degreeslon = ["-"] 12DIGIT ["." 17DIGIT]</p>
<p>제목 = whereval</p>
<p>whereval = 1 (ALPHA/DIGIT/"-"/"."/"_"/pct 인코딩된/"!"/"$"/"'"/"("/")"/""/"+"/","/";"/":"/"/"/" @" / " ?")</p>


<p>예:</p>
<p>rtp = adr. 산지% 20View,% 20VIEW ~ adr. SFO</p>
<p>rtp = adr. 1% 20Microsoft% 20Microsoft,% 20Microsoft,% 20MICROSOFT ~ 45.23423 _-122.1232_My% 20Microsoft% 20Microsoft</p></td>
<td align="left"><p>지도에 그릴 경로의 시작과 끝을 물결표 ()로 구분 하 여 정의 합니다 <b>~</b> . 각 waypoints는 ltitude, 경도 및 선택적 제목 또는 주소 식별자를 사용 하는 위치에서 정의 됩니다.</p>
<p>전체 경로는 정확히 두 개의 waypoints를 포함 합니다. 예를 들어 두 개의 waypoints가 있는 경로는에 의해 정의 됩니다 <code>rtp="A"~"B"</code> .</p>
<p>불완전 한 경로를 지정 하는 것도 허용 됩니다. 예를 들어를 사용 하 여 경로의 시작만 정의할 수 있습니다 <code>rtp="A"~</code> . 이 경우에는 <b>시작</b> 필드에 제공 된 이동 경로를 사용 하 여 방향 입력이 표시 되 고 <b>대상</b> 필드에 포커스가 표시 됩니다.</p>
<p>와 같이 경로의 끝만 지정 하면 제공 된 이동 경로가 끝 <code>rtp=~"B"</code> 필드에 표시 됩니다. <b>To</b> 정확한 현재 위치를 사용할 수 있는 경우 포커스가 있는 <b>From</b> 필드에 현재 위치가 미리 채워집니다.</p>
<p>불완전 한 경로를 지정 하면 경로 선이 그려지지 않습니다.</p>
<p><b>Mode</b> 매개 변수와 함께 사용 하 여 교통 (구동, 전송 또는 이동)의 모드를 지정 합니다. <b>모드</b> 를 지정 하지 않으면 사용자의 교통 기본 설정 모드를 사용 하 여 방향이 제공 됩니다.</p>
<div class="alert">
<b>참고</b>    위치가 <b>pos</b> 매개 변수 값으로 지정 된 경우 위치에 대 한 제목을 사용할 수 있습니다. 위도 및 경도를 표시 하는 대신 제목이 표시 됩니다.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p><b>mode</b></p></td>
<td align="left"><p>교통 모드</p></td>
<td align="left"><p>mode = "mode =" ("d"/"t"/"w")</p>
<p>예제:</p>
<p>모드 = d</p></td>
<td align="left"><p>교통 모드를 정의 합니다. 이 매개 변수에 유효한 값은 다음과 같습니다.</p>
<ul>
<li><b>d</b>: 주행 방향에 대 한 경로 개요를 표시 합니다.</li>
<li><b>t</b>: 전송 방향에 대 한 경로 개요를 표시 합니다.</li>
<li><b>w</b>: 탐색 방향에 대 한 경로 개요를 표시 합니다.</li>
</ul>
<p>교통 방향에 대 한 <b>rtp</b> 매개 변수와 함께 사용 합니다. <b>모드</b> 를 지정 하지 않으면 사용자의 교통 기본 설정 모드를 사용 하 여 방향이 제공 됩니다. 경로 매개 변수 없이 <b>모드</b> 를 제공 하 여 현재 위치에서 해당 모드에 대 한 방향 입력을 입력할 수 있습니다.</p></td>
</tr>

<tr class="even">
<td align="left"><p><b>컬렉션</b></p></td>
<td align="left"><p>수집</p></td>
<td align="left"><p>collection = "collection =" (name "~"/) point ["~" point]</p>
<p>name = "name." whereval </p>
<p>whereval = 1 (ALPHA/DIGIT/"-"/"."/"_"/pct 인코딩된/"!"/"$"/"'"/"("/")"/""/"+"/","/";"/":"/"/"/" @" / " ?") </p>
<p>point = "point" pointval ["_" title] </p>
<p>pointval = degreeslat "" degreeslon </p>
<p>degreeslat = ["-"] 13DIGIT ["." 17DIGIT] </p>
<p>degreeslon = ["-"] 12DIGIT ["." 17DIGIT] </p>
<p>제목 = whereval</p>


<p>예제:</p>
<p>collection = name. % 20Trip% 20trip _-115.176753_Las% 20Vegas ~ 37.8268 _-122.4798_Golden% 20Trip% 20Trip</p></td>
<td align="left"><p>지도 및 목록에 추가할 점의 컬렉션입니다. Name 매개 변수를 사용 하 여 지점의 컬렉션 이름을 지정할 수 있습니다. 위도, 경도 및 선택적 제목을 사용 하 여 점을 지정 합니다.</p>
<p>이름 및 여러 점이 물결표 ()로 구분 <b>~</b> 됩니다.</p>
<p>지정한 항목에 물결표가 포함 되어 있으면 물결표가로 인코딩 되었는지 확인 합니다 <code>%7E</code> . 중심점 및 확대/축소 수준 매개 변수가 함께 제공 되지 않으면 컬렉션에서 최상의 지도 보기를 제공 합니다.</p>

<p><b>중요</b> 지정 하는 항목에 밑줄이 포함 된 경우 밑줄 은% 255F으로 이중 인코딩 되어야 합니다.</p></td>
</tr>
</tbody>
</table>

  
<span id="ms-drive-to-param-reference"/>

## <a name="ms-drive-to-parameter-reference"></a>ms-드라이브: 매개 변수 참조


턴 턴 방향에 대 한 요청을 시작 하는 URI는 인코딩할 필요가 없으며 다음과 같은 형식입니다.

> **참고**  이 URI 체계에서 시작점을 지정 하지 않습니다. 시작 지점은 항상 현재 위치로 간주 됩니다. 현재 위치 이외의 시작점을 지정 해야 하는 경우 [방향 및 트래픽 표시](#display-directions-and-traffic)를 참조 하세요.

 

| 매개 변수 | 정의 | 예제 | 세부 정보 |
|------------|-----------|---------|---------|
| **destination. 위도** | 대상 위도 | 예: destination. 위도 = 47.6451413797194 | 대상의 위도입니다. 유효한 위도 값은-90에서 + 90 포함 사이입니다. |
| **destination. 경도** | 대상 경도 | 예: destination =-122.141964733601 | 대상의 경도입니다. 유효한 경도 값은-180에서 + 180 포함 사이입니다. |
| **destination.name** | 대상의 이름입니다. | 예: destination. name = Redmond, WA | 대상의 이름입니다. **Destination.name** 값은 인코딩할 필요가 없습니다. |

 
<span id="ms-walk-to-param-reference"/>

## <a name="ms-walk-to-parameter-reference"></a>ms-연습: 매개 변수 참조


단계별 진행 방향에 대 한 요청을 시작 하는 URI는 인코딩할 필요가 없으며 다음 형식을 사용 합니다.

> **참고**  이 URI 체계에서 시작점을 지정 하지 않습니다. 시작 지점은 항상 현재 위치로 간주 됩니다. 현재 위치 이외의 시작점을 지정 해야 하는 경우 [방향 및 트래픽 표시](#display-directions-and-traffic)를 참조 하세요.
 

| 매개 변수 | 정의 | 예제 | 세부 정보 |
|-----------|------------|---------|----------|
| **destination. 위도** | 대상 위도 | 예: destination. 위도 = 47.6451413797194 | 대상의 위도입니다. 유효한 위도 값은-90에서 + 90 포함 사이입니다. |
| **destination. 경도** | 대상 경도 | 예: destination =-122.141964733601 | 대상의 경도입니다. 유효한 경도 값은-180에서 + 180 포함 사이입니다. |
| **destination.name** | 대상의 이름입니다. | 예: destination. name = Redmond, WA | 대상의 이름입니다. **Destination.name** 값은 인코딩할 필요가 없습니다. |

## <a name="ms-settings-parameter-reference"></a>ms-설정: 매개 변수 참조

에 대 한 구문은 응용 프로그램 관련 매개 변수를 **설정 합니다.:** URI 체계는 아래에 정의 되어 있습니다. **maps-downloadmaps** 은 **ms 설정:** **maps-downloadmaps?** 형식의 URI와 함께 지정 되어 오프 라인 맵 설정 페이지를 표시 합니다. 

| 매개 변수 | 정의 | 예제 | 세부 정보 |
|-----------|------------|---------|----------|
| **latlong** | 오프 라인 맵 영역을 정의 하는 지점입니다. | 예: latlong = 47.6,-122.3 | Geopoint는 쉼표로 구분 된 위도 및 경도로 지정 됩니다. 유효한 위도 값은-90에서 + 90 포함 사이입니다. 유효한 경도 값은-180에서 + 180 포함 사이입니다. |
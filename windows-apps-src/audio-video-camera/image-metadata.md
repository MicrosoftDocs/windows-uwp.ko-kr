---
author: laurenhughes
ms.assetid: D5D98044-7221-4C2A-9724-56E59F341AB0
description: 이 문서에서는 이미지 메타데이터를 읽고 쓰는 방법과 GeotagHelper 유틸리티 클래스를 사용하여 파일에 지오태그를 추가하는 방법을 보여 줍니다.
title: 이미지 메타데이터
ms.author: lahugh
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a3e2f10174412b49ce60f3da6a4bb73b2efc4411
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/14/2018
ms.locfileid: "6837431"
---
# <a name="image-metadata"></a>이미지 메타데이터



이 문서에서는 이미지 메타데이터를 읽고 쓰는 방법과 [**GeotagHelper**](https://msdn.microsoft.com/library/windows/apps/dn903683) 유틸리티 클래스를 사용하여 파일에 지오태그를 추가하는 방법을 보여 줍니다.

## <a name="image-properties"></a>이미지 속성

[**StorageFile.Properties**](https://msdn.microsoft.com/library/windows/apps/br227225) 속성은 파일에 대한 콘텐츠 관련 정보에 액세스할 수 있도록 하는 [**StorageItemContentProperties**](https://msdn.microsoft.com/library/windows/apps/hh770642) 개체를 반환합니다. [**GetImagePropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/hh770646)를 호출하여 이미지 관련 속성을 가져옵니다. 반환된 [**ImageProperties**](https://msdn.microsoft.com/library/windows/apps/br207718) 개체는 이미지의 제목 및 캡처 날짜와 같은 기본적인 이미지 메타데이터 필드를 포함하는 멤버를 노출합니다.

[!code-cs[GetImageProperties](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetGetImageProperties)]

더 큰 파일 메타데이터 집합에 액세스하려면 고유한 문자열 식별자로 검색할 수 있는 파일 메타데이터 속성 집합인 Windows 속성 시스템을 사용합니다. 문자열 목록을 만들고 검색하려는 각 속성의 식별자를 추가합니다. [**ImageProperties.RetrievePropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br207732) 메서드는 문자열 목록을 사용하고 키가 속성 식별자에 해당하고 값이 속성 값인 키/값 쌍의 사전을 반환합니다.

[!code-cs[GetWindowsProperties](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetGetWindowsProperties)]

-   각 속성의 식별자 및 형식을 비롯한 Windows 속성의 전체 목록을 보려면 [Windows 속성](https://msdn.microsoft.com/library/windows/desktop/dd561977)을 참조하세요.

-   일부 속성은 특정 파일 컨테이너 및 이미지 코덱에 대해서만 지원됩니다. 각 이미지 형식에 대해 지원되는 이미지 메타데이터 목록을 보려면 [사진 메타데이터 정책](https://msdn.microsoft.com/library/windows/desktop/ee872003)을 참조하세요.

-   지원되지 않는 속성은 검색 시 null 값을 반환할 수 있으므로 반환된 메타데이터 값을 사용하기 전에 null이 있는지 확인합니다.

## <a name="geotag-helper"></a>지오태그 도우미

GeotagHelper는 메타데이터 형식을 수동으로 구문 분석하거나 생성할 필요 없이 [**Windows.Devices.Geolocation**](https://msdn.microsoft.com/library/windows/apps/br225603) API를 직접 사용하여 지리적 데이터가 있는 이미지에 쉽게 태그를 지정하는 유틸리티 클래스입니다.

이미지에 태그를 지정하려는 위치를 나타내는 [**Geopoint**](https://msdn.microsoft.com/library/windows/apps/dn263675) 개체가 이미 있는 경우 이전에 사용했던 지리적 위치 API나 기타 소스에서 [**GeotagHelper.SetGeotagAsync**](https://msdn.microsoft.com/library/windows/apps/dn903685)를 호출하고 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 및 **Geopoint**를 제공하여 지오태그 데이터를 설정할 수 있습니다.

[!code-cs[SetGeoDataFromPoint](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetSetGeoDataFromPoint)]

디바이스의 현재 위치를 사용하여 지오태그 데이터를 설정하려면 새 [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) 개체를 만들고 [**GeotagHelper.SetGeotagFromGeolocatorAsync**](https://msdn.microsoft.com/library/windows/apps/dn903686)를 호출한 다음 **Geolocator** 및 태그를 지정할 파일을 전달합니다.

[!code-cs[SetGeoDataFromGeolocator](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetSetGeoDataFromGeolocator)]

-   [**SetGeotagFromGeolocatorAsync**](https://msdn.microsoft.com/library/windows/apps/dn903686) API를 사용하려면 앱 매니페스트에 **location** 디바이스 기능을 포함해야 합니다.

-   [**SetGeotagFromGeolocatorAsync**](https://msdn.microsoft.com/library/windows/apps/dn903686)를 호출하기 전에 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn859152)를 호출하여 사용자가 자신의 위치를 사용할 수 있는 권한을 앱에 부여했는지 확인해야 합니다.

-   지리적 위치 API에 대한 자세한 내용은 [지도 및 위치](https://msdn.microsoft.com/library/windows/apps/mt219699)를 참조하세요.

이미지 파일의 지오태그가 지정된 위치를 나타내는 GeoPoint를 가져오려면 [**GetGeotagAsync**](https://msdn.microsoft.com/library/windows/apps/dn903684)를 호출합니다.

[!code-cs[GetGeoData](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetGetGeoData)]

## <a name="decode-and-encode-image-metadata"></a>이미지 메타데이터 디코드 및 인코드

이미지 데이터로 작업하는 좀 더 수준 높은 방법은 [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/br226176) 또는 [BitmapEncoder](bitmapencoder-options-reference.md)를 사용하여 스트림 수준에서 속성을 읽고 쓰는 것입니다. 이러한 작업의 경우 Windows 속성을 사용하여 읽거나 쓰는 데이터를 지정할 수 있지만 요청된 속성에 대한 경로를 지정하기 위해 WIC(Windows 이미징 구성 요소)에서 제공하는 메타데이터 쿼리 언어를 사용할 수도 있습니다.

이 기술을 사용하여 이미지 메타데이터를 읽으려면 원본 이미지 파일 스트림으로 만든 [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/br226176)가 있어야 합니다. 이 작업을 수행하는 방법에 대한 자세한 내용은 [이미징](imaging.md)을 참조하세요.

일단 디코더가 있으면 Windows 속성 식별자 문자열 또는 WIC 메타데이터 쿼리를 사용하여 문자열의 목록을 만들고 검색하려는 각 메타데이터 속성에 대한 새 항목을 추가합니다. 디코더의 [**BitmapProperties**](https://msdn.microsoft.com/library/windows/apps/br226248) 멤버에 대해 [**BitmapPropertiesView.GetPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br226250) 메서드를 호출하여 지정된 속성을 요청합니다. 속성은 속성 이름 또는 경로와 속성 값을 포함하는 키/값 쌍의 사전으로 반환됩니다.

[!code-cs[ReadImageMetadata](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetReadImageMetadata)]

-   WIC 메타데이터 쿼리 언어 및 지원되는 속성에 대한 자세한 내용은 [WIC 이미지 형식 네이티브 메타데이터 쿼리](https://msdn.microsoft.com/library/windows/desktop/ee719904)를 참조하세요.

-   많은 메타데이터 속성이 이미지 형식의 하위 집합에서만 지원됩니다. [**GetPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br226250)는 요청된 속성 중 하나가 디코더와 연결된 이미지에서 지원하지 않는 경우 0x88982F41 오류 코드로 실패하고 이미지가 메타데이터를 전혀 지원하지 않을 경우 0x88982F81 오류 코드로 실패합니다. 이러한 오류 코드와 관련된 상수는 WINCODEC\_ERR\_PROPERTYNOTSUPPORTED 및 WINCODEC\_ERR\_UNSUPPORTEDOPERATION이며 winerror.h 헤더 파일에 정의되어 있습니다.
-   이미지가 특정 속성 값을 포함할 수도 있고 그렇지 않을 수도 있으므로 액세스를 시도하기 전에 **IDictionary.ContainsKey**를 사용하여 결과에 속성이 있는지 확인합니다.

이미지 메타데이터를 스트림에 쓰려면 이미지 출력 파일과 연결된 **BitmapEncoder**가 필요합니다.

설정할 속성 값을 포함할 [**BitmapPropertySet**](https://msdn.microsoft.com/library/windows/apps/hh974338) 개체를 만듭니다. 속성 값을 나타낼 [**BitmapTypedValue**](https://msdn.microsoft.com/library/windows/apps/hh700687) 개체를 만듭니다. 이 개체는 **object**를 값으로 사용하고 해당 값의 형식을 정의하는 [**PropertyType**](https://msdn.microsoft.com/library/windows/apps/br225871) 열거형의 멤버를 사용합니다. **BitmapPropertySet**에 **BitmapTypedValue**를 추가하고 [**BitmapProperties.SetPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br226252)를 호출하여 인코더가 스트림에 속성을 쓰도록 합니다.

[!code-cs[WriteImageMetadata](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetWriteImageMetadata)]

-   어떤 이미지 파일 형식에 대해 어떤 속성이 지원되는지를 자세히 알아보려면 [Windows 속성](https://msdn.microsoft.com/library/windows/desktop/dd561977), [사진 메타데이터 정책](https://msdn.microsoft.com/library/windows/desktop/ee872003) 및 [WIC 이미지 형식 네이티브 메타데이터 쿼리](https://msdn.microsoft.com/library/windows/desktop/ee719904)를 참조하세요.

-   [**SetPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br226252)는 요청된 속성 중 하나가 인코더와 연결된 이미지에서 지원하지 않는 경우 0x88982F41 오류 코드로 실패합니다.

## <a name="related-topics"></a>관련 항목

* [이미징](imaging.md)
 

 





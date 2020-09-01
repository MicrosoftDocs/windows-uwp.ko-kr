---
ms.assetid: D5D98044-7221-4C2A-9724-56E59F341AB0
description: 이 문서에서는 이미지 메타 데이터 속성을 읽고 쓰는 방법과 GeotagHelper 유틸리티 클래스를 사용 하 여 파일을 geotag 하는 방법을 보여 줍니다.
title: 이미지 메타데이터
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ca2a5abe5c0a7f60246322dd81fad9af8f0def77
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157497"
---
# <a name="image-metadata"></a>이미지 메타데이터



이 문서에서는 이미지 메타 데이터 속성을 읽고 쓰는 방법과 [**Geotaghelper**](/uwp/api/Windows.Storage.FileProperties.GeotagHelper) 유틸리티 클래스를 사용 하 여 파일을 geotag 하는 방법을 보여 줍니다.

## <a name="image-properties"></a>이미지 속성

[**StorageFile**](/uwp/api/windows.storage.storagefile.properties) 속성은 파일에 대 한 내용 관련 정보에 대 한 액세스를 제공 하는 [**Storageitemcontentproperties**](/uwp/api/Windows.Storage.FileProperties.StorageItemContentProperties) 개체를 반환 합니다. [**GetImagePropertiesAsync**](/uwp/api/windows.storage.fileproperties.storageitemcontentproperties.getimagepropertiesasync)를 호출 하 여 이미지 관련 속성을 가져옵니다. 반환 된 [**Imageproperties**](/uwp/api/Windows.Storage.FileProperties.ImageProperties) 개체는 이미지 제목 및 캡처 날짜와 같은 기본 이미지 메타 데이터 필드를 포함 하는 멤버를 노출 합니다.

[!code-cs[GetImageProperties](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetGetImageProperties)]

더 큰 파일 메타 데이터 집합에 액세스 하려면 고유한 문자열 식별자를 사용 하 여 검색할 수 있는 파일 메타 데이터 속성 집합인 Windows 속성 시스템을 사용 합니다. 문자열 목록을 만들고 검색 하려는 각 속성에 대 한 식별자를 추가 합니다. [**RetrievePropertiesAsync**](/uwp/api/windows.storage.fileproperties.imageproperties.retrievepropertiesasync) 메서드는이 문자열 목록을 사용 하 고 키/값 쌍의 사전을 반환 합니다. 여기서 키는 속성 식별자이 고 값은 속성 값입니다.

[!code-cs[GetWindowsProperties](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetGetWindowsProperties)]

-   각 속성에 대 한 식별자 및 형식을 포함 하 여 Windows 속성의 전체 목록은 [Windows 속성](/windows/desktop/properties/props)을 참조 하세요.

-   일부 속성은 특정 파일 컨테이너 및 이미지 코덱에 대해서만 지원 됩니다. 각 이미지 형식에 대해 지원 되는 이미지 메타 데이터 목록은 [사진 메타 데이터 정책](/windows/desktop/wic/photo-metadata-policies)을 참조 하세요.

-   지원 되지 않는 속성은 검색 시 null 값을 반환할 수 있으므로 반환 된 메타 데이터 값을 사용 하기 전에 항상 null을 확인 해야 합니다.

## <a name="geotag-helper"></a>Geotag 도우미

GeotagHelper는 메타 데이터 형식을 수동으로 구문 분석 하거나 생성 하지 않고도 직접 지리적 데이터를 사용 하 여 이미지에 태그를 지정할 수 있게 해 주는 유틸리티 클래스 [**입니다.**](/uwp/api/Windows.Devices.Geolocation)

이전에 지리적 위치 Api 또는 다른 소스를 사용 하 여 이미지에 태그를 지정할 위치를 나타내는 [**Geopoint**](/uwp/api/Windows.Devices.Geolocation.Geopoint) 개체가 이미 있는 경우 geotag 데이터를 설정 하 여 [**GeStorageFile**](/uwp/api/Windows.Storage.StorageFile) 와 **geopoint**를 전달할 [**GeotagHelper.SetGeotagAsync**](/uwp/api/windows.storage.fileproperties.geotaghelper.setgeotagasync) 수 있습니다 (영문).

[!code-cs[SetGeoDataFromPoint](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetSetGeoDataFromPoint)]

장치의 현재 위치를 사용 하 여 geotag 데이터를 설정 하려면 새 [**geolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator) 개체를 만들고, **geolocator** 및 태그를 지정할 파일에를 전달 하 여 [**Ge\agerfromgeolocagasync**](/uwp/api/windows.storage.fileproperties.geotaghelper.setgeotagfromgeolocatorasync) 를 호출 합니다.

[!code-cs[SetGeoDataFromGeolocator](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetSetGeoDataFromGeolocator)]

-   [**Setgeotagfromgeolocatorasync**](/uwp/api/windows.storage.fileproperties.geotaghelper.setgeotagfromgeolocatorasync) API를 사용 하려면 앱 매니페스트에 **위치** 장치 기능을 포함 해야 합니다.

-   [**Setgeotagfromgeolocatorasync**](/uwp/api/windows.storage.fileproperties.geotaghelper.setgeotagfromgeolocatorasync) 를 호출 하기 전에 [**requestaccessasync**](/uwp/api/windows.devices.geolocation.geolocator.requestaccessasync) 를 호출 하 여 사용자가 해당 위치를 사용할 수 있는 권한을 앱에 부여 했는지 확인 해야 합니다.

-   지리적 위치 Api에 대 한 자세한 내용은 [맵 및 위치](../maps-and-location/index.md)를 참조 하세요.

이미지 파일의 geotagged 위치를 나타내는 GeoPoint를 가져오려면 [**Getgeotagasync**](/uwp/api/windows.storage.fileproperties.geotaghelper.getgeotagasync)를 호출 합니다.

[!code-cs[GetGeoData](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetGetGeoData)]

## <a name="decode-and-encode-image-metadata"></a>이미지 메타 데이터 디코딩 및 인코딩

이미지 데이터를 사용 하는 가장 고급 방법은 [**bitmapdecoder에서**](/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) 또는 [BitmapEncoder](bitmapencoder-options-reference.md)를 사용 하 여 스트림 수준에서 속성을 읽고 쓰는 것입니다. 이러한 작업의 경우 Windows 속성을 사용 하 여 읽고 쓰는 데이터를 지정할 수 있지만, WIC (Windows Imaging Component)에서 제공 하는 메타 데이터 쿼리 언어를 사용 하 여 요청 된 속성의 경로를 지정할 수도 있습니다.

이 기술을 사용 하 여 이미지 메타 데이터를 읽으면 원본 이미지 파일 스트림으로 만든 [**bitmapdecoder에서**](/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) 이 있어야 합니다. 이 작업을 수행 하는 방법에 대 한 자세한 내용은 [Imaging](imaging.md)을 참조 하세요.

디코더가 있으면 Windows 속성 식별자 문자열 또는 WIC 메타 데이터 쿼리를 사용 하 여 문자열 목록을 만들고 검색 하려는 각 메타 데이터 속성에 대 한 새 항목을 추가 합니다. 지정 된 속성을 요청 하려면 디코더의 [**BitmapProperties**](/uwp/api/Windows.Graphics.Imaging.BitmapProperties) 멤버에 대해 GetPropertiesAsync 메서드를 호출 [**BitmapPropertiesView.**](/uwp/api/windows.graphics.imaging.bitmappropertiesview.getpropertiesasync) 속성은 속성 이름 또는 경로와 속성 값을 포함 하는 키/값 쌍의 사전에 반환 됩니다.

[!code-cs[ReadImageMetadata](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetReadImageMetadata)]

-   WIC 메타 데이터 쿼리 언어 및 지원 되는 속성에 대 한 자세한 내용은 [wic 이미지 형식 네이티브 메타 데이터 쿼리](/windows/desktop/wic/-wic-native-image-format-metadata-queries)를 참조 하세요.

-   많은 메타 데이터 속성은 이미지 형식의 하위 집합 에서만 지원 됩니다. 요청 된 속성 중 하나가 디코더에 연결 된 이미지에서 지원 되지 않는 경우 [**GetPropertiesAsync**](/uwp/api/windows.graphics.imaging.bitmappropertiesview.getpropertiesasync) 는 0x88982F41 오류 코드와 함께 실패 하 고, 이미지에서 메타 데이터를 전혀 지원 하지 않는 경우 0x88982F81을 사용 하 여 실패 합니다. 이러한 오류 코드와 연결 된 상수는 WINCODEC \_ err \_ propertynotsupported 및 wincodec \_ err \_ UNSUPPORTEDOPERATION 이며 winerror.h 헤더 파일에 정의 되어 있습니다.
-   특정 속성에 대 한 값을 포함 하거나 포함 하지 않을 수 있습니다. **ContainsKey** 를 사용 하 여 액세스를 시도 하기 전에 속성이 결과에 있는지 확인 합니다.

스트림에 이미지 메타 데이터를 쓰려면 이미지 출력 파일과 연결 된 **BitmapEncoder** 가 필요 합니다.

설정 하려는 속성 값을 포함 하는 [**BitmapPropertySet**](/uwp/api/Windows.Graphics.Imaging.BitmapPropertySet) 개체를 만듭니다. 속성 값을 나타내는 [**BitmapTypedValue**](/uwp/api/Windows.Graphics.Imaging.BitmapTypedValue) 개체를 만듭니다. 이 개체는 **개체** 를 값의 형식을 정의 하는 [**PropertyType**](/uwp/api/Windows.Foundation.PropertyType) 열거형의 멤버 및 값으로 사용 합니다. **BitmapPropertySet** 에 **BitmapTypedValue** 를 추가 하 고 BitmapProperties를 호출 하 여 인코더가 속성을 스트림에 쓰도록 합니다 [**.**](/uwp/api/windows.graphics.imaging.bitmapproperties.setpropertiesasync)

[!code-cs[WriteImageMetadata](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetWriteImageMetadata)]

-   이미지 파일 형식에 대해 지원 되는 속성에 대 한 자세한 내용은 [Windows 속성](/windows/desktop/properties/props), [사진 메타 데이터 정책](/windows/desktop/wic/photo-metadata-policies)및 [WIC 이미지 형식 네이티브 메타 데이터 쿼리](/windows/desktop/wic/-wic-native-image-format-metadata-queries)를 참조 하세요.

-   요청 된 속성 중 하나가 인코더와 연결 된 이미지에서 지원 되지 않는 경우 [**SetPropertiesAsync**](/uwp/api/windows.graphics.imaging.bitmapproperties.setpropertiesasync) 는 오류 코드 0x88982F41와 함께 실패 합니다.

## <a name="related-topics"></a>관련 항목

* [이미징](imaging.md)
 

 
---
description: 이 문서에서는 UWP (유니버설 Windows 플랫폼) 앱에서 공유 계약을 지 원하는 방법을 설명 합니다.
title: 데이터 공유
ms.assetid: 32287F5E-EB86-4B98-97FF-8F6228D06782
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9c11c4b630e6b38dd567fece782686743925e214
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161327"
---
# <a name="share-data"></a>데이터 공유


이 문서에서는 UWP (유니버설 Windows 플랫폼) 앱에서 공유 계약을 지 원하는 방법을 설명 합니다. 공유 계약은 앱 간에 텍스트, 링크, 사진 및 비디오와 같은 데이터를 빠르게 공유 하는 쉬운 방법입니다. 예를 들어 사용자는 소셜 네트워킹 앱을 사용 하 여 웹 페이지를 동료와 공유 하거나 나중에 참조할 수 있도록 notes 앱에 링크를 저장할 수 있습니다.

> [!NOTE]
> 이 문서의 코드 예제는 UWP 앱 용으로 작성 되었습니다. WPF, Windows Forms 및 c + +/Win32 데스크톱 앱은 [IDataTransferManagerInterop](/windows/win32/api/shobjidl_core/nn-shobjidl_core-idatatransfermanagerinterop) 인터페이스를 사용 하 여 특정 창에 대 한 [DataTransferManager](/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager) 개체를 가져와야 합니다. 자세한 내용은 [ShareSource](https://github.com/microsoft/Windows-classic-samples/tree/master/Samples/ShareSource) 샘플을 참조 하세요.

## <a name="set-up-an-event-handler"></a>이벤트 처리기 설정

사용자가 공유를 호출할 때마다 호출 되는 [**datarequested**](/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.datarequested) 이벤트 처리기를 추가 합니다. 사용자가 앱의 컨트롤을 탭 하거나 (예: 단추 또는 앱 바 명령) 특정 시나리오에서 자동으로 (예: 사용자가 수준을 마치고 높은 점수를 가져오는 경우) 발생할 수 있습니다.

[!code-cs[Main](./code/share_data/cs/MainPage.xaml.cs#SnippetPrepareToShare)]

[**Datarequested**](/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.datarequested) 된 이벤트가 발생 하면 앱은 [**DataRequest**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataRequest) 개체를 수신 합니다. 여기에는 사용자가 공유 하려는 콘텐츠를 제공 하는 데 사용할 수 있는 [**DataPackage**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) 포함 됩니다. 공유할 제목과 데이터를 제공 해야 합니다. 설명은 선택 사항 이지만 권장 됩니다.

[!code-cs[Main](./code/share_data/cs/MainPage.xaml.cs#SnippetCreateRequest)]

## <a name="choose-data"></a>데이터 선택

다음을 포함 하 여 다양 한 유형의 데이터를 공유할 수 있습니다.

-   일반 텍스트
-   Uniform Resource Identifier(URI)
-   HTML
-   서식 있는 텍스트
-   비트맵
-   Files
-   사용자 지정 개발자 정의 데이터

[**DataPackage**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) 개체는 이러한 형식 중 하나 이상을 조합 하 여 포함할 수 있습니다. 다음 예제에서는 텍스트를 공유 하는 방법을 보여 줍니다.

[!code-cs[Main](./code/share_data/cs/MainPage.xaml.cs#SnippetSetContent)]

## <a name="set-properties"></a>속성 설정

공유를 위해 데이터를 패키지할 때 공유 되는 콘텐츠에 대 한 추가 정보를 제공 하는 다양 한 속성을 제공할 수 있습니다. 이러한 속성을 통해 대상 앱에서 사용자 환경을 향상 시킬 수 있습니다. 예를 들어, 설명은 사용자가 둘 이상의 앱과 콘텐츠를 공유 하는 경우에 유용 합니다. 이미지 또는 웹 페이지에 대 한 링크를 공유할 때 축소판 그림을 추가 하면 사용자에 대 한 시각적 참조가 제공 됩니다. 자세한 내용은 [**DataPackagePropertySet**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackagePropertySet)를 참조 하세요.

제목을 제외한 모든 속성은 선택 사항입니다. Title 속성은 필수 이며 설정 해야 합니다.

[!code-cs[Main](./code/share_data/cs/MainPage.xaml.cs#SnippetSetProperties)]

## <a name="launch-the-share-ui"></a>공유 UI 시작

공유에 대 한 UI는 시스템에서 제공 됩니다. 이를 시작 하려면 [**Showshareui**](/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.showshareui) 메서드를 호출 합니다.

[!code-cs[Main](./code/share_data/cs/MainPage.xaml.cs#SnippetShowUI)]

## <a name="handle-errors"></a>오류 처리

대부분의 경우 콘텐츠 공유는 간단한 프로세스입니다. 그러나 예기치 않은 상황이 발생할 수 있습니다. 예를 들어 앱에서 사용자가 공유 하기 위해 콘텐츠를 선택 하도록 요구 하지만, 사용자가 아무 것도 선택 하지 않았을 수 있습니다. 이러한 상황을 처리 하려면 [**FailWithDisplayText**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataRequest#Windows_ApplicationModel_DataTransfer_DataRequest_FailWithDisplayText_System_String_) 메서드를 사용 합니다 .이 메서드는 문제가 발생 한 경우 사용자에 게 메시지를 표시 합니다.

## <a name="delay-share-with-delegates"></a>대리자로 공유 지연

사용자가 바로 공유 하려는 데이터를 준비 하는 것이 적합 하지 않을 수도 있습니다. 예를 들어 응용 프로그램에서 여러 가지 가능한 형식으로 많은 이미지 파일을 보내는 것을 지 원하는 경우 사용자가 선택 하기 전에 해당 이미지를 모두 만드는 것은 비효율적입니다.

이 문제를 해결 하기 위해 [**DataPackage**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) 는 수신 앱에서 데이터를 요청할 때 호출 되는 대리자 (대리자)를 포함할 수 있습니다. 사용자가 공유 하려는 데이터가 리소스를 많이 사용 하는 경우 언제 든 지 대리자를 사용 하는 것이 좋습니다.

<!-- For some reason, this snippet was inline in the WDCML topic. Suggest moving to VS project with rest of snippets. -->
```cs
async void OnDeferredImageRequestedHandler(DataProviderRequest request)
{
    // Provide updated bitmap data using delayed rendering
    if (this.imageStream != null)
    {
        DataProviderDeferral deferral = request.GetDeferral();
        InMemoryRandomAccessStream inMemoryStream = new InMemoryRandomAccessStream();

        // Decode the image.
        BitmapDecoder imageDecoder = await BitmapDecoder.CreateAsync(this.imageStream);

        // Re-encode the image at 50% width and height.
        BitmapEncoder imageEncoder = await BitmapEncoder.CreateForTranscodingAsync(inMemoryStream, imageDecoder);
        imageEncoder.BitmapTransform.ScaledWidth = (uint)(imageDecoder.OrientedPixelWidth * 0.5);
        imageEncoder.BitmapTransform.ScaledHeight = (uint)(imageDecoder.OrientedPixelHeight * 0.5);
        await imageEncoder.FlushAsync();

        request.SetData(RandomAccessStreamReference.CreateFromStream(inMemoryStream));
        deferral.Complete();
    }
}
```

## <a name="see-also"></a>참고 항목 

* [앱 간 통신](index.md)
* [데이터 수신](receive-data.md)
* [DataPackage](/uwp/api/windows.applicationmodel.datatransfer.datapackage)
* [DataPackagePropertySet](/uwp/api/windows.applicationmodel.datatransfer.datapackagepropertyset)
* [DataRequest](/uwp/api/windows.applicationmodel.datatransfer.datarequest)
* [DataRequested 됨](/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.datarequested)
* [FailWithDisplayText](/uwp/api/windows.applicationmodel.datatransfer.datarequest.failwithdisplaytext)
* [ShowShareUi](/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.showshareui)
 
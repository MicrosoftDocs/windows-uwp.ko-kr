---
description: 이 문서에서는 UWP(Universal Windows Platform) 앱에서 공유 계약을 지원하는 방법을 설명합니다.
title: 데이터 공유
ms.assetid: 32287F5E-EB86-4B98-97FF-8F6228D06782
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5ed9be96ee44635249f01e7b919f3789d84305e1
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8755797"
---
# <a name="share-data"></a>데이터 공유


이 문서에서는 UWP(유니버설 Windows 플랫폼) 앱에서 공유 계약을 지원하는 방법을 설명합니다. 공유 계약은 텍스트, 링크, 사진과 같은 데이터를 앱 간에 신속하게 공유할 수 있는 편리한 방법입니다. 예를 들어 사용자가 소셜 네트워킹 앱을 사용하여 친구와 웹 페이지를 공유하거나 링크를 나중에 참조하기 위해 노트 기록 앱에 저장할 수 있습니다.

## <a name="set-up-an-event-handler"></a>이벤트 처리기 설정

사용자가 공유를 호출할 때마다 호출될 [**DataRequested**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataTransferManager.DataRequested) 이벤트 처리기를 추가합니다. 이는 사용자가 앱에서 컨트롤(예: 단추 또는 앱 바 명령)을 탭할 때 발생하거나 특정 시나리오(예: 사용자가 한 레벨을 마치고 높은 점수를 얻는 경우)에서 자동으로 발생할 수 있습니다.

[!code-cs[Main](./code/share_data/cs/MainPage.xaml.cs#SnippetPrepareToShare)]

[**DataRequested**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataTransferManager.DataRequested) 이벤트가 발생하면 앱이 [**DataRequest**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataRequest) 개체를 받습니다. 여기에는 사용자가 공유하려는 콘텐츠를 제공하는 데 사용할 수 있는 [**DataPackage**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackage)가 포함되어 있습니다. 공유할 제목과 데이터를 제공해야 합니다. 설명은 선택 사항이지만 사용하는 것이 좋습니다.

[!code-cs[Main](./code/share_data/cs/MainPage.xaml.cs#SnippetCreateRequest)]

## <a name="choose-data"></a>데이터 선택

다음과 같은 다양한 형식의 데이터를 공유할 수 있습니다.

-   일반 텍스트
-   URI(Uniform Resource Identifier)
-   HTML
-   서식 있는 텍스트
-   비트맵
-   파일
-   사용자 지정 개발자 정의 데이터

[**DataPackage**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackage) 개체에는 하나 이상의 이러한 형식이 임의 조합으로 포함될 수 있습니다. 다음 예제는 텍스트 공유를 보여 줍니다.

[!code-cs[Main](./code/share_data/cs/MainPage.xaml.cs#SnippetSetContent)]

## <a name="set-properties"></a>속성 설정

공유할 데이터를 패키지로 만들 경우 공유할 내용에 대한 추가 정보를 제공하는 다양한 속성을 제공할 수 있습니다. 이러한 속성을 사용하면 대상 앱의 사용자 경험을 향상할 수 있습니다. 예를 들어, 사용자가 두 개 이상의 앱과 콘텐츠를 공유하는 경우 설명이 있으면 도움이 됩니다. 이미지나 웹 페이지 링크를 공유할 때 미리 보기를 추가하면 사용자에게 시각적 참조를 제공할 수 있습니다. 자세한 내용은 [**DataPackagePropertySet**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackagePropertySet)을 참조하세요.

제목을 제외한 모든 속성은 선택 사항입니다. title 속성은 필수 사항이므로 설정해야 합니다.

[!code-cs[Main](./code/share_data/cs/MainPage.xaml.cs#SnippetSetProperties)]

## <a name="launch-the-share-ui"></a>공유 UI 시작

공유를 위한 UI가 시스템에서 제공됩니다. 시작하려면 [**ShowShareUI**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataTransferManager.ShowShareUI) 메서드를 호출합니다.

[!code-cs[Main](./code/share_data/cs/MainPage.xaml.cs#SnippetShowUI)]

## <a name="handle-errors"></a>오류 처리

대부분의 경우 콘텐츠 공유는 간단한 프로세스입니다. 그러나 항상 예기치 않은 문제가 발생할 수 있습니다. 예를 들어 앱에서 사용자가 공유할 콘텐츠를 선택해야 하지만 아무것도 선택하지 않았을 수 있습니다. 이러한 상황을 처리하려면 잘못된 부분이 있는 경우 사용자에게 메시지를 표시하는 [**FailWithDisplayText**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataRequest.FailWithDisplayText(System.String)) 메서드를 사용합니다.

## <a name="delay-share-with-delegates"></a>대리자와 공유 지연

사용자가 공유하려는 데이터를 즉시 준비하는 것이 비효율적인 경우도 있습니다. 예를 들어, 앱에서 여러 다른 형식으로 용량이 큰 이미지 파일을 전송하도록 지원하는 경우 사용자가 선택하기 전에 해당 이미지를 모두 만드는 것은 비효율적입니다.

이 문제를 해결하기 위해 [**DataPackage**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackage)에는 수신 앱이 데이터를 요청할 때 호출되는 함수인 대리자가 포함될 수 있습니다. 사용자가 공유하려는 데이터가 리소스를 많이 사용하는 경우 항상 대리자를 사용하는 것이 좋습니다.

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
        imageEncoder.BitmapTransform.ScaledWidth = (uint)(imageDecoder.OrientedPixelHeight * 0.5);
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
* [DataPackage](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackage.aspx)
* [DataPackagePropertySet](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackagepropertyset.aspx)
* [DataRequest](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datarequest.aspx)
* [DataRequested](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datatransfermanager.datarequested.aspx)
* [FailWithDisplayText](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datarequest.failwithdisplaytext.aspx)
* [ShowShareUi](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datatransfermanager.showshareui.aspx)
 


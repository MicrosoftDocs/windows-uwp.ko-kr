---
ms.assetid: BF877F23-1238-4586-9C16-246F3F25AE35
description: 이 문서에서는 Microsoft PlayReady 콘텐츠 보호와 함께 멀티미디어 콘텐츠의 적응 스트리밍을 UWP(유니버설 Windows 플랫폼) 앱에 추가하는 방법을 설명합니다.
title: PlayReady를 사용한 적응 스트리밍
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 36cd006b4608d82999281ebd407fd32e168ae38b
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8933263"
---
# <a name="adaptive-streaming-with-playready"></a>PlayReady를 사용한 적응 스트리밍


이 문서에서는 Microsoft PlayReady 콘텐츠 보호와 함께 멀티미디어 콘텐츠의 적응 스트리밍을 UWP(유니버설 Windows 플랫폼) 앱에 추가하는 방법을 설명합니다. 

이 기능은 현재 DASH(Dynamic Streaming over HTTP) 콘텐츠 재생을 지원합니다.

HLS(Apple의 HTTP 라이브 스트리밍)는 PlayReady에서 지원되지 않습니다.

부드러운 스트리밍도 현재 기본적으로 지원되지 않습니다. 그러나 PlayReady는 확장 가능하고, 추가 코드 또는 라이브러리를 사용하여 PlayReady에서 보호하는 부드러운 스트리밍을 지원할 수 있으므로 소프트웨어나 하드웨어 DRM(디지털 권한 관리)까지 활용됩니다.

이 문서에서는 PlayReady 관련 적응 스트리밍 측면만 다룹니다. 적응 스트리밍의 일반적인 구현에 대한 자세한 내용은 [적응 스트리밍](adaptive-streaming.md)을 참조하세요.

이 문서에서는 GitHub의 Microsoft **Windows-universal-samples** 리포지토리에서 [적응 스트리밍 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AdaptiveStreaming) 코드를 사용합니다. 시나리오 4에서는 PlayReady를 이용한 적응 스트리밍 사용을 다룹니다. 저장소의 루트 수준으로 이동하고 **ZIP 다운로드** 단추를 선택하여 ZIP 파일의 리포지토리를 다운로드할 수 있습니다.

다음 **using** 문이 필요합니다.

```csharp
using LicenseRequest;
using System;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Runtime.InteropServices;
using System.Threading.Tasks;
using Windows.Foundation.Collections;
using Windows.Media.Protection;
using Windows.Media.Protection.PlayReady;
using Windows.Media.Streaming.Adaptive;
using Windows.UI.Xaml.Controls;
```

**LicenseRequest** 네임스페이스는 Microsoft에서 라이선스 실시권자에게 제공하는 PlayReady 파일인 **CommonLicenseRequest.cs**에서 가져온 네임스페이스입니다.

몇 가지 전역 변수를 선언해야 합니다.

```csharp
private AdaptiveMediaSource ams = null;
private MediaProtectionManager protectionManager = null;
private string playReadyLicenseUrl = "";
private string playReadyChallengeCustomData = "";
```

다음 상수 선언이 필요할 수도 있습니다.

```csharp
private const uint MSPR_E_CONTENT_ENABLING_ACTION_REQUIRED = 0x8004B895;
```

## <a name="setting-up-the-mediaprotectionmanager"></a>MediaProtectionManager 설정

PlayReady 콘텐츠 보호를 UWP 앱에 추가하려면 [MediaProtectionManager](https://msdn.microsoft.com/library/windows/apps/br207040) 개체를 설정해야 합니다. [**AdaptiveMediaSource**](https://msdn.microsoft.com/library/windows/apps/dn946912) 개체를 초기화할 때 이 작업을 수행합니다.

다음 코드는 [MediaProtectionManager](https://msdn.microsoft.com/library/windows/apps/br207040)를 설정합니다.

```csharp
private void SetUpProtectionManager(ref MediaElement mediaElement)
{
    protectionManager = new MediaProtectionManager();

    protectionManager.ComponentLoadFailed += 
        new ComponentLoadFailedEventHandler(ProtectionManager_ComponentLoadFailed);

    protectionManager.ServiceRequested += 
        new ServiceRequestedEventHandler(ProtectionManager_ServiceRequested);

    PropertySet cpSystems = new PropertySet();

    cpSystems.Add(
        "{F4637010-03C3-42CD-B932-B48ADF3A6A54}", 
        "Windows.Media.Protection.PlayReady.PlayReadyWinRTTrustedInput");

    protectionManager.Properties.Add("Windows.Media.Protection.MediaProtectionSystemIdMapping", cpSystems);

    protectionManager.Properties.Add(
        "Windows.Media.Protection.MediaProtectionSystemId", 
        "{F4637010-03C3-42CD-B932-B48ADF3A6A54}");

    protectionManager.Properties.Add(
        "Windows.Media.Protection.MediaProtectionContainerGuid", 
        "{9A04F079-9840-4286-AB92-E65BE0885F95}");

    mediaElement.ProtectionManager = protectionManager;
}
```

이 코드를 앱에 복사하여 필수 작업인 콘텐츠 보호를 간단하게 설정할 수 있습니다.

[ComponentLoadFailed](https://msdn.microsoft.com/library/windows/apps/br207041) 이벤트는 이진 데이터를 로드하지 못했을 때 발생합니다. 로드가 완료되지 않았음을 알리는 이벤트 처리기를 추가하여 이 문제를 처리해야 합니다.

```csharp
private void ProtectionManager_ComponentLoadFailed(
    MediaProtectionManager sender, 
    ComponentLoadFailedEventArgs e)
{
    e.Completion.Complete(false);
}
```

마찬가지로 서비스가 요청될 때 발생하는 [ServiceRequested](https://msdn.microsoft.com/library/windows/apps/br207045) 이벤트에 대한 이벤트 처리기를 추가해야 합니다. 이 코드는 서비스의 요청 종류를 확인하고 적절하게 응답합니다.

```csharp
private async void ProtectionManager_ServiceRequested(
    MediaProtectionManager sender, 
    ServiceRequestedEventArgs e)
{
    if (e.Request is PlayReadyIndividualizationServiceRequest)
    {
        PlayReadyIndividualizationServiceRequest IndivRequest = 
            e.Request as PlayReadyIndividualizationServiceRequest;

        bool bResultIndiv = await ReactiveIndivRequest(IndivRequest, e.Completion);
    }
    else if (e.Request is PlayReadyLicenseAcquisitionServiceRequest)
    {
        PlayReadyLicenseAcquisitionServiceRequest licenseRequest = 
            e.Request as PlayReadyLicenseAcquisitionServiceRequest;

        LicenseAcquisitionRequest(
            licenseRequest, 
            e.Completion, 
            playReadyLicenseUrl, 
            playReadyChallengeCustomData);
    }
}
```

## <a name="individualization-service-requests"></a>개별화 서비스 요청

다음 코드는 사후 대응적으로 PlayReady 개별화 서비스 요청을 만듭니다. 함수에 매개 변수로 요청을 전달합니다. 호출을 try/catch 블록으로 둘러싸고 예외가 없는 경우 요청이 성공적으로 완료되었다고 합니다.

```csharp
async Task<bool> ReactiveIndivRequest(
    PlayReadyIndividualizationServiceRequest IndivRequest, 
    MediaProtectionServiceCompletion CompletionNotifier)
{
    bool bResult = false;
    Exception exception = null;

    try
    {
        await IndivRequest.BeginServiceRequest();
    }
    catch (Exception ex)
    {
        exception = ex;
    }
    finally
    {
        if (exception == null)
        {
            bResult = true;
        }
        else
        {
            COMException comException = exception as COMException;
            if (comException != null && comException.HResult == MSPR_E_CONTENT_ENABLING_ACTION_REQUIRED)
            {
                IndivRequest.NextServiceRequest();
            }
        }
    }

    if (CompletionNotifier != null) CompletionNotifier.Complete(bResult);
    return bResult;
}
```

또는 개별화 서비스 요청을 사전 대응적으로 만들 수 있으며 이 경우에는 `ProtectionManager_ServiceRequested`의 `ReactiveIndivRequest`을 호출하는 코드 대신 다음 함수를 호출합니다.

```csharp
async void ProActiveIndivRequest()
{
    PlayReadyIndividualizationServiceRequest indivRequest = new PlayReadyIndividualizationServiceRequest();
    bool bResultIndiv = await ReactiveIndivRequest(indivRequest, null);
}
```

## <a name="license-acquisition-service-requests"></a>라이선스 취득 서비스 요청

요청이 [PlayReadyLicenseAcquisitionServiceRequest](https://msdn.microsoft.com/library/windows/apps/dn986285)인 경우에는 다음 함수를 호출하여 PlayReady 라이선스를 요청하고 취득합니다. 요청이 성공했는지 여부를 전달했다고 **MediaProtectionServiceCompletion** 개체에 알리고 요청을 완료합니다.

```csharp
async void LicenseAcquisitionRequest(
    PlayReadyLicenseAcquisitionServiceRequest licenseRequest, 
    MediaProtectionServiceCompletion CompletionNotifier, 
    string Url, 
    string ChallengeCustomData)
{
    bool bResult = false;
    string ExceptionMessage = string.Empty;

    try
    {
        if (!string.IsNullOrEmpty(Url))
        {
            if (!string.IsNullOrEmpty(ChallengeCustomData))
            {
                System.Text.UTF8Encoding encoding = new System.Text.UTF8Encoding();
                byte[] b = encoding.GetBytes(ChallengeCustomData);
                licenseRequest.ChallengeCustomData = Convert.ToBase64String(b, 0, b.Length);
            }

            PlayReadySoapMessage soapMessage = licenseRequest.GenerateManualEnablingChallenge();

            byte[] messageBytes = soapMessage.GetMessageBody();
            HttpContent httpContent = new ByteArrayContent(messageBytes);

            IPropertySet propertySetHeaders = soapMessage.MessageHeaders;

            foreach (string strHeaderName in propertySetHeaders.Keys)
            {
                string strHeaderValue = propertySetHeaders[strHeaderName].ToString();

                if (strHeaderName.Equals("Content-Type", StringComparison.OrdinalIgnoreCase))
                {
                    httpContent.Headers.ContentType = MediaTypeHeaderValue.Parse(strHeaderValue);
                }
                else
                {
                    httpContent.Headers.Add(strHeaderName.ToString(), strHeaderValue);
                }
            }

            CommonLicenseRequest licenseAcquision = new CommonLicenseRequest();

            HttpContent responseHttpContent = 
                await licenseAcquision.AcquireLicense(new Uri(Url), httpContent);

            if (responseHttpContent != null)
            {
                Exception exResult = licenseRequest.ProcessManualEnablingResponse(
                                         await responseHttpContent.ReadAsByteArrayAsync());

                if (exResult != null)
                {
                    throw exResult;
                }
                bResult = true;
            }
            else
            {
                ExceptionMessage = licenseAcquision.GetLastErrorMessage();
            }
        }
        else
        {
            await licenseRequest.BeginServiceRequest();
            bResult = true;
        }
    }
    catch (Exception e)
    {
        ExceptionMessage = e.Message;
    }

    CompletionNotifier.Complete(bResult);
}
```

## <a name="initializing-the-adaptivemediasource"></a>AdaptiveMediaSource 초기화

마지막으로, 지정된 [Uri](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx) 및 [MediaElement](https://msdn.microsoft.com/library/windows/apps/br242926)에서 만든 [AdaptiveMediaSource](https://msdn.microsoft.com/library/windows/apps/dn946912)를 초기화하는 함수가 필요합니다. **Uri**는 미디어 파일(HLS 또는 DASH)에 대한 링크여야 하며 **MediaElement**는 XAML에 정의되어야 합니다.

```csharp
async private void InitializeAdaptiveMediaSource(System.Uri uri, MediaElement m)
{
    AdaptiveMediaSourceCreationResult result = await AdaptiveMediaSource.CreateFromUriAsync(uri);
    if (result.Status == AdaptiveMediaSourceCreationStatus.Success)
    {
        ams = result.MediaSource;
        SetUpProtectionManager(ref m);
        m.SetMediaStreamSource(ams);
    }
    else
    {
        // Error handling
    }
}
```

적응 스트리밍의 시작을 처리하는 모든 이벤트(예: 단추 Click 이벤트)에서 이 기능을 호출할 수 있습니다.

## <a name="see-also"></a>참고 항목
- [PlayReady DRM](playready-client-sdk.md)





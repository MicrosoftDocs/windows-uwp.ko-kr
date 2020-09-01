---
ms.assetid: BF877F23-1238-4586-9C16-246F3F25AE35
description: 이 문서에서는 Microsoft PlayReady content protection을 사용 하는 멀티미디어 콘텐츠의 적응 스트리밍을 유니버설 Windows 플랫폼 (UWP) 앱에 추가 하는 방법을 설명 합니다.
title: PlayReady를 사용 하 여 적응 스트리밍
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3bef1e1061948c4327426485621b9f611fc51f21
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161247"
---
# <a name="adaptive-streaming-with-playready"></a>PlayReady를 사용한 적응 스트리밍


이 문서에서는 Microsoft PlayReady content protection을 사용 하는 멀티미디어 콘텐츠의 적응 스트리밍을 유니버설 Windows 플랫폼 (UWP) 앱에 추가 하는 방법을 설명 합니다. 

이 기능은 현재 HTTP (대시) 콘텐츠를 통한 동적 스트리밍 재생을 지원 합니다.

HLS (Apple의 HTTP 라이브 스트리밍)는 PlayReady에서 지원 되지 않습니다.

부드러운 스트리밍은 현재 기본적으로 지원 되지 않습니다. 그러나 PlayReady는 확장 가능 하며 추가 코드 또는 라이브러리를 사용 하 여 소프트웨어 또는 하드웨어 DRM (디지털 권한 관리)을 활용 하 여 PlayReady로 보호 된 부드러운 스트리밍을 지원할 수 있습니다.

이 문서는 PlayReady와 관련 된 적응 스트리밍의 측면만 다룹니다. 일반에서 적응 스트리밍을 구현 하는 방법에 대 한 자세한 내용은 [적응 스트리밍](adaptive-streaming.md)을 참조 하세요.

이 문서에서는 GitHub의 Microsoft **Windows 유니버설 샘플** 리포지토리에서 [적응 스트리밍 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AdaptiveStreaming) 의 코드를 사용 합니다. 시나리오 4에서는 PlayReady로 적응 스트리밍을 사용 하는 방법을 다룹니다. 리포지토리의 루트 수준으로 이동 하 고 **Zip 다운로드** 단추를 선택 하 여 zip 파일에서 리포지토리를 다운로드할 수 있습니다.

다음 **using 문을 사용** 해야 합니다.

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

**LicenseRequest** 네임 스페이스는 Microsoft에서 정식으로 제공 하는 PlayReady 파일인 **CommonLicenseRequest.cs**에서 가져온 것입니다.

몇 가지 전역 변수를 선언 해야 합니다.

```csharp
private AdaptiveMediaSource ams = null;
private MediaProtectionManager protectionManager = null;
private string playReadyLicenseUrl = "";
private string playReadyChallengeCustomData = "";
```

다음 상수를 선언 하는 것도 좋습니다.

```csharp
private const uint MSPR_E_CONTENT_ENABLING_ACTION_REQUIRED = 0x8004B895;
```

## <a name="setting-up-the-mediaprotectionmanager"></a>MediaProtectionManager 설정

UWP 앱에 PlayReady 콘텐츠 보호를 추가 하려면 [MediaProtectionManager](/uwp/api/Windows.Media.Protection.MediaProtectionManager) 개체를 설정 해야 합니다. [**AdaptiveMediaSource**](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource) 개체를 초기화할 때이 작업을 수행 합니다.

다음 코드는 [MediaProtectionManager](/uwp/api/Windows.Media.Protection.MediaProtectionManager)을 설정 합니다.

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

이 코드는 콘텐츠 보호를 설정 하는 데 필요 하므로 앱에 간단 하 게 복사할 수 있습니다.

Blob [Loadfailed](/uwp/api/windows.media.protection.mediaprotectionmanager.componentloadfailed) 이벤트는 이진 데이터의 로드가 실패할 때 발생 합니다. 이를 처리 하는 이벤트 처리기를 추가 해야 합니다 .이를 통해 로드가 완료 되지 않았다는 신호를 발생 시킬 수 있습니다.

```csharp
private void ProtectionManager_ComponentLoadFailed(
    MediaProtectionManager sender, 
    ComponentLoadFailedEventArgs e)
{
    e.Completion.Complete(false);
}
```

마찬가지로 서비스가 요청 될 때 발생 하는 [ServiceRequested](/uwp/api/windows.media.protection.mediaprotectionmanager.servicerequested) 이벤트에 대 한 이벤트 처리기를 추가 해야 합니다. 이 코드는 요청 종류를 확인 하 고 적절 하 게 응답 합니다.

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

다음 코드 대응적는 PlayReady 개별화 서비스 요청을 수행 합니다. 요청을 함수에 대 한 매개 변수로 전달 합니다. Try/catch 블록에 호출을 묶고, 예외가 없는 경우 요청이 성공적으로 완료 되었다고 표시 합니다.

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

또는에서를 호출 하는 코드 대신 다음 함수를 호출 하는 경우 개별화 서비스 요청을 사전에 만들 수 있습니다 `ReactiveIndivRequest` `ProtectionManager_ServiceRequested` .

```csharp
async void ProActiveIndivRequest()
{
    PlayReadyIndividualizationServiceRequest indivRequest = new PlayReadyIndividualizationServiceRequest();
    bool bResultIndiv = await ReactiveIndivRequest(indivRequest, null);
}
```

## <a name="license-acquisition-service-requests"></a>라이선스 취득 서비스 요청

대신 요청이 [PlayReadyLicenseAcquisitionServiceRequest](/uwp/api/Windows.Media.Protection.PlayReady.PlayReadyLicenseAcquisitionServiceRequest)인 경우 다음 함수를 호출 하 여 PlayReady 라이선스를 요청 하 고 가져옵니다. 요청이 성공 했는지 여부를 전달 하는 **MediaProtectionServiceCompletion** 개체를 알리고 요청을 완료 합니다.

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

마지막으로, 지정 된 [Uri](/dotnet/api/system.uri) 및 [MediaElement](/uwp/api/Windows.UI.Xaml.Controls.MediaElement)에서 만든 [AdaptiveMediaSource](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource)를 초기화 하는 함수가 필요 합니다. **Uri** 는 미디어 파일에 대 한 링크 (HLS 또는 대시) 여야 합니다. **MediaElement** 는 XAML에서 정의 해야 합니다.

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

적응 스트리밍의 시작을 처리 하는 모든 이벤트에서이 함수를 호출할 수 있습니다. 예를 들어 단추 클릭 이벤트에 있습니다.

## <a name="see-also"></a>참고 항목
- [PlayReady DRM](playready-client-sdk.md)
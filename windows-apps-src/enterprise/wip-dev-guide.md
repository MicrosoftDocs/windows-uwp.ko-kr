---
Description: 이 가이드는 Windows Information Protection (WIP) 정책 및 개인 데이터에 의해 관리 되는 엔터프라이즈 데이터를 처리 하도록 앱을 간소화 하는 데 도움이 됩니다.
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Windows Information Protection (WIP) 개발자 가이드
ms.date: 06/21/2017
ms.topic: article
keywords: windows 10, uwp, wip, Windows Information Protection, 엔터프라이즈 데이터, 엔터프라이즈 데이터 보호, edp, 지원 apps
ms.assetid: 913ac957-ea49-43b0-91b3-e0f6ca01ef2c
ms.localizationpriority: medium
ms.openlocfilehash: d6454fdf63fb757c703ec31dba46a86e2a46aec6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163257"
---
# <a name="windows-information-protection-wip-developer-guide"></a>Windows Information Protection (WIP) 개발자 가이드

*지원* 앱은 회사와 개인 데이터를 구분 하 고 관리자가 정의한 Windows INFORMATION PROTECTION (WIP) 정책을 기반으로 보호 해야 하는 것을 알고 있습니다.

이 가이드에서는 빌드하는 방법을 보여 줍니다. 완료 되 면 정책 관리자는 앱을 신뢰 하 여 조직의 데이터를 사용할 수 있습니다. 또한 직원은 조직의 MDM(모바일 디바이스 관리)에서 등록을 취소하거나 조직에서 완전히 퇴사한 경우에도 자신의 개인 데이터가 디바이스에 그대로 유지되기를 바랍니다.

__참고__ 이 가이드를 통해 UWP 앱을 간편 하 게 사용할 수 있습니다. C + + Windows 데스크톱 앱을 밝게 하려면 [windows Information Protection (WIP) 개발자 가이드 (c + +)](/previous-versions/windows/desktop/EDP/wip-developer-guide)를 참조 하세요.

WIP 및 지원 apps에 대 한 자세한 내용은 [Windows Information Protection (wip)](wip-hub.md)를 참조 하세요.

[여기](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/EnterpriseDataProtection)에서 전체 샘플을 찾을 수 있습니다.

각 작업을 진행할 준비가 되었으면 시작 해 보겠습니다.

## <a name="first-gather-what-you-need"></a>먼저 필요한 항목을 수집 합니다.

다음이 필요 합니다.

* Windows 10 버전 1607 이상을 실행 하는 테스트 가상 머신 (VM). 이 테스트 VM에 대해 앱을 디버그 합니다.

* Windows 10 버전 1607 이상을 실행 하는 개발 컴퓨터. Visual Studio가 설치 되어 있는 경우 테스트 VM이 될 수 있습니다.

## <a name="setup-your-development-environment"></a>개발 환경 설정

다음 작업을 수행 합니다.

* [테스트 VM에 WIP 설정 개발자 도우미 설치](#install-assistant)

* [WIP 설정 개발자 도우미를 사용 하 여 보호 정책 만들기](#create-protection-policy)

* [Visual Studio 프로젝트 설정](#setup-vs-project)

* [원격 디버깅 설정](#setup-remote-debugging)

* [코드 파일에 네임 스페이스 추가](#add-namespaces)

<a id="install-assistant" />

### <a name="install-the-wip-setup-developer-assistant-onto-your-test-vm"></a>테스트 VM에 WIP 설정 개발자 도우미 설치

 이 도구를 사용 하 여 테스트 VM에서 Windows Information Protection 정책을 설정 합니다.

 [WIP 설치 개발자 도우미](https://www.microsoft.com/store/p/wip-setup-developer-assistant/9nblggh526jf)에서 도구를 다운로드 합니다.

<a id="create-protection-policy" />

### <a name="create-a-protection-policy"></a>보호 정책 만들기

WIP 설정 개발자 도우미의 각 섹션에 정보를 추가 하 여 정책을 정의 합니다. 사용 방법에 대 한 자세한 내용을 보려면 설정 옆에 있는 도움말 아이콘을 선택 합니다.

이 도구를 사용 하는 방법에 대 한 일반적인 지침은 앱 다운로드 페이지의 버전 참고 섹션을 참조 하세요.

<a id="setup-vs-project" />

### <a name="setup-a-visual-studio-project"></a>Visual Studio 프로젝트 설정

1. 개발 컴퓨터에서 프로젝트를 엽니다.

2. 데스크톱 및 UWP (모바일 확장 유니버설 Windows 플랫폼)에 대 한 참조를 추가 합니다.

    ![UWP 확장 추가](images/extensions.png)

3. 패키지 매니페스트 파일에이 기능을 추가 합니다.

    ```xml
       <rescap:Capability Name="enterpriseDataPolicy"/>
    ```
   >*선택적 읽기*: "rescap" 접두사는 *제한 된 기능*을 의미 합니다. [특수 기능 및 제한 된 기능](../packaging/app-capability-declarations.md)을 참조 하세요.

4. 패키지 매니페스트 파일에이 네임 스페이스를 추가 합니다.

    ```xml
      xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
    ```
5. 패키지 매니페스트 파일의 요소에 네임 스페이스 접두사를 추가 ``<ignorableNamespaces>`` 합니다.

    ```xml
        <IgnorableNamespaces="uap mp rescap">
    ```

    이러한 방식으로 앱이 제한 된 기능을 지원 하지 않는 Windows 운영 체제 버전에서 실행 되는 경우 Windows에서 기능을 무시 ``enterpriseDataPolicy`` 합니다.

<a id="setup-remote-debugging" />

### <a name="setup-remote-debugging"></a>원격 디버깅 설정

VM이 아닌 컴퓨터에서 앱을 개발 하는 경우에만 테스트 VM에 Visual Studio 원격 도구를 설치 합니다. 그런 다음 개발 컴퓨터에서 원격 디버거를 시작 하 고 테스트 VM에서 앱이 실행 되는지 확인 합니다.

[원격 PC 지침](../debug-test-perf/deploying-and-debugging-uwp-apps.md)을 참조 하세요.

<a id="add-namespaces" />

### <a name="add-these-namespaces-to-your-code-files"></a>이러한 네임 스페이스를 코드 파일에 추가 합니다.

이러한 using 문을 코드 파일의 맨 위에 추가 합니다 .이 가이드의 코드 조각은이를 사용 합니다.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Windows.Security.EnterpriseData;
using Windows.Web.Http;
using Windows.Storage.Streams;
using Windows.ApplicationModel.DataTransfer;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml;
using Windows.ApplicationModel.Activation;
using Windows.Web.Http.Filters;
using Windows.Storage;
using Windows.Data.Xml.Dom;
using Windows.Foundation.Metadata;
using Windows.Web.Http.Headers;
```

## <a name="determine-whether-to-use-wip-apis-in-your-app"></a>앱에서 WIP Api를 사용할지 여부 결정

앱을 실행 하는 운영 체제가 WIP를 지원 하 고 WIP가 장치에서 사용 되는지 확인 합니다.

```csharp
bool use_WIP_APIs = false;

if ((ApiInformation.IsApiContractPresent
    ("Windows.Security.EnterpriseData.EnterpriseDataContract", 3)
    && ProtectionPolicyManager.IsProtectionEnabled))
{
    use_WIP_APIs = true;
}
else
{
    use_WIP_APIs = false;
}
```
운영 체제가 WIP를 지원 하지 않거나 장치에서 WIP를 사용 하도록 설정 하지 않은 경우 WIP Api를 호출 하지 마세요.

## <a name="read-enterprise-data"></a>엔터프라이즈 데이터 읽기

공유 계약에서 받아들이는 보호 된 파일, 네트워크 끝점, 클립보드 데이터 및 데이터를 읽으려면 앱에서 액세스를 요청 해야 합니다.

앱이 보호 정책의 허용 된 목록에 있는 경우 Windows Information Protection에서 앱 사용 권한을 부여 합니다.

**섹션 내용**

* [파일에서 데이터 읽기](#read-file)
* [네트워크 끝점에서 데이터 읽기](#read-network)
* [클립보드에서 데이터 읽기](#read-clipboard)
* [공유 계약에서 데이터 읽기](#read-share)

<a id="read-file" />

### <a name="read-data-from-a-file"></a>파일에서 데이터 읽기

**1 단계: 파일 핸들 가져오기**

```csharp
    Windows.Storage.StorageFolder storageFolder =
        Windows.Storage.ApplicationData.Current.LocalFolder;

    Windows.Storage.StorageFile file =
        await storageFolder.GetFileAsync(fileName);
```

**2 단계: 앱이 파일을 열 수 있는지 확인**

FileProtectionManager를 호출 하 여 앱이 파일을 열 수 있는지 여부를 확인 [합니다](/uwp/api/windows.security.enterprisedata.fileprotectionmanager.getprotectioninfoasync) .

```csharp
FileProtectionInfo protectionInfo = await FileProtectionManager.GetProtectionInfoAsync(file);

if ((protectionInfo.Status != FileProtectionStatus.Protected &&
    protectionInfo.Status != FileProtectionStatus.Unprotected))
{
    return false;
}
else if (protectionInfo.Status == FileProtectionStatus.Revoked)
{
    // Code goes here to handle this situation. Perhaps, show UI
    // saying that the user's data has been revoked.
}
```

[FileProtectionStatus](/uwp/api/windows.security.enterprisedata.fileprotectionstatus) 값이 **protected** 이면 파일이 보호 된 것 이며 앱이 정책의 허용 목록에 있기 때문에 앱을 열 수 있습니다.

[FileProtectionStatus](/uwp/api/windows.security.enterprisedata.fileprotectionstatus) 은 파일이 보호 되지 않으며 앱이 정책의 허용 목록에 없는 경우에도 파일을 열 수 **있음을 의미 합니다** .

> **API** <br>
[FileProtectionManager.GetProtectionInfoAsync](/uwp/api/windows.security.enterprisedata.fileprotectionmanager.getprotectioninfoasync)<br>
[FileProtectionInfo](/uwp/api/windows.security.enterprisedata.fileprotectioninfo)<br>
[FileProtectionStatus](/uwp/api/windows.security.enterprisedata.fileprotectionstatus)<br>
[ProtectionPolicyManager](/uwp/api/windows.security.enterprisedata.protectionpolicymanager.isidentitymanaged)

**3 단계: 파일을 스트림 또는 버퍼에 읽기**

*파일을 스트림으로 읽기*

```csharp
var stream = await file.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite);
```

*파일을 버퍼로 읽기*

```csharp
var buffer = await Windows.Storage.FileIO.ReadBufferAsync(file);
```
<a id="read-network" />

### <a name="read-data-from-a-network-endpoint"></a>네트워크 끝점에서 데이터 읽기

엔터프라이즈 끝점에서 읽을 보호 된 스레드 컨텍스트를 만듭니다.

**1 단계: 네트워크 끝점의 id 가져오기**

```csharp
Uri resourceURI = new Uri("http://contoso.com/stockData.xml");

Windows.Networking.HostName hostName =
    new Windows.Networking.HostName(resourceURI.Host);

string identity = await ProtectionPolicyManager.
    GetPrimaryManagedIdentityForNetworkEndpointAsync(hostName);
```

끝점이 정책에 의해 관리 되지 않는 경우 빈 문자열을 다시 받게 됩니다.

> **API** <br>
[ProtectionPolicyManager.GetPrimaryManagedIdentityForNetworkEndpointAsync](/uwp/api/windows.security.enterprisedata.protectionpolicymanager.getprimarymanagedidentityfornetworkendpointasync)


**2 단계: 보호 된 스레드 컨텍스트 만들기**

끝점이 정책에 의해 관리 되는 경우 보호 된 스레드 컨텍스트를 만듭니다. 이는 동일한 스레드에서 만든 모든 네트워크 연결을 id에 태그 지정 합니다.

또한 해당 정책에 의해 관리 되는 엔터프라이즈 네트워크 리소스에 대 한 액세스를 제공 합니다.

```csharp
if (!string.IsNullOrEmpty(identity))
{
    using (ThreadNetworkContext threadNetworkContext =
            ProtectionPolicyManager.CreateCurrentThreadNetworkContext(identity))
    {
        return await GetDataFromNetworkRedirectHelperMethod(resourceURI);
    }
}
else
{
    return await GetDataFromNetworkRedirectHelperMethod(resourceURI);
}
```
이 예제에서는 소켓 호출을 블록으로 묶습니다 ``using`` . 이렇게 하지 않으면 리소스를 검색 한 후에 스레드 컨텍스트를 닫아야 합니다. [Threadnetworkcontext를](/uwp/api/windows.security.enterprisedata.threadnetworkcontext.close)참조 하십시오.

해당 파일이 자동으로 암호화 되므로 보호 된 스레드에서 개인 파일을 만들지 마세요.

[**ProtectionPolicyManager CreateCurrentThreadNetworkContext**](/uwp/api/windows.security.enterprisedata.protectionpolicymanager.createcurrentthreadnetworkcontext) 메서드는 끝점이 정책에 의해 관리 되 고 있는지 여부에 관계 없이 [**threadnetworkcontext**](/uwp/api/windows.security.enterprisedata.threadnetworkcontext) 개체를 반환 합니다. 앱이 개인 및 엔터프라이즈 리소스를 모두 처리 하는 경우 모든 id에 대해 CreateCurrentThreadNetworkContext를 호출 [**ProtectionPolicyManager.**](/uwp/api/windows.security.enterprisedata.protectionpolicymanager.createcurrentthreadnetworkcontext)  리소스를 가져온 후에는 ThreadNetworkContext를 삭제 하 여 현재 스레드에서 모든 id 태그를 지웁니다.

> **API** <br>
[ProtectionPolicyManager.GetForCurrentView](/uwp/api/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview)<br>
[ProtectionPolicyManager](/uwp/api/windows.security.enterprisedata.protectionpolicymanager)<br>
[ProtectionPolicyManager.CreateCurrentThreadNetworkContext](/uwp/api/windows.security.enterprisedata.protectionpolicymanager.createcurrentthreadnetworkcontext)

**3 단계: 리소스를 버퍼로 읽기**

```csharp
private static async Task<IBuffer> GetDataFromNetworkHelperMethod(Uri resourceURI)
{
    HttpClient client;

    client = new HttpClient();

    try { return await client.GetBufferAsync(resourceURI); }

    catch (Exception) { return null; }
}
```

**필드 보호 된 스레드 컨텍스트를 만드는 대신 헤더 토큰을 사용 합니다.**

```csharp
public static async Task<IBuffer> GetDataFromNetworkbyUsingHeader(Uri resourceURI)
{
    HttpClient client;

    Windows.Networking.HostName hostName =
        new Windows.Networking.HostName(resourceURI.Host);

    string identity = await ProtectionPolicyManager.
        GetPrimaryManagedIdentityForNetworkEndpointAsync(hostName);

    if (!string.IsNullOrEmpty(identity))
    {
        client = new HttpClient();

        HttpRequestHeaderCollection headerCollection = client.DefaultRequestHeaders;

        headerCollection.Add("X-MS-Windows-HttpClient-EnterpriseId", identity);

        return await GetDataFromNetworkbyUsingHeaderHelperMethod(client, resourceURI);
    }
    else
    {
        client = new HttpClient();
        return await GetDataFromNetworkbyUsingHeaderHelperMethod(client, resourceURI);
    }

}

private static async Task<IBuffer> GetDataFromNetworkbyUsingHeaderHelperMethod(HttpClient client, Uri resourceURI)
{

    try { return await client.GetBufferAsync(resourceURI); }

    catch (Exception) { return null; }
}
```

**페이지 리디렉션 처리**

웹 서버에서 트래픽을 더 최신 버전의 리소스로 리디렉션하는 경우도 있습니다.

이를 처리 하려면 요청의 응답 상태 값이 **OK**가 될 때까지 요청을 수행 합니다.

그런 다음 해당 응답의 URI를 사용 하 여 끝점의 id를 가져옵니다. 이 작업을 수행 하는 한 가지 방법은 다음과 같습니다.

```csharp
private static async Task<IBuffer> GetDataFromNetworkRedirectHelperMethod(Uri resourceURI)
{
    HttpClient client = null;

    HttpBaseProtocolFilter filter = new HttpBaseProtocolFilter();
    filter.AllowAutoRedirect = false;

    client = new HttpClient(filter);

    HttpResponseMessage response = null;

        HttpRequestMessage message = new HttpRequestMessage(HttpMethod.Get, resourceURI);
        response = await client.SendRequestAsync(message);

    if (response.StatusCode == HttpStatusCode.MultipleChoices ||
        response.StatusCode == HttpStatusCode.MovedPermanently ||
        response.StatusCode == HttpStatusCode.Found ||
        response.StatusCode == HttpStatusCode.SeeOther ||
        response.StatusCode == HttpStatusCode.NotModified ||
        response.StatusCode == HttpStatusCode.UseProxy ||
        response.StatusCode == HttpStatusCode.TemporaryRedirect ||
        response.StatusCode == HttpStatusCode.PermanentRedirect)
    {
        message = new HttpRequestMessage(HttpMethod.Get, message.RequestUri);
        response = await client.SendRequestAsync(message);

        try { return await response.Content.ReadAsBufferAsync(); }

        catch (Exception) { return null; }
    }
    else
    {
        try { return await response.Content.ReadAsBufferAsync(); }

        catch (Exception) { return null; }
    }
}

```

> **API** <br>
[ProtectionPolicyManager.GetPrimaryManagedIdentityForNetworkEndpointAsync](/uwp/api/windows.security.enterprisedata.protectionpolicymanager.getprimarymanagedidentityfornetworkendpointasync)<br>
[ProtectionPolicyManager.CreateCurrentThreadNetworkContext](/uwp/api/windows.security.enterprisedata.protectionpolicymanager.createcurrentthreadnetworkcontext)<br>
[ProtectionPolicyManager.GetForCurrentView](/uwp/api/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview)<br>
[ProtectionPolicyManager](/uwp/api/windows.security.enterprisedata.protectionpolicymanager)

<a id="read-clipboard" />

### <a name="read-data-from-the-clipboard"></a>클립보드에서 데이터 읽기

**클립보드의 데이터를 사용할 수 있는 권한 가져오기**

클립보드에서 데이터를 가져오려면 Windows에 사용 권한을 요청 합니다. [**DataPackageView**](/uwp/api/windows.applicationmodel.datatransfer.datapackageview.requestaccessasync) 를 사용 하 여이 작업을 수행 합니다.

```csharp
public static async Task PasteText(TextBox textBox)
{
    DataPackageView dataPackageView = Clipboard.GetContent();

    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        ProtectionPolicyEvaluationResult result = await dataPackageView.RequestAccessAsync();

        if (result == ProtectionPolicyEvaluationResult..Allowed)
        {
            string contentsOfClipboard = await dataPackageView.GetTextAsync();
            textBox.Text = contentsOfClipboard;
        }
    }
}
```

> **API** <br>
[DataPackageView](/uwp/api/windows.applicationmodel.datatransfer.datapackageview.requestaccessasync)

**클립보드 데이터를 사용 하는 기능 숨기기 또는 사용 안 함**

현재 뷰에 클립보드에 있는 데이터를 가져올 수 있는 권한이 있는지 여부를 확인 합니다.

그렇지 않으면 사용자가 클립보드에서 정보를 붙여넣거나 내용을 미리 볼 수 있는 컨트롤을 사용 하지 않도록 설정 하거나 숨길 수 있습니다.

```csharp
private bool IsClipboardAllowedAsync()
{
    ProtectionPolicyEvaluationResult protectionPolicyEvaluationResult = ProtectionPolicyEvaluationResult.Blocked;

    DataPackageView dataPackageView = Clipboard.GetContent();

    if (dataPackageView.Contains(StandardDataFormats.Text))

        protectionPolicyEvaluationResult =
            ProtectionPolicyManager.CheckAccess(dataPackageView.Properties.EnterpriseId,
                ProtectionPolicyManager.GetForCurrentView().Identity);

    return (protectionPolicyEvaluationResult == ProtectionPolicyEvaluationResult.Allowed |
        protectionPolicyEvaluationResult == ProtectionPolicyEvaluationResult.ConsentRequired);
}
```

> **API** <br>
[ProtectionPolicyEvaluationResult](/uwp/api/windows.security.enterprisedata.protectionpolicyevaluationresult)<br>
[ProtectionPolicyManager.GetForCurrentView](/uwp/api/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview)<br>
[ProtectionPolicyManager](/uwp/api/windows.security.enterprisedata.protectionpolicymanager)

**사용자에 게 동의 대화 상자가 표시 되지 않도록 방지**

새 문서는 *개인* 또는 *엔터프라이즈*가 아닙니다. 바로 새로운 기능입니다. 사용자가 엔터프라이즈 데이터를 해당 사용자에 게 붙여 넣으면 Windows에서 정책을 적용 하 고 사용자에 게 동의 대화 상자가 표시 됩니다. 이 코드는이를 방지 합니다. 이 작업은 데이터를 보호 하는 데 도움이 되지 않습니다. 앱에서 새 항목을 만드는 경우 사용자가 동의 대화 상자를 수신 하지 못하도록 하는 것이 더 좋습니다.

```csharp
private async void PasteText(bool isNewEmptyDocument)
{
    DataPackageView dataPackageView = Clipboard.GetContent();

    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        if (!string.IsNullOrEmpty(dataPackageView.Properties.EnterpriseId))
        {
            if (isNewEmptyDocument)
            {
                ProtectionPolicyManager.TryApplyProcessUIPolicy(dataPackageView.Properties.EnterpriseId);
                string contentsOfClipboard = contentsOfClipboard = await dataPackageView.GetTextAsync();
                // add this string to the new item or document here.          

            }
            else
            {
                ProtectionPolicyEvaluationResult result = await dataPackageView.RequestAccessAsync();

                if (result == ProtectionPolicyEvaluationResult.Allowed)
                {
                    string contentsOfClipboard = contentsOfClipboard = await dataPackageView.GetTextAsync();
                    // add this string to the new item or document here.
                }
            }
        }
    }
}
```

> **API** <br>
[DataPackageView](/uwp/api/windows.applicationmodel.datatransfer.datapackageview.requestaccessasync)<br>
[ProtectionPolicyEvaluationResult](/uwp/api/windows.security.enterprisedata.protectionpolicyevaluationresult)<br>
[ProtectionPolicyManager에 대 한](/uwp/api/windows.security.enterprisedata.protectionpolicymanager.tryapplyprocessuipolicy)

<a id="read-share" />

### <a name="read-data-from-a-share-contract"></a>공유 계약에서 데이터 읽기

직원 들이 자신의 정보를 공유할 앱을 선택 하면 앱이 해당 콘텐츠를 포함 하는 새 항목을 엽니다.

앞서 언급 했 듯이 새 항목은 *개인* 또는 *엔터프라이즈*가 아닙니다. 바로 새로운 기능입니다. 코드에서 엔터프라이즈 콘텐츠를 항목에 추가 하는 경우 Windows는 정책을 적용 하 고 사용자에 게 동의 대화 상자를 표시 합니다. 이 코드는이를 방지 합니다.

```csharp
protected override async void OnShareTargetActivated(ShareTargetActivatedEventArgs args)
{
    bool isNewEmptyDocument = true;
    string identity = "corp.microsoft.com";

    ShareOperation shareOperation = args.ShareOperation;
    if (shareOperation.Data.Contains(StandardDataFormats.Text))
    {
        if (!string.IsNullOrEmpty(shareOperation.Data.Properties.EnterpriseId))
        {
            if (isNewEmptyDocument)
                // If this is a new and empty document, and we're allowed to access
                // the data, then we can avoid popping the consent dialog
                ProtectionPolicyManager.TryApplyProcessUIPolicy(shareOperation.Data.Properties.EnterpriseId);
            else
            {
                // In this case, we can't optimize the workflow, so we just
                // request consent from the user in this case.

                ProtectionPolicyEvaluationResult protectionPolicyEvaluationResult = await shareOperation.Data.RequestAccessAsync();

                if (protectionPolicyEvaluationResult == ProtectionPolicyEvaluationResult.Allowed)
                {
                    string text = await shareOperation.Data.GetTextAsync();

                    // Do something with that text.
                }
            }
        }
        else
        {
            // If the data has no enterprise identity, then we already have access.
            string text = await shareOperation.Data.GetTextAsync();

            // Do something with that text.
        }

    }

}
```

> **API** <br>
[ProtectionPolicyManager](/uwp/api/windows.security.enterprisedata.protectionpolicymanager.requestaccessasync)<br>
[ProtectionPolicyEvaluationResult](/uwp/api/windows.security.enterprisedata.protectionpolicyevaluationresult)<br>
[ProtectionPolicyManager에 대 한](/uwp/api/windows.security.enterprisedata.protectionpolicymanager.tryapplyprocessuipolicy)

## <a name="protect-enterprise-data"></a>엔터프라이즈 데이터 보호

앱을 떠나는 엔터프라이즈 데이터를 보호합니다. 데이터는 응용 프로그램을 페이지에 표시 하거나, 파일 또는 네트워크 끝점에 저장 하거나, 공유 계약을 통해 응용 프로그램을 그대로 유지 합니다.

**섹션 내용**

* [페이지에 표시 되는 데이터 보호](#protect-pages)
* [파일에 대 한 데이터를 백그라운드 프로세스로 보호](#protect-background)
* [파일의 일부 보호](#protect-part-file)
* [파일의 보호 된 부분 읽기](#read-protected)
* [폴더에 대 한 데이터 보호](#protect-folder)
* [네트워크 끝점으로 데이터 보호](#protect-network)
* [공유 계약을 통해 앱이 공유 하는 데이터 보호](#protect-share)
* [다른 위치로 복사 하는 파일 보호](#protect-other-location)
* [장치의 화면이 잠길 때 엔터프라이즈 데이터 보호](#protect-locked)

<a id="protect-pages" />

### <a name="protect-data-that-appears-in-pages"></a>페이지에 표시 되는 데이터 보호

페이지에 데이터를 표시 하는 경우 Windows에서 데이터 형식 (개인 또는 엔터프라이즈)을 알 수 있습니다. 이렇게 하려면 현재 앱 보기에 *태그* 를 표시 하거나 전체 앱 프로세스에 태그를 표시 합니다.

보기 또는 프로세스에 태그를 적용 하면 Windows에서 정책을 적용 합니다. 이렇게 하면 응용 프로그램에서 제어 하지 않는 작업을 통해 발생 하는 데이터 누출을 방지할 수 있습니다. 예를 들어 컴퓨터에서 사용자는 CTRL + V를 사용 하 여 보기에서 엔터프라이즈 정보를 복사한 다음 해당 정보를 다른 앱에 붙여 넣을 수 있습니다. Windows에서이를 방지 합니다. Windows는 공유 계약을 적용 하는 데도 도움이 됩니다.

**현재 앱 보기에 태그 표시**

앱에 일부 보기가 엔터프라이즈 데이터를 사용 하 고 일부 보기가 개인 데이터를 사용 하는 여러 보기가 있는 경우이 작업을 수행 합니다.

```csharp

// tag as enterprise data. "identity" the string that contains the enterprise ID.
// You'd get that from a file, network endpoint, or clipboard data package.
ProtectionPolicyManager.GetForCurrentView().Identity = identity;

// tag as personal data.
ProtectionPolicyManager.GetForCurrentView().Identity = String.Empty;
```

> **API** <br>
[ProtectionPolicyManager.GetForCurrentView](/uwp/api/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview)<br>
[ProtectionPolicyManager](/uwp/api/windows.security.enterprisedata.protectionpolicymanager)

**프로세스 태그**

앱의 모든 보기가 한 유형의 데이터 (개인 또는 엔터프라이즈)만 사용 하는 경우이 작업을 수행 합니다.

이렇게 하면 독립적으로 태그가 지정 된 보기를 관리할 필요가 없습니다.

```csharp


// tag as enterprise data. "identity" the string that contains the enterprise ID.
// You'd get that from a file, network endpoint, or clipboard data package.
bool result =
            ProtectionPolicyManager.TryApplyProcessUIPolicy(identity);

// tag as personal data.
ProtectionPolicyManager.ClearProcessUIPolicy();
```

> **API** <br>
[ProtectionPolicyManager에 대 한](/uwp/api/windows.security.enterprisedata.protectionpolicymanager.tryapplyprocessuipolicy)

<a id="protect-file" />

### <a name="protect-data-to-a-file"></a>파일에 대 한 데이터 보호

보호 된 파일을 만든 다음 여기에 씁니다.

**1 단계: 앱에서 엔터프라이즈 파일을 만들 수 있는지 확인**

Id 문자열이 정책에 의해 관리 되 고 앱이 해당 정책의 허용 목록에 있으면 앱에서 엔터프라이즈 파일을 만들 수 있습니다.

```csharp
  if (!ProtectionPolicyManager.IsIdentityManaged(identity)) return false;
```

> **API** <br>
[ProtectionPolicyManager](/uwp/api/windows.security.enterprisedata.protectionpolicymanager.isidentitymanaged)


**2 단계: 파일을 만들고 id로 보호**

```csharp
StorageFolder storageFolder = ApplicationData.Current.LocalFolder;
StorageFile storageFile = await storageFolder.CreateFileAsync("sample.txt",
    CreationCollisionOption.ReplaceExisting);

FileProtectionInfo fileProtectionInfo =
    await FileProtectionManager.ProtectAsync(storageFile, identity);
```

> **API** <br>
[FileProtectionManager.ProtectAsync](/uwp/api/windows.security.enterprisedata.fileprotectionmanager.protectasync)

**3 단계: 해당 스트림 또는 버퍼를 파일에 씁니다.**

*스트림 작성*

```csharp
    if (fileProtectionInfo.Status == FileProtectionStatus.Protected)
    {
        var stream = await storageFile.OpenAsync(FileAccessMode.ReadWrite);

        using (var outputStream = stream.GetOutputStreamAt(0))
        {
            using (var dataWriter = new DataWriter(outputStream))
            {
                dataWriter.WriteString(enterpriseData);
            }
        }

    }
```

*버퍼 작성*

```csharp
     if (fileProtectionInfo.Status == FileProtectionStatus.Protected)
     {
         var buffer = Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(
             enterpriseData, Windows.Security.Cryptography.BinaryStringEncoding.Utf8);

         await FileIO.WriteBufferAsync(storageFile, buffer);

      }
```

> **API** <br>
[FileProtectionInfo](/uwp/api/windows.security.enterprisedata.fileprotectioninfo)<br>
[FileProtectionStatus](/uwp/api/windows.security.enterprisedata.fileprotectionstatus)<br>

<a id="protect-background" />

### <a name="protect-data-to-a-file-as-a-background-process"></a>파일에 대 한 데이터를 백그라운드 프로세스로 보호

이 코드는 장치의 화면이 잠겨 있는 동안 실행할 수 있습니다. 관리자가 보안 "잠금 상태에서 데이터 보호" (DPL) 정책을 구성한 경우 Windows는 장치 메모리에서 보호 된 리소스에 액세스 하는 데 필요한 암호화 키를 제거 합니다. 이렇게 하면 장치를 분실 한 경우 데이터 누수가 방지 됩니다. 이 기능을 사용 하면 핸들을 닫을 때 보호 된 파일에 연결 된 키도 제거 됩니다.

파일을 만들 때 파일 핸들을 열린 상태로 유지 하는 방법을 사용 해야 합니다.  

**1 단계: 엔터프라이즈 파일을 만들 수 있는지 확인**

사용 중인 id가 정책에 의해 관리 되 고 앱이 해당 정책의 허용 목록에 있는 경우 엔터프라이즈 파일을 만들 수 있습니다.

```csharp
if (!ProtectionPolicyManager.IsIdentityManaged(identity)) return false;
```

> **API** <br>
[ProtectionPolicyManager](/uwp/api/windows.security.enterprisedata.protectionpolicymanager.isidentitymanaged)

**2 단계: 파일을 만들고 id로 보호**

FileProtectionManager는 파일을 작성 하는 동안 파일 핸들을 열어 둡니다 [**. CreateProtectedAndOpenAsync**](/uwp/api/windows.security.enterprisedata.fileprotectionmanager.createprotectedandopenasync) 는 보호 된 파일을 만듭니다.

```csharp
StorageFolder storageFolder = ApplicationData.Current.LocalFolder;

ProtectedFileCreateResult protectedFileCreateResult =
    await FileProtectionManager.CreateProtectedAndOpenAsync(storageFolder,
        "sample.txt", identity, CreationCollisionOption.ReplaceExisting);
```

> **API** <br>
[FileProtectionManager.CreateProtectedAndOpenAsync](/uwp/api/windows.security.enterprisedata.fileprotectionmanager.createprotectedandopenasync)

**3 단계: 파일에 스트림 또는 버퍼 작성**

이 예제에서는 스트림을 파일에 씁니다.

```csharp
if (protectedFileCreateResult.ProtectionInfo.Status == FileProtectionStatus.Protected)
{
    IOutputStream outputStream =
        protectedFileCreateResult.Stream.GetOutputStreamAt(0);

    using (DataWriter writer = new DataWriter(outputStream))
    {
        writer.WriteString(enterpriseData);
        await writer.StoreAsync();
        await writer.FlushAsync();
    }

    outputStream.Dispose();
}
else if (protectedFileCreateResult.ProtectionInfo.Status == FileProtectionStatus.AccessSuspended)
{
    // Perform any special processing for the access suspended case.
}

```

> **API** <br>
[ProtectedFileCreateResult.ProtectionInfo](/uwp/api/windows.security.enterprisedata.protectedfilecreateresult.protectioninfo)<br>
[FileProtectionStatus](/uwp/api/windows.security.enterprisedata.fileprotectionstatus)<br>
[ProtectedFileCreateResult](/uwp/api/windows.security.enterprisedata.protectedfilecreateresult.stream)<br>

<a id="protect-part-file" />

### <a name="protect-part-of-a-file"></a>파일의 일부 보호

대부분의 경우 엔터프라이즈 및 개인 데이터를 별도로 저장 하는 것이 더 깔끔하고, 원하는 경우 동일한 파일에 저장할 수 있습니다. 예를 들어 Microsoft Outlook은 단일 보관 파일에 개인 메일을 함께 하는 엔터프라이즈 메일을 저장할 수 있습니다.

전체 파일이 아닌 엔터프라이즈 데이터를 암호화 합니다. 이렇게 하면 사용자가 MDM에서 등록을 취소 하거나 해당 엔터프라이즈 데이터 액세스 권한이 해지 된 경우에도 해당 파일을 계속 사용할 수 있습니다. 또한 앱은 암호화 된 데이터를 추적 하 여 파일을 메모리로 다시 읽을 때 보호할 데이터를 알고 있어야 합니다.

**1 단계: 암호화 된 스트림 또는 버퍼에 엔터프라이즈 데이터 추가**

```csharp
string enterpriseDataString = "<employees><employee><name>Bill</name><social>xxx-xxx-xxxx</social></employee></employees>";

var enterpriseData= Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(
        enterpriseDataString, Windows.Security.Cryptography.BinaryStringEncoding.Utf8);

BufferProtectUnprotectResult result =
   await DataProtectionManager.ProtectAsync(enterpriseData, identity);

enterpriseData= result.Buffer;
```

> **API** <br>
[Microsoft.systemcenter.dataprotectionmanager.2012.reporting.mp. ProtectAsync](/uwp/api/windows.security.enterprisedata.dataprotectionmanager.protectasync)<br>
[BufferProtectUnprotectResult](/uwp/api/windows.security.enterprisedata.bufferprotectunprotectresult.buffer)


**2 단계: 암호화 되지 않은 스트림 또는 버퍼에 개인 데이터 추가**

```csharp
string personalDataString = "<recipies><recipe><name>BillsCupCakes</name><cooktime>30</cooktime></recipe></recipies>";

var personalData = Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(
    personalDataString, Windows.Security.Cryptography.BinaryStringEncoding.Utf8);
```

**3 단계: 파일에 스트림 또는 버퍼 작성**

```csharp
StorageFolder storageFolder = ApplicationData.Current.LocalFolder;

StorageFile storageFile = await storageFolder.CreateFileAsync("data.xml",
    CreationCollisionOption.ReplaceExisting);

 // Write both buffers to the file and save the file.

var stream = await storageFile.OpenAsync(FileAccessMode.ReadWrite);

using (var outputStream = stream.GetOutputStreamAt(0))
{
    using (var dataWriter = new DataWriter(outputStream))
    {
        dataWriter.WriteBuffer(enterpriseData);
        dataWriter.WriteBuffer(personalData);

        await dataWriter.StoreAsync();
        await outputStream.FlushAsync();
    }
}
```

**4 단계: 파일에서 엔터프라이즈 데이터의 위치 추적**

엔터프라이즈 소유 파일의 데이터를 추적 하는 것은 앱의 책임입니다.

파일, 데이터베이스 또는 파일의 일부 헤더 텍스트와 연결 된 속성에 해당 정보를 저장할 수 있습니다.

이 예에서는이 정보를 별도의 XML 파일에 저장 합니다.

```csharp
StorageFile metaDataFile = await storageFolder.CreateFileAsync("metadata.xml",
   CreationCollisionOption.ReplaceExisting);

await Windows.Storage.FileIO.WriteTextAsync
    (metaDataFile, "<EnterpriseDataMarker start='0' end='" + enterpriseData.Length.ToString() +
    "'></EnterpriseDataMarker>");
```
<a id="read-protected" />

### <a name="read-the-protected-part-of-a-file"></a>파일의 보호 된 부분 읽기

이 파일에서 엔터프라이즈 데이터를 읽는 방법은 다음과 같습니다.

**1 단계: 파일에서 엔터프라이즈 데이터의 위치를 가져옵니다.**

```csharp
Windows.Storage.StorageFolder storageFolder =
    Windows.Storage.ApplicationData.Current.LocalFolder;

 Windows.Storage.StorageFile metaDataFile =
   await storageFolder.GetFileAsync("metadata.xml");

string metaData = await Windows.Storage.FileIO.ReadTextAsync(metaDataFile);

XmlDocument doc = new XmlDocument();

doc.LoadXml(metaData);

uint startPosition =
    Convert.ToUInt16((doc.FirstChild.Attributes.GetNamedItem("start")).InnerText);

uint endPosition =
    Convert.ToUInt16((doc.FirstChild.Attributes.GetNamedItem("end")).InnerText);
```

**2 단계: 데이터 파일을 열고 보호 되지 않는지 확인 합니다.**

```csharp
Windows.Storage.StorageFile dataFile =
    await storageFolder.GetFileAsync("data.xml");

FileProtectionInfo protectionInfo =
    await FileProtectionManager.GetProtectionInfoAsync(dataFile);

if (protectionInfo.Status == FileProtectionStatus.Protected)
    return false;
```

> **API** <br>
[FileProtectionManager.GetProtectionInfoAsync](/uwp/api/windows.security.enterprisedata.fileprotectionmanager.getprotectioninfoasync)<br>
[FileProtectionInfo](/uwp/api/windows.security.enterprisedata.fileprotectioninfo)<br>
[FileProtectionStatus](/uwp/api/windows.security.enterprisedata.fileprotectionstatus)<br>

**3 단계: 파일에서 엔터프라이즈 데이터 읽기**

```csharp
var stream = await dataFile.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite);

stream.Seek(startPosition);

Windows.Storage.Streams.Buffer tempBuffer = new Windows.Storage.Streams.Buffer(50000);

IBuffer enterpriseData = await stream.ReadAsync(tempBuffer, endPosition, InputStreamOptions.None);
```

**4 단계: 엔터프라이즈 데이터를 포함 하는 버퍼 암호 해독**

```csharp
DataProtectionInfo dataProtectionInfo =
   await DataProtectionManager.GetProtectionInfoAsync(enterpriseData);

if (dataProtectionInfo.Status == DataProtectionStatus.Protected)
{
    BufferProtectUnprotectResult result = await DataProtectionManager.UnprotectAsync(enterpriseData);
    enterpriseData = result.Buffer;
}
else if (dataProtectionInfo.Status == DataProtectionStatus.Revoked)
{
    // Code goes here to handle this situation. Perhaps, show UI
    // saying that the user's data has been revoked.
}

```

> **API** <br>
[DataProtectionInfo](/uwp/api/windows.security.enterprisedata.dataprotectioninfo)<br>
[Microsoft.systemcenter.dataprotectionmanager.2012.reporting.mp. GetProtectionInfoAsync](/uwp/api/windows.security.enterprisedata.dataprotectionmanager.getstreamprotectioninfoasync)<br>

<a id="protect-folder" />

### <a name="protect-data-to-a-folder"></a>폴더에 대 한 데이터 보호

폴더를 만들고 보호할 수 있습니다. 이렇게 하면 해당 폴더에 추가 하는 모든 항목이 자동으로 보호 됩니다.

```csharp
private async Task<bool> CreateANewFolderAndProtectItAsync(string folderName, string identity)
{
    if (!ProtectionPolicyManager.IsIdentityManaged(identity)) return false;

    StorageFolder storageFolder = ApplicationData.Current.LocalFolder;
    StorageFolder newStorageFolder =
        await storageFolder.CreateFolderAsync(folderName);

    FileProtectionInfo fileProtectionInfo =
        await FileProtectionManager.ProtectAsync(newStorageFolder, identity);

    if (fileProtectionInfo.Status != FileProtectionStatus.Protected)
    {
        // Protection failed.
        return false;
    }
    return true;
}
```

보호 하기 전에 폴더가 비어 있는지 확인 합니다. 이미 항목이 들어 있는 폴더는 보호할 수 없습니다.

> **API** <br>
[ProtectionPolicyManager](/uwp/api/windows.security.enterprisedata.protectionpolicymanager.isidentitymanaged)<br>
[FileProtectionManager.ProtectAsync](/uwp/api/windows.security.enterprisedata.fileprotectionmanager.protectasync)<br>
[FileProtectionInfo](/uwp/api/windows.security.enterprisedata.fileprotectioninfo.identity)<br>
[FileProtectionInfo](/uwp/api/windows.security.enterprisedata.fileprotectioninfo.status)

<a id="protect-network" />

### <a name="protect-data-to-a-network-end-point"></a>네트워크 끝점으로 데이터 보호

엔터프라이즈 끝점에 해당 데이터를 보내는 보호 된 스레드 컨텍스트를 만듭니다.  

**1 단계: 네트워크 끝점의 id 가져오기**

```csharp
Windows.Networking.HostName hostName =
    new Windows.Networking.HostName(resourceURI.Host);

string identity = await ProtectionPolicyManager.
    GetPrimaryManagedIdentityForNetworkEndpointAsync(hostName);
```

> **API** <br>
[ProtectionPolicyManager.GetPrimaryManagedIdentityForNetworkEndpointAsync](/uwp/api/windows.security.enterprisedata.protectionpolicymanager.getprimarymanagedidentityfornetworkendpointasync)

**2 단계: 보호 된 스레드 컨텍스트를 만들고 네트워크 끝점으로 데이터 보내기**

```csharp
HttpClient client = null;

if (!string.IsNullOrEmpty(m_EnterpriseId))
{
    ProtectionPolicyManager.GetForCurrentView().Identity = identity;

    using (ThreadNetworkContext threadNetworkContext =
            ProtectionPolicyManager.CreateCurrentThreadNetworkContext(identity))
    {
        client = new HttpClient();
        HttpRequestMessage message = new HttpRequestMessage(HttpMethod.Put, resourceURI);
        message.Content = new HttpStreamContent(dataToWrite);

        HttpResponseMessage response = await client.SendRequestAsync(message);

        if (response.StatusCode == HttpStatusCode.Ok)
            return true;
        else
            return false;
    }
}
else
{
    return false;
}
```

> **API** <br>
[ProtectionPolicyManager.GetForCurrentView](/uwp/api/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview)<br>
[ProtectionPolicyManager](/uwp/api/windows.security.enterprisedata.protectionpolicymanager)<br>
[ProtectionPolicyManager.CreateCurrentThreadNetworkContext](/uwp/api/windows.security.enterprisedata.protectionpolicymanager.createcurrentthreadnetworkcontext)

<a id="protect-share" />

### <a name="protect-data-that-your-app-shares-through-a-share-contract"></a>공유 계약을 통해 앱이 공유 하는 데이터 보호

사용자가 앱에서 콘텐츠를 공유 하도록 하려면 공유 계약을 구현 하 고 [**DataTransferManager 요청**](/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.datarequested) 된 이벤트를 처리 해야 합니다.

이벤트 처리기에서 데이터 패키지의 엔터프라이즈 id 컨텍스트를 설정 합니다.

```csharp
private void OnShareSourceOperation(object sender, RoutedEventArgs e)
{
    // Register the current page as a share source (or you could do this earlier in your app).
    DataTransferManager.GetForCurrentView().DataRequested += OnDataRequested;
    DataTransferManager.ShowShareUI();
}

private void OnDataRequested(DataTransferManager sender, DataRequestedEventArgs args)
{
    if (!string.IsNullOrEmpty(this.shareSourceContent))
    {
        var protectionPolicyManager = ProtectionPolicyManager.GetForCurrentView();
        DataPackage requestData = args.Request.Data;
        requestData.Properties.Title = this.shareSourceTitle;
        requestData.Properties.EnterpriseId = protectionPolicyManager.Identity;
        requestData.SetText(this.shareSourceContent);
    }
}
```

> **API** <br>
[ProtectionPolicyManager.GetForCurrentView](/uwp/api/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview)<br>
[ProtectionPolicyManager](/uwp/api/windows.security.enterprisedata.protectionpolicymanager)

<a id="protect-other-location" />

### <a name="protect-files-that-you-copy-to-another-location"></a>다른 위치로 복사 하는 파일 보호

```csharp
private async void CopyProtectionFromOneFileToAnother
    (StorageFile sourceStorageFile, StorageFile targetStorageFile)
{
    bool copyResult = await
        FileProtectionManager.CopyProtectionAsync(sourceStorageFile, targetStorageFile);

    if (!copyResult)
    {
        // Copying failed. To diagnose, you could check the file's status.
        // (call FileProtectionManager.GetProtectionInfoAsync and
        // check FileProtectionInfo.Status).
    }
}
```

> **API** <br>
[FileProtectionManager.CopyProtectionAsync](/uwp/api/windows.security.enterprisedata.fileprotectionmanager.copyprotectionasync)<br>

<a id="protect-locked" />

### <a name="protect-enterprise-data-when-the-screen-of-the-device-is-locked"></a>장치의 화면이 잠길 때 엔터프라이즈 데이터 보호

장치가 잠겨 있을 때 메모리에서 중요 한 데이터를 모두 제거 합니다. 사용자가 장치를 잠금 해제할 때 앱에서 해당 데이터를 안전 하 게 추가할 수 있습니다.

[**ProtectionPolicyManager ProtectedAccessSuspending**](/uwp/api/windows.security.enterprisedata.protectionpolicymanager.protectedaccesssuspending) 이벤트를 처리 하 여 응용 프로그램이 화면을 잠글 때를 알 수 있습니다. 이 이벤트는 관리자가 잠금 정책에서 보안 데이터 보호를 구성 하는 경우에만 발생 합니다. Windows는 장치에 프로 비전 된 데이터 보호 키를 일시적으로 제거 합니다. 장치가 잠겨 있는 동안에는 암호화 된 데이터에 무단으로 액세스할 수 없고 소유자를 소유 하 고 있지 않을 수 있도록 Windows에서 이러한 키를 제거 합니다.  

[**ProtectionPolicyManager ProtectedAccessResumed**](/uwp/api/windows.security.enterprisedata.protectionpolicymanager.protectedaccessresumed) 이벤트를 처리 하 여 화면 잠금 해제 시점을 응용 프로그램에서 알 수 있도록 합니다. 이 이벤트는 관리자가 잠금 정책에서 보안 데이터 보호를 구성 하는지 여부에 관계 없이 발생 합니다.

#### <a name="remove-sensitive-data-in-memory-when-the-screen-is-locked"></a>화면이 잠길 때 메모리의 중요 한 데이터 제거

중요 한 데이터를 보호 하 고, 앱이 보호 된 파일에서 연 파일 스트림을 닫아 시스템이 중요 한 데이터를 메모리에 캐시 하지 않도록 합니다.

이 예제에서는 textblock의 콘텐츠를 암호화 된 버퍼에 저장 하 고 해당 textblock에서 콘텐츠를 제거 합니다.

```csharp
private async void ProtectionPolicyManager_ProtectedAccessSuspending(object sender, ProtectedAccessSuspendingEventArgs e)
{
    Deferral deferral = e.GetDeferral();

    if (ProtectionPolicyManager.GetForCurrentView().Identity != String.Empty)
    {
        IBuffer documentBodyBuffer = CryptographicBuffer.ConvertStringToBinary
           (documentTextBlock.Text, BinaryStringEncoding.Utf8);

        BufferProtectUnprotectResult result = await DataProtectionManager.ProtectAsync
            (documentBodyBuffer, ProtectionPolicyManager.GetForCurrentView().Identity);

        if (result.ProtectionInfo.Status == DataProtectionStatus.Protected)
        {
            this.protectedDocumentBuffer = result.Buffer;
            documentTextBlock.Text = null;
        }
    }

    // Close any open streams that you are actively working with
    // to make sure that we have no unprotected content in memory.

    // Optionally, code goes here to use e.Deadline to determine whether we have more
    // than 15 seconds left before the suspension deadline. If we do then process any
    // messages queued up for sending while we are still able to access them.

    deferral.Complete();
}
```

> **API** <br>
[ProtectionPolicyManager.ProtectedAccessSuspending](/uwp/api/windows.security.enterprisedata.protectionpolicymanager.protectedaccesssuspending)<br>
[ProtectionPolicyManager.GetForCurrentView](/uwp/api/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview)<br>
[ProtectionPolicyManager](/uwp/api/windows.security.enterprisedata.protectionpolicymanager)</br>
[Microsoft.systemcenter.dataprotectionmanager.2012.reporting.mp. ProtectAsync](/uwp/api/windows.security.enterprisedata.dataprotectionmanager.protectasync)<br>
[BufferProtectUnprotectResult](/uwp/api/windows.security.enterprisedata.bufferprotectunprotectresult.buffer)<br>
[ProtectedAccessSuspendingEventArgs. GetDeferral](/uwp/api/windows.security.enterprisedata.protectedaccesssuspendingeventargs.getdeferral)<br>
[지연 완료](/uwp/api/windows.foundation.deferral.complete)<br>

#### <a name="add-back-sensitive-data-when-the-device-is-unlocked"></a>장치의 잠금을 해제할 때 중요 한 데이터를 다시 추가 합니다.

[**ProtectionPolicyManager**](/uwp/api/windows.security.enterprisedata.protectionpolicymanager.protectedaccessresumed) 장치를 잠금 해제 하 고 장치에서 키를 다시 사용할 수 있는 경우 ProtectedAccessResumed이 발생 합니다.

관리자가 잠금 정책에서 보안 데이터 보호를 구성 하지 않은 경우에는 ProtectedAccessResumedEventArgs가 빈 컬렉션입니다 [**.**](/uwp/api/windows.security.enterprisedata.protectedaccessresumedeventargs.identities)

이 예제에서는 이전 예제를 반대로 수행 합니다. 버퍼의 암호를 해독 하 고 해당 버퍼의 정보를 다시 텍스트 상자에 추가한 다음 버퍼를 삭제 합니다.

```csharp
private async void ProtectionPolicyManager_ProtectedAccessResumed(object sender, ProtectedAccessResumedEventArgs e)
{
    if (ProtectionPolicyManager.GetForCurrentView().Identity != String.Empty)
    {
        BufferProtectUnprotectResult result = await DataProtectionManager.UnprotectAsync
            (this.protectedDocumentBuffer);

        if (result.ProtectionInfo.Status == DataProtectionStatus.Unprotected)
        {
            // Restore the unprotected version.
            documentTextBlock.Text = CryptographicBuffer.ConvertBinaryToString
                (BinaryStringEncoding.Utf8, result.Buffer);
            this.protectedDocumentBuffer = null;
        }
    }

}
```

> **API** <br>
[ProtectionPolicyManager.ProtectedAccessResumed](/uwp/api/windows.security.enterprisedata.protectionpolicymanager.protectedaccessresumed)<br>
[ProtectionPolicyManager.GetForCurrentView](/uwp/api/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview)<br>
[ProtectionPolicyManager](/uwp/api/windows.security.enterprisedata.protectionpolicymanager)</br>
[Microsoft.systemcenter.dataprotectionmanager.2012.reporting.mp. UnprotectAsync](/uwp/api/windows.security.enterprisedata.dataprotectionmanager.unprotectasync)<br>
[BufferProtectUnprotectResult](/uwp/api/windows.security.enterprisedata.bufferprotectunprotectresult)<br>

## <a name="handle-enterprise-data-when-protected-content-is-revoked"></a>보호 된 콘텐츠가 해지 될 때 엔터프라이즈 데이터 처리

MDM에서 장치 등록을 취소 하거나 정책 관리자가 엔터프라이즈 데이터에 대 한 액세스를 명시적으로 취소 하는 경우 앱에 알리도록 하려면 [**ProtectionPolicyManager_ProtectedContentRevoked**](/uwp/api/windows.security.enterprisedata.protectionpolicymanager.protectedcontentrevoked) 이벤트를 처리 합니다.

이 예에서는 전자 메일 앱에 대 한 엔터프라이즈 사서함의 데이터가 해지 되었는지 여부를 확인 합니다.

```csharp
private string mailIdentity = "contoso.com";

void MailAppSetup()
{
    ProtectionPolicyManager.ProtectedContentRevoked += ProtectionPolicyManager_ProtectedContentRevoked;
    // Code goes here to set up mailbox for 'mailIdentity'.
}

private void ProtectionPolicyManager_ProtectedContentRevoked(object sender, ProtectedContentRevokedEventArgs e)
{
    if (!new System.Collections.Generic.List<string>(e.Identities).Contains
        (this.mailIdentity))
    {
        // This event is not for our identity.
        return;
    }

    // Code goes here to delete any metadata associated with 'mailIdentity'.
}
```

> **API** <br>
[ProtectionPolicyManager_ProtectedContentRevoked](/uwp/api/windows.security.enterprisedata.protectionpolicymanager.protectedcontentrevoked)<br>

## <a name="related-topics"></a>관련 항목

[WIP (Windows Information Protection) 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/EnterpriseDataProtection)
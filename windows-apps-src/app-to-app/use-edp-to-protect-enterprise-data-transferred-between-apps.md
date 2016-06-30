---
author: awkoren
Description: "이 항목에서는 가장 일반적인 데이터 전송 관련 EDP(엔터프라이즈 데이터 보호) 시나리오 중 일부를 달성하기 위해 필요한 코딩 작업의 예를 보여 줍니다."
MS-HAID: dev\_app\_to\_app.use\_edp\_to\_protect\_enterprise\_data\_transferred\_between\_apps
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: "EDP를 사용하여 앱 간에 전송되는 엔터프라이즈 데이터 보호"
translationtype: Human Translation
ms.sourcegitcommit: 36bc5dcbefa6b288bf39aea3df42f1031f0b43df
ms.openlocfilehash: 77533d4aca3cc84e0a021a0faac57f5afbbefdd7

---

# EDP를 사용하여 앱 간에 전송되는 엔터프라이즈 데이터 보호

__참고__ Windows 10 버전 1511(빌드 10586) 이전에서는 EDP(엔터프라이즈 데이터 보호) 정책을 적용할 수 없습니다.

이 항목에서는 가장 일반적인 데이터 전송 관련 EDP(엔터프라이즈 데이터 보호) 시나리오 중 일부를 달성하기 위해 필요한 코딩 작업의 예를 보여 줍니다. EDP 기능이 파일, 스트림, 클립보드, 네트워킹, 백그라운드 작업 및 잠금 상태의 데이터 보호와 어떤 관계가 있는지를 보여 주는 전체 개발자 그림을 보려면 [EDP(엔터프라이즈 데이터 보호)](../enterprise/edp-hub.md)를 참조하세요.

**참고** [EDP(엔터프라이즈 데이터 보호) 샘플](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409)에서는 이 항목에 설명된 것과 동일한 여러 시나리오를 다룹니다.

## 필수 조건


-   **EDP 설정**

    [EDP를 위한 컴퓨터 설정](../enterprise/edp-hub.md#set-up-your-computer-for-EDP)을 참조하세요.

-   **엔터프라이즈 인식(enterprise-enlightened) 앱 빌드 구현**

    자체적으로 엔터프라이즈 데이터가 엔터프라이즈를 통해 관리되도록 하는 앱을 엔터프라이즈 인식(enterprise-enlightened) 앱이라고 합니다. 인식 앱은 비인식 앱보다 더 강력하고 스마트하고 유연하며 신뢰할 수 있습니다. 앱은 제한된 **enterpriseDataPolicy** 기능을 선언하여 인식되도록 시스템에 알립니다. 그렇지만 기능을 설정하는 것 이상의 효과가 있습니다. 자세한 내용은 [엔터프라이즈 인식(enterprise-enlightened) 앱](../enterprise/edp-hub.md#enterprise-enlightened-apps)을 참조하세요.

-   **UWP(유니버설 Windows 플랫폼) 앱에 대한 비동기 프로그래밍 이해**

    C\# 또는 Visual Basic에서 비동기 앱을 작성하는 방법에 대한 자세한 내용은 [C\# 또는 Visual Basic에서 비동기 API 호출](https://msdn.microsoft.com/library/windows/apps/mt187337)을 참조하세요. C++에서 비동기 앱을 작성하는 방법에 대한 자세한 내용은 [C++의 비동기 프로그래밍](https://msdn.microsoft.com/library/windows/apps/mt187334)을 참조하세요.

## 간단한 클립보드 소스


이 시나리오에서 사용자 앱은 개인 파일과 엔터프라이즈 파일 둘 다를 처리하는 메모장 유형의 앱입니다. 여기서는 앱이 복사하여 붙여넣기 논리를 변경할 필요가 없으며 사용자가 엔터프라이즈 문서를 열고 콘텐츠를 표시할 때마다 [**ProtectionPolicyManager.TryApplyProcessUIPolicy**](https://msdn.microsoft.com/library/windows/apps/dn705791)를 호출하기만 하면 됩니다. 콘텐츠가 앱의 UI에 표시되면 사용자가 복사하여 다른 앱에 붙여넣을 수 있으며, UI 정책 설정이 중요한 것은 이런 이유 때문입니다. 이 경우 운영 체제가 보호된 데이터와 관련된 붙여넣기 작업에 현재 설정된 정책을 적용할 수 있습니다. 마찬가지로, UI 정책이 더 이상 필요하지 않으면 즉시 정책을 지워 사용자가 다시 자유롭게 개인 데이터를 복사하여 붙여넣을 수 있도록 하는 것이 중요합니다. ID 인수가 엔터프라이즈 정책에 의해 관리되지 않는 경우 **TryApplyProcessUIPolicy**에서 false가 반환됩니다.

```CSharp
using Windows.Security.EnterpriseData;

...

private void OnFileLoaded(FileProtectionInfo fileProtectionInfo, string contentsOfFile)
{
    if (fileProtectionInfo.Status == FileProtectionStatus.Protected)
    {
        bool isIdentityManaged = ProtectionPolicyManager.TryApplyProcessUIPolicy
            (fileProtectionInfo.Identity);
        if (isIdentityManaged)
        {
            // UI policy is now in effect for the file's identity.
        }
        else
        {
            // Enterprise policy is not in effect, because the file's identity
            // is not managed. In this case, we have a file protected to an
            // unmanaged identity, which is not a valid situation.
            // We still have to call ClearProcessUIPolicy if we want to clear the policy.
            ProtectionPolicyManager.ClearProcessUIPolicy();
        }
    }
    else
    {
        // In case we applied the policy for the previous file, now we need to clear it.
        ProtectionPolicyManager.ClearProcessUIPolicy();
    }
    // Code goes here to display "contentsOfFile" in your UI. Ready for copy-paste...
}
```

## 간단한 클립보드 대상


이 시나리오에서 사용자 앱은 개인 계정과 회사 계정 둘 다를 처리하는 메일 앱입니다. 사용자가 개인 계정을 사용하여 메일 회신에 데이터를 붙여넣으려고 합니다. 이 경우 앱이 콘텐츠를 검색하기 전에 [**DataPackage.RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706636)를 호출하기만 하면 됩니다. 이미 액세스 권한이 있는 경우 해당 메서드가 즉시 반환됩니다. 그러나 액세스 권한이 없는 경우 메서드로 인해 사용자 승인을 요청하고 승인이 부여되면 데이터 패키지를 "잠금 해제"하는 대화 상자가 표시됩니다. 이는 사용자가 실수로 수행한 붙여넣기 작업을 취소할 수 있는 기회를 제공하기 위한 것입니다.

```CSharp
using Windows.ApplicationModel.DataTransfer;

...

private async void OnPasteSimple()
{
    DataPackageView dataPackageView = Clipboard.GetContent();
    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        // In case we don't already have acccess, request consent from the user
        // for us to access the clipboard data.
        await dataPackageView.RequestAccessAsync();

        try
        {
            string contentsOfClipboard = await dataPackageView.GetTextAsync();
            // Code goes here to insert the text into the email...
        }
        catch (Exception)
        {
            // Code goes here to handle the exception retrieving text from the clipboard.
        }
    }
    else
    {
        // The value on the clipboard is not in the text format.
    }
}
```

## 클립보드 대상이 비어 있는 중립 문서임


이 시나리오에서 사용자 앱은 워드 프로세싱 앱입니다. 새 문서를 만든 후 앱은 문서가 비어 있는 한 해당 문서를 중립(엔터프라이즈 문서도 아니고 개인 문서도 아님)으로 처리합니다. 그런 다음 사용자가 이 중립 컨텍스트 문서에 엔터프라이즈 데이터를 붙여넣습니다. 문서가 중립 컨텍스트에 있으므로 실제로 문서를 엔터프라이즈 컨텍스트로 전환하기만 하면 [**DataPackage.RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706636)를 호출하지 않아도 됩니다(이 경우 대화 상자를 표시할 필요가 없음). 따라서 데이터가 보호된 경우 엔터프라이즈 컨텍스트로 전환한 다음 데이터를 붙여넣으면 됩니다.

```CSharp
using Windows.ApplicationModel.DataTransfer;

...

private async void OnPasteWithApplyPolicy()
{
    DataPackageView dataPackageView = Clipboard.GetContent();
    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        // If the data has no enterprise identity, then we already have access.
        if (!string.IsNullOrEmpty(dataPackageView.Properties.EnterpriseId))
        {
            ProtectionPolicyEvaluationResult policyResult =
                await dataPackageView.RequestAccessAsync(dataPackageView.Properties.EnterpriseId);
            if (this.isNewEmptyDocument &&
                policyResult == ProtectionPolicyEvaluationResult.Allowed)
            {
                // If this is a new and empty document, and we're allowed to access
                // the data, then we can avoid popping the consent dialog.
                bool isIdentityManaged = ProtectionPolicyManager.TryApplyProcessUIPolicy(dataPackageView.Properties.EnterpriseId);
                // You can use "isIdentityManaged", but it's not necessary.
            }
            else
            {
                // In this case, we can't optimize the workflow, so we just
                // request consent from the user in this case.
                await dataPackageView.RequestAccessAsync();
            }
        }

        try
        {
            string contentsOfClipboard = await dataPackageView.GetTextAsync();
            // Code goes here to insert the text into the email...
        }
        catch (Exception)
        {
            // Code goes here to handle the exception retrieving text from the clipboard.
        }
    }
    else
    {
        // The value on the clipboard is not in the text format.
    }
}
```

## 클립보드 소스 및 명시적 엔터프라이즈 ID


이 시나리오에서 사용자 앱은 워드 프로세싱 앱입니다. 이 앱은 백그라운드 스레드를 사용하여 사용자의 복사 작업을 커밋합니다. 사용자는 엔터프라이즈 파일에서 일부 데이터를 복사한 다음 개인 파일로 즉시 전환하며, 이때 앱의 전역 컨텍스트가 개인이 됩니다. 전역 상태가 변경되었으며 사용하면 안 되므로 이때 앱의 백그라운드 스레드에서 들어오는 데이터가 엔터프라이즈임을 클립보드에 명시적으로 알려야 합니다. 이렇게 하려면 [**DataPackagePropertySet.EnterpriseId**](https://msdn.microsoft.com/library/windows/apps/dn913861) 속성을 설정합니다.

```CSharp
using Windows.ApplicationModel.DataTransfer;

...

private void CopyToClipboard(string stringToCopy, string identity)
{
    // Copy the string to the clipboard.
    var dataPackage = new DataPackage();
    dataPackage.SetText(stringToCopy);
    dataPackage.Properties.EnterpriseId = identity;
    Clipboard.SetContent(dataPackage);
}
```

## 엔터프라이즈 ID로 특정 창에 태그 지정


이 시나리오에서 사용자 앱은 다양한 창에서 일부는 엔터프라이즈 문서이고 다른 일부는 개인 문서인 여러 문서를 처리하는 워드 프로세싱 앱입니다. 이 앱은 개인 문서 창에 붙여넣는 데이터가 올바르게 확인되고(즉, 엔터프라이즈 데이터인 경우 거부되거나 승인됨) 엔터프라이즈 문서 창에서 보내는 모든 데이터가 올바르게 표시되도록 합니다. 이렇게 하려면 [**ProtectionPolicyManager.Identity**](https://msdn.microsoft.com/library/windows/apps/dn705785) 속성을 설정합니다.

```CSharp
using Windows.Security.EnterpriseData;

...

private void TagCurrentViewWithEnterpriseId(string identity)
{
    ProtectionPolicyManager.GetForCurrentView().Identity = identity;
}
```

## 엔터프라이즈 ID로 보내는 끌기 개체에 태그 지정


사용자 앱에 끌 수 있는 엔터프라이즈 콘텐츠가 포함된 개인 창이 열려 있습니다. 사용자가 이 콘텐츠의 일부를 끌기 시작하며, 앱은 콘텐츠가 엔터프라이즈로 표시되는지 확인해야 합니다. 이 시나리오에서는 새 API가 사용되지 않습니다. 이 시나리오의 경우 앱이 [**DataPackagePropertySet.EnterpriseId**](https://msdn.microsoft.com/library/windows/apps/dn913861) 속성을 설정합니다(위의 [클립보드 소스 및 명시적 엔터프라이즈 ID 시나리오](#clipboard_source_explicit_id) 참조).

## 쿼리가 엔터프라이즈 ID에 대한 끌기 개체를 받음


앱에는 비어 있는 한 중립으로 가정되는 새로운 빈 문서가 열려 있으며, 사용자가 일부 콘텐츠를 문서로 끌어서 놓습니다. 이제 앱이 문서의 상태를 적절하게 변경하기 위해 개체의 엔터프라이즈 ID를 확인해야 합니다. 이 시나리오의 경우 앱이 [**DataPackagePropertySet.EnterpriseId**](https://msdn.microsoft.com/library/windows/apps/dn913861)를 읽어 데이터 패키지에서 **EnterpriseId** 속성을 가져옵니다(위의 [클립보드 소스 및 명시적 엔터프라이즈 ID 시나리오](#clipboard_source_explicit_id) 참조).

## 공유 계약 소스로 사용되는 사용자 앱


사용자 앱에서 공유 계약을 지원하는 경우 공유 소스를 설정하려면 이 코드 예제와 같이 [**DataPackage**](https://msdn.microsoft.com/library/windows/apps/br205873)에서 엔터프라이즈 ID 컨텍스트를 설정합니다.

**참고** 이 코드 예제에서는 현재 보기에 대한 보호 정책 관리자 개체의 ID를 이미 설정했다고 가정합니다([엔터프라이즈 ID로 특정 창에 태그 지정](#tag_window_with_id) 참조). 그렇지 않으면 [**ProtectionPolicyManager.Identity**](https://msdn.microsoft.com/library/windows/apps/dn705785) 속성에 빈 문자열이 포함됩니다.



```CSharp
using Windows.ApplicationModel.DataTransfer;
using Windows.Security.EnterpriseData;

...

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

## 공유 계약 대상으로 사용되는 사용자 앱


이 코드 예제에서는 작업 중인 파일이 비어 있는 한 들어오는 데이터에 맞게 컨텍스트를 자유롭게 전환할 수 있고 가능한 경우 승인 UI 표시를 방지하는 정책을 계속합니다. 따라서 앱이 공유 계약 대상으로 데이터를 받을 경우 기존 콘텐츠가 없으면 [**ProtectionPolicyManager.TryApplyProcessUIPolicy**](https://msdn.microsoft.com/library/windows/apps/dn705791)를 호출해야 하고, 그렇지 않으면 [**DataPackage.RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706636)를 호출해야 합니다. 코드 예제에서 방법을 보여 줍니다.

```CSharp
using Windows.ApplicationModel.DataTransfer;
using Windows.ApplicationModel.DataTransfer.ShareTarget;

...

string identity = "microsoft.com";

protected override async void OnShareTargetActivated(ShareTargetActivatedEventArgs args)
{
    ShareOperation shareOperation = args.ShareOperation;
    if (shareOperation.Data.Contains(StandardDataFormats.Text))
    {
        // If the data has no enterprise identity, then we already have access.
        if (!string.IsNullOrEmpty(shareOperation.Data.Properties.EnterpriseId))
        {
            ProtectionPolicyEvaluationResult protectionPolicyEvaluationResult =
                await ProtectionPolicyManager.RequestAccessAsync
                    (shareOperation.Data.Properties.EnterpriseId, identity);
            if (this.isNewEmptyDocument && protectionPolicyEvaluationResult ==
                ProtectionPolicyEvaluationResult.Allowed)
            {
                // If this is a new and empty document, and we're allowed to access
                // the data, then we can avoid popping the consent dialog.
                bool isIdentityManaged = ProtectionPolicyManager.TryApplyProcessUIPolicy
                    (shareOperation.Data.Properties.EnterpriseId);
                // You can use "isIdentityManaged", but it';s not necessary.
            }
            else
            {
                // In this case, we can't optimize the workflow, so we just
                // request consent from the user in this case.
                protectionPolicyEvaluationResult = await shareOperation.Data.RequestAccessAsync();
            }
        }

        try
        {
            // Get the text from the share operation...
            App.shareTargetContent = await shareOperation.Data.GetTextAsync();
        }
        catch (Exception)
        {
            // Code goes here to handle the exception retrieving text from the share operation.
        }
    }
    else
    {
        // The value in the share operation is not in the text format.
    }
}
```

## 소극적으로 복사-붙여넣기 정책 쿼리


이 시나리오에서 사용자 앱은 데이터가 클립보드에 있는 경우에만 붙여넣기 UI를 사용할 수 있도록 합니다. 이 기능의 경우 정책의 소극적 확인을 허용하는 [**ProtectionPolicyManager.CheckAccess**](https://msdn.microsoft.com/library/windows/apps/dn705783) 메서드를 사용할 수 있습니다.

**참고** 이 코드 예제에서는 현재 보기에 대한 보호 정책 관리자 개체의 ID를 이미 설정했다고 가정합니다([엔터프라이즈 ID로 특정 창에 태그 지정](#tag_window_with_id) 참조). 그렇지 않으면 [**ProtectionPolicyManager.Identity**](https://msdn.microsoft.com/library/windows/apps/dn705785) 속성에 빈 문자열이 포함됩니다.



```CSharp
using Windows.ApplicationModel.DataTransfer;
using Windows.Security.EnterpriseData;

...

private bool IsClipboardPeekAllowedAsync()
{
    ProtectionPolicyEvaluationResult protectionPolicyEvaluationResult = ProtectionPolicyEvaluationResult.Blocked;
    DataPackageView dataPackageView = Clipboard.GetContent();
    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        protectionPolicyEvaluationResult =
            ProtectionPolicyManager.CheckAccess(dataPackageView.Properties.EnterpriseId,
                ProtectionPolicyManager.GetForCurrentView().Identity);
    }

    // Since this is a convenience feature to allow a peek of clipboard content,
    // if state is Blocked or ConsentRequired, do not show peek if this helper
    // method returns false.
    return (protectionPolicyEvaluationResult == ProtectionPolicyEvaluationResult.Allowed);
}
```

## 복사-붙여넣기 작업에 대한 액세스 요청


이 시나리오에서는 붙여넣기 작업에 대한 액세스 권한을 확인하는 방법을 보여 줍니다.

**참고** 이 코드 예제에서는 현재 보기에 대한 보호 정책 관리자 개체의 ID를 이미 설정했다고 가정합니다([엔터프라이즈 ID로 특정 창에 태그 지정](#tag_window_with_id) 참조). 그렇지 않으면 [**ProtectionPolicyManager.Identity**](https://msdn.microsoft.com/library/windows/apps/dn705785) 속성에 빈 문자열이 포함됩니다.



```CSharp
using Windows.ApplicationModel.DataTransfer;
using Windows.Security.EnterpriseData;

...

private async void OnPasteWithRequestAccess()
{
    DataPackageView dataPackageView = Clipboard.GetContent();
    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        ProtectionPolicyEvaluationResult protectionPolicyEvaluationResult =
            ProtectionPolicyManager.CheckAccess(dataPackageView.Properties.EnterpriseId,
                ProtectionPolicyManager.GetForCurrentView().Identity);

        if (protectionPolicyEvaluationResult == ProtectionPolicyEvaluationResult.Allowed)
        {
            try
            {
                string contentsOfClipboard = await dataPackageView.GetTextAsync();
                // Code goes here to insert the text into the app...
                this.fileContentsTextBox.Text = contentsOfClipboard;
            }
            catch (Exception)
            {
                // Code goes here to handle the exception retrieving text from the clipboard.
            }
        }
        else
        {
            // Paste from the enterprise context is not allowed by policy.
        }
    }
    else
    {
        // The value on the clipboard is not in the text format.
    }
}
```

**참고** 이 문서는 UWP(유니버설 Windows 플랫폼) 앱을 작성하는 Windows 10 개발자용입니다. Windows 8.x 또는 Windows Phone 8.x를 개발하는 경우 [보관된 문서](http://go.microsoft.com/fwlink/p/?linkid=619132)를 참조하세요.



## 관련 항목


[EDP(엔터프라이즈 데이터 보호) 샘플](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409)

[**Windows.Security.EnterpriseData 네임스페이스**](https://msdn.microsoft.com/library/windows/apps/dn279153)








<!--HONumber=Jun16_HO4-->



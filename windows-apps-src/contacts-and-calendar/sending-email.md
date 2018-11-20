---
author: normesta
description: 사용자가 메일 메시지를 보낼 수 있도록 메일 작성 대화 상자를 시작하는 방법을 보여 줍니다. 대화 상자를 표시하기 전에 메일의 필드에 데이터를 미리 채울 수 있습니다. 메시지는 사용자가 보내기 단추를 탭할 때까지 전송되지 않습니다.
title: 메일 보내기
ms.assetid: 74511E90-9438-430E-B2DE-24E196A111E5
keywords: 연락처, 메일, 보내기
ms.author: normesta
ms.date: 10/11/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 0a28809210f71bf523e3cc5f9c8da1db9fbcc90c
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7284720"
---
# <a name="send-email"></a>메일 보내기

사용자가 메일 메시지를 보낼 수 있도록 메일 작성 대화 상자를 시작하는 방법을 보여 줍니다. 대화 상자를 표시하기 전에 메일의 필드에 데이터를 미리 채울 수 있습니다. 메시지는 사용자가 보내기 단추를 탭할 때까지 전송되지 않습니다.

**문서 내용**

-   [메일 작성 대화 상자 시작](#launch-the-compose-email-dialog)
-   [요약 및 다음 단계](#summary-and-next-steps)
-   [관련 항목](#related-topics)

## <a name="launch-the-compose-email-dialog"></a>메일 작성 대화 상자 시작

메일 작성 대화 상자에서 새 [**EmailMessage**](https://msdn.microsoft.com/library/windows/apps/Dn631270) 개체를 만들고 미리 채울 데이터를 설정합니다. [**ShowComposeNewEmailAsync**](https://msdn.microsoft.com/library/windows/apps/Dn631269)를 호출하여 대화 상자를 표시합니다.

``` cs
private async Task ComposeEmail(Windows.ApplicationModel.Contacts.Contact recipient,
    string subject, string messageBody)
{
    var emailMessage = new Windows.ApplicationModel.Email.EmailMessage();
    emailMessage.Body = messageBody;

    var email = recipient.Emails.FirstOrDefault<Windows.ApplicationModel.Contacts.ContactEmail>();
    if (email != null)
    {
        var emailRecipient = new Windows.ApplicationModel.Email.EmailRecipient(email.Address);
        emailMessage.To.Add(emailRecipient);
        emailMessage.Subject = subject;
    }

    await Windows.ApplicationModel.Email.EmailManager.ShowComposeNewEmailAsync(emailMessage);
}
```

>[!NOTE]
> 첨부 파일에 추가 하는 메일 [EmailAttachment](https://docs.microsoft.com/uwp/api/windows.applicationmodel.email.emailattachment) 클래스를 사용 하 여 메일 앱에만 표시 됩니다. 사용자의 기본 메일 프로그램으로 구성 된 메일 프로그램 있으면 첨부 파일 없이 작성 창이 표시 됩니다. 알려진된 문제입니다.

## <a name="summary-and-next-steps"></a>요약 및 다음 단계

이 항목에서는 메일 작성 대화 상자를 시작하는 방법을 살펴보았습니다. 메일 메시지의 수신자로 사용할 연락처를 선택하는 방법에 대한 자세한 내용은 [연락처 선택](selecting-contacts.md)을 참조하세요. 메일 첨부 파일로 사용할 파일을 선택하려면 [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/JJ635275)를 참조하세요.

## <a name="related-topics"></a>관련 항목

* [연락처 선택](selecting-contacts.md)
* [파일 선택기를 호출한 후 Windows Phone 앱을 계속하는 방법](https://msdn.microsoft.com/library/windows/apps/xaml/Dn614994)
 

 

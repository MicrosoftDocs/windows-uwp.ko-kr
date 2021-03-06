---
description: 사용자가 메일 메시지를 보낼 수 있도록 메일 작성 대화 상자를 시작하는 방법을 보여 줍니다. 대화 상자를 표시하기 전에 메일의 필드에 데이터를 미리 채울 수 있습니다. 메시지는 사용자가 보내기 단추를 탭할 때까지 전송되지 않습니다.
title: 전자 메일 보내기
ms.assetid: 74511E90-9438-430E-B2DE-24E196A111E5
keywords: 연락처, 전자 메일, 보내기
ms.date: 10/11/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 47d07fa1932aae87704f7922762a8f0b7e430444
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154667"
---
# <a name="send-email"></a>전자 메일 보내기

사용자가 메일 메시지를 보낼 수 있도록 메일 작성 대화 상자를 시작하는 방법을 보여 줍니다. 대화 상자를 표시하기 전에 메일의 필드에 데이터를 미리 채울 수 있습니다. 메시지는 사용자가 보내기 단추를 탭할 때까지 전송되지 않습니다.

**이 문서의 내용**

-   [전자 메일 작성 대화 상자 시작](#launch-the-compose-email-dialog)
-   [요약 및 다음 단계](#summary-and-next-steps)
-   [관련 항목](#related-topics)

## <a name="launch-the-compose-email-dialog"></a>전자 메일 작성 대화 상자 시작

새 [**Emailmessage**](/uwp/api/Windows.ApplicationModel.Email.EmailMessage) 개체를 만들고 전자 메일 작성 대화 상자에서 미리 채울 데이터를 설정 합니다. [**ShowComposeNewEmailAsync**](/uwp/api/windows.applicationmodel.email.emailmanager.showcomposenewemailasync) 를 호출 하 여 대화 상자를 표시 합니다.

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
> [Emailattachment](/uwp/api/windows.applicationmodel.email.emailattachment) 클래스를 사용 하 여 전자 메일에 추가 하는 첨부 파일은 메일 앱에만 표시 됩니다. 사용자가 기본 메일 프로그램으로 구성 된 다른 메일 프로그램을 사용 하는 경우에는 첨부 파일 없이 작성 창이 표시 됩니다. 이것은 알려진 문제입니다.

## <a name="summary-and-next-steps"></a>요약 및 다음 단계

이 항목에서는 전자 메일 작성 대화 상자를 시작 하는 방법을 보여 줍니다. 전자 메일 메시지의 받는 사람으로 사용할 연락처를 선택 하는 방법에 대 한 자세한 내용은 [연락처 선택](selecting-contacts.md)을 참조 하세요. 전자 메일 첨부 파일로 사용할 파일을 선택 하려면 [**PickSingleFileAsync**](/uwp/api/windows.storage.pickers.fileopenpicker.picksinglefileasync) 을 참조 하세요.

## <a name="related-topics"></a>관련 항목

* [연락처 선택](selecting-contacts.md)
 
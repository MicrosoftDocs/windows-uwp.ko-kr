---
description: 이 항목에서는 사용자가 SMS 메시지를 보낼 수 있도록 SMS 작성 대화 상자를 시작하는 방법을 보여 줍니다. 대화 상자를 표시하기 전에 SMS 필드에 데이터를 미리 채울 수 있습니다. 메시지는 사용자가 보내기 단추를 탭할 때까지 전송되지 않습니다.
title: SMS 메시지 보내기
ms.assetid: 4D7B509B-1CF0-4852-9691-E96D8352A4D6
keywords: 연락처, SMS, 보내기
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 01f6bd595de369afae8ac091a70c857bbd198519
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66360335"
---
# <a name="send-an-sms-message"></a>SMS 메시지 보내기

이 항목에서는 사용자가 SMS 메시지를 보낼 수 있도록 SMS 작성 대화 상자를 시작하는 방법을 보여 줍니다. 대화 상자를 표시하기 전에 SMS 필드에 데이터를 미리 채울 수 있습니다. 메시지는 사용자가 보내기 단추를 탭할 때까지 전송되지 않습니다.

이 코드를 호출 하려면 선언 된 **채팅**를 **smsSend**, 및 **chatSystem** 패키지 매니페스트에 기능입니다. 이들은 [기능을 제한](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations#special-and-restricted-capabilities) 하지만 앱에서 사용할 수 있습니다. 앱 스토어에 게시 하려는 경우에 승인을 해야 합니다. 참조 [계정 형식, 위치 및 수수료](https://docs.microsoft.com/windows/uwp/publish/account-types-locations-and-fees)합니다.

## <a name="launch-the-compose-sms-dialog"></a>SMS 작성 대화 상자 시작

메일 작성 대화 상자에서 새 [**ChatMessage**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.chat.chatmessage) 개체를 만들고 미리 채울 데이터를 설정합니다. [  **ShowComposeSmsMessageAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.chat.chatmessagemanager.showcomposesmsmessageasync)를 호출하여 대화 상자를 표시합니다.

```cs
private async void ComposeSms(Windows.ApplicationModel.Contacts.Contact recipient,
    string messageBody,
    StorageFile attachmentFile,
    string mimeType)
{
    var chatMessage = new Windows.ApplicationModel.Chat.ChatMessage();
    chatMessage.Body = messageBody;

    if (attachmentFile != null)
    {
        var stream = Windows.Storage.Streams.RandomAccessStreamReference.CreateFromFile(attachmentFile);

        var attachment = new Windows.ApplicationModel.Chat.ChatMessageAttachment(
            mimeType,
            stream);

        chatMessage.Attachments.Add(attachment);
    }

    var phone = recipient.Phones.FirstOrDefault<Windows.ApplicationModel.Contacts.ContactPhone>();
    if (phone != null)
    {
        chatMessage.Recipients.Add(phone.Number);
    }
    await Windows.ApplicationModel.Chat.ChatMessageManager.ShowComposeSmsMessageAsync(chatMessage);
}
```

장치 앱을 실행 하는 SMS 메시지를 보낼 수 있으려면 되는지 확인 하려면 다음 코드를 사용할 수 있습니다.

```csharp
if (Windows.Foundation.Metadata.ApiInformation.IsTypePresent("Windows.ApplicationModel.Chat"))
{
   // Call code here.
}
```

## <a name="summary-and-next-steps"></a>요약 및 다음 단계

이 항목에서는 SMS 작성 대화 상자를 시작하는 방법을 살펴보았습니다. SMS 메시지의 수신자로 사용할 연락처를 선택하는 방법에 대한 자세한 내용은 [연락처 선택](selecting-contacts.md)을 참조하세요. 백그라운드 작업을 사용하여 SMS 메시지를 보내고 받는 방법에 대한 더 많은 예제를 보려면 GitHub에서 [유니버설 Windows 앱 샘플](https://go.microsoft.com/fwlink/p/?linkid=619979)을 다운로드합니다.

## <a name="related-topics"></a>관련 항목

* [연락처 선택](selecting-contacts.md)

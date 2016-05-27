---
author: Xansky
description: 이 항목에서는 사용자가 SMS 메시지를 보낼 수 있도록 SMS 작성 대화 상자를 시작하는 방법을 보여 줍니다. 대화 상자를 표시하기 전에 SMS 필드에 데이터를 미리 채울 수 있습니다. 메시지는 사용자가 보내기 단추를 탭할 때까지 전송되지 않습니다.
title: SMS 메시지 보내기
ms.assetid: 4D7B509B-1CF0-4852-9691-E96D8352A4D6
keywords: contacts, SMS, send
---

# SMS 메시지 보내기

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


이 항목에서는 사용자가 SMS 메시지를 보낼 수 있도록 SMS 작성 대화 상자를 시작하는 방법을 보여 줍니다. 대화 상자를 표시하기 전에 SMS 필드에 데이터를 미리 채울 수 있습니다. 메시지는 사용자가 보내기 단추를 탭할 때까지 전송되지 않습니다.

## SMS 작성 대화 상자 시작

메일 작성 대화 상자에서 새 [**ChatMessage**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.chat.chatmessage) 개체를 만들고 미리 채울 데이터를 설정합니다. [
            **ShowComposeSmsMessageAsync**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.chat.chatmessagemanager.showcomposesmsmessageasync)를 호출하여 대화 상자를 표시합니다.

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

## 요약 및 다음 단계

이 항목에서는 SMS 작성 대화 상자를 시작하는 방법을 살펴보았습니다. SMS 메시지의 수신자로 사용할 연락처를 선택하는 방법에 대한 자세한 내용은 [연락처 선택](selecting-contacts.md)을 참조하세요. 백그라운드 작업을 사용하여 SMS 메시지를 보내고 받는 방법에 대한 더 많은 예제를 보려면 GitHub에서 [유니버설 Windows 앱 샘플](http://go.microsoft.com/fwlink/p/?linkid=619979)을 다운로드합니다.

## 관련 항목

* [연락처 선택](selecting-contacts.md)


<!--HONumber=May16_HO2-->



---
description: 이 항목에서는 사용자가 SMS 메시지를 보낼 수 있도록 SMS 작성 대화 상자를 시작하는 방법을 보여 줍니다. 대화 상자를 표시하기 전에 SMS 필드에 데이터를 미리 채울 수 있습니다. 메시지는 사용자가 보내기 단추를 탭할 때까지 전송되지 않습니다.
title: SMS 메시지 보내기
ms.assetid: 4D7B509B-1CF0-4852-9691-E96D8352A4D6
keywords: 연락처, SMS, 송신
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ceaeffdb0d8b3207a95e981f16590fe8e9e12e46
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154657"
---
# <a name="send-an-sms-message"></a>SMS 메시지 보내기

이 항목에서는 사용자가 SMS 메시지를 보낼 수 있도록 SMS 작성 대화 상자를 시작하는 방법을 보여 줍니다. 대화 상자를 표시하기 전에 SMS 필드에 데이터를 미리 채울 수 있습니다. 메시지는 사용자가 보내기 단추를 탭할 때까지 전송되지 않습니다.

이 코드를 호출 하려면 패키지 매니페스트에서 **채팅**, **Smssend**및 기타 **시스템** 기능을 선언 합니다. 이러한 [기능은 제한](../packaging/app-capability-declarations.md#special-and-restricted-capabilities) 되어 있지만 앱에서 사용할 수 있습니다. 스토어에 앱을 게시 하려는 경우에만 승인이 필요 합니다. [계정 유형, 위치 및 요금](../publish/account-types-locations-and-fees.md)을 참조 하세요.

## <a name="launch-the-compose-sms-dialog"></a>SMS 구성 대화 상자 시작

새 기능 [**메시지**](/uwp/api/windows.applicationmodel.chat.chatmessage) 개체를 만들고 전자 메일 작성 대화 상자에서 미리 채울 데이터를 설정 합니다. [**ShowComposeSmsMessageAsync**](/uwp/api/windows.applicationmodel.chat.chatmessagemanager.showcomposesmsmessageasync) 를 호출 하 여 대화 상자를 표시 합니다.

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

다음 코드를 사용 하 여 앱을 실행 하는 장치에서 SMS 메시지를 보낼 수 있는지 여부를 확인할 수 있습니다.

```csharp
if (Windows.Foundation.Metadata.ApiInformation.IsTypePresent("Windows.ApplicationModel.Chat"))
{
   // Call code here.
}
```

## <a name="summary-and-next-steps"></a>요약 및 다음 단계

이 항목에서는 SMS 구성 대화 상자를 시작 하는 방법을 보여 줍니다. SMS 메시지의 수신자로 사용할 연락처를 선택하는 방법에 대한 자세한 내용은 [연락처 선택](selecting-contacts.md)을 참조하세요. 백그라운드 작업을 사용 하 여 SMS 메시지를 보내고 받는 방법에 대 한 더 많은 예제를 보려면 GitHub에서 [유니버설 Windows 앱 샘플](https://github.com/Microsoft/Windows-universal-samples) 을 다운로드 하세요.

## <a name="related-topics"></a>관련 항목

* [연락처 선택](selecting-contacts.md)
---
description: 다른 유형의 패키지 되지 않은 apps에서 로컬 알림 메시지를 보내고 알림 메시지를 클릭 하 여 사용자를 처리 하는 방법을 알아봅니다.
title: 다른 유형의 패키지 되지 않은 apps에서 로컬 알림 메시지 보내기
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Send a local toast notification from other types of unpackaged apps
template: detail.hbs
ms.date: 11/17/2020
ms.topic: article
keywords: windows 10, 알림 메시지 보내기, 알림, 알림 전송, 알림, 알림, 방법, 빠른 시작, 시작, 코드 샘플, 연습, 기타 앱 유형, 패키지 되지 않은
ms.localizationpriority: medium
ms.openlocfilehash: 71c0facc0cd77383ac6682f4934334a7356eb0e8
ms.sourcegitcommit: 5e718720d1032a7089dea46a7c5aefa6cda3385f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/12/2021
ms.locfileid: "103231550"
---
# <a name="send-a-local-toast-notification-from-other-types-of-unpackaged-apps"></a>다른 유형의 패키지 되지 않은 apps에서 로컬 알림 메시지 보내기

MSIX/UWP 또는 스파스 서명 된 패키지를 사용 하지 않고 c # 또는 c + +가 아닌 응용 프로그램을 개발 하는 경우에는이 페이지가 표시 됩니다.

알림 메시지는 앱이 현재 앱 내부에 있지 않은 상태에서 사용자를 생성 하 고 제공할 수 있는 메시지입니다. 이 빠른 시작에서는 Windows 10 알림 메시지를 만들고, 제공 하 고, 표시 하는 단계를 안내 합니다. 이러한 빠른 시작은 가장 간단한 구현 알림 인 로컬 알림을 사용 합니다.

> [!IMPORTANT]
> C # 앱을 작성 하는 경우 [c # 설명서](send-local-toast.md)를 참조 하세요. C + + 응용 프로그램을 작성 하는 경우 [c + + UWP](send-local-toast-cpp-uwp.md) 또는 [c + + WRL](send-local-toast-desktop-cpp-wrl.md) 설명서를 참조 하세요.



## <a name="step-1-register-your-app-in-the-registry"></a>1 단계: 레지스트리에 앱 등록

앱을 식별 하는 고유한 AUMID 앱의 표시 이름, 아이콘 및 COM 활성기 GUID를 포함 하 여 앱의 정보를 레지스트리에 등록 해야 합니다.

```xml
<registryKey keyName="HKEY_LOCAL_MACHINE\Software\Classes\AppUserModelId\<YOUR_AUMID>">
    <registryValue
        name="DisplayName"
        value="My App"
        valueType="REG_EXPAND_SZ" />
    <registryValue
        name="IconUri"
        value="C:\icon.png"
        valueType="REG_EXPAND_SZ" />
    <registryValue
        name="IconBackgroundColor"
        value="AARRGGBB"
        valueType="REG_SZ" />
    <registryValue
        name="CustomActivator"
        value="{YOUR COM ACTIVATOR GUID HERE}"
        valueType="REG_SZ" />
</registryKey>
```

## <a name="step-2-set-up-your-com-activator"></a>2 단계: COM 활성기 설정

앱이 실행 되 고 있지 않은 경우에도 언제 든 지 알림을 클릭할 수 있습니다. 따라서 알림 활성화는 COM 활성기를 통해 처리 됩니다. COM 클래스는 인터페이스를 구현 해야 합니다 `INotificationActivationCallback` . COM 클래스의 GUID는 레지스트리 CustomActivator 값에 지정 된 GUID와 일치 해야 합니다.

```cpp
struct callback : winrt::implements<callback, INotificationActivationCallback>
{
    HRESULT __stdcall Activate(
        LPCWSTR app,
        LPCWSTR args,
        [[maybe_unused]] NOTIFICATION_USER_INPUT_DATA const* data,
        [[maybe_unused]] ULONG count) noexcept final
    {
        try
        {
            std::wcout << this_app_name << L" has been called back from a notification." << std::endl;
            std::wcout << L"Value of the 'app' parameter is '" << app << L"'." << std::endl;
            std::wcout << L"Value of the 'args' parameter is '" << args << L"'." << std::endl;
            return S_OK;
        }
        catch (...)
        {
            return winrt::to_hresult();
        }
    }
};
```



## <a name="step-3-send-a-toast"></a>3 단계: 알림 보내기

Windows 10에서는 알림이 표시 되는 방식을 유연 하 게 사용할 수 있는 적응 언어를 사용 하 여 알림 콘텐츠를 설명 합니다. 자세한 내용은 [알림 콘텐츠 설명서](adaptive-interactive-toasts.md) 를 참조 하세요.

간단한 텍스트 기반 알림을 시작 합니다. 알림 라이브러리를 사용 하 여 알림 콘텐츠를 생성 하 고 알림을 표시 합니다.

> [!IMPORTANT]
> 앱에서 알림이 표시 되도록 알림을 보낼 때 이전에 AUMID를 사용 해야 합니다.

<img alt="Simple text notification" src="images/send-toast-01.png" width="364"/>

```cpp
// Construct the toast template
XmlDocument doc;
doc.LoadXml(L"<toast>\
    <visual>\
        <binding template=\"ToastGeneric\">\
            <text></text>\
            <text></text>\
        </binding>\
    </visual>\
</toast>");

// Populate with text and values
doc.SelectSingleNode(L"//text[1]").InnerText(L"Andrew sent you a picture");
doc.SelectSingleNode(L"//text[2]").InnerText(L"Check this out, The Enchantments in Washington!");

// Construct the notification
ToastNotification notif{ doc };

// And send it! Use the AUMID you specified earlier.
ToastNotificationManager::CreateToastNotifier(L"MyPublisher.MyApp").Show(notif);
```


## <a name="step-4-handling-activation"></a>4 단계: 활성화 처리

알림을 클릭 하면 COM 활성기가 활성화 됩니다.


## <a name="more-details"></a>자세한 내용

### <a name="aumid-restrictions"></a>AUMID 제한 사항

AUMID의 길이는 129 자이 하 여야 합니다. AUMID가 129 자를 초과 하면 예약 된 알림 알림이 작동 하지 않습니다. 예약 된 알림을 추가할 때 다음과 같은 예외가 발생 합니다. *시스템 호출에 전달 된 데이터 영역이 너무 작습니다. (0x8007007A)*.
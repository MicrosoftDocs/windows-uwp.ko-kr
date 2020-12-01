---
description: 'Agile 개체는 모든 스레드에서 액세스할 수 있는 개체입니다. C #/Winrt는 안전 하 게 아파트에서 agile이 아닌 개체를 마샬링하는 데 필요한 경우 agile 참조를 지원 합니다.'
title: 'C #/Winrt를 사용 하는 Agile 개체'
ms.date: 11/17/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a963f1a9918e6732028ad74903b096a38a14ec8f
ms.sourcegitcommit: 3b10880007fe9e29ea2b9305fe62ced239d974be
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/30/2020
ms.locfileid: "96355238"
---
# <a name="agile-objects-in-cwinrt"></a>C #/Winrt의 Agile 개체

대부분의 Windows 런타임 클래스는 *agile* 이며,이는 서로 다른 아파트에 있는 모든 스레드에서 액세스할 수 있음을 의미 합니다. 작성 하는 c #/Winrt 형식은 기본적으로 agile 이며 해당 형식에 대해이 동작을 옵트아웃 (opt out) 할 수 없습니다.

그러나 투영 된 c #/Winrt 형식 (Windows SDK 및 WinUI 라이브러리에서 제공 하는 Windows 런타임 형식 포함)은 agile 일 수도 있고 그렇지 않을 수도 있습니다. 예를 들어, UI 개체를 나타내는 대부분의 형식은 agile이 아닙니다. Agile이 아닌 형식을 사용 하는 경우 스레딩 모델 및 마샬링 동작을 고려해 야 합니다. C #/Winrt는 안전 하 게 아파트에서 agile이 아닌 개체를 마샬링하는 데 필요한 경우 agile 참조를 지원 합니다.

> [!NOTE]
> Windows 런타임은 COM을 기반으로 합니다. COM 용어로 Agile 클래스는 `ThreadingModel` = *Both* 로 등록됩니다. COM 스레딩 모델 및 아파트에 대한 자세한 내용은 [COM 스레딩 모델의 이해와 사용](/previous-versions/ms809971(v=msdn.10))을 참조하세요.

## <a name="check-for-agile-support"></a>Agile 지원 확인

Windows 런타임 개체가 agile 인지 여부를 확인 하려면 다음 코드를 사용 하 여 개체가 [IAgileObject](/windows/desktop/api/objidl/nn-objidl-iagileobject) 인터페이스를 지원 하는지 여부를 확인 합니다.

```csharp
var queryAgileObject = testObject.As<IAgileObject>();

if (queryAgileObject != null) {
    // testObject is agile.
}
```

## <a name="create-an-agile-reference"></a>Agile 참조 만들기

Agile이 아닌 개체에 대 한 agile 참조를 만들려면 확장 메서드를 사용할 수 있습니다 `AsAgile` . `AsAgile` 는 예상 되는 c #/Winrt 형식에 적용할 수 있는 제네릭 확장 메서드입니다. 형식이 프로젝션 된 형식이 아닌 경우 예외가 throw 됩니다. Windows SDK에서 agile이 아닌 형식인 [PopupMenu](/uwp/api/Windows.UI.Popups.PopupMenu) 개체를 사용 하는 예제는 다음과 같습니다.

```csharp
var nonAgileObj = new Windows.UI.Popups.PopupMenu();
AgileReference<Windows.UI.Popups.PopupMenu> agileReference = nonAgileObj.AsAgile();
```

이제 `agileReference` 다른 아파트의 스레드에 전달 하 고 여기에 사용할 수 있습니다.

```csharp
await Task.Run(() => {
        Windows.UI.Popups.PopupMenu nonAgileObjAgain = agileReference.Get()
    });
```

---
author: lex
Description: The following article describes all of the properties and elements within the toast content XML payload.
title: 알림 콘텐츠 XML 스키마
ms.assetid: AF49EFAC-447E-44C3-93C3-CCBEDCF07D22
label: Toast content XML schema
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bcfc56264ab3063995fd9f2b06bd93e9406cd37e
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/16/2018
ms.locfileid: "6989808"
---
# <a name="toast-content-xml-schema"></a>알림 콘텐츠 XML 스키마

 

다음은 알림 콘텐츠 XML 페이로드 내의 모든 속성과 요소에 대한 설명입니다.

다음 XML 스키마에서 "?" 접미사는 특성이 선택 사항임을 의미합니다.

## <a name="ltvisualgt-and-ltaudiogt"></a>&lt;visual&gt; 및 &lt;audio&gt;

```
<toast launch? duration? activationType? scenario? >
  <visual lang? baseUri? addImageQuery? >
    <binding template? lang? baseUri? addImageQuery? >
      <text lang? hint-maxLines? >content</text>
      <image src placement? alt? addImageQuery? hint-crop? />
      <group>
        <subgroup hint-weight? hint-textStacking? >
          <text />
          <image />
        </subgroup>
      </group>
    </binding>
  </visual>
  <audio src? loop? silent? />
</toast>
```

**&lt;toast&gt;의 특성**

launch?

-   launch? = string
-   선택적 특성입니다.
-   알림에 의해 활성화될 때 응용 프로그램에 전달되는 문자열입니다.
-   activationType 값에 따라 이 값은 백그라운드 작업 내부 포그라운드의 앱이 수신하거나 원래 앱에서 시작된 프로토콜인 다른 앱이 수신할 수 있습니다.
-   이 문자열의 형식과 콘텐츠는 앱에서 앱 용도에 맞게 정의됩니다.
-   사용자가 알림을 탭하거나 클릭하여 연결된 앱을 시작하면 시작 문자열은 기본 방법으로 앱을 시작하는 대신 사용자에게 알림 콘텐츠에 관련된 뷰를 표시할 수 있도록 하는 앱 컨텍스트를 제공합니다.
-   사용자가 알림 본문 대신 작업을 클릭해서 활성화가 발생했다면 개발자는 다시 &lt;toast&gt; 태그에 미리 정의된 "launch" 대신 해당 &lt;action&gt; 태그에 미리 정의된 "arguments"를 검색합니다.

duration?

-   duration? = "short|long"
-   선택적 특성입니다. 기본값은 "short"입니다.
-   이 특성은 특정 시나리오 및 appCompat에만 해당합니다. 알람 시나리오에서는 이 특성이 더 필요하지 않습니다.
-   이 속성을 사용하지 않는 것이 좋습니다.

activationType?

-   activationType? = "foreground | background | protocol | system"
-   선택적 특성입니다.
-   기본값은 "foreground"입니다.

scenario?

-   scenario? = "default | alarm | reminder | incomingCall"
-   선택적 특성이고 기본값은 "default"입니다.
-   시나리오에서 알람, 미리 알림 또는 수신 전화를 표시하지 않을 경우 이 특성이 필요하지 않습니다.
-   화면에 알림을 계속 표시하기 위한 목적만으로 이 특성을 사용하지 마세요.

**&lt;visual&gt;의 특성**

lang?

-   이 선택적 특성에 대한 자세한 내용은 [이 요소 스키마 문서](https://msdn.microsoft.com/library/windows/apps/br230847)를 참조하세요.

baseUri?

-   이 선택적 특성에 대한 자세한 내용은 [이 요소 스키마 문서](https://msdn.microsoft.com/library/windows/apps/br230847)를 참조하세요.

addImageQuery?

-   이 선택적 특성에 대한 자세한 내용은 [이 요소 스키마 문서](https://msdn.microsoft.com/library/windows/apps/br230847)를 참조하세요.

**&lt;binding&gt;의 특성**

template?

-   \[Important\] template? = "ToastGeneric"
-   새로운 적응형 및 대화형 알림 기능을 사용 중이면 레거시 템플릿 대신 "ToastGeneric" 템플릿을 사용하여 시작하는지 확인하세요.
-   이제 레거시 템플릿을 새 작업과 함께 사용할 수 있지만 이는 의도된 사용 사례가 아니고 작업이 계속될지 보장할 수 없습니다.

lang?

-   이 선택적 특성에 대한 자세한 내용은 [이 요소 스키마 문서](https://msdn.microsoft.com/library/windows/apps/br230847)를 참조하세요.

baseUri?

-   이 선택적 특성에 대한 자세한 내용은 [이 요소 스키마 문서](https://msdn.microsoft.com/library/windows/apps/br230847)를 참조하세요.

addImageQuery?

-   이 선택적 특성에 대한 자세한 내용은 [이 요소 스키마 문서](https://msdn.microsoft.com/library/windows/apps/br230847)를 참조하세요.

**&lt;text&gt;의 특성**

lang?

-   이 선택적 특성에 대한 자세한 내용은 [이 요소 스키마 문서](https://msdn.microsoft.com/library/windows/apps/br230847)를 참조하세요.

**&lt;image&gt;의 특성**

src

-   이 필수 특성에 대한 자세한 내용은 [이 요소 스키마 문서](https://msdn.microsoft.com/library/windows/apps/br230844)를 참조하세요.

placement?

-   placement? = "inline" | "appLogoOverride"
-   이 특성은 선택 사항입니다.
-   이는 이 이미지가 표시되는 위치를 지정합니다.
-   "inline"은 알림 본문 내부, 텍스트 아래를 의미하고, "appLogoOverride"는 알림의 왼쪽 위에 나타나는 응용 프로그램 아이콘 배치를 의미합니다.
-   각 배치 값에 대한 이미지를 하나까지 포함할 수 있습니다.

alt?

-   이 선택적 특성에 대한 자세한 내용은 [이 요소 스키마 문서](https://msdn.microsoft.com/library/windows/apps/br230844)를 참조하세요.

addImageQuery?

-   이 선택적 특성에 대한 자세한 내용은 [이 요소 스키마 문서](https://msdn.microsoft.com/library/windows/apps/br230844)를 참조하세요.

hint-crop?

-   hint-crop? = "none" | "circle"
-   이 특성은 선택 사항입니다.
-   "none"은 자르기가 없음을 의미하는 기본값입니다.
-   "circle"은 이미지를 원 모양으로 자릅니다. 연락처의 프로필 이미지, 개인 이미지 등에 이 모양을 사용합니다.

**&lt;audio&gt;의 특성**

src?

-   이 선택적 특성에 대한 자세한 내용은 [이 요소 스키마 문서](https://msdn.microsoft.com/library/windows/apps/br230842)를 참조하세요.

loop?

-   이 선택적 특성에 대한 자세한 내용은 [이 요소 스키마 문서](https://msdn.microsoft.com/library/windows/apps/br230842)를 참조하세요.

silent?

-   이 선택적 특성에 대한 자세한 내용은 [이 요소 스키마 문서](https://msdn.microsoft.com/library/windows/apps/br230842)를 참조하세요.

## <a name="schemas-ltactiongt"></a>스키마: &lt;action&gt;


다음 XML 스키마에서 "?" 접미사는 특성이 선택 사항임을 의미합니다.

```
<toast>
  <visual>
  </visual>
  <audio />
  <actions>
    <input id type title? placeHolderContent? defaultInput? >
      <selection id content />
    </input>
    <action content arguments activationType? imageUri? hint-inputId />
  </actions>
</toast>
```

**&lt;input&gt;의 특성**

id

-   id = string
-   이 특성은 필수입니다.
-   id 특성은 필수이고 앱이 포그라운드 또는 백그라운드에서 활성화된 후 개발자가 사용자 입력을 검색하는 데 사용됩니다.

유형

-   type = "text | selection"
-   이 특성은 필수입니다.
-   텍스트 입력 및 미리 정의된 선택 목록의 입력을 지정하는 데 사용됩니다.
-   모바일 및 데스크톱에서 이 특성은 텍스트 상자 입력 또는 목록 상자 입력을 원하는지 지정합니다.

title?

-   title? = string
-   title 특성은 선택 사항이고 개발자가 어포던스가 있을 때 렌더링할 셸 입력의 제목을 지정하는 데 사용됩니다.
-   모바일 및 데스크톱에서 이 제목은 입력 위에 표시됩니다.

placeHolderContent?

-   placeHolderContent? = string
-   placeHolderContent 특성은 선택 사항이고 텍스트 입력 형식에 대한 회색으로 표시된 힌트 텍스트입니다. 입력 형식이 “text”가 아니면 이 특성이 무시됩니다.

defaultInput?

-   defaultInput? = string
-   defaultInput 특성은 선택 사항이고 기본 입력 값을 제공하는 데 사용됩니다.
-   입력 형식이 "text"이면 이는 문자열 입력으로 처리됩니다.
-   입력 형식이 "selection"이면 이는 이 입력 요소 내부에서 사용 가능한 선택 항목 중 하나의 ID여야 합니다.

**&lt;selection&gt;의 특성**

id

-   이 특성은 필수입니다. 사용자 선택을 식별하는 데 사용됩니다. id가 앱에 반환됩니다.

콘텐츠

-   이 특성은 필수입니다. 이 선택 요소에 대해 표시할 문자열을 제공합니다.

**&lt;action&gt;의 특성**

콘텐츠

-   content = string
-   content 특성은 필수입니다. 단추에 표시되는 텍스트 문자열을 제공합니다.

인수

-   arguments = string
-   arguments 특성은 필수입니다. 사용자가 이 작업을 수행해서 앱이 활성화된 후 앱에서 나중에 검색할 수 있는 앱 정의 데이터를 설명합니다.

activationType?

-   activationType? = "foreground | background | protocol | system"
-   activationType 특성은 선택 사항이고 기본값은 "foreground"입니다.
-   이 작업을 통해 수행되는 활성화 종류를 포그라운드, 백그라운드 또는 프로토콜 실행을 통한 다른 앱 시작이나 시스템 작업 호출로 설명합니다.

imageUri?

-   imageUri? = string
-   imageUri는 선택 사항이고 텍스트 콘텐츠와 함께 단추 내부에 표시할 이 작업의 이미지 아이콘을 제공하는 데 사용됩니다.

hint-inputId

-   hint-inputId = string
-   hint-inpudId 특성은 필수입니다. 특히 빠른 회신 시나리오에 사용됩니다.
-   값은 연결할 입력 요소의 ID여야 합니다.
-   모바일 및 데스크톱에서 이 특성은 입력 상자의 오른쪽 옆에 단추를 배치합니다.

## <a name="attributes-for-system-handled-actions"></a>시스템 처리 작업의 특성


앱이 알림의 다시 알림/재예약을 백그라운드 작업으로 처리하지 않게 하려면 알림을 다시 알리고 해제하기 위한 작업을 시스템에서 처리할 수 있습니다. 시스템 처리 작업을 결합하거나 개별적으로 지정할 수 있지만 다시 알림 작업을 구현하려면 해제 작업을 포함하는 것이 좋습니다.

시스템 명령 콤보: SnoozeAndDismiss

```
<toast>
  <visual>
  </visual>
  <actions hint-systemCommands="SnoozeAndDismiss" />
</toast>
```

개별 시스템 처리 작업

```
<toast>
  <visual>
  </visual>
  <actions>
  <input id="snoozeTime" type="selection" defaultInput="10">
    <selection id="5" content="5 minutes" />
    <selection id="10" content="10 minutes" />
    <selection id="20" content="20 minutes" />
    <selection id="30" content="30 minutes" />
    <selection id="60" content="1 hour" />
  </input>
  <action activationType="system" arguments="snooze" hint-inputId="snoozeTime" content=""/>
  <action activationType="system" arguments="dismiss" content=""/>
  </actions>
</toast>
```

개별 다시 알림 및 해제 작업을 구성하려면 다음을 수행합니다.

-   activationType = "system" 지정
-   arguments = "snooze" | "dismiss" 지정
-   콘텐츠 지정:
    -   "snooze" 및 "dismiss"의 지역화된 문자열을 작업에 표시하려면 콘텐츠를 빈 문자열: &lt;action content = ""/&gt;로 지정합니다.
    -   사용자 지정 문자열이 필요하면 해당 값:&lt; action content="Remind me later" /&gt;을 제공합니다.
-   입력 지정:
    -   사용자가 다시 알림 간격을 선택하게 하지 않고 OS 전체에서 일관성 있는 시스템 정의 시간 간격에 한 번만 알림을 다시 발생하게 하려면 &lt;input&gt;을 구성하지 마세요.
    -   다시 알림 간격 선택 항목을 제공하려면:
        -   다시 알림 작업에서 hint-inputId를 지정합니다.
        -   입력 ID가 다시 알림 작업의: &lt;input id="snoozeTime"&gt;&lt;/input&gt;&lt;action hint-inputId="snoozeTime"/&gt;와 일치하도록 설정합니다.
        -   선택 ID를 다시 알림 간격(분)을 나타내는 nonNegativeInteger로 지정합니다. &lt;selection id="240" /&gt;은 4시간 간격으로 다시 알림을 의미합니다.
        -   &lt;input&gt;의 defaultInput 값이 &lt;selection&gt; 자식 요소의 ID 중 하나와 일치하는지 확인합니다.
        -   최대 5개의 &lt;selection&gt; 값을 제공합니다.
---
Description: 다음 문서에서는 toast 콘텐츠 XML 페이로드의 모든 속성과 요소에 대해 설명 합니다.
title: Toast 콘텐츠 XML 스키마
ms.assetid: AF49EFAC-447E-44C3-93C3-CCBEDCF07D22
label: Toast content XML schema
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: cf3bf3733e17312ee0750006d2b8f94c70dbbd43
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156657"
---
# <a name="toast-content-xml-schema"></a>Toast 콘텐츠 XML 스키마

 

다음은 toast 콘텐츠 XML 페이로드의 모든 속성 및 요소에 대 한 설명입니다.

다음 XML 스키마에서 "?" 접미사는 특성이 선택적 임을 의미 합니다.

## <a name="ltvisualgt-and-ltaudiogt"></a>&lt;시각적 개체 &gt; 및 &lt; 오디오&gt;

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

**알림 특성 &lt;&gt;**

시작한?

-   시작한? = 문자열
-   이는 선택적 특성입니다.
-   알림 메시지를 활성화할 때 응용 프로그램에 전달 되는 문자열입니다.
-   ActivationType의 값에 따라이 값은 포그라운드, 백그라운드 작업 내부 또는 원래 앱에서 시작 된 프로토콜인 다른 앱에서 받을 수 있습니다.
-   이 문자열의 형식과 내용은 앱에서 자체 사용을 위해 정의 됩니다.
-   사용자가 연결 된 앱을 시작 하는 알림 메시지를 탭 하거나 클릭 하면 시작 문자열은 앱에 컨텍스트를 제공 하 여 기본 방식으로 시작 하는 대신 알림 콘텐츠와 관련 된 보기를 사용자에 게 표시할 수 있도록 합니다.
-   사용자가 알림 본문 대신 작업을 클릭 했기 때문에 활성화가 발생 한 경우 개발자는 &lt; &gt; 알림 태그에 미리 정의 된 "시작" 대신 해당 작업 태그에 미리 정의 된 "인수"를 다시 검색 합니다 &lt; &gt; .

작업?

-   작업? = "short | long"
-   이는 선택적 특성입니다. 기본값은 "short"입니다.
-   이는 특정 시나리오 및 appCompat에만 해당 됩니다. 알람 시나리오에는 더 이상 필요 하지 않습니다.
-   이 속성을 사용 하지 않는 것이 좋습니다.

ActivationType?

-   ActivationType? = "포그라운드 | 배경 | 프로토콜 | 컴퓨터
-   이는 선택적 특성입니다.
-   기본값은 "포그라운드"입니다.

방법은?

-   방법은? = "default | 알람 | 미리 알림 | incomingCall"
-   이는 선택적 특성입니다. 기본값은 "default"입니다.
-   사용자의 시나리오가 경보, 미리 알림 또는 들어오는 통화를 팝 하는 경우를 제외 하 고는 필요 하지 않습니다.
-   알림을 화면에 영구적으로 유지 하기 위해이 방법을 사용 하지 마십시오.

**&lt;시각적 개체의 특성&gt;**

lang?

-   이 선택적 특성에 대 한 자세한 내용은 [이 요소 스키마 문서](/uwp/schemas/tiles/toastschema/element-visual) 를 참조 하세요.

BaseUri?

-   이 선택적 특성에 대 한 자세한 내용은 [이 요소 스키마 문서](/uwp/schemas/tiles/toastschema/element-visual) 를 참조 하세요.

addImageQuery?

-   이 선택적 특성에 대 한 자세한 내용은 [이 요소 스키마 문서](/uwp/schemas/tiles/toastschema/element-visual) 를 참조 하세요.

**바인딩의 특성 &lt;&gt;**

할당량?

-   \[중요 한 \] 템플릿 = "To Generic"
-   새 적응 및 대화형 알림 기능을 사용 하는 경우 레거시 템플릿 대신 "To Generic" 템플릿을 사용 하 여 시작 해야 합니다.
-   새 작업이 포함 된 레거시 템플릿을 사용할 수는 있지만,이는 의도 된 사용 사례가 아니며에서 작업을 계속 하는 것을 보장할 수 없습니다.

lang?

-   이 선택적 특성에 대 한 자세한 내용은 [이 요소 스키마 문서](/uwp/schemas/tiles/toastschema/element-visual) 를 참조 하세요.

BaseUri?

-   이 선택적 특성에 대 한 자세한 내용은 [이 요소 스키마 문서](/uwp/schemas/tiles/toastschema/element-visual) 를 참조 하세요.

addImageQuery?

-   이 선택적 특성에 대 한 자세한 내용은 [이 요소 스키마 문서](/uwp/schemas/tiles/toastschema/element-visual) 를 참조 하세요.

**텍스트의 특성 &lt;&gt;**

lang?

-   이 선택적 특성에 대 한 자세한 내용은 [이 요소 스키마 문서](/uwp/schemas/tiles/toastschema/element-visual) 를 참조 하세요.

**이미지의 특성 &lt;&gt;**

src

-   이 필수 특성에 대 한 자세한 내용은 [이 요소 스키마 문서](/uwp/schemas/tiles/toastschema/element-image) 를 참조 하세요.

배치가?

-   배치가? = "inline" | "appLogoOverride"
-   이 특성은 선택 사항입니다.
-   이는 이 이미지가 표시되는 위치를 지정합니다.
-   "인라인"은 알림 본문 내에서 텍스트 아래를 의미 합니다. "appLogoOverride"는 응용 프로그램 아이콘을 대체 함을 의미 합니다 (알림 왼쪽 상단에 표시 됨).
-   각 배치 값에 대해 최대 하나의 이미지를 포함할 수 있습니다.

#b0?

-   이 선택적 특성에 대 한 자세한 내용은 [이 요소 스키마 문서](/uwp/schemas/tiles/toastschema/element-image) 를 참조 하세요.

addImageQuery?

-   이 선택적 특성에 대 한 자세한 내용은 [이 요소 스키마 문서](/uwp/schemas/tiles/toastschema/element-image) 를 참조 하세요.

힌트-자르기?

-   힌트-자르기? = "none" | 원으로
-   이 특성은 선택 사항입니다.
-   "none"이 기본값입니다 .이 값은 자르기가 없음을 의미 합니다.
-   "circle"은 이미지를 원형 도형으로 자릅니다. 연락처의 프로필 이미지, 개인의 이미지 등에 사용 됩니다.

**오디오의 특성 &lt;&gt;**

소스?

-   이 선택적 특성에 대 한 자세한 내용은 [이 요소 스키마 문서](/uwp/schemas/tiles/toastschema/element-audio) 를 참조 하세요.

실행?

-   이 선택적 특성에 대 한 자세한 내용은 [이 요소 스키마 문서](/uwp/schemas/tiles/toastschema/element-audio) 를 참조 하세요.

무음?

-   이 선택적 특성에 대 한 자세한 내용은 [이 요소 스키마 문서](/uwp/schemas/tiles/toastschema/element-audio) 를 참조 하세요.

## <a name="schemas-ltactiongt"></a>스키마: &lt; 작업&gt;


다음 XML 스키마에서 "?" 접미사는 특성이 선택적 임을 의미 합니다.

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

**입력의 특성 &lt;&gt;**

id

-   id = 문자열
-   필수 특성입니다.
-   Id 특성은 필수 이며 응용 프로그램이 활성화 되 면 (포그라운드 또는 백그라운드에서) 개발자가 사용자 입력을 검색 하는 데 사용 됩니다.

형식

-   type = "text | 선택
-   필수 특성입니다.
-   미리 정의 된 선택 목록에서 텍스트 입력 또는 입력을 지정 하는 데 사용 됩니다.
-   모바일 및 데스크톱에서 텍스트 상자 입력 또는 listbox 입력을 사용할지 여부를 지정 합니다.

제목과?

-   제목과? = 문자열
-   Title 특성은 선택 사항이 며, affordance 있을 때 개발자가 셸에 대 한 입력의 제목을 지정 하 여 렌더링할 수 있습니다.
-   모바일 및 데스크톱의 경우이 제목이 입력 위에 표시 됩니다.

placeHolderContent?

-   placeHolderContent? = 문자열
-   PlaceHolderContent 특성은 선택 사항이 며 텍스트 입력 형식에 대 한 회색 아웃 힌트 텍스트입니다. 입력 형식이 "텍스트"가 아닌 경우이 특성은 무시 됩니다.

defaultInput?

-   defaultInput? = 문자열
-   DefaultInput 특성은 선택 사항이 며 기본 입력 값을 제공 하는 데 사용 됩니다.
-   입력 형식이 "text" 이면 문자열 입력으로 처리 됩니다.
-   입력 형식이 "선택" 인 경우이 입력의 요소 내에서 사용 가능한 선택 항목 중 하나의 id로 예상 됩니다.

**선택 영역에 있는 특성 &lt;&gt;**

id

-   필수 특성입니다. 사용자 선택 항목을 식별 하는 데 사용 됩니다. Id가 앱으로 반환 됩니다.

콘텐츠

-   필수 특성입니다. 이 선택 요소에 대해 표시할 문자열을 제공 합니다.

**작업 중인 특성 &lt;&gt;**

콘텐츠

-   content = 문자열
-   content 특성은 필수입니다. 단추에 표시 되는 텍스트 문자열을 제공 합니다.

인수

-   arguments = 문자열
-   arguments 특성은 필수입니다. 앱이이 작업을 수행 하는 사용자 로부터 활성화 된 후 나중에 검색할 수 있는 앱 정의 데이터에 대해 설명 합니다.

ActivationType?

-   ActivationType? = "포그라운드 | 배경 | 프로토콜 | 컴퓨터
-   activationType 특성은 선택 사항이고 기본값은 "foreground"입니다.
-   이 작업으로 인해 발생 하는 활성화의 종류를 설명 합니다 .이 작업은 포그라운드, 백그라운드 또는 프로토콜 시작을 통해 다른 응용 프로그램을 시작 하거나 시스템 작업을 호출 하는 방법을 설명 합니다.

imageUri?

-   imageUri? = 문자열
-   imageUri는 선택 사항이 며 텍스트 콘텐츠를 사용 하 여 단추 내부에만 표시 하기 위해이 작업에 대 한 이미지 아이콘을 제공 하는 데 사용 됩니다.

힌트-inputId

-   힌트-inputId = 문자열
-   InpudId 특성이 필요 합니다. 이는 특히 빠른 회신 시나리오에 사용 됩니다.
-   값은 연결할 입력 요소의 id 여야 합니다.
-   모바일 및 데스크톱에서 입력 상자 바로 옆에 단추가 배치 됩니다.

## <a name="attributes-for-system-handled-actions"></a>시스템 처리 작업에 대 한 특성


앱에서 알림의 snoozing/재조정을 백그라운드 작업으로 처리 하지 않도록 하려면 시스템에서 snoozing 및 해제 알림 작업을 처리할 수 있습니다. 시스템 처리 작업은 조합 하거나 개별적으로 지정할 수 있지만 해제 작업 없이는 다시 알림 작업을 구현 하지 않는 것이 좋습니다.

시스템 명령 콤보 상자: SnoozeAndDismiss

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

개별 다시 알림을 생성 하 고 작업을 해제 하려면 다음을 수행 합니다.

-   ActivationType = "system"을 지정 합니다.
-   인수 지정 = "다시 알림" | 취소
-   콘텐츠 지정:
    -   지역화 된 문자열 "다시 알림" 및 "해제"를 작업에 표시 하려면 콘텐츠를 빈 문자열로 지정 합니다. &lt; 동작 내용 = ""/&gt;
    -   사용자 지정 문자열을 원하는 경우 해당 값을 제공 합니다. &lt; 동작 내용 = "나중에 알림"/&gt;
-   입력 지정:
    -   사용자가 다시 알림 간격을 선택 하지 않고 시스템에 정의 된 시간 간격 (OS 전체에 걸쳐 일치)에 대해 알림을 한 번만 다시 알리도록 하려는 경우에는 어떠한 입력도 구성 하지 않습니다 &lt; &gt; .
    -   다시 알림 간격 선택 항목을 제공 하려면 다음을 수행 합니다.
        -   다시 알림 작업에서 힌트-inputId 지정
        -   입력 id를 다시 알림 작업의 힌트-inputId와 일치 시킵니다. &lt; 입력 id = "snoozeTime" &gt; &lt; /input &gt; &lt; action Hint-inputId = "snoozeTime"/&gt;
        -   선택 id를 몇 분 내에 다시 알림 간격을 나타내는 nonNegativeInteger로 지정 합니다. &lt; 선택 id = "240"/ &gt; 4 시간에 대 한 snoozing를 의미 합니다.
        -   입력의 defaultInput 값이 &lt; &gt; &lt; selection &gt; 자식 요소의 id 중 하 나와 일치 하는지 확인 합니다.
        -   최대 5 개의 &lt; 선택 값을 제공 합니다. &gt;
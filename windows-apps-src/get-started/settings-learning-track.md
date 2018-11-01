---
author: TylerMSFT
title: UWP 앱의 설정 저장 및 로드
description: 유니버설 Windows 플랫폼 앱의 앱 설정을 로드하고 저장하는 방법을 알아보세요.
ms.author: twhitney
ms.date: 05/07/2018
ms.topic: article
keywords: 시작, uwp, windows 10, 학습 트랙, 설정, 설정 저장, 설정 로드
ms.localizationpriority: medium
ms.openlocfilehash: 18b11ea100915f8b6ff52db5223284da6f24a1d4
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5882365"
---
# <a name="save-and-load-settings-in-a-uwp-app"></a>UWP 앱의 설정 저장 및 로드

이 항목은 유니버설 Windows 플랫폼(UWP) 앱에서 설정을 로드하고 저장하기를 시작하기 위해 알아야 할 사항에 대해 다룹니다. 자세한 내용을 알아보는 것을 돕기 위해 주 API를 소개하며 링크가 제공됩니다.

앱에서 사용자 지정 가능한 측면을 기억하기 위해 설정을 사용합니다. 예를 들어, 뉴스 뷰어는 앱 설정을 사용하여 표시할 뉴스 소스와 문서 읽기에 사용할 글꼴을 저장합니다.

로컬 및 로밍 설정을 비롯한 앱 설정을 저장 및 로드하는 코드를 살펴보겠습니다.

## <a name="what-do-you-need-to-know"></a>알아야 할 사항

앱 설정을 사용하여 사용자의 기본 설정 및 앱 상태 등 구성 데이터를 저장합니다.  디바이스에 관련된 설정은 로컬로 저장됩니다. 앱이 설치된 장치에 적용되는 설정은 로밍 데이터 저장소에 저장됩니다. 설정은 사용자가 동일한 Microsoft 계정으로 로그인한 동일한 버전의 앱이 설치되어 있는 디바이스 간에 로밍됩니다.

다음 데이터 형식은 정수, 두 배로 증가, 부동, 문자, 문자열, 포인트, DateTime 등의 설정으로 사용할 수 있습니다. 여러 설정을 한 단위로 처리해야 하는 경우 유용한 [ApplicationDataCompositeValue](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataCompositeValue) 클래스 인스턴스를 저장할 수도 있습니다. 예를 들어 앱의 읽기 창에 텍스트를 표시하기 위한 글꼴 이름 및 크기는 하나의 단위로 저장/복원되어야 합니다. 이렇게 하면 다른 설정 이전에 한 설정을 로밍하는 지연 때문에 한 설정이 다른 설정과 비동기화되는 것을 방지할 수 있습니다.

앱 설정을 저장하거나 로드하는 데 알아야 할 주요 API는 다음과 같습니다.

- [Windows.Storage.ApplicationData.Current.LocalSettings](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationData#Windows_Storage_ApplicationData_LocalSettings)는 로컬 앱 데이터 저장소에서 응용 프로그램 설정 컨테이너를 가져옵니다. 설정은 이 장치의 특정 상태에 관련되어 있거나 크기가 너무 크기 때문에 여기에서 장치 간에 설정을 로밍하는 것은 적절하지 않습니다.
- [Windows.Storage.ApplicationData.Current.RoamingSettings](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingsettings#Windows_Storage_ApplicationData_RoamingSettings)는 로밍 앱 데이터 저장소에서 응용 프로그램 설정 컨테이너를 가져옵니다. 이 데이터는 디바이스 간에 로밍합니다.
- [Windows.Storage.ApplicationDataContainer](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer)는 앱 설정을 키/값 쌍으로 나타내는 컨테이너입니다. 이 클래스를 사용하여 설정 값을 만들고 검색합니다.
- [Windows.Storage.ApplicationDataCompositeValue](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataCompositeValue)는 한 단위로 직렬화해야 하는 여러 앱 설정을 나타냅니다. 한 설정을 다른 앱과 개별적으로 업데이트해서는 안 될 때 유용합니다.

## <a name="save-app-settings"></a>앱 설정 저장

이 소개에서는 앱 설정을 로컬로 저장 및 로드하고 장치 간에 복합 글꼴/글꼴 크기 설정을 로밍하는 단순한 두 가지 시나리오에 중점을 둡니다.

 ```csharp
// Save a setting locally on the device
ApplicationDataContainer localSettings = Windows.Storage.ApplicationData.Current.LocalSettings;
localSettings.Values["test setting"] = "a device specific setting";

// Save a composite setting that will be roamed between devices
ApplicationDataContainer roamingSettings = Windows.Storage.ApplicationData.Current.RoamingSettings;
Windows.Storage.ApplicationDataCompositeValue composite = new Windows.Storage.ApplicationDataCompositeValue();
composite["Font"] = "Calibri";
composite["FontSize"] = 11;
roamingSettings.Values["RoamingFontInfo"] = composite;
 ```

먼저 `Windows.Storage.ApplicationData.Current.LocalSettings`로 로컬 설정 데이터 저장소에 대한 **ApplicationDataContainer**를 가져와 로컬 장치에 설정을 저장합니다. 이 인스턴스에 할당한 키/값 사전 쌍은 로컬 장치 설정 데이터 저장소에 저장됩니다.

비슷한 패턴을 사용하여 로밍 설정을 저장합니다. 먼저 `Windows.Storage.ApplicationData.Current.RoamingSettings`로 로밍 설정 데이터 저장소에 대한 **ApplicationDataContainer**를 가져옵니다. 그런 다음 이 인스턴스에 키/값 쌍을 할당합니다.  이러한 키/값 쌍은 자동으로 디바이스 간에 로밍됩니다.

위의 코드 조각에서 **ApplicationDataCompositeValue**는 여러 키/값 쌍을 저장합니다. 복합 값은 서로 비동기화되어서는 안 되는 여러 설정을 사용하는 경우 유용합니다. **ApplicationDataCompositeValue**를 저장하면 값이 저장되고 하나의 단위로 또는 원자 단위로 자동으로 로드됩니다. 이를 통해 설정이 개별적이지 않고 한 단위로 로밍되기 때문에 관련된 설정이 비동기화되지 않습니다.

## <a name="load-app-settings"></a>앱 설정 로드

```csharp
// load a setting that is local to the device
ApplicationDataContainer localSettings = Windows.Storage.ApplicationData.Current.LocalSettings;
String localValue = localSettings.Values["test setting"] as string;

// load a composite setting that roams between devices
ApplicationDataContainer roamingSettings = Windows.Storage.ApplicationData.Current.RoamingSettings;
Windows.Storage.ApplicationDataCompositeValue composite = (ApplicationDataCompositeValue)roamingSettings.Values["RoamingFontInfo"];
if (composite != null)
{
    String fontName = composite["Font"] as string;
    int fontSize = (int)composite["FontSize"];
}
```

`Windows.Storage.ApplicationData.Current.LocalSettings`로 로컬 설정 데이터 저장소에 대한 **ApplicationDataContainer** 인스턴스를 가져와 로컬 장치에서 설정을 로드합니다. 이를 사용하여 키/값 쌍을 검색합니다.

비슷한 패턴을 따라 로밍 설정을 로드합니다. 먼저 `Windows.Storage.ApplicationData.Current.RoamingSettings`로 로밍 설정 데이터 저장소에서 **ApplicationDataContainer** 인스턴스를 가져옵니다. 해당 인스턴스에서 키/값 쌍에 액세스합니다. 데이터가 아직 설정에 액세스하는 장치에 로밍되지 않은 경우 null **ApplicationDataContainer**를 얻습니다. 그러한 이유 때문에 위 코드 예제에서 `if (composite != null)` 확인이 있습니다.

## <a name="useful-apis-and-docs"></a>유용한 API 및 문서

여기에 앱 설정 저장 및 로드를 시작하는 데 도움이 되는 API의 빠른 요약 및 다른 유용한 문서가 제공됩니다.

### <a name="useful-apis"></a>유용한 API

| API | 설명 |
|------|---------------|
| [ApplicationData.LocalSettings](https://msdn.microsoft.com/library/windows/apps/windows.storage.applicationdata.temporaryfolder) | 로컬 앱 데이터 저장소에서 응용 프로그램 설정 컨테이너를 가져옵니다. |
| [ApplicationData.RoamingSettings](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingsettings) | 로밍 앱 데이터 저장소에서 응용 프로그램 설정 컨테이너를 가져옵니다. |
| [ApplicationDataContainer](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer) | 컨테이너 계층 만들기, 삭제, 열거 및 통과를 지원하는 앱 설정에 대한 컨테이너입니다. |
| [Windows.UI.ApplicationSettings 네임스페이스](https://docs.microsoft.com/uwp/api/windows.ui.applicationsettings) | Windows 셸의 설정 창에 표시되는 앱 설정을 정의하는 데 사용할 수 있는 클래스를 제공합니다. |

### <a name="useful-docs"></a>유용한 문서

| 항목 | 설명 |
|-------|----------------|
| [앱 설정에 대한 지침](https://docs.microsoft.com/windows/uwp/design/app-settings/guidelines-for-app-settings) | 앱 설정을 만들고 표시하기 위한 모범 사례를 설명합니다. |
| [설정 및 기타 앱 데이터 저장 및 검색](https://docs.microsoft.com/windows/uwp/design/app-settings/store-and-retrieve-app-data#create-and-read-a-local-file) | 로밍 설정을 포함하여 설정을 저장하고 검색하는 단계를 안내합니다. |

## <a name="useful-code-samples"></a>유용한 코드 샘플

| 코드 샘플 | 설명 |
|-----------------|---------------|
| [응용 프로그램 데이터 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ApplicationData) | 설정의 2-4 포커스 |
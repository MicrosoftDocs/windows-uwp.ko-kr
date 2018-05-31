---
author: mijacobs
Description: Learn how to store and retrieve local, roaming, and temporary app data.
title: 설정 및 기타 앱 데이터 저장 및 검색
ms.assetid: 41676A02-325A-455E-8565-C9EC0BC3A8FE
label: App settings and data
template: detail.hbs
ms.author: mijacobs
ms.date: 11/14/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 47383cd8f89e5da104ac73878b0613c364240459
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/30/2018
ms.locfileid: "1817301"
---
# <a name="store-and-retrieve-settings-and-other-app-data"></a>설정 및 기타 앱 데이터 저장 및 검색

*앱 데이터*는 특정 앱에 대한 변경 가능한 데이터입니다. 여기에는 런타임 상태, 사용자 기본 설정 및 기타 설정이 포함됩니다. 앱 데이터는 사용자가 앱을 사용할 때 만들고 관리하는 데이터인 *사용자 데이터*와 다릅니다. 사용자 데이터에는 문서 또는 미디어 파일, 메일 또는 통신 기록, 사용자가 만든 콘텐츠를 보유하는 데이터베이스 레코드 등이 포함됩니다. 사용자 데이터는 둘 이상의 앱에 유용하거나 의미가 있을 수 있습니다. 사용자가 앱 자체와 별개로 문서와 같은 엔터티로 전송하거나 조작하려는 데이터인 경우가 많습니다.

**앱 데이터에 대한 중요 정보:  ** - 앱 데이터의 수명은 앱의 수명으로 제한됩니다. 앱이 제거되면 결과적으로 앱 데이터가 모두 손실됩니다. 사용자에게 중요하거나 대체할 수 없는 사용자 데이터나 항목을 저장하는 데 앱 데이터를 사용하지 마세요. 이러한 종류의 정보는 사용자 라이브러리 및 Microsoft OneDrive를 사용하여 저장하는 것이 좋습니다. 앱 데이터는 앱 관련 사용자 기본 설정, 설정 및 즐겨찾기를 저장하는 데 적합합니다.

## <a name="types-of-app-data"></a>앱 데이터의 유형


앱 데이터에는 설정과 파일의 두 가지 유형이 있습니다.

-   **설정**

    설정을 사용하여 사용자 기본 설정 및 응용 프로그램 상태 정보를 저장합니다. 앱 데이터 API를 사용하면 설정을 쉽게 만들고 검색할 수 있습니다(이 문서의 뒷부분에 나오는 예제 참조).

    앱 설정에 사용할 수 있는 데이터 형식은 다음과 같습니다.

    -   **UInt8**, **Int16**, **UInt16**, **Int32**, **UInt32**, **Int64**, **UInt64**, **Single**, **Double**
    -   **부울**
    -   **Char16**, **String**
    -   [**DateTime**](https://msdn.microsoft.com/library/windows/apps/br206576), [**TimeSpan**](https://msdn.microsoft.com/library/windows/apps/br225996)
    -   **GUID**, [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870), [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995), [**Rect**](https://msdn.microsoft.com/library/windows/apps/br225994)
    -   [**ApplicationDataCompositeValue**](https://msdn.microsoft.com/library/windows/apps/br241588): 원자 단위로 직렬화 및 역직렬화해야 하는 관련 앱 설정 집합입니다. 상호 의존적인 설정의 원자성 업데이트를 쉽게 처리하려면 복합 설정을 사용합니다. 시스템은 동시 액세스 및 로밍 중에 복합 설정의 무결성을 보장합니다. 복합 설정은 소량의 데이터에 최적화되어 있으며 대규모 데이터 집합에 사용할 경우에는 성능이 저하됩니다.
-   **파일**

    파일을 사용하여 이진 데이터를 저장하거나 고유한 사용자 지정된 직렬화된 형식을 사용하도록 설정합니다.

## <a name="storing-app-data-in-the-app-data-stores"></a>앱 데이터 저장소에 앱 데이터 저장


앱이 설치되면 시스템은 설정 및 파일에 대한 사용자별 데이터 저장소를 앱에 할당합니다. 시스템이 실제 저장소를 관리하므로 개발자는 이 데이터가 어디에 또는 어떻게 저장되는지는 알 필요가 없습니다. 데이터가 다른 앱 및 다른 사용자와 격리된 상태로 유지됩니다. 또한 시스템은 사용자가 앱에 업데이트를 설치할 때는 이러한 데이터 저장소의 콘텐츠를 보호하고, 앱을 제거할 때는 데이터 저장소의 콘텐츠가 완전하게 깨끗하게 제거합니다.

각 앱의 앱 데이터 저장소 내에는 시스템에서 정의한 루트 디렉터리가 있습니다. 총 3개로, 각각 로컬 파일, 로밍 파일, 임시 파일을 위한 것입니다. 이러한 각 루트 디렉터리에 새 파일 및 새 컨테이너를 추가할 수 있습니다.

## <a name="local-app-data"></a>로컬 앱 데이터


로컬 앱 데이터는 앱 세션 간에 유지해야 하고 로밍 앱 데이터에 적절하지 않은 모든 정보에 사용해야 합니다. 다른 장치에서 적용할 수 없는 데이터도 여기에 저장해야 합니다. 저장된 로컬 데이터에 대한 일반 크기 제한은 없습니다. 로밍하기 적절하지 않은 데이터 및 대규모 데이터 집합에 대해 로컬 앱 데이터 저장소를 사용합니다.

### <a name="retrieve-the-local-app-data-store"></a>로컬 앱 데이터 저장소 검색

로컬 앱 데이터를 읽거나 쓰려면 먼저 로컬 앱 데이터 저장소를 검색해야 합니다. 로컬 앱 데이터 저장소를 검색하려면 [**ApplicationData.LocalSettings**](https://msdn.microsoft.com/library/windows/apps/br241622) 속성을 사용하여 앱의 로컬 설정을 [**ApplicationDataContainer**](https://msdn.microsoft.com/library/windows/apps/br241599) 개체로 가져옵니다. [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br241621) 개체의 파일을 가져오려면 [**ApplicationData.LocalFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) 속성을 사용합니다. 백업 및 복원에 포함되지 않는 파일을 저장할 수 있는 로컬 앱 데이터 저장소의 폴더를 가져오려면 [**ApplicationData.LocalCacheFolder**](https://msdn.microsoft.com/library/windows/apps/dn633825) 속성을 사용합니다.

```CSharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;
```

### <a name="create-and-retrieve-a-simple-local-setting"></a>간단한 로컬 설정 만들기 및 검색

설정을 만들거나 작성하려면 앞 단계에서 가져온 `localSettings` 컨테이너의 설정에 액세스하려면 [**ApplicationDataContainer.Values**](https://msdn.microsoft.com/library/windows/apps/br241615) 속성을 사용합니다. 이 예제에서는 `exampleSetting`이라는 설정을 만듭니다.

```CSharp
// Simple setting

localSettings.Values["exampleSetting"] = "Hello Windows";
```

설정을 검색하려면 설정을 만들 때 사용한 것과 동일한 [**ApplicationDataContainer.Values**](https://msdn.microsoft.com/library/windows/apps/br241615) 속성을 사용합니다. 다음 예제에서는 방금 만든 설정을 검색하는 방법을 보여 줍니다.

```CSharp
// Simple setting
Object value = localSettings.Values["exampleSetting"];
```

### <a name="create-and-retrieve-a-local-composite-value"></a>로컬 복합 값 만들기 및 검색

복합 값을 만들거나 작성하려면 [**ApplicationDataCompositeValue**](https://msdn.microsoft.com/library/windows/apps/br241588) 개체를 만듭니다. 이 예제에서는 `exampleCompositeSetting`이라는 복합 설정을 만들어 `localSettings` 컨테이너에 추가합니다.

```CSharp
// Composite setting

Windows.Storage.ApplicationDataCompositeValue composite = 
    new Windows.Storage.ApplicationDataCompositeValue();
composite["intVal"] = 1;
composite["strVal"] = "string";

localSettings.Values["exampleCompositeSetting"] = composite;
```

다음 예제에서는 방금 만든 복합 값을 검색하는 방법을 보여 줍니다.

```CSharp
// Composite setting

Windows.Storage.ApplicationDataCompositeValue composite = 
   (Windows.Storage.ApplicationDataCompositeValue)localSettings.Values["exampleCompositeSetting"];

if (composite == null)
{
   // No data
}
else
{
   // Access data in composite["intVal"] and composite["strVal"]
}
```

### <a name="create-and-read-a-local-file"></a>로컬 파일 만들기 및 읽기

로컬 앱 데이터 저장소에 파일을 만들고 업데이트하려면 파일 API(예: [**Windows.Storage.StorageFolder.CreateFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227249) 및 [**Windows.Storage.FileIO.WriteTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701505))를 사용합니다. 이 예제에서는 `dataFile.txt`라는 파일을 `localFolder` 컨테이너에 만들고 현재 날짜와 시간을 이 파일에 기록합니다. [**CreationCollisionOption**](https://msdn.microsoft.com/library/windows/apps/br241631) 열거형의 **ReplaceExisting** 값은 파일이 이미 있는 경우 파일을 바꾸어야 함을 나타냅니다.

```csharp
async void WriteTimestamp()
{
   Windows.Globalization.DateTimeFormatting.DateTimeFormatter formatter = 
       new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("longtime");

   StorageFile sampleFile = await localFolder.CreateFileAsync("dataFile.txt", 
       CreationCollisionOption.ReplaceExisting);
   await FileIO.WriteTextAsync(sampleFile, formatter.Format(DateTimeOffset.Now));
}
```

로컬 앱 데이터 저장소에서 파일을 열고 읽으려면 [**Windows.Storage.StorageFolder.GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272), [**Windows.Storage.StorageFile.GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741) 및 [**Windows.Storage.FileIO.ReadTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701482) 같은 파일 API를 사용합니다. 이 예제에서는 앞 단계에서 만든 `dataFile.txt` 파일을 열고 파일에서 날짜를 읽습니다. 다양한 위치에서 파일 리소스 로드에 대한 자세한 내용은 [파일 리소스를 로드하는 방법](https://msdn.microsoft.com/library/windows/apps/xaml/hh965322)을 참조하세요.

```csharp
async void ReadTimestamp()
{
   try
   {
      StorageFile sampleFile = await localFolder.GetFileAsync("dataFile.txt");
      String timestamp = await FileIO.ReadTextAsync(sampleFile);
      // Data is contained in timestamp
   }
   catch (Exception)
   {
      // Timestamp not found
   }
}
```

## <a name="roaming-data"></a>데이터 로밍


앱에서 로밍 데이터를 사용하는 경우 사용자는 손쉽게 앱의 앱 데이터를 여러 장치 간에 동기화 상태로 유지할 수 있습니다. 사용자가 여러 장치에 앱을 설치하는 경우, OS는 앱 데이터를 동기화 상태로 유지하여 사용자가 두 번째 장치에 앱을 설치할 때 수행해야 할 설치 작업이 줄어듭니다. 또한 로밍을 사용하면 목록 작성 같은 작업을 중단했다가 다른 장치에서 바로 이어서 진행할 수 있습니다. OS는 로밍 데이터가 업데이트되면 이를 클라우드로 복제하고, 앱이 설치된 다른 장치의 데이터와 동기화합니다.

OS에서는 각 앱이 로밍할 수 있는 앱 데이터의 크기를 제한합니다. [**ApplicationData.RoamingStorageQuota**](https://msdn.microsoft.com/library/windows/apps/br241625)을 참조하세요. 앱이 이 제한에 도달하면 앱의 로밍된 총 앱 데이터가 다시 제한보다 낮아질 때까지 앱의 앱 데이터가 클라우드로 복제되지 않습니다. 따라서 사용자 기본 설정, 링크 및 소규모 데이터 파일에만 로밍 데이터를 사용하는 것이 가장 좋습니다.

앱의 로밍 데이터는 사용자가 일부 장치에서 정해진 기간 내에 액세스하는 경우에는 클라우드에서 사용할 수 있습니다. 사용자가 이 기간보다 오래 앱을 실행하지 않으면 로밍 데이터는 클라우드에서 제거됩니다. 사용자가 앱을 제거하면 로밍 데이터는 클라우드에서 자동으로 제거되지 않고 보존됩니다. 사용자가 정해진 기간 내에 앱을 다시 설치하면 로밍 데이터는 클라우드에서 동기화됩니다.

### <a name="roaming-data-dos-and-donts"></a>로밍 데이터 권장 사항 및 금지 사항

-   로밍은 사용자 기본 설정 및 사용자 지정, 링크 및 작은 데이터 파일에 사용합니다. 예를 들어 로밍을 사용하여 모든 장치에서 사용자의 배경색 기본 설정을 유지할 수 있습니다.
-   로밍을 사용하여 사용자가 여러 장치에서 작업을 계속할 수 있도록 합니다. 예를 들어 뷰어 앱에서 최근에 본 페이지나 임시 메일의 내용과 같은 앱 데이터를 로밍합니다.
-   앱 데이터를 업데이트하여 [**DataChanged**](https://msdn.microsoft.com/library/windows/apps/br241620) 이벤트를 처리합니다. 이 이벤트는 클라우드에서 앱 데이터 동기화가 방금 완료된 경우 발생합니다.
-   원시 데이터가 아닌 콘텐츠에 대한 참조를 로밍합니다. 예를 들어 온라인 기사의 내용이 아닌 URL을 로밍합니다.
-   시간이 중요한 설정의 경우 [**RoamingSettings**](https://msdn.microsoft.com/library/windows/apps/br241624)와 관련된 *HighPriority* 설정을 사용합니다.
-   장치와 관련된 앱 데이터는 로밍하지 않습니다. 로컬 파일 리소스의 경로 이름과 같은 일부 정보는 로컬에만 관련됩니다. 로컬 정보를 로밍하려는 경우 정보가 보조 장치에서 유효하지 않으면 앱을 복구할 수 있는지 확인해야 합니다.
-   큰 앱 데이터 집합은 로밍하지 않습니다. 앱에서 로밍할 수 있는 앱 데이터의 양에는 제한이 있습니다. [**RoamingStorageQuota**](https://msdn.microsoft.com/library/windows/apps/br241625) 속성을 사용하여 최대값을 가져옵니다. 앱이 이 제한에 도달하면 앱 데이터 저장소의 크기가 이 제한을 더 이상 초과하지 않을 때까지 데이터가 로밍되지 않습니다. 앱을 디자인할 때는 큰 데이터에 대한 제한을 두어 제한을 초과하지 않도록 하는 방법을 고려해야 합니다. 예를 들어 게임 상태 저장에 각각 10KB가 필요한 경우 앱에서 사용자가 최대 10개의 게임을 저장하도록 허용할 수 있습니다.
-   즉각적인 동기화를 사용하는 데이터에는 로밍을 사용하지 않습니다. Windows는 즉각적인 동기화를 보증하지 않으므로 사용자가 오프라인 상태이거나 지연 시간이 긴 네트워크에 있을 경우 로밍이 현저하게 지연될 수 있습니다. UI가 즉각적인 동기화를 사용하지 않는지 확인하세요.
-   자주 변경되는 데이터에는 로밍을 사용하지 않습니다. 예를 들어 앱이 노래의 초 단위 위치와 같이 자주 변경되는 정보를 추적하는 경우 이 정보를 로밍 앱 데이터로 저장하지 마세요. 대신 현재 재생 중인 노래와 같이 좋은 사용자 환경을 제공하는 자주 사용되지 않는 표현을 선택합니다.

### <a name="roaming-pre-requisites"></a>로밍 필수 조건

Microsoft 계정을 사용하여 장치에 로그온한 사용자는 누구나 로밍 앱 데이터를 활용할 수 있습니다. 그러나 사용자 및 그룹 정책 관리자는 언제든지 장치에서 로밍 앱 데이터를 전환할 수 있습니다. 사용자가 Microsoft 계정을 사용하지 않도록 선택하거나 데이터 로밍 접근 권한 값을 사용하지 않도록 설정하면 앱은 계속 사용할 수 있지만 앱 데이터는 각 장치에 대해 로컬이 됩니다.

[**PasswordVault**](https://msdn.microsoft.com/library/windows/apps/br227081)에 저장된 데이터는 사용자가 장치를 "신뢰할 수 있는 장치"로 지정한 경우에만 전환됩니다. 장치를 신뢰할 수 없으면 이 자격 증명 모음에 보관된 데이터가 로밍되지 않습니다.

### <a name="conflict-resolution"></a>충돌 해결

로밍 앱 데이터는 한 번에 둘 이상의 장치에서 동시에 사용할 수 없습니다. 두 장치에서 특정 데이터 단위가 변경되어 동기화 중에 충돌이 발생하면 시스템은 항상 마지막으로 기록된 값을 선호합니다. 따라서 앱은 최신 정보를 사용하게 됩니다. 데이터 단위가 설정 복합인 경우 충돌 해결은 여전히 설정 단위 수준에서 이루어집니다. 즉, 마지막으로 변경된 복합이 동기화됩니다.

### <a name="when-to-write-data"></a>데이터를 기록하는 시기

설정의 예상 수명에 따라 데이터는 다른 시간에 기록되어야 합니다. 가끔 또는 느리게 변경되는 앱 데이터는 즉시 기록되어야 합니다. 그러나 자주 변경되는 앱 데이터는 정기적으로(예: 5분마다 한 번씩), 그리고 앱이 일시 중단된 경우에만 기록되어야 합니다. 예를 들어 음악 앱에서는 새로운 곡이 재생되기 시작할 때마다 "현재 곡" 설정을 기록할 수 있지만 곡의 실제 위치는 일시 중단 시에만 기록해야 합니다.

### <a name="excessive-usage-protection"></a>과도한 사용 보호

시스템에 다양한 보호 메커니즘이 갖추어져 있어야 부적절한 리소스 사용을 방지할 수 있습니다. 앱 데이터가 예상대로 전환되지 않는 경우 장치가 일시적으로 제한된 상태이기 때문일 수 있습니다. 얼마간 기다리면 대개 이 문제가 자동으로 해결되며 별도의 작업이 필요하지 않습니다.

### <a name="versioning"></a>버전 관리

앱 데이터는 버전 관리를 사용하여 하나의 데이터 구조에서 다른 데이터 구조로 업그레이드될 수 있습니다. 버전 번호는 앱 번호와 다르며 마음대로 설정할 수 있습니다. 최신 데이터를 나타내는 더 낮은 데이터 버전 번호로 전환하려고 하면 원치 않는 혼란(데이터 손실 포함)이 발생할 수 있으므로 가능하면 증가하는 버전 번호를 사용하는 것이 가장 좋습니다.

앱 데이터는 버전 번호가 동일한 앱 간에만 로밍됩니다. 예를 들어 버전 2의 장치는 서로 간에 데이터를 전환하고 버전 3의 장치도 마찬가지이지만 버전 2를 실행하는 장치와 버전 3을 실행하는 장치 간에 로밍이 수행되지 않습니다. 다양한 버전 번호를 사용하는 새 앱을 다른 장치에 설치하면 새로 설치된 앱이 가장 높은 버전 번호와 관련된 앱 데이터를 동기화합니다.

### <a name="testing-and-tools"></a>테스트 및 도구

개발자는 로밍 앱 데이터의 동기화를 트리거하기 위해 장치를 잠글 수 있습니다. 앱 데이터가 특정 기간 내에 전환되지 않는 것 같으면 다음 항목을 확인하세요.

-   로밍 데이터가 최대 크기를 초과하지 않습니다(자세한 내용은 [**RoamingStorageQuota**](https://msdn.microsoft.com/library/windows/apps/br241625) 참조).
-   파일이 올바로 닫히고 릴리스되었습니다.
-   동일한 버전의 앱을 실행하는 장치가 2개 이상 있습니다.


### <a name="register-to-receive-notification-when-roaming-data-changes"></a>로밍 데이터가 변경되면 알림을 받도록 등록

로밍 앱 데이터를 사용하려면 설정을 읽고 쓸 수 있도록 로밍 데이터 변경을 등록하고 로밍 데이터 컨테이너를 검색해야 합니다.

1.  로밍 데이터가 변경되면 알림을 받도록 등록

    [**DataChanged**](https://msdn.microsoft.com/library/windows/apps/br241620) 이벤트는 로밍 데이터가 변경된 경우 알려 줍니다. 이 예제에서는 `DataChangeHandler`를 로밍 데이터 변경에 대한 처리기로 설정합니다.

```csharp
void InitHandlers()
    {
       Windows.Storage.ApplicationData.Current.DataChanged += 
          new TypedEventHandler<ApplicationData, object>(DataChangeHandler);
    }

    void DataChangeHandler(Windows.Storage.ApplicationData appData, object o)
    {
       // TODO: Refresh your data
    }
```

2.  앱 설정 및 파일 컨테이너 가져오기

    설정을 가져오려면 [**ApplicationData.RoamingSettings**](https://msdn.microsoft.com/library/windows/apps/br241624) 속성을 사용하고 파일을 가져오려면 [**ApplicationData.RoamingFolder**](https://msdn.microsoft.com/library/windows/apps/br241623) 속성을 사용합니다.

```csharp
Windows.Storage.ApplicationDataContainer roamingSettings = 
        Windows.Storage.ApplicationData.Current.RoamingSettings;
    Windows.Storage.StorageFolder roamingFolder = 
        Windows.Storage.ApplicationData.Current.RoamingFolder;
```

### <a name="create-and-retrieve-roaming-settings"></a>로밍 설정 만들기 및 검색

앞 섹션에서 가져온 `roamingSettings` 컨테이너의 설정에 액세스하려면 [**ApplicationDataContainer.Values**](https://msdn.microsoft.com/library/windows/apps/br241615) 속성을 사용합니다. 다음 예제에서는 `exampleSetting`이라는 간단한 설정 및 `composite`라는 복합 값을 만듭니다.

```csharp
// Simple setting

roamingSettings.Values["exampleSetting"] = "Hello World";
// High Priority setting, for example, last page position in book reader app
roamingSettings.values["HighPriority"] = "65";

// Composite setting

Windows.Storage.ApplicationDataCompositeValue composite = 
    new Windows.Storage.ApplicationDataCompositeValue();
composite["intVal"] = 1;
composite["strVal"] = "string";

roamingSettings.Values["exampleCompositeSetting"] = composite;

```

다음 예제에서는 방금 만든 설정을 검색합니다.

```csharp
// Simple setting

Object value = roamingSettings.Values["exampleSetting"];

// Composite setting

Windows.Storage.ApplicationDataCompositeValue composite = 
   (Windows.Storage.ApplicationDataCompositeValue)roamingSettings.Values["exampleCompositeSetting"];

if (composite == null)
{
   // No data
}
else
{
   // Access data in composite["intVal"] and composite["strVal"]
}
```

### <a name="create-and-retrieve-roaming-files"></a>로밍 파일 만들기 및 검색

로밍 앱 데이터 저장소에서 파일을 만들고 업데이트하려면 [**Windows.Storage.StorageFolder.CreateFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227249) 및 [**Windows.Storage.FileIO.WriteTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701505) 같은 파일 API를 사용합니다. 이 예제에서는 `dataFile.txt`라는 파일을 `roamingFolder` 컨테이너에 만들고 현재 날짜와 시간을 이 파일에 기록합니다. [**CreationCollisionOption**](https://msdn.microsoft.com/library/windows/apps/br241631) 열거형의 **ReplaceExisting** 값은 파일이 이미 있는 경우 파일을 바꾸어야 함을 나타냅니다.

```csharp
async void WriteTimestamp()
{
   Windows.Globalization.DateTimeFormatting.DateTimeFormatter formatter = 
       new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("longtime");

   StorageFile sampleFile = await roamingFolder.CreateFileAsync("dataFile.txt", 
       CreationCollisionOption.ReplaceExisting);
   await FileIO.WriteTextAsync(sampleFile, formatter.Format(DateTimeOffset.Now));
}
```

로밍 앱 데이터 저장소에서 파일을 열고 읽으려면 [**Windows.Storage.StorageFolder.GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272), [**Windows.Storage.StorageFile.GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741) 및 [**Windows.Storage.FileIO.ReadTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701482) 같은 파일 API를 사용합니다. 이 예제에서는 앞 섹션에서 만든 `dataFile.txt` 파일을 열고 파일에서 날짜를 읽습니다. 다양한 위치에서 파일 리소스 로드에 대한 자세한 내용은 [파일 리소스를 로드하는 방법](https://msdn.microsoft.com/library/windows/apps/xaml/hh965322)을 참조하세요.

```csharp
async void ReadTimestamp()
{
   try
   {
      StorageFile sampleFile = await roamingFolder.GetFileAsync("dataFile.txt");
      String timestamp = await FileIO.ReadTextAsync(sampleFile);
      // Data is contained in timestamp
   }
   catch (Exception)
   {
      // Timestamp not found
   }
}
```


## <a name="temporary-app-data"></a>임시 앱 데이터


임시 앱 데이터 저장소는 캐시처럼 작동합니다. 파일이 로밍되지 않으면 언제든 제거할 수 있습니다. 시스템 유지 관리 작업에서 이 위치에 저장된 데이터를 언제든지 자동으로 삭제할 수 있습니다. 사용자는 디스크 정리를 수행하여 임시 데이터 저장소에서 파일을 지울 수도 있습니다. 임시 앱 데이터는 앱 세션 중 임시 정보를 저장하는 데 사용할 수 있습니다. 시스템에서 필요한 경우 사용된 공간을 재사용할 수 있으므로 앱 세션이 끝난 이후까지 이 데이터가 유지된다고 보장할 수는 없습니다. 위치는 [**temporaryFolder**](https://msdn.microsoft.com/library/windows/apps/br241629) 속성을 통해 사용할 수 있습니다.

### <a name="retrieve-the-temporary-data-container"></a>임시 데이터 컨테이너 검색

파일을 가져오려면 [**ApplicationData.TemporaryFolder**](https://msdn.microsoft.com/library/windows/apps/br241629) 속성을 사용합니다. 다음 단계에서는 이 단계의 `temporaryFolder` 변수를 사용합니다.

```csharp
Windows.Storage.StorageFolder temporaryFolder = ApplicationData.Current.TemporaryFolder;
```

### <a name="create-and-read-temporary-files"></a>임시 파일 만들기 및 읽기

임시 앱 데이터 저장소에서 파일을 만들고 업데이트하려면 [**Windows.Storage.StorageFolder.CreateFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227249) 및 [**Windows.Storage.FileIO.WriteTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701505) 같은 파일 API를 사용합니다. 이 예제에서는 `dataFile.txt`라는 파일을 `temporaryFolder` 컨테이너에 만들고 현재 날짜와 시간을 이 파일에 기록합니다. [**CreationCollisionOption**](https://msdn.microsoft.com/library/windows/apps/br241631) 열거형의 **ReplaceExisting** 값은 파일이 이미 있는 경우 파일을 바꾸어야 함을 나타냅니다.


```csharp
async void WriteTimestamp()
{
   Windows.Globalization.DateTimeFormatting.DateTimeFormatter formatter = 
       new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("longtime");

   StorageFile sampleFile = await temporaryFolder.CreateFileAsync("dataFile.txt", 
       CreateCollisionOption.ReplaceExisting);
   await FileIO.WriteTextAsync(sampleFile, formatter.Format(DateTimeOffset.Now));
}
```

임시 앱 데이터 저장소에서 파일을 열고 읽으려면 [**Windows.Storage.StorageFolder.GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272), [**Windows.Storage.StorageFile.GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741) 및 [**Windows.Storage.FileIO.ReadTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701482) 같은 파일 API를 사용합니다. 이 예제에서는 앞 단계에서 만든 `dataFile.txt` 파일을 열고 파일에서 날짜를 읽습니다. 다양한 위치에서 파일 리소스 로드에 대한 자세한 내용은 [파일 리소스를 로드하는 방법](https://msdn.microsoft.com/library/windows/apps/xaml/hh965322)을 참조하세요.

```csharp
async void ReadTimestamp()
{
   try
   {
      StorageFile sampleFile = await temporaryFolder.GetFileAsync("dataFile.txt");
      String timestamp = await FileIO.ReadTextAsync(sampleFile);
      // Data is contained in timestamp
   }
   catch (Exception)
   {
      // Timestamp not found
   }
}
```

## <a name="organize-app-data-with-containers"></a>컨테이너를 사용하여 앱 데이터 구성


앱 데이터 설정 및 파일을 정리하려면 디렉터리로 직접 작업하는 대신 컨테이너([**ApplicationDataContainer**](https://msdn.microsoft.com/library/windows/apps/br241599) 개체로 표현됨)를 만듭니다. 로컬, 로밍 및 임시 앱 데이터 저장소에 컨테이너를 추가할 수 있습니다. 컨테이너는 최대 32개 수준까지 중첩될 수 있습니다.

설정 컨테이너를 만들려면 [**ApplicationDataContainer.CreateContainer**](https://msdn.microsoft.com/library/windows/apps/br241611) 메서드를 호출합니다. 다음 예제에서는 `exampleContainer`라는 로컬 설정 컨테이너를 만들고 `exampleSetting`이라는 설정을 추가합니다. [**ApplicationDataCreateDisposition**](https://msdn.microsoft.com/library/windows/apps/br241616) 열거형의 **Always** 값은 컨테이너가 아직 없는 경우 컨테이너를 만들어야 함을 나타냅니다.

```csharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Setting in a container
Windows.Storage.ApplicationDataContainer container = 
   localSettings.CreateContainer("exampleContainer", Windows.Storage.ApplicationDataCreateDisposition.Always);

if (localSettings.Containers.ContainsKey("exampleContainer"))
{
   localSettings.Containers["exampleContainer"].Values["exampleSetting"] = "Hello Windows";
}
```

## <a name="delete-app-settings-and-containers"></a>앱 설정 및 컨테이너 삭제


앱에 더 이상 필요 없는 간단한 설정을 삭제하려면 [**ApplicationDataContainerSettings.Remove**](https://msdn.microsoft.com/library/windows/apps/br241608) 메서드를 사용합니다. 다음 예제에서는 앞에서 만든 `exampleSetting` 로컬 설정을 삭제합니다.

```csharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Delete simple setting

localSettings.Values.Remove("exampleSetting");
```

복합 설정을 삭제하려면 [**ApplicationDataCompositeValue.Remove**](https://msdn.microsoft.com/library/windows/apps/br241597) 메서드를 사용합니다. 다음 예제에서는 이전 예제에서 만든 로컬 `exampleCompositeSetting` 복합 설정을 삭제합니다.

```csharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Delete composite setting

localSettings.Values.Remove("exampleCompositeSetting");
```

컨테이너를 삭제하려면 [**ApplicationDataContainer.DeleteContainer**](https://msdn.microsoft.com/library/windows/apps/br241612) 메서드를 호출합니다. 다음 예제에서는 앞에서 만든 `exampleContainer` 로컬 설정 컨테이너를 삭제합니다.

```csharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Delete container

localSettings.DeleteContainer("exampleContainer");
```

## <a name="versioning-your-app-data"></a>앱 데이터 버전 관리


선택적으로, 앱에 대한 앱 데이터에 버전을 지정할 수 있습니다. 이렇게 하면 앱의 이전 버전과 호환성 문제를 일으키지 않고 앱 데이터의 형식을 변경하는 차기 앱 버전을 만들 수 있습니다. 앱은 데이터 저장소에서 앱 데이터의 버전을 확인하며, 그 버전이 앱 버전보다 낮을 경우 앱은 앱 데이터를 새 형식으로 업데이트하고 버전을 업데이트해야 합니다. 자세한 내용은 [**Application.Version**](https://msdn.microsoft.com/library/windows/apps/br241630) 속성 및 [**ApplicationData.SetVersionAsync**](https://msdn.microsoft.com/library/windows/apps/hh701429) 메서드를 참조하세요.

## <a name="related-articles"></a>관련 문서

* [**Windows.Storage.ApplicationData**](https://msdn.microsoft.com/library/windows/apps/br241587)
* [**Windows.Storage.ApplicationData.RoamingSettings**](https://msdn.microsoft.com/library/windows/apps/br241624)
* [**Windows.Storage.ApplicationData.RoamingFolder**](https://msdn.microsoft.com/library/windows/apps/br241623)
* [**Windows.Storage.ApplicationData.RoamingStorageQuota**](https://msdn.microsoft.com/library/windows/apps/br241625)
* [**Windows.Storage.ApplicationDataCompositeValue**](https://msdn.microsoft.com/library/windows/apps/br241588)

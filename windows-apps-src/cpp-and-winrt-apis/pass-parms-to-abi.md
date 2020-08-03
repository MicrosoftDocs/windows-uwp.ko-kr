---
description: C++/WinRT는 일반적인 경우에 대한 자동 변환을 제공하여 매개 변수를 ABI 경계에 전달하는 것을 단순화합니다.
title: 매개 변수를 ABI 경계로 전달
ms.date: 07/10/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, 프로젝션, 전달, 매개 변수, ABI
ms.localizationpriority: medium
ms.openlocfilehash: 51cde2332d3d9df9d1f488aa7f8246f9e1e2ed36
ms.sourcegitcommit: e1104689fc1db5afb85701205c2580663522ee6d
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997980"
---
# <a name="passing-parameters-into-the-abi-boundary"></a>매개 변수를 ABI 경계로 전달

**winrt::param** 네임스페이스의 형식을 사용하여 C++/WinRT는 일반적인 경우에 대한 자동 변환을 제공하여 매개 변수를 ABI 경계에 전달하는 것을 단순화합니다. [문자열 처리](/windows/uwp/cpp-and-winrt-apis/strings) 및 [표준 C++ 데이터 형식 및 C++/WinRT](/windows/uwp/cpp-and-winrt-apis/std-cpp-data-types)에서 자세한 내용과 코드 예제를 볼 수 있습니다.

> [!IMPORTANT]
> **winrt::param** 네임스페이스에서 형식을 직접 사용하지 않도록 해야 합니다. 이는 프로젝션의 이점을 위한 것입니다.

많은 형식이 동기 버전과 비동기 버전으로 제공됩니다. C++/WinRT는 매개 변수가 동기 메서드에 전달되는 경우에는 동기 버전을 사용하고 매개 변수가 비동기 메서드에 전달되는 경우에는 비동기 버전을 사용합니다. 비동기 버전에서는 작업이 완료되기 전에 호출자가 컬렉션을 변경하기 어렵게 하기 위해 추가적인 단계를 수행합니다. 그러나 어떠한 변형도 다른 스레드에서 컬렉션이 변경되는 것을 막지는 못합니다. 이를 방지하는 것은 사용자의 책임입니다.

## <a name="string-parameters"></a>문자열 매개 변수

**winrt::param::hstring**은 **HSTRING**을 사용하는 API로의 매개 변수 전달을 간소화합니다.

|전달할 수 있는 형식|참고|
|-|-|
|`{}`|빈 문자열을 전달합니다.|
|**winrt::hstring**||
|**std::wstring_view**|리터럴의 경우 `L"Name"sv`를 작성할 수 있습니다. 보기는 끝 부분 뒤에 null 종결자가 있어야 합니다.|
|**std::wstring**|-|
|**wchar_t const\***|null로 끝나는 문자열입니다.|

`nullptr`은 허용되지 않습니다. 대신 `{}`를 사용하세요.

컴파일러는 컴파일 시 문자열 리터럴에서 `wcslen`을 평가하는 방법을 알고 있습니다. 따라서 리터럴의 경우 `L"Name"sv`와 `L"Name"`은 동일합니다.

**std::wstring_view** 개체는 null로 끝나지 않지만 C++WinRT에서는 문자열 끝의 뒤에 오는 문자가 null이어야 합니다. null로 끝나지 않는 **std::wstring_view**를 전달하면 프로세스가 종료됩니다.

## <a name="iterable-parameters"></a>반복 가능한 매개 변수

**winrt::param::iterable\<T\>** 및 **winrt::param::async_iterable\<T\>** 을 사용하면 **IIterable\<T\>** 을 사용하는 API로 간단하게 매개 변수를 전달할 수 있습니다.

Windows 런타임 컬렉션은 이미 **IIterable**입니다.

|전달할 수 있는 형식|동기화|Async|참고|
|-|-|-|-|
| `nullptr` | 예 | 예 | 기본 메서드에서 `nullptr`을 지원하는지 확인해야 합니다.|
| **IIterable\<T\>** | 예 | 예 | 또는 모든 항목을 변환할 수 있습니다.|
| **std::vector\<T\> const&** | 예 | 아니요 ||
| **std::vector\<T\>&&** | 예 | 예 | 변경되지 않도록 콘텐츠가 반복자로 이동됩니다.|
| **std::initializer_list\<T\>** | 예 | 예 | 비동기 버전은 항목을 복사합니다.|
| **std::initializer_list\<U\>** | 예 | 아니요 | **U**를 **T**로 변환할 수 있어야 합니다.|
| `{ ForwardIt begin, ForwardIt end }` | 예 | 아니요 | `*begin`을 **T**로 변환할 수 있어야 합니다.|

**U**를 **T**로 변환할 수 있더라도 **IIterable\<U\>** 및 **std::vector\<U\>** 는 허용되지 않습니다. **std::vector\<U\>** 의 경우 이중 반복자 버전을 사용할 수 있습니다(자세한 내용은 아래 참조).

경우에 따라서는 사용자가 가진 개체가 실제로 사용자가 원하는 **IIterable**을 구현할 수 있습니다. 예를 들어 [**FileOpenPicker.PickMultipleFilesAsync**](/uwp/api/windows.storage.pickers.fileopenpicker.pickmultiplefilesasync)에 의해 생성된 **IVectorView\<StorageFile\>** 는 **IIterable\<StorageFile\>** 을 구현합니다. 또한 **IIterable\<IStorageItem\>** 도 구현하지만 사용자가 명시적으로 요청해야 합니다.

```cppwinrt
IVectorView<StorageFile> pickedFiles{ co_await filePicker.PickMultipleFilesAsync() };
requestData.SetStorageItems(storageItems.as<IIterable<IStorageItem>>());
```

다른 경우에는 이중 반복자 버전을 사용할 수 있습니다.

```cppwinrt
std::vector<StorageFile> storageFiles;
requestData.SetStorageItems({ storageFiles.begin(), storageFiles.end() });
```

이중 반복자는 전체를 반복하여 **T**로 변환할 수 있는 항목을 생성할 수 있을 때 일반적으로 위의 어떤 시나리오에도 맞지 않는 컬렉션이 있는 경우 더 잘 작동합니다. 위에서는 파생된 형식의 한 벡터에 대해 반복하기 위해 사용했습니다. 여기서는 파생된 형식의 벡터 외 항목에 대해 반복하기 위해 사용됩니다.

```cppwinrt
std::array<StorageFile, 3> storageFiles;
requestData.SetStorageItems(storageFiles); // This doesn't work.
requestData.SetStorageItems({ storageFiles.begin(), storageFiles.end() }); // But this works.
```

[**IIterator\<T\>.GetMany(T\[\])** ](/uwp/api/windows.foundation.collections.iiterator-1.getmany)의 구현은 반복자가 `RandomAcessIt`인 경우에 더 효율적입니다. 그렇지 않으면 해당 범위에 대해 여러 번 전달할 수 있습니다.

|전달할 수 있는 형식|동기화|Async|참고|
|-|-|-|-|
| `nullptr` | 예 | 예 | 기본 메서드에서 `nullptr`을 지원하는지 확인해야 합니다.|
| **IIterable\<IKeyValuePair\<K, V\>\>** | 예 | 예 | 또는 모든 항목을 변환할 수 있습니다.|
| **std::map\<K, V\> const&** | 예 | 아니요 ||
| **std::map\<K, V\>&&** | 예 | 예 | 변경되지 않도록 콘텐츠가 반복자로 이동됩니다.|
| **std::unordered_map\<K, V\> const&** | 예 | 아니요 ||
| **std::unordered_map\<K, V\>&&** | 예 | 예 | 변경되지 않도록 콘텐츠가 반복자로 이동됩니다.|
| **std::initializer_list\<std::pair\<K, V\>\>** | 예 | 예 | 형식 **K** 및 **V**는 정확하게 일치해야 합니다. 키는 중복될 수 없습니다. 비동기 버전은 항목을 복사합니다.|
| `{ ForwardIt begin, ForwardIt end }` | 예 | 아니요 | `begin->first` 및 `begin->second`를 각각 **K** 및 **V**로 변환할 수 있어야 합니다.|

## <a name="vector-view-parameters"></a>벡터 보기 매개 변수

**winrt::param::vector_view\<T\>** 및 **winrt::param::async_vector_view\<T\>** 를 사용하면 **IVectorView\<T\>** 를 사용하는 API로 간단하게 매개 변수를 전달할 수 있습니다.

[**IVector\<T\>.GetView**](/uwp/api/windows.foundation.collections.ivector-1.getview)를 사용하면 **IVector**에서 **IVectorView**를 가져올 수 있습니다.

|전달할 수 있는 형식|동기화|Async|참고|
|-|-|-|-|
| `nullptr` | 예 | 예 | 기본 메서드에서 `nullptr`을 지원하는지 확인해야 합니다.|
| **IVectorView\<T\>** | 예 | 예 | 또는 모든 항목을 변환할 수 있습니다.|
| **std::vector\<T\>const&** | 예 | 아니요 ||
| **std::vector\<T\>&&** | 예 | 예 | 변경되지 않도록 콘텐츠가 보기로 이동됩니다.|
| **std::initializer_list\<T\>** | 예 | 예 | 형식은 정확하게 일치해야 합니다. 비동기 버전은 항목을 복사합니다.|
| `{ ForwardIt begin, ForwardIt end }` | 예 | 아니요 | `*begin`을 **T**로 변환할 수 있어야 합니다.|

이중 반복자 버전은 직접 전달하기 위한 요구 사항에 맞지 않는 벡터 보기를 만드는 데 사용할 수 있습니다. 하지만 벡터는 직접 액세스를 지원하므로 `RandomAcessIter`를 전달하는 것이 좋습니다.

## <a name="map-view-parameters"></a>맵 보기 매개 변수

**winrt::param::map_view\<T\>** 및 **winrt::param::async_map_view\<T\>** 를 사용하면 **IMapView\<T\>** 를 사용하는 API로 간단하게 매개 변수를 전달할 수 있습니다.

**IMap::GetView**를 사용하면 **IMap**에서 **IMapView**를 가져올 수 있습니다.

|전달할 수 있는 형식|동기화|Async|참고|
|-|-|-|-|
| `nullptr` | 예 | 예 | 기본 메서드에서 `nullptr`을 지원하는지 확인해야 합니다.|
| **IMapView\<K, V\>** | 예 | 예 | 또는 모든 항목을 변환할 수 있습니다.|
| **std::map\<K, V\> const&** | 예 | 아니요 ||
| **std::map\<K, V\>&&** | 예 | 예 | 변경되지 않도록 콘텐츠가 보기로 이동됩니다.|
| **std::unordered_map\<K, V\> const&**  | 예 | 아니요 ||
| **std::unordered_map\<K, V\>&&** | 예 | 예 | 변경되지 않도록 콘텐츠가 보기로 이동됩니다.|
| **std::initializer_list\<std::pair\<K, V\>\>** | 예 | 예 | 동기 및 비동기 버전에서 모두 항목을 복사합니다. 키를 중복할 수 없습니다.|

## <a name="vector-parameters"></a>벡터 매개 변수

**winrt::param::vector\<T\>** 를 사용하면 **IVector\<T\>** 를 사용하는 API로 간단하게 매개 변수를 전달할 수 있습니다.

|전달할 수 있는 형식|참고|
|-|-|
| `nullptr` | 기본 메서드에서 `nullptr`을 지원하는지 확인해야 합니다.|
| **IVector\<T\>** | 또는 모든 항목을 변환할 수 있습니다.|
| **std::vector\<T\>&&** | 변경되지 않도록 콘텐츠가 매개 변수로 이동됩니다. 결과가 다시 이동되지는 않습니다.|
| **std::initializer_list\<T\>** | 변경되지 않도록 콘텐츠가 매개 변수로 복사됩니다.|

메서드에서 벡터를 변경하는 경우 변경을 확인하는 유일한 방법은 **IVector**를 직접 전달하는 것입니다. **std::vector**를 전달하면 메서드에서 복사본을 변경하며 원본은 변경하지 않습니다.

## <a name="map-parameters"></a>맵 매개 변수

**winrt::param::map\<T\>** 을 사용하면 **IMap\<T\>** 을 사용하는 API로 간단하게 매개 변수를 전달할 수 있습니다.

|전달할 수 있는 형식|참고|
|-|-|
| `nullptr` | 기본 메서드에서 `nullptr`을 지원하는지 확인해야 합니다.|
| **IMap\<T\>** | 또는 모든 항목을 변환할 수 있습니다.|
| **std::map\<K, V\>&&** | 변경되지 않도록 콘텐츠가 매개 변수로 이동됩니다. 결과가 다시 이동되지는 않습니다.|
| **std::unordered_map\<K, V\>&&** | 변경되지 않도록 콘텐츠가 매개 변수로 이동됩니다. 결과가 다시 이동되지는 않습니다.|
| **std::initializer_list\<std::pair\<K, V\>\>** | 변경되지 않도록 콘텐츠가 매개 변수로 복사됩니다.|

메서드에서 맵을 변경하는 경우 변경을 확인하는 유일한 방법은 **IMap**을 직접 전달하는 것입니다. **std::map** 또는 **std::unordered_map**을 전달하면 메서드에서 복사본을 변경하며 원본은 변경하지 않습니다.

## <a name="array-parameters"></a>배열 매개 변수

**winrt::array_view\<T\>** 는 **winrt::param** 네임스페이스에 없지만, C 스타일 배열인 매개 변수에 사용되며, 이 배열을 *준수 배열*이라고도 합니다.

|전달할 수 있는 형식|참고|
|-|-|
| `{}` | 빈 배열입니다.|
| **array** | C와 호환되는 배열(즉, `C array[N];`)이며 여기서 **C**는 **T** 및 `sizeof(C) == sizeof(T)`로 변환 가능합니다. |
| **std::array<C, N>** | **C**의 C++ **std::array**이며 여기서 **C**는 **T** 및 `sizeof(C) == sizeof(T)`로 변환 가능합니다. |
| **std::vector<C>** | **C**의 C++ **std::vector**이며 여기서 **C**는 **T** 및 `sizeof(C) == sizeof(T)`로 변환 가능합니다. |
| `{ T*, T* }` | 포인터의 쌍은 범위 [시작, 종료]를 나타냅니다.|
| **std::initializer_list\<T\>** ||

또한 [Windows 런타임 ABI 경계에서 C 스타일 배열을 전달하는 다양한 패턴](https://devblogs.microsoft.com/oldnewthing/20200205-00/?p=103398) 블로그 게시물도 참조하세요.
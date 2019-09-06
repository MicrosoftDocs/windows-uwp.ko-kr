---
title: Windows 런타임 형식의 .NET 매핑
description: 다음 표에서는 .NET에서 유니버설 Windows 플랫폼 (UWP) 형식과 .NET 형식 사이에서 수행 하는 매핑을 보여 줍니다.
ms.assetid: 5317D771-808D-4B97-8063-63492B23292F
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c035db58fc6aa484f9d47a9af61176a2b05d55ee
ms.sourcegitcommit: d38e2f31c47434cd6dbbf8fe8d01c20b98fabf02
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70393655"
---
# <a name="net-mappings-of-windows-runtime-types"></a>Windows 런타임 형식의 .NET 매핑

다음 표에서는 .NET에서 유니버설 Windows 플랫폼 (UWP) 형식과 .NET 형식 사이에서 수행 하는 매핑을 보여 줍니다. 관리 코드를 사용 하 여 작성 된 유니버설 Windows 앱에서 Visual Studio IntelliSense는 UWP 형식 대신 .NET 형식을 표시 합니다. 예를 들어 Windows 런타임 메서드가&lt;IVector string&gt;형식의 매개 변수를 사용 하는 경우 IntelliSense는 IList&lt;문자열&gt;형식의 매개 변수를 표시 합니다. 마찬가지로, 관리 코드를 사용 하 여 작성 된 Windows 런타임 구성 요소에서는 멤버 시그니처에서 .NET 형식을 사용 합니다. [Windows 런타임 Metadata Export Tool (Winmdexp)](/dotnet/framework/tools/winmdexp-exe-windows-runtime-metadata-export-tool) 에서 Windows 런타임 구성 요소를 생성할 때 .net 형식은 해당 UWP 형식으로 변환 됩니다.

UWP 및 .NET에서 네임 스페이스 이름 및 형식 이름이 동일한 대부분의 형식은 구조체 (또는 열거형과 같은 구조체와 연결 된 형식)입니다. UWP에서 구조체에는 필드 이외의 멤버가 없으며 .NET에서 숨기는 도우미 형식이 필요 합니다. 이러한 구조체의 .NET 버전에는 숨겨진 도우미 형식의 기능을 제공 하는 속성과 메서드가 있습니다.

## <a name="uwp-types-that-map-to-net-types-with-the-same-name-and-namespace"></a>동일한 이름 및 네임 스페이스를 사용 하 여 .NET 형식에 매핑되는 UWP 형식

### <a name="in-net-assembly-systemobjectmodeldll"></a>.NET 어셈블리의 경우 ObjectModel .dll

| Namespace | 형식 |
|-|-|
| Windows.UI.Xaml.Input | ICommand |

### <a name="in-net-assembly-systemruntimewindowsruntimedll"></a>.NET 어셈블리 WindowsRuntime에서

| Namespace | 형식 |
|-|-|
| Windows.Foundation | 점 |
| Windows.Foundation | Rect |
| Windows.Foundation | Size |
| Windows.UI | 색 |

### <a name="in-net-assembly-systemruntimewindowsruntimeuixamldll"></a>.NET 어셈블리 WindowsRuntime에서. x m l.

| Namespace | 형식 |
|-|-|
| Windows.UI.Xaml | CornerRadius |
| Windows.UI.Xaml | Duration |
| Windows.UI.Xaml | DurationType |
| Windows.UI.Xaml | GridLength |
| Windows.UI.Xaml | GridUnitType |
| Windows.UI.Xaml | Thickness |
| Windows.UI.Xaml.Controls.Primitives | GeneratorPosition |
| Windows.UI.Xaml.Media | 행렬 |
| Windows.UI.Xaml.Media.Animation | KeyTime |
| Windows.UI.Xaml.Media.Animation | RepeatBehavior |
| Windows.UI.Xaml.Media.Animation | RepeatBehaviorType |
| Windows.UI.Xaml.Media.Media3D | Matrix3D |

## <a name="uwp-types-that-map-to-net-types-with-a-different-name-andor-namespace"></a>다른 이름 및/또는 네임 스페이스를 사용 하 여 .NET 형식에 매핑되는 UWP 형식

### <a name="in-net-assembly-systemobjectmodeldll"></a>.NET 어셈블리의 경우 ObjectModel .dll

| UWP 형식/네임스페이스 | .NET 형식/네임 스페이스 |
|-|-|
| INotifyCollectionChanged (Windows.UI.Xaml.Interop) | INotifyCollectionChanged (System.Collections.Specialized) | 
| NotifyCollectionChangedEventHandler (Windows.UI.Xaml.Interop) | NotifyCollectionChangedEventHandler (System.Collections.Specialized) | 
| NotifyCollectionChangedEventArgs (Windows.UI.Xaml.Interop) | NotifyCollectionChangedEventArgs (System.Collections.Specialized) | 
| NotifyCollectionChangedAction (Windows.UI.Xaml.Interop) | NotifyCollectionChangedAction (System.Collections.Specialized) | 
| INotifyPropertyChanged (Windows.UI.Xaml.Data) | INotifyPropertyChanged (System.ComponentModel) | 
| PropertyChangedEventHandler (Windows.UI.Xaml.Data) | PropertyChangedEventHandler (System.ComponentModel) | 
| PropertyChangedEventArgs (Windows.UI.Xaml.Data) | PropertyChangedEventArgs (System.ComponentModel) | 

### <a name="in-net-assembly-systemruntimedll"></a>.NET 어셈블리의 경우. Runtime .dll

| UWP 형식/네임스페이스 | .NET 형식/네임 스페이스 |
|-|-|
| AttributeUsageAttribute (Windows.Foundation.Metadata) | AttributeUsageAttribute (System) |
| AttributeTargets (Windows.Foundation.Metadata) | AttributeTargets (System) |
| DateTime (Windows.Foundation) | DateTimeOffset (System) |
| EventHandler&lt;T&gt;(Windows.Foundation) | EventHandler&lt;T&gt;(시스템) |
| HResult (Windows.Foundation) | Exception (System) |
| IReference&lt;T&gt;(Windows.Foundation) | Nullable&lt;T&gt;(시스템) |
| TimeSpan (Windows.Foundation) | TimeSpan (System) |
| Uri (Windows.Foundation) | Uri (System) |
| IClosable (Windows.Foundation) | IDisposable (System) |
| IIterable&lt;T&gt;(Windows.Foundation.Collections) | IEnumerable&lt;T&gt;(System.Collections.Generic) |
| IVector&lt;T&gt;(Windows.Foundation.Collections) | IList&lt;T&gt;(System.Collections.Generic) |
| IVectorView&lt;T&gt;(Windows.Foundation.Collections) | IReadOnlyList&lt;T&gt;(System.Collections.Generic) |
| IMap&lt;K,V&gt;(Windows.Foundation.Collections) | IDictionary&lt;TKey,TValue&gt;(System.Collections.Generic) |
| IMapView&lt;K,V&gt;(Windows.Foundation.Collections) | IReadOnlyDictionary&lt;TKey,TValue&gt;(System.Collections.Generic) |
| IKeyValuePair&lt;K,V&gt;(Windows.Foundation.Collections) | KeyValuePair&lt;TKey,TValue&gt;(System.Collections.Generic) |
| IBindableIterable (Windows.UI.Xaml.Interop) | IEnumerable (System.Collections) |
| IBindableVector (Windows.UI.Xaml.Interop) | IList (System.Collections) |
| TypeName (Windows.UI.Xaml.Interop) | Type (System) |

### <a name="in-net-assembly-systemruntimeinteropserviceswindowsruntimedll"></a>.NET 어셈블리 T e m. WindowsRuntime.

| UWP 형식/네임스페이스 | .NET 형식/네임 스페이스 |
|-|-|
| EventRegistrationToken (Windows.Foundation) | EventRegistrationToken (System.Runtime.InteropServices.WindowsRuntime) |

## <a name="related-topics"></a>관련 항목

* [및를 사용 C# 하 여 구성 요소 Windows 런타임 Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md)
---
title: Windows 런타임 형식의 .NET Framework 매핑
description: 다음 표에는 UWP(유니버설 Windows 플랫폼) 형식과 .NET Framework 형식 간에 .NET Framework가 수행하는 매핑이 나열되어 있습니다.
ms.assetid: 5317D771-808D-4B97-8063-63492B23292F
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ef98f3f4a9d20e836d5f9bddbc111a232f864bf5
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8885242"
---
# <a name="net-framework-mappings-of-windows-runtime-types"></a>Windows 런타임 형식의 .NET Framework 매핑



다음 표에 UWP(유니버설 Windows 플랫폼) 형식과 .NET Framework 형식 사이에서 .NET Framework가 만드는 매핑을 나열합니다. 관리 코드로 작성된 유니버설 Windows 앱에서 IntelliSense는 UWP 형식 대신 .NET Framework 형식을 보여 줍니다. 예를 들어 Windows 런타임 메서드가 IVector&lt;string&gt; 형식의 매개 변수를 사용하는 경우 IntelliSense가 IList&lt;string&gt; 형식의 매개 변수를 표시합니다. 마찬가지로 관리 코드로 작성한 Windows 런타임 구성 요소에서 멤버 서명에 .NET Framework 형식을 사용합니다. [Windows 런타임 메타데이터 내보내기 도구(Winmdexp.exe)](https://msdn.microsoft.com/library/hh925576.aspx)가 Windows 런타임 구성 요소를 생성할 때 .NET Framework 형식은 해당하는 UWP 형식이 됩니다.

## <a name="mapping-tables"></a>매핑 표


UWP와 .NET Framework에서 네임스페이스 이름과 형식 이름이 동일한 대부분의 형식은 구조(또는 열거 등의 구조와 관련된 형식)입니다. UWP에서 구조에는 필드 외에 다른 멤버가 없기 때문에 .NET Framework가 숨기고 있는 도우미 형식이 필요합니다. 이러한 구조의 .NET Framework 버전에는 숨겨진 도우미 형식의 기능을 제공하는 속성 및 메서드가 있습니다.

표 1: 다른 이름 및/또는 네임스페이스를 사용하여 .NET Framework 형식에 매핑하는 UWP 형식

| UWP 형식/네임스페이스                                            | .NET Framework 형식/네임스페이스                                          | .NET Framework 어셈블리                           |
|---------------------------------------------------------------|------------------------------------------------------------------------|---------------------------------------------------|
| AttributeUsageAttribute (Windows.Foundation.Metadata)         | AttributeUsageAttribute (System)                                       | System.Runtime.dll                                |
| AttributeTargets (Windows.Foundation.Metadata)                | AttributeTargets (System)                                              | System.Runtime.dll                                |
| DateTime (Windows.Foundation)                                 | DateTimeOffset (System)                                                | System.Runtime.dll                                |
| EventHandler&lt;T&gt;(Windows.Foundation)                    | EventHandler&lt;T&gt;(시스템)                                         | System.Runtime.dll                                |
| EventRegistrationToken (Windows.Foundation)                   | EventRegistrationToken (System.Runtime.InteropServices.WindowsRuntime) | System.Runtime.InteropServices.WindowsRuntime.dll |
| HResult (Windows.Foundation)                                  | Exception (System)                                                     | System.Runtime.dll                                |
| IReference&lt;T&gt;(Windows.Foundation)                      | Nullable&lt;T&gt;(시스템)                                             | System.Runtime.dll                                |
| TimeSpan (Windows.Foundation)                                 | TimeSpan (System)                                                      | System.Runtime.dll                                |
| Uri (Windows.Foundation)                                      | Uri (System)                                                           | System.Runtime.dll                                |
| IClosable (Windows.Foundation)                                | IDisposable (System)                                                   | System.Runtime.dll                                |
| IIterable&lt;T&gt;(Windows.Foundation.Collections)           | IEnumerable&lt;T&gt;(System.Collections.Generic)                      | System.Runtime.dll                                |
| IVector&lt;T&gt;(Windows.Foundation.Collections)             | IList&lt;T&gt;(System.Collections.Generic)                            | System.Runtime.dll                                |
| IVectorView&lt;T&gt;(Windows.Foundation.Collections)         | IReadOnlyList&lt;T&gt;(System.Collections.Generic)                    | System.Runtime.dll                                |
| IMap&lt;K,V&gt;(Windows.Foundation.Collections)              | IDictionary&lt;TKey,TValue&gt;(System.Collections.Generic)            | System.Runtime.dll                                |
| IMapView&lt;K,V&gt;(Windows.Foundation.Collections)          | IReadOnlyDictionary&lt;TKey,TValue&gt;(System.Collections.Generic)    | System.Runtime.dll                                |
| IKeyValuePair&lt;K,V&gt;(Windows.Foundation.Collections)     | KeyValuePair&lt;TKey,TValue&gt;(System.Collections.Generic)           | System.Runtime.dll                                |
| IBindableIterable (Windows.UI.Xaml.Interop)                   | IEnumerable (System.Collections)                                       | System.Runtime.dll                                |
| IBindableVector (Windows.UI.Xaml.Interop)                     | IList (System.Collections)                                             | System.Runtime.dll                                |
| INotifyCollectionChanged (Windows.UI.Xaml.Interop)            | INotifyCollectionChanged (System.Collections.Specialized)              | System.ObjectModel.dll                            |
| NotifyCollectionChangedEventHandler (Windows.UI.Xaml.Interop) | NotifyCollectionChangedEventHandler (System.Collections.Specialized)   | System.ObjectModel.dll                            |
| NotifyCollectionChangedEventArgs (Windows.UI.Xaml.Interop)    | NotifyCollectionChangedEventArgs (System.Collections.Specialized)      | System.ObjectModel.dll                            |
| NotifyCollectionChangedAction (Windows.UI.Xaml.Interop)       | NotifyCollectionChangedAction (System.Collections.Specialized)         | System.ObjectModel.dll                            |
| INotifyPropertyChanged (Windows.UI.Xaml.Data)                 | INotifyPropertyChanged (System.ComponentModel)                         | System.ObjectModel.dll                            |
| PropertyChangedEventHandler (Windows.UI.Xaml.Data)            | PropertyChangedEventHandler (System.ComponentModel)                    | System.ObjectModel.dll                            |
| PropertyChangedEventArgs (Windows.UI.Xaml.Data)               | PropertyChangedEventArgs (System.ComponentModel)                       | System.ObjectModel.dll                            |
| TypeName (Windows.UI.Xaml.Interop)                            | Type (System)                                                          | System.Runtime.dll                                |

 

표 2: 동일한 이름 및 네임스페이스를 사용하여 .NET Framework 형식에 매핑하는 UWP 형식

| 네임스페이스                           | 형식               | .NET Framework 어셈블리                   |
|-------------------------------------|--------------------|-------------------------------------------|
| Windows.UI                          | Color              | System.Runtime.WindowsRuntime.dll         |
| Windows.Foundation                  | Point              | System.Runtime.WindowsRuntime.dll         |
| Windows.Foundation                  | Rect               | System.Runtime.WindowsRuntime.dll         |
| Windows.Foundation                  | Size               | System.Runtime.WindowsRuntime.dll         |
| Windows.UI.Xaml.Input               | ICommand           | System.ObjectModel.dll                    |
| Windows.UI.Xaml                     | CornerRadius       | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml                     | Duration           | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml                     | DurationType       | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml                     | GridLength         | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml                     | GridUnitType       | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml                     | Thickness          | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml.Controls.Primitives | GeneratorPosition  | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml.Media               | Matrix             | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml.Media.Animation     | KeyTime            | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml.Media.Animation     | RepeatBehavior     | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml.Media.Animation     | RepeatBehaviorType | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml.Media.Media3D       | Matrix3D           | System.Runtime.WindowsRuntime.UI.Xaml.dll |

 

## <a name="related-topics"></a>관련 항목

* [C# 및 Visual Basic에서 Windows 런타임 구성 요소 만들기](creating-windows-runtime-components-in-csharp-and-visual-basic.md)

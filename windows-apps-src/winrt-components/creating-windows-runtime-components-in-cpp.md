---
title: C++/CX가 포함된 Windows 런타임 구성 요소
description: 이 항목에서는 C++/CX를 사용하여 Windows 런타임 언어를 통해 빌드된 유니버설 Windows 앱에서 호출할 수 있는 구성 요소인 Windows 런타임 구성 요소&mdash;를 만드는 방법을 보여줍니다.
ms.assetid: F7E06AA2-DCEC-427E-BD5D-9CA2A0ED2612
ms.date: 05/14/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9cda36c6027ae74df9beb5d1de68f69f273dc5f0
ms.sourcegitcommit: 4cafc1c55511741dd1e5bfe4496d9950a9b4de1b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/04/2021
ms.locfileid: "97860106"
---
# <a name="windows-runtime-components-with-ccx"></a>C++/CX가 포함된 Windows 런타임 구성 요소

> [!NOTE]
> 이 항목은 C++/CX 애플리케이션 유지에 도움을 주기 위해 작성되었습니다. 하지만 새로운 응용 프로그램에 대해 [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md)를 사용하는 것이 좋습니다. C++/WinRT는 헤더 파일 기반 라이브러리로 구현된 WinRT(Windows 런타임) API용 최신의 완전한 표준 C++17 언어 프로젝션이며, 최신 Windows API에 최고 수준의 액세스를 제공하도록 설계되었습니다. C++/WinRT를 사용하여 Windows 런타임 구성 요소를 만드는 방법에 자세한 내용은 [C++/WinRT를 사용한 Windows 런타임 구성 요소](./create-a-windows-runtime-component-in-cppwinrt.md)를 참조하세요.

이 항목에서는 c + +/CX를 사용 하 여 &mdash; Windows 런타임 언어 (c #, Visual Basic, c + + 또는 Javascript)를 사용 하 여 빌드된 유니버설 Windows 앱에서 호출할 수 있는 구성 요소를 Windows 런타임 만드는 방법을 보여 줍니다.

C + +에서 Windows 런타임 구성 요소를 빌드하는 데는 몇 가지 이유가 있습니다.
- 복잡하거나 많은 계산이 필요한 작업에서 C++의 성능 이점을 얻을 수 있습니다.
- 이미 작성되고 테스트된 코드를 다시 사용할 수 있습니다.

JavaScript 또는 .NET 프로젝트와 Windows 런타임 구성 요소 프로젝트가 포함된 솔루션을 빌드할 때, JavaScript 프로젝트 파일 및 컴파일된 DLL이 하나의 패키지로 병합되어 시뮬레이터에서 로컬로 또는 테더링된 디바이스에서 원격으로 디버그할 수 있습니다. 또한 구성 요소 프로젝트만 확장 SDK로 배포할 수 있습니다. 자세한 내용은 [소프트웨어 개발 키트 만들기](/visualstudio/extensibility/creating-a-software-development-kit)를 참조하세요.

일반적으로 c + +/CX 구성 요소를 코딩할 때 다른 winmd 패키지의 코드에서 데이터를 전달 하는 ABI (추상 이진 인터페이스) 경계를 제외 하 고 일반 c + + 라이브러리 및 기본 제공 형식을 사용 합니다. 여기에는 Windows 런타임 형식 및 c + +/CX에서 이러한 형식을 만들고 조작 하는 데 지원 되는 특수 구문이 사용 됩니다. 또한 c + +/CX 코드에서 대리자 및 이벤트와 같은 형식을 사용 하 여 구성 요소에서 발생 하 고 JavaScript, Visual Basic, c + + 또는 c #에서 처리 될 수 있는 이벤트를 구현 합니다. C + +/CX 구문에 대 한 자세한 내용은 [Visual C++ 언어 참조 (c + +/cx)](/cpp/cppcx/visual-c-language-reference-c-cx)를 참조 하세요.

## <a name="casing-and-naming-rules"></a>대/소문자 표기 및 명명 규칙

### <a name="javascript"></a>JavaScript
JavaScript는 대/소문자를 구분합니다. 따라서 이러한 대/소문자 표기 규칙을 따라야 합니다.

-   C++ 네임스페이스와 클래스를 참조하는 경우 C++ 쪽에서 사용한 동일한 대/소문자 표기를 사용합니다.
-   메서드를 호출할 때, 메서드 이름이 C++ 쪽에서 대문자로 표시되는 경우에도 카멜식 대/소문자 표기법을 사용합니다. 예를 들어 C++ 메서드 GetDate()는 JavaScript에서 getDate()로 호출해야 합니다.
-   활성화 가능한 클래스 이름 및 네임스페이스 이름에는 유니코드 문자를 포함할 수 없습니다.

### <a name="net"></a>.NET
.NET 언어는 일반 대/소문자 표기 규칙을 따릅니다.

## <a name="instantiating-the-object"></a>개체 인스턴스화
Windows 런타임 형식만 ABI 경계를 통해 전달할 수 있습니다. 구성 요소에 공용 메서드의 매개 변수나 반환 형식으로 std::wstring과 같은 형식이 있으면 컴파일러가 오류를 발생시킵니다. Visual C++ 구성 요소 확장(C++/CX) 제공 형식에는 int 및 double과 같은 일반적인 스칼라와 해당 typedef인 int32, float64 등이 포함됩니다. 자세한 내용은 [형식 시스템(C++/CX)](/cpp/cppcx/type-system-c-cx)을 참조하세요.

```cpp
// ref class definition in C++
public ref class SampleRefClass sealed
{
    // Class members...

    // #include <valarray>
public:
    double LogCalc(double input)
    {
        // Use C++ standard library as usual.
        return std::log(input);
    }

};
```

```javascript
//Instantiation in JavaScript (requires "Add reference > Project reference")
var nativeObject = new CppComponent.SampleRefClass();
```

```csharp
//Call a method and display result in a XAML TextBlock
var num = nativeObject.LogCalc(21.5);
ResultText.Text = num.ToString();
```

## <a name="ccx-built-in-types-library-types-and-windows-runtime-types"></a>C + +/CX 기본 제공 형식, 라이브러리 형식 및 Windows 런타임 형식
활성화 가능한 클래스(ref 클래스라고도 함)는 JavaScript, C# 또는 Visual Basic과 같은 다른 언어에서 인스턴스화할 수 있습니다. 다른 언어에서 사용할 수 있으려면 구성 요소에 하나 이상의 활성화 가능한 클래스가 포함되어야 합니다.

Windows 런타임 구성 요소에는 여러 활성화 가능한 공용 클래스와 구성 요소에 내부적으로만 알려진 추가 클래스가 포함될 수 있습니다. JavaScript에 표시 되지 않는 C++/cx 형식에 [WebHostHidden](/uwp/api/windows.foundation.metadata.webhosthiddenattribute) 특성을 적용합니다.

모든 공용 클래스는 구성 요소 메타데이터 파일과 같은 이름을 가진 같은 루트 네임스페이스에 있어야 합니다. 예를 들어 이름이 A.B.C.MyClass인 클래스는 이름이 A.winmd, A.B.winmd 또는 A.B.C.winmd인 메타데이터 파일에 정의된 경우에만 인스턴스화될 수 있습니다. DLL의 이름은 .winmd 파일 이름과 일치하지 않아도 됩니다.

클라이언트 코드는 모든 클래스에 대해서와 마찬가지로 **new**(Visual Basic에서는 **New**) 키워드를 사용하여 구성 요소의 인스턴스를 만듭니다.

활성화 가능한 클래스는 **public ref class sealed** 로 선언해야 합니다. **ref class** 키워드가 컴파일러에게 Windows 런타임 호환 형식으로 클래스를 만들도록 지시하면 봉인된 키워드는 클래스가 상속될 수 없다고 지정합니다. Windows 런타임은 현재 유니버설 상속 모델을 지원하지 않고, 제한된 상속 모델이 사용자 지정 XAML 컨트롤의 생성을 지원합니다. 자세한 내용은 [Ref 클래스 및 구조(C++/CX)](/cpp/cppcx/ref-classes-and-structs-c-cx)를 참조하세요.

C + +/CX의 경우 모든 숫자 기본 형식은 기본 네임 스페이스에 정의 됩니다. [Platform](/cpp/cppcx/platform-namespace-c-cx) 네임 스페이스에는 Windows 런타임 형식 시스템에만 적용 되는 c + +/cx 클래스가 포함 되어 있습니다. 여기에는 [Platform::String](/cpp/cppcx/platform-string-class) 클래스 및 [Platform::Object](/cpp/cppcx/platform-object-class) 클래스가 포함됩니다. [Platform::Collections::Map](/cpp/cppcx/platform-collections-map-class) 클래스 및 [Platform::Collections::Vector](/cpp/cppcx/platform-collections-vector-class) 클래스와 같은 구체적인 컬렉션 형식은 [Platform::Collections](/cpp/cppcx/platform-collections-namespace) 네임스페이스에서 정의됩니다. 이러한 형식이 구현하는 공용 인터페이스는 [Windows::Foundation::Collections 네임스페이스(C++/CX)](/cpp/cppcx/windows-foundation-collections-namespace-c-cx)에서 정의됩니다. 이러한 인터페이스 형식이 JavaScript, C# 및 Visual Basic에서 사용하는 형식입니다. 자세한 내용은 [형식 시스템(C++/CX)](/cpp/cppcx/type-system-c-cx)을 참조하세요.

## <a name="method-that-returns-a-value-of-built-in-type"></a>기본 제공 형식의 값을 반환하는 메서드
```cpp
    // #include <valarray>
public:
    double LogCalc(double input)
    {
        // Use C++ standard library as usual.
        return std::log(input);
    }
```

```javascript
//Call a method
var nativeObject = new CppComponent.SampleRefClass;
var num = nativeObject.logCalc(21.5);
document.getElementById('P2').innerHTML = num;
```

## <a name="method-that-returns-a-custom-value-struct"></a>사용자 지정 값 구조를 반환하는 메서드
```cpp
namespace CppComponent
{
    // Custom struct
    public value struct PlayerData
    {
        Platform::String^ Name;
        int Number;
        double ScoringAverage;
    };

    public ref class Player sealed
    {
    private:
        PlayerData m_player;
    public:
        property PlayerData PlayerStats
        {
            PlayerData get(){ return m_player; }
            void set(PlayerData data) {m_player = data;}
        }
    };
}
```

ABI를 통해 사용자 정의 값 구조체를 전달 하려면 c + +/CX에 정의 된 값 struct와 동일한 멤버를 포함 하는 JavaScript 개체를 정의 합니다. 그런 다음 해당 개체를 c + +/CX 메서드에 인수로 전달 하 여 개체를 c + +/CX 형식으로 암시적으로 변환할 수 있습니다.

```javascript
// Get and set the value struct
function GetAndSetPlayerData() {
    // Create an object to pass to C++
    var myData =
        { name: "Bob Homer", number: 12, scoringAverage: .357 };
    var nativeObject = new CppComponent.Player();
    nativeObject.playerStats = myData;

    // Retrieve C++ value struct into new JavaScript object
    var myData2 = nativeObject.playerStats;
    document.getElementById('P3').innerHTML = myData.name + " , " + myData.number + " , " + myData.scoringAverage.toPrecision(3);
}
```

다른 접근 방식은 IPropertySet(표시되지 않음)를 구현하는 클래스를 정의하는 것입니다.

.NET 언어에서는 c + +/CX 구성 요소에 정의 된 형식의 변수를 만듭니다.

```csharp
private void GetAndSetPlayerData()
{
    // Create a ref class
    var player = new CppComponent.Player();

    // Create a variable of a value struct
    // type that is defined in C++
    CppComponent.PlayerData myPlayer;
    myPlayer.Name = "Babe Ruth";
    myPlayer.Number = 12;
    myPlayer.ScoringAverage = .398;

    // Set the property
    player.PlayerStats = myPlayer;

    // Get the property and store it in a new variable
    CppComponent.PlayerData myPlayer2 = player.PlayerStats;
    ResultText.Text += myPlayer.Name + " , " + myPlayer.Number.ToString() +
        " , " + myPlayer.ScoringAverage.ToString();
}
```

## <a name="overloaded-methods"></a>오버로드된 메서드
C + +/CX public ref 클래스는 오버 로드 된 메서드를 포함할 수 있지만 JavaScript는 오버 로드 된 메서드를 구분 하는 기능을 제한 합니다. 예를 들어 다음 서명 간의 차이는 구분할 수 있습니다.

```cpp
public ref class NumberClass sealed
{
public:
    int GetNumber(int i);
    int GetNumber(int i, Platform::String^ str);
    double GetNumber(int i, MyData^ d);
};
```

하지만 다음과 같은 차이점을 알 수는 없습니다.

```cpp
int GetNumber(int i);
double GetNumber(double d);
```

모호한 경우에는 JavaScript가 [Windows::Foundation::Metadata::DefaultOverload](/uwp/api/windows.foundation.metadata.defaultoverloadattribute) 특성을 헤더 파일의 메서드 서명에 적용하여 항상 특정 오버로드를 호출합니다.

이 JavaScript는 항상 특성이 지정된 오버로드를 호출합니다.

```javascript
var nativeObject = new CppComponent.NumberClass();
var num = nativeObject.getNumber(9);
document.getElementById('P4').innerHTML = num;
```

## <a name="net"></a>.NET
.NET 언어는 .NET 클래스에서와 마찬가지로 c + +/CX ref 클래스의 오버 로드를 인식 합니다.

## <a name="datetime"></a>DateTime
Windows 런타임에서 [Windows::Foundation::DateTime](/uwp/api/windows.foundation.datetime) 개체는 단지 1601년 1월 1일 전 또는 후의 100나노초 간격 수를 나타내는 64비트의 부호 있는 정수입니다. Windows:Foundation::DateTime 개체에는 메서드가 없습니다. 대신 각 언어는 해당 언어에 대 한 기본 형식으로 DateTime을 프로젝션 합니다. JavaScript의 Date 개체와 .NET의 system.string 및 system.string 형식입니다.

```cpp
public  ref class MyDateClass sealed
{
public:
    property Windows::Foundation::DateTime TimeStamp;
    void SetTime(Windows::Foundation::DateTime dt)
    {
        auto cal = ref new Windows::Globalization::Calendar();
        cal->SetDateTime(dt);
        TimeStamp = cal->GetDateTime(); // or TimeStamp = dt;
    }
};
```

C + +/CX에서 JavaScript로 DateTime 값을 전달 하는 경우 JavaScript는 해당 값을 날짜 개체로 받아들이고 기본적으로 긴 형식 날짜 문자열로 표시 합니다.

```javascript
function SetAndGetDate() {
    var nativeObject = new CppComponent.MyDateClass();

    var myDate = new Date(1956, 4, 21);
    nativeObject.setTime(myDate);

    var myDate2 = nativeObject.timeStamp;

    //prints long form date string
    document.getElementById('P5').innerHTML = myDate2;

}
```

.NET 언어에서 c + +/CX 구성 요소에 시스템. DateTime을 전달 하는 경우 메서드는이를 Windows:: Foundation::D ateTime으로 받아들입니다. 구성 요소가 Windows:: Foundation::D ateTime을 .NET 메서드에 전달할 때 프레임 워크 메서드는이를 DateTimeOffset으로 받아들입니다.

```csharp
private void DateTimeExample()
{
    // Pass a System.DateTime to a C++ method
    // that takes a Windows::Foundation::DateTime
    DateTime dt = DateTime.Now;
    var nativeObject = new CppComponent.MyDateClass();
    nativeObject.SetTime(dt);

    // Retrieve a Windows::Foundation::DateTime as a
    // System.DateTimeOffset
    DateTimeOffset myDate = nativeObject.TimeStamp;

    // Print the long-form date string
    ResultText.Text += myDate.ToString();
}
```

## <a name="collections-and-arrays"></a>컬렉션 및 배열
컬렉션은 항상 ABI 경계 전반에서 Windows::Foundation::Collections::IVector^ 및 Windows::Foundation::Collections::IMap^과 같은 Windows 런타임 형식에 핸들로 전달됩니다. 예를 들어 핸들을 Platform::Collections::Map으로 반환하는 경우 이것이 Windows::Foundation::Collections::IMap^으로 암시적으로 변환됩니다. 컬렉션 인터페이스는 구체적 구현을 제공 하는 c + +/CX 클래스와는 별도의 네임 스페이스에 정의 됩니다. JavaScript와 .NET 언어는 인터페이스를 사용합니다. 자세한 내용은 [컬렉션(C++/CX)](/cpp/cppcx/collections-c-cx) 및 [배열과 WriteOnlyArray(C++/CX)](/cpp/cppcx/array-and-writeonlyarray-c-cx)를 참조하세요.

## <a name="passing-ivector"></a>IVector 전달
```cpp
// Windows::Foundation::Collections::IVector across the ABI.
//#include <algorithm>
//#include <collection.h>
Windows::Foundation::Collections::IVector<int>^ SortVector(Windows::Foundation::Collections::IVector<int>^ vec)
{
    std::sort(begin(vec), end(vec));
    return vec;
}
```

```javascript
var nativeObject = new CppComponent.CollectionExample();
// Call the method to sort an integer array
var inVector = [14, 12, 45, 89, 23];
var outVector = nativeObject.sortVector(inVector);
var result = "Sorted vector to array:";
for (var i = 0; i < outVector.length; i++)
{
    outVector[i];
    result += outVector[i].toString() + ",";
}
document.getElementById('P6').innerHTML = result;
```

.NET 언어는 IVector&lt;T&gt;를 IList&lt;T&gt;로 표시합니다.

```csharp
private void SortListItems()
{
    IList<int> myList = new List<int>();
    myList.Add(5);
    myList.Add(9);
    myList.Add(17);
    myList.Add(2);

    var nativeObject = new CppComponent.CollectionExample();
    IList<int> mySortedList = nativeObject.SortVector(myList);

    foreach (var item in mySortedList)
    {
        ResultText.Text += " " + item.ToString();
    }
}
```

## <a name="passing-imap"></a>IMap 전달
```cpp
// #include <map>
//#include <collection.h>
Windows::Foundation::Collections::IMap<int, Platform::String^> ^GetMap(void)
{    
    Windows::Foundation::Collections::IMap<int, Platform::String^> ^ret =
        ref new Platform::Collections::Map<int, Platform::String^>;
    ret->Insert(1, "One ");
    ret->Insert(2, "Two ");
    ret->Insert(3, "Three ");
    ret->Insert(4, "Four ");
    ret->Insert(5, "Five ");
    return ret;
}
```

```javascript
// Call the method to get the map
var outputMap = nativeObject.getMap();
var mStr = "Map result:" + outputMap.lookup(1) + outputMap.lookup(2)
    + outputMap.lookup(3) + outputMap.lookup(4) + outputMap.lookup(5);
document.getElementById('P7').innerHTML = mStr;
```

.NET 언어는 IMap 및 IDictionary&lt;K, V&gt;를 표시합니다.

```csharp
private void GetDictionary()
{
    var nativeObject = new CppComponent.CollectionExample();
    IDictionary<int, string> d = nativeObject.GetMap();
    ResultText.Text += d[2].ToString();
}
```

## <a name="properties"></a>속성
C + +/CX 구성 요소 확장의 public ref 클래스는 property 키워드를 사용 하 여 public 데이터 멤버를 속성으로 노출 합니다. 개념은 .NET 속성과 동일 합니다. Trivial 속성은 그 기능이 암시적이므로 데이터 멤버와 비슷합니다. NSEOTrivial이 아닌 속성은 명시적인 get 및 set 접근자와 해당 값에 대한 "백업 저장소"인 명명된 프라이빗 변수가 있습니다. 이 예제에서 private 멤버 변수 \_ propertyavalue는 PropertyA에 대 한 백업 저장소입니다. 속성은 값이 변경될 때 이벤트를 발생시킬 수 있으며 클라이언트 앱은 해당 이벤트를 받도록 등록할 수 있습니다.

```cpp
//Properties
public delegate void PropertyChangedHandler(Platform::Object^ sender, int arg);
public ref class PropertyExample  sealed
{
public:
    PropertyExample(){}

    // Event that is fired when PropertyA changes
    event PropertyChangedHandler^ PropertyChangedEvent;

    // Property that has custom setter/getter
    property int PropertyA
    {
        int get() { return m_propertyAValue; }
        void set(int propertyAValue)
        {
            if (propertyAValue != m_propertyAValue)
            {
                m_propertyAValue = propertyAValue;
                // Fire event. (See event example below.)
                PropertyChangedEvent(this, propertyAValue);
            }
        }
    }

    // Trivial get/set property that has a compiler-generated backing store.
    property Platform::String^ PropertyB;

private:
    // Backing store for propertyA.
    int m_propertyAValue;
};
```

```javascript
var nativeObject = new CppComponent.PropertyExample();
var propValue = nativeObject.propertyA;
document.getElementById('P8').innerHTML = propValue;

//Set the string property
nativeObject.propertyB = "What is the meaning of the universe?";
document.getElementById('P9').innerHTML += nativeObject.propertyB;
```

.Net 언어는 .NET 개체에서와 동일한 방식으로 네이티브 c + +/CX 개체의 속성에 액세스 합니다.

```csharp
private void GetAProperty()
{
    // Get the value of the integer property
    // Instantiate the C++ object
    var obj = new CppComponent.PropertyExample();

    // Get an integer property
    var propValue = obj.PropertyA;
    ResultText.Text += propValue.ToString();

    // Set a string property
    obj.PropertyB = " What is the meaning of the universe?";
    ResultText.Text += obj.PropertyB;

}
```

## <a name="delegates-and-events"></a>대리자 및 이벤트
대리자는 함수 개체를 나타내는 Windows 런타임 형식입니다. 이벤트, 콜백 및 비동기 메서드 호출과 관련된 대리자를 사용하여 나중에 수행할 작업을 지정할 수 있습니다. 함수 개체와 마찬가지로 대리자는 컴파일러에서 함수의 매개 변수 형식 및 반환 형식을 확인할 수 있도록 함으로써 형식의 안전성을 제공합니다. 대리자의 선언은 함수 서명과 비슷하고 구현은 클래스 정의와 비슷하며 호출은 함수 호출과 비슷합니다.

## <a name="adding-an-event-listener"></a>이벤트 수신기 추가
이벤트 키워드를 사용하여 지정된 대리자 형식의 공용 멤버를 선언할 수 있습니다. 클라이언트 코드는 특정 언어로 제공되는 표준 메커니즘을 사용하여 이벤트를 구독합니다.

```cpp
public:
    event SomeHandler^ someEvent;
```

이 예제에서는 이전 속성 섹션과 동일한 C++ 코드를 사용합니다.

```javascript
function Button_Click() {
    var nativeObj = new CppComponent.PropertyExample();
    // Define an event handler method
    var singlecasthandler = function (ev) {
        document.getElementById('P10').innerHTML = "The button was clicked and the value is " + ev;
    };

    // Subscribe to the event
    nativeObj.onpropertychangedevent = singlecasthandler;

    // Set the value of the property and fire the event
    var propValue = 21;
    nativeObj.propertyA = 2 * propValue;

}
```

.NET 언어에서 c + + 구성 요소의 이벤트를 구독 하는 것은 .NET 클래스에서 이벤트를 구독 하는 것과 같습니다.

```csharp
//Subscribe to event and call method that causes it to be fired.
private void TestMethod()
{
    var objWithEvent = new CppComponent.PropertyExample();
    objWithEvent.PropertyChangedEvent += objWithEvent_PropertyChangedEvent;

    objWithEvent.PropertyA = 42;
}

//Event handler method
private void objWithEvent_PropertyChangedEvent(object __param0, int __param1)
{
    ResultText.Text = "the event was fired and the result is " +
         __param1.ToString();
}
```

## <a name="adding-multiple-event-listeners-for-one-event"></a>하나의 이벤트에 여러 이벤트 수신기 추가
JavaScript에는 여러 처리기가 단일 이벤트를 구독할 수 있는 addEventListener 메서드가 있습니다.

```cpp
public delegate void SomeHandler(Platform::String^ str);

public ref class LangSample sealed
{
public:
    event SomeHandler^ someEvent;
    property Platform::String^ PropertyA;

    // Method that fires an event
    void FireEvent(Platform::String^ str)
    {
        someEvent(Platform::String::Concat(str, PropertyA->ToString()));
    }
    //...
};
```

```javascript
// Add two event handlers
var multicast1 = function (ev) {
    document.getElementById('P11').innerHTML = "Handler 1: " + ev.target;
};
var multicast2 = function (ev) {
    document.getElementById('P12').innerHTML = "Handler 2: " + ev.target;
};

var nativeObject = new CppComponent.LangSample();
//Subscribe to the same event
nativeObject.addEventListener("someevent", multicast1);
nativeObject.addEventListener("someevent", multicast2);

nativeObject.propertyA = "42";

// This method should fire an event
nativeObject.fireEvent("The answer is ");
```

C#에서는 이전 예제에 표시된 대로 += 연산자를 사용하여 임의 개수의 이벤트 처리기가 이벤트를 구독할 수 있습니다.

## <a name="enums"></a>열거형
C + +/CX의 Windows 런타임 열거형은 public 클래스 열거를 사용 하 여 선언 됩니다. 표준 c + +의 범위가 지정 된 열거형과 유사 합니다.

```cpp
public enum class Direction {North, South, East, West};

public ref class EnumExampleClass sealed
{
public:
    property Direction CurrentDirection
    {
        Direction  get(){return m_direction; }
    }

private:
    Direction m_direction;
};
```

열거형 값은 c + +/CX와 JavaScript 간에 정수로 전달 됩니다. 필요에 따라 c + +/CX 열거형과 같은 명명 된 값을 포함 하는 JavaScript 개체를 선언 하 고 다음과 같이 사용할 수 있습니다.

```javascript
var Direction = { 0: "North", 1: "South", 2: "East", 3: "West" };
//. . .

var nativeObject = new CppComponent.EnumExampleClass();
var curDirection = nativeObject.currentDirection;
document.getElementById('P13').innerHTML =
Direction[curDirection];
```

C# 및 Visual Basic 둘 다 열거에 대해 언어를 지원합니다. 이러한 언어는 .NET 열거형이 표시 되는 것 처럼 c + + public enum 클래스를 참조 합니다.

## <a name="asynchronous-methods"></a>비동기 메서드
다른 Windows 런타임 개체에서 제공하는 비동기 메서드를 사용하려면 [작업 클래스(동시성 런타임)](/cpp/parallel/concrt/reference/task-class)를 사용합니다. 자세한 내용은 [작업 병렬 처리(동시성 런타임)](/cpp/parallel/concrt/task-parallelism-concurrency-runtime)를 참조하세요.

C + +/CX에서 비동기 메서드를 구현 하려면 ppltasks.h에 정의 된 [create \_ async](/cpp/parallel/concrt/reference/concurrency-namespace-functions?view=vs-2017&preserve-view=true) 함수를 사용 합니다. 자세한 내용은 [UWP 앱 용 c + +/cx에서 비동기 작업 만들기](/cpp/parallel/concrt/creating-asynchronous-operations-in-cpp-for-windows-store-apps)를 참조 하세요. 예제는 [c + +/cx Windows 런타임 구성 요소를 만들고 JavaScript 또는 c #에서 호출](walkthrough-creating-a-basic-windows-runtime-component-in-cpp-and-calling-it-from-javascript-or-csharp.md)하는 연습을 참조 하세요. .Net 언어는 .NET에 정의 된 비동기 메서드와 마찬가지로 c + +/CX 비동기 메서드를 사용 합니다.

## <a name="exceptions"></a>예외
Windows 런타임에서 정의된 모든 예외 형식을 throw할 수 있습니다. 일부 Windows 런타임 예외 형식에서는 사용자 지정 형식을 파생시킬 수 없습니다. 그러나 COMException을 throw하고 예외를 catch하는 코드에서 액세스할 수 있는 사용자 지정 HRESULT를 제공할 수 있습니다. COMException에서 사용자 지정 메시지를 지정할 수 있는 방법이 없습니다.

## <a name="debugging-tips"></a>디버깅 팁
구성 요소 DLL이 있는 JavaScript 솔루션을 디버그할 때, 디버거가 스크립트를 단계별로 또는 구성 요소의 네이티브 코드를 단계별로 실행하도록 설정할 수 있지만 동시에 둘 다 실행하도록 할 수 없습니다. 설정을 변경하려면 솔루션 탐색기에서 JavaScript 프로젝트 노드를 선택한 다음 속성, 디버깅, 디버거 형식을 선택합니다.

패키지 디자이너에서 적절한 기능을 선택해야 합니다. 예를 들어 Windows 런타임 API를 사용하여 사용자의 사진 라이브러리에서 이미지 파일을 열려고 하는 경우 매니페스트 디자이너의 기능 창에서 사진 라이브러리 확인란을 선택해야 합니다.

JavaScript 코드가 구성 요소의 public 속성 또는 메서드를 인식하지 못하는 경우 JavaScript에서 카멜식 대/소문자 표기를 사용하고 있는지 확인합니다. 예를 들어 LogCalc c + +/CX 메서드는 JavaScript에서 logCalc로 참조 되어야 합니다.

솔루션에서 c + +/CX Windows 런타임 구성 요소 프로젝트를 제거 하는 경우에는 JavaScript 프로젝트 에서도 프로젝트 참조를 수동으로 제거 해야 합니다. 이렇게 하지 않으면 이후 디버그 또는 빌드 작업을 수행할 수 없습니다. 필요한 경우 DLL에 대한 어셈블리 참조를 추가할 수 있습니다.

## <a name="related-topics"></a>관련 항목
* [C++/CX Windows 런타임 구성 요소를 만들고 JavaScript 또는 C#에서 호출하는 연습](walkthrough-creating-a-basic-windows-runtime-component-in-cpp-and-calling-it-from-javascript-or-csharp.md)

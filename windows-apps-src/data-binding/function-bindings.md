---
description: XBind 태그 확장 기능을 태그에서 사용할 수 있습니다.
title: X:bind 함수
ms.date: 04/26/2018
ms.topic: article
keywords: windows 10, uwp, xBind
ms.localizationpriority: medium
ms.openlocfilehash: 38573bf4602c88d2e04d4bf29b39191045eddec8
ms.sourcegitcommit: 58783d1ea22e632b9c50dcfbaa1cc57686bcdd8e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/22/2019
ms.locfileid: "9024220"
---
# <a name="functions-in-xbind"></a>X:bind 함수

**참고**바인딩 **{x: Bind}** 를 사용 하 여 (및는 비교 **{x: Bind}** **{Binding}** 사이 대 한) 앱에서 데이터를 사용 하는 방법에 대 한 일반 정보에 대 한 [데이터 바인딩 심층 분석을](https://msdn.microsoft.com/library/windows/apps/mt210946)참조 하세요.

Windows10 버전 1607부터 **{x:Bind}** 는 함수를 바인딩 경로의 리프 단계로 사용할 수 있습니다. 이 통해:

- 간단한 값 변환 방법
- 둘 이상의 매개 변수를 사용하는 바인딩 방식

> [!NOTE]
> **{x:Bind}** 와 함께 함수를 사용하려면 앱의 최소 대상 SDK 버전이 14393 이상이어야 합니다. 앱이 이전 버전의 Windows 10을 대상으로 하는 경우 함수를 사용할 수 없습니다. 대상 버전에 대한 자세한 내용은 [버전 적응 코드](https://msdn.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code)를 참조하세요.

다음 예제에서는 항목의 배경과 전경이 색 매개 변수에 따라 변환을 수행하는 함수에 바인딩됩니다.

```xaml
<DataTemplate x:DataType="local:ColorEntry">
    <Grid Background="{x:Bind local:ColorEntry.Brushify(Color), Mode=OneWay}" Width="240">
        <TextBlock Text="{x:Bind ColorName}" Foreground="{x:Bind TextColor(Color)}" Margin="10,5" />
    </Grid>
</DataTemplate>
```

```csharp
class ColorEntry
{
    public string ColorName { get; set; }
    public Color Color { get; set; }

    public static SolidColorBrush Brushify(Color c)
    {
        return new SolidColorBrush(c);
    }

    public SolidColorBrush TextColor(Color c)
    {
        return new SolidColorBrush(((c.R * 0.299 + c.G * 0.587 + c.B * 0.114) > 150) ? Colors.Black : Colors.White);
    }
}
```

## <a name="xaml-attribute-usage"></a>XAML 특성 사용

``` syntax
<object property="{x:Bind pathToFunction.FunctionName(functionParameter1, functionParameter2, ...), bindingProperties}" ... />
```

## <a name="path-to-the-function"></a>함수 경로

함수 경로는 다른 속성 경로처럼 지정되며 점(.), 인덱서 또는 함수를 찾을 캐스트를 포함할 수 있습니다.

정적 함수는 XMLNamespace:ClassName.MethodName 구문을 사용하여 지정할 수 있습니다. 예를 들어, 사용 하 여는 코드 숨김에 정적 함수에 대 한 바인딩 구문은 아래 합니다.

```xaml
<Page 
     xmlns:local="using:MyNamespace">
     ...
    <StackPanel>
        <TextBlock x:Name="BigTextBlock" FontSize="20" Text="Big text" />
        <TextBlock FontSize="{x:Bind local:MyHelpers.Half(BigTextBlock.FontSize)}" 
                   Text="Small text" />
    </StackPanel>
</Page>
```
```csharp
namespace MyNamespace
{
    static public class MyHelpers
    {
        public static double Half(double value) => value / 2.0;
    }
}
```

예를 들어 날짜 서식을, 텍스트, 텍스트 연결 등과 같은 간단한 시나리오를 수행 하기 위해 태그에서 직접 시스템 함수를 사용할 수 있습니다.
```xaml
<Page 
     xmlns:sys="using:System"
     xmlns:local="using:MyNamespace">
     ...
     <CalendarDatePicker Date="{x:Bind sys:DateTime.Parse(TextBlock1.Text)}" />
     <TextBlock Text="{x:Bind sys:String.Format('{0} is now available in {1}', local:MyPage.personName, local:MyPage.location)}" />
</Page>
```

모드가 OneWay/TwoWay인 경우 함수 경로에 수행된 변경 검색이 포함되고 이러한 개체가 변경된 경우 바인딩이 다시 평가됩니다.

바인딩할 함수는 다음을 수행해야 합니다.

- 코드 및 메타데이터에 액세스할 수 있어야 합니다. 그러므로 internal/private은 C#에서 작업하지만 C++/CX에는 public WinRT 메서드가 필요합니다.
- 오버로드는 형식이 아닌 인수 개수를 기반으로 하며 해당 개수의 인수를 사용하는 첫 번째 오버로드와 일치시키려고 합니다.
- 인수 형식은 전달 중인 데이터와 일치해야 합니다. 변환을 축소하지 않습니다.
- 함수의 반환 형식은 바인딩을 사용 중인 속성의 형식과 일치해야 합니다.

바인딩 엔진은 Windows 10의 다음 주요 업데이트부터 속성 변경 알림을 발생 하는 함수 이름에 반응 및 필요에 따라 바인딩이 다시 평가 합니다. 예를 들면 다음과 같습니다. 

```XAML
<DataTemplate x:DataType="local:Person">
   <StackPanel>
      <TextBlock Text="{x:Bind FullName}" />
      <Image Source="{x:Bind IconToBitmap(Icon, CancellationToken), Mode=OneWay}" />
   </StackPanel>
</DataTemplate>
```
```csharp
public class Person:INotifyPropertyChanged
{
    //Implementation for an Icon property and a CancellationToken property with PropertyChanged notifications
    ...

    //IconToBitmap function is essentially a multi binding converter between several options.
    public Uri IconToBitmap (Uri icon, Uri cancellationToken)
    {
        Uri foo = new Uri(...);        
        if (isCancelled)
        {
            foo = cancellationToken;
        }
        else 
        {
            if (this.fullName.Contains("Sr"))
            {
               //pass a different Uri back
               foo = new Uri(...);
            }
            else
            {
                foo = icon;
            }
        }
        return foo;
    }

    //Ensure FullName property handles change notification on itself as well as IconToBitmap since the function uses it
    public string FullName
    {
        get { return this.fullName; }
        set 
        {
            this.fullName = value;
            this.OnPropertyChanged ();
            this.OnPropertyChanged ("IconToBitmap"); 
            //this ensures Image.Source binding re-evaluates when FullName changes in addition to Icon and CancellationToken
        }
    }
}
```

> [!TIP]
> WPF의 다중 바인딩 및 변환기를 통해 지원 된 항목으로 동일한 시나리오를 달성 하기 위해 x:bind에서 함수를 사용할 수 있습니다.

## <a name="function-arguments"></a>함수 인수

함수 인수를 여러 개 지정할 경우 쉼표(,)를 사용하여 구분합니다.

- 바인딩 경로 – 해당 개체에 직접 바인딩할 때와 동일한 구문입니다.
  - 모드가 OneWay/TwoWay인 경우 변경 검색이 수행되고 개체가 변경될 때 바인딩이 다시 평가됩니다.
- 따옴표로 묶인 상수 문자열 – 문자열로 지정하려면 따옴표가 필요합니다. 문자열에서 따옴표를 이스케이프할 때는 캐럿(^)을 사용합니다.
- 상수 - 예를 들어 123.456입니다.
- 부울 – "x:True" 또는 "x:False"로 지정합니다.

### <a name="two-way-function-bindings"></a>양방향 함수 바인딩

양방향 바인딩 시나리오에서는 두 번째 함수를 바인딩의 반대 방향으로 지정해야 합니다. 이 작업은 수행 **하지** 바인딩 속성을 사용 하 여 합니다. 에 함수 아래 예에서는 값 모델로 다시 푸시 해야 하는 인수 하나를 수행 해야 합니다.
```xaml
<TextBlock Text="{x:Bind a.MyFunc(b), BindBack=a.MyFunc2, Mode=TwoWay}" />
```

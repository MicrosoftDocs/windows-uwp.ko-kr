---
Description: Windows 앱에서 양식에 대한 레이아웃 지침
title: 양식
template: detail.hbs
ms.date: 11/07/2017
ms.topic: article
keywords: windows 10, uwp, fluent
ms.openlocfilehash: 69ffaf4ff67d4ee78e78c195d759ae242a069e8e
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82968514"
---
# <a name="forms"></a>양식
양식은 사용자가 데이터를 제출할 수 있고 이를 수집할 수 있는 컨트롤 그룹입니다. 일반적으로 양식은 설정 페이지, 설문 조사, 계정 생성 등에 사용됩니다. 

이 문서에서는 양식에 대한 XAML 레이아웃을 만드는 디자인 지침에 대해 설명합니다.

![양식 예제](images/PivotHeader.png)

## <a name="when-should-you-use-a-form"></a>양식은 언제 사용해야 하나요?
양식은 서로 명확하게 관련된 데이터 입력을 수집하기 위한 전용 페이지입니다. 사용자로부터 데이터를 명시적으로 수집해야 하는 경우 양식을 사용해야 합니다. 사용자는 다음 작업을 위한 양식을 만들 수 있습니다.
- 계정에 로그인
- 계정에 가입
- 프라이버시 또는 표시 옵션과 같은 앱 설정 변경
- 설문 조사 실시
- 항목 구매
- 피드백 제공

## <a name="types-of-forms"></a>양식 유형

사용자 입력이 제출되고 표시되는 방식을 살펴보고 있는 경우 다음 두 가지 양식이 있습니다.

### <a name="1-instantly-updating"></a>1. 즉시 업데이트
![설정 페이지](images/control-examples/toggle-switch-news.png)

사용자가 양식에서 값을 변경한 결과를 즉시 볼 수 있도록 하려면 즉시 업데이트되는 양식을 사용합니다. 예를 들어 설정 페이지에는 현재 선택 항목이 표시되고 변경된 선택이 즉시 적용됩니다. 앱의 변경 내용을 승인하려면 각 입력 컨트롤에 [이벤트 처리기를 추가](controls-and-events-intro.md)해야 합니다. 사용자가 입력 컨트롤을 변경하면 앱이 적절하게 응답할 수 있습니다.

### <a name="2-submitting-with-button"></a>2. 단추로 제출
다른 유형의 양식을 사용하면 사용자가 단추 클릭으로 데이터를 제출할 시기를 선택할 수 있습니다.

![일정 새 이벤트 추가 페이지](images/calendar-form.png)

이 유형의 양식을 사용하면 사용자가 유연하게 응답할 수 있습니다. 일반적으로 이 유형의 양식은 자유 형식 입력 필드를 포함하므로 훨씬 더 다양한 응답을 받습니다. 제출 시 유효한 사용자 입력 및 올바른 형식의 데이터를 제공하려면 다음 권장 사항을 고려하세요.

- 올바른 컨트롤을 사용하여(일정 날짜에 대해 TextBox 대신 CalendarDatePicker 사용) 잘못된 정보를 제출할 수 없도록 합니다. 나중에 입력 컨트롤 섹션의 양식에서 적절한 입력 컨트롤을 선택하는 방법을 자세히 알아보세요.
- TextBox 컨트롤을 사용하는 경우 사용자에게 [PlaceholderText](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox.PlaceholderText) 속성을 사용하여 원하는 입력 형식의 힌트를 제공합니다.
- [InputScope](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.inputscope) 속성으로 컨트롤의 예상 입력을 설명하여 사용자에게 적절한 화상 키보드를 제공합니다.
- 레이블에 별표(*)로 필수 입력을 표시합니다.
- 모든 필수 정보가 입력될 때까지 제출 단추를 사용하지 않도록 설정합니다.
- 제출 시 잘못된 데이터가 있는 경우 강조 표시된 필드나 테두리로 잘못된 입력이 포함된 컨트롤을 표시하고 사용자가 양식을 다시 제출하도록 합니다.
- 실패한 네트워크 연결과 같은 기타 오류의 경우 사용자에게 적절한 오류 메시지를 표시해야 합니다. 


## <a name="layout"></a>레이아웃

사용자 환경을 지원하고 사용자가 올바른 내용을 입력하도록 하려면 다음 권장 사항을 고려하여 양식 레이아웃을 디자인하세요. 

### <a name="labels"></a>레이블
[레이블](labels.md)은 왼쪽 맞춤으로 정렬되고 입력 컨트롤 위에 놓여야 합니다. 대부분의 컨트롤에는 레이블을 표시하기 위한 기본 제공 Header 속성이 있습니다. 컨트롤에 Header 속성이 없거나 컨트롤 그룹에 레이블을 지정하려는 경우 [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)을 대신 사용할 수 있습니다.

[접근성을 위해 디자인](../accessibility/accessibility.md)하려면 사용자와 화면 읽기 프로그램이 분명히 인식하도록 모든 개별 컨트롤과 컨트롤 그룹에 레이블을 지정하세요. 

글꼴 스타일의 경우 기본 [Windows 유형 램프](../style/typography.md)를 사용합니다. 페이지 제목에는 `TitleTextBlockStyle`을, 그룹 제목에는 `SubtitleTextBlockStyle`을, 컨트롤 레이블에는 `BodyTextBlockStyle`을 사용합니다.

<div class="mx-responsive-img">
<table>
<tr><th>수행</th><th>안 함</th></tr>
<tr>
<td><img src="../controls-and-patterns/images/forms-shortform1col.png" alt="form with top labels"></td>
<td><img src="../controls-and-patterns/images/forms-leftlabel-donot1.png" alt="form with left labels don't"></td>
</tr>
</table>
</div>

### <a name="spacing"></a>간격
컨트롤 그룹을 서로 시각적으로 구분하려면 [맞춤, 여백 및 안쪽 여백](../layout/alignment-margin-padding.md)을 사용합니다. 개별 입력 컨트롤은 높이가 80px이며 24px 간격으로 배치되어야 합니다. 입력 컨트롤 그룹은 48px 간격으로 배치되어야 합니다.

![양식 그룹](images/forms-groups.png)

### <a name="columns"></a>열
열을 만들면 특히 더 큰 화면 크기를 가진 양식에서 불필요한 공백을 줄일 수 있습니다. 그러나 다중 열 양식을 만들려는 경우 열 수는 페이지의 입력 컨트롤 수 및 앱 창의 화면 크기에 따라 결정되어야 합니다. 많은 입력 컨트롤로 화면을 가득 채우기보다는 양식용으로 여러 페이지를 만드는 것이 좋습니다.  

<div class="mx-responsive-img">
<table>
<tr><th>수행</th><th>안 함</th></tr>
<tr>
<td><img src="../controls-and-patterns/images/forms-2cols.png" alt="form with 2 columns"></td>
<td><img src="../controls-and-patterns/images/forms-2cols-bad.png" alt="form with 2 bad columns"></td>
</tr>
<tr><td><img src="../controls-and-patterns/images/forms-3cols.png" alt="form with 3 columns"></td></tr>
</table>

</div>

### <a name="responsive-layout"></a>반응형 레이아웃
화면 또는 창 크기가 변경되면 양식 크기가 조정되므로 사용자는 입력 필드를 못 보고 넘어가지 않습니다. 자세한 내용은 [반응형 디자인 기술](../layout/responsive-design.md)을 참조하세요. 예를 들어 화면 크기에 관계없이 보기에서 양식의 특정 영역을 항상 표시하려고 할 수 있습니다.

![양식 포커스](images/forms-focus2.png)

### <a name="tab-stops"></a>탭 정지
사용자는 키보드를 사용하여 [탭 정지](../input/keyboard-interactions.md#tab-stops)를 통해 컨트롤을 탐색할 수 있습니다. 기본적으로 컨트롤의 탭 순서는 XAML에서 만든 순서를 반영합니다. 기본 동작을 재정의하려면 컨트롤의 **IsTabStop** 또는 **TabIndex** 속성을 변경합니다. 

![양식의 컨트롤에 대한 탭 포커스](images/forms-focus1.png)

## <a name="input-controls"></a>입력 컨트롤
입력 컨트롤은 사용자가 양식에 정보를 입력할 수 있는 UI 요소입니다. 양식에 추가할 수 있는 몇 가지 일반적인 컨트롤이 해당 컨트롤을 사용하는 경우에 대한 정보와 함께 나열됩니다.

### <a name="text-input"></a>텍스트 입력
컨트롤 | Windows Server Update Services와 함께 | 예제
 - | - | -
[TextBox](text-box.md) | 한 줄 또는 여러 줄의 텍스트 캡처 | 이름, 자유 형식 양식 응답 또는 피드백
[PasswordBox](password-box.md) | 문자를 숨겨 프라이빗 데이터 수집 | 암호, SSN(사회 보장 번호), PIN, 신용 카드 정보 
[AutoSuggestBox](auto-suggest-box.md) | 사용자가 입력할 때 해당하는 데이터 세트를 기반으로 한 제안 목록을 사용자에게 표시 | 데이터베이스 검색, 메일 받는 사람: 줄, 이전 쿼리
[RichEditBox](rich-edit-box.md) | 서식 있는 텍스트, 하이퍼링크 및 이미지가 있는 텍스트 파일 편집 | 앱에서 파일 업로드, 미리 보기 및 편집

### <a name="selection"></a>선택
컨트롤 | Windows Server Update Services와 함께 | 예제
- | - | - 
| [CheckBox](checkbox.md) | 하나 이상의 작업 항목 선택 또는 선택 취소 | 사용 약관에 동의, 선택적 항목 추가, 적용되는 모든 항목 선택
[RadioButton](radio-button.md) | 두 가지 이상의 옵션 중 하나 선택 | 인도 유형, 운송 방법 등
[ToggleSwitch](toggles.md) | 함께 사용할 수 없는 두 옵션 중 하나 선택 | 켜기/끄기

> **참고**: 5개 이상의 선택 항목이 있는 경우 목록 컨트롤을 사용합니다.

### <a name="lists"></a>목록
컨트롤 | Windows Server Update Services와 함께 | 예제
- | - | -
[ComboBox](combo-box.md) | 컴팩트 상태로 시작하고 확장하여 선택 가능한 항목 목록 표시 | 시/도 또는 국가와 같은 긴 항목 목록에서 선택
[ListView](https://docs.microsoft.com/windows/uwp/controls-and-patterns/lists#list-views) | 항목을 분류하고 그룹 헤더 할당, 항목 끌어서 놓기, 콘텐츠 조정, 항목 순서 변경 | 옵션 순위 지정
[GridView](https://docs.microsoft.com/windows/uwp/controls-and-patterns/lists#grid-views) | 이미지 기반 컬렉션 정렬 및 찾아보기 | 사진, 색, 디스플레이 테마 선택

### <a name="numeric-input"></a>숫자 입력
컨트롤 | Windows Server Update Services와 함께 | 예제
- | - | -
[슬라이더](slider.md) | 연속 숫자 값 범위에서 숫자 선택 | 백분율, 볼륨, 재생 속도
[평가](rating.md) | 스타로 평가 | 고객 의견

### <a name="date-and-time"></a>날짜 및 시간

컨트롤 | Windows Server Update Services와 함께 
- | - 
[CalendarView](calendar-view.md) | 항상 표시되는 달력에서 단일 날짜 또는 날짜 범위 선택 
[CalendarDatePicker](calendar-date-picker.md) | 상황에 맞는 달력에서 단일 날짜 선택 
[DatePicker](date-picker.md) | 컨텍스트 정보가 중요하지 않을 때 단일 지역화된 날짜 선택
[TimePicker](time-picker.md) | 단일 시간 값 선택

### <a name="additional-controls"></a>추가 컨트롤 
UWP 컨트롤의 전체 목록을 보려면 [기능별 컨트롤 인덱스](controls-by-function.md)를 참조하세요.

더 많은 복합 및 사용자 지정 UI 컨트롤을 알아보려면 [Telerik](https://www.telerik.com/), [SyncFusion](https://www.syncfusion.com/uwp-ui-controls), [DevExpress](https://www.devexpress.com/Products/NET/Controls/Win10Apps/), [Infragistics](https://www.infragistics.com/products/universal-windows-platform), [ComponentOne](https://www.componentone.com/Studio/Platform/UWP) 및 [ActiPro](https://www.actiprosoftware.com/products/controls/universal)와 같은 회사에서 제공되는 리소스를 확인하세요.

## <a name="one-column-form-example"></a>하나의 열 양식 예제
이 예제에서는 Acrylic [마스터/세부 정보](master-details.md) [목록 보기](lists.md) 및 [NavigationView](navigationview.md) 컨트롤을 사용합니다.
![다른 양식 예제의 스크린샷](images/FormExample2.png)
```xaml
<StackPanel>
    <TextBlock Text="New Customer" Style="{StaticResource TitleTextBlockStyle}"/>
    <Button Height="100" Width="100" Background="LightGray" Content="Add photo" Margin="0,24" Click="AddPhotoButton_Click"/>
    <TextBox x:Name="Name" Header= "Name" Margin="0,24,0,0" MaxLength="32" Width="400" HorizontalAlignment="Left" InputScope="PersonalFullName"/>
    <TextBox x:Name="PhoneNumber" Header="Phone Number" Margin="0,24,0,0" MaxLength="15" Width="400" HorizontalAlignment="Left" InputScope="TelephoneNumber" />
    <TextBox x:Name="Email" Header="Email" Margin="0,24,0,0" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="EmailNameOrAddress" />
    <TextBox x:Name="Address" Header="Address" PlaceholderText="Address" Margin="0,24,0,0" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="EmailNameOrAddress" />
    <TextBox x:Name="Address2" Margin="0,24,0,0" PlaceholderText="Address 2" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="EmailNameOrAddress" />
    <RelativePanel>
        <TextBox x:Name="City" PlaceholderText="City" Margin="0,24,0,0" MaxLength="50" Width="200" HorizontalAlignment="Left" InputScope="EmailNameOrAddress" />
        <ComboBox x:Name="State" PlaceholderText="State" Margin="24,24,0,0"  Width="100" RelativePanel.RightOf="City">
             <x:String>WA</x:String>
        </ComboBox>
    </RelativePanel>
    <TextBox x:Name="ZipCode" PlaceholderText="Zip Code" Margin="0,24,0,0" MaxLength="6" Width="100" HorizontalAlignment="Left" InputScope="Number" />
    <StackPanel Orientation="Horizontal">
        <Button Content="Save" Margin="0,24" Click="SaveButton_Click"/>
        <Button Content="Cancel" Margin="24" Click="CancelButton_Click"/>
    </StackPanel>  
</StackPanel>
```

## <a name="two-column-form-example"></a>두 개의 열 양식 예제
이 예제에서는 입력 컨트롤 외에 [Pivot](pivot.md) 컨트롤, [Acrylic](../style/acrylic.md) 배경 및 [CommandBar](app-bars.md)를 사용합니다.
![양식 예제의 스크린샷](images/FormExample.png)
```xaml
<Grid>
    <Pivot Background="{ThemeResource SystemControlAccentAcrylicWindowAccentMediumHighBrush}" >
        <Pivot.TitleTemplate>
            <DataTemplate>
                <Grid>
                    <TextBlock Text="Company Name" Style="{ThemeResource HeaderTextBlockStyle}"/>
                </Grid>
            </DataTemplate>
        </Pivot.TitleTemplate>
        <PivotItem Header="Orders" Margin="0"/>    
        <PivotItem Header="Customers" Margin="0">
            <!--Form Example-->
            <Grid Background="White">
                <RelativePanel>
                    <StackPanel x:Name="Customer" Margin="20">
                        <TextBox x:Name="CustomerName" Header= "Customer Name" Margin="0,24,0,0" MaxLength="320" Width="400" HorizontalAlignment="Left" InputScope="PersonalFullName"/>
                        <TextBox x:Name="CustomerPhoneNumber" Header="Phone Number" Margin="0,24,0,0" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="TelephoneNumber" />
                        <TextBox x:Name="Address" Header="Address" PlaceholderText="Address" Margin="0,24,0,0" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="AlphanumericFullWidth" />
                        <TextBox x:Name="Address2" Margin="0,24,0,0" PlaceholderText="Address 2" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="AlphanumericFullWidth" />
                        <RelativePanel>
                            <TextBox x:Name="City" PlaceholderText="City" Margin="0,24,0,0" MaxLength="50" Width="200" HorizontalAlignment="Left" InputScope="Text" />
                            <ComboBox x:Name="State" PlaceholderText="State" Margin="24,24,0,0"  Width="100" RelativePanel.RightOf="City">
                                <x:String>WA</x:String>
                            </ComboBox>
                        </RelativePanel>
                        <TextBox x:Name="ZipCode" PlaceholderText="Zip Code" Margin="0,24,0,0" MaxLength="6" Width="100" HorizontalAlignment="Left" InputScope="Number" />
                    </StackPanel>
                    <StackPanel x:Name="Associate" Margin="20" RelativePanel.RightOf="Customer">
                        <TextBox x:Name="AssociateName" Header= "Associate" Margin="0,24,0,0" MaxLength="320" Width="400" HorizontalAlignment="Left" InputScope="PersonalFullName"/>
                        <TextBox x:Name="AssociatePhoneNumber" Header="Phone Number" Margin="0,24,0,0" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="TelephoneNumber" />
                        <DatePicker x:Name="TargetInstallDate" Header="Target install Date" HorizontalAlignment="Left" Margin="0,24,0,0"></DatePicker>
                        <TimePicker x:Name="InstallTime" Header="Install Time" HorizontalAlignment="Left" Margin="0,24,0,0"></TimePicker>
                    </StackPanel>
                </RelativePanel>
            </Grid>
        </PivotItem>
        <PivotItem Header="Invoices"/>
        <PivotItem Header="Stock"/>
        <Pivot.RightHeader>
            <CommandBar OverflowButtonVisibility="Collapsed" Background="Transparent">
                <AppBarButton Icon="Add"/>
                <AppBarSeparator/>
                <AppBarButton Icon="Edit" />
                <AppBarButton Icon="Delete"/>
                <AppBarSeparator/>
                <AppBarButton Icon="Save"/>
            </CommandBar>
        </Pivot.RightHeader>
    </Pivot>
</Grid>
```

## <a name="customer-orders-database-sample"></a>고객 주문 데이터베이스 샘플
![고객 주문 데이터베이스 스크린샷](images/customerorderform.png)**Azure** 데이터베이스에 양식 입력을 연결하는 방법을 알아보고 완전히 구현된 양식을 보려면 [고객 주문 데이터베이스](https://github.com/Microsoft/Windows-appsample-customers-orders-database) 앱 샘플을 참조하세요.

## <a name="related-topics"></a>관련 항목
- [입력 컨트롤](controls-and-events-intro.md)
- [입력 체계](../style/typography.md)

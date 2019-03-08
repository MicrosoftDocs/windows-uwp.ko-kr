---
Description: UWP 앱의 폼 레이아웃 지침입니다.
title: 양식
template: detail.hbs
ms.date: 11/07/2017
ms.topic: article
keywords: windows 10, uwp, fluent
ms.openlocfilehash: 8a57f13e168a248569bca1beeceed7b4f6c89f69
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658158"
---
# <a name="forms"></a>양식
폼이 수집 하 고 사용자의 데이터를에서 제출 하는 컨트롤 그룹입니다. 양식 설정 페이지에 대 한 일반적으로 사용 됩니다 설문 조사, 계정 및 등을 만들 합니다. 

이 문서에서는 forms XAML 레이아웃 만들기에 대 한 디자인 지침을 설명 합니다.

![폼의 예제](images/PivotHeader.png)

## <a name="when-should-you-use-a-form"></a>폼 때 사용 하나요?
양식은 서로 명확 하 게 관련 된 데이터 입력을 수집 하는 것에 대 한 전용된 페이지입니다. 명시적으로 사용자 로부터 데이터를 수집 해야 할 경우에 폼을 사용 해야 합니다. 사용자가 폼을 만들 수 있습니다.
- 계정에 로그인
- 계정에 등록
- 개인 정보 같은 앱 설정을 변경 하거나 표시 옵션
- 설문 조사
- 품목을 구입
- 피드백 제공

## <a name="types-of-forms"></a>폼의 형식

사용자 입력을 제출 하 고 표시 하는 방법에 대 한 생각을 하는 경우는 두 가지 유형의 양식

### <a name="1-instantly-updating"></a>1. 즉시 업데이트
![설정 페이지](images/control-examples/toggle-switch-news.png)

사용자가 즉시 결과를 보려면 형식에서 값을 변경 하려는 경우 즉시 업데이트 된 폼을 사용 합니다. 예를 들어, 설정 페이지에서 현재 선택 항목을 표시 하 고 선택 항목에 대 한 변경 내용을 즉시 적용 됩니다. 앱에서 변경을 승인 하려면 해야 [이벤트 처리기를 추가](controls-and-events-intro.md) 입력의 각 컨트롤입니다. 입력된 컨트롤을 변경 하는 사용자, 앱 적절 하 게 응답할 수 있습니다.

### <a name="2-submitting-with-button"></a>2. 단추를 사용 하 여 제출
다른 유형의 폼의 단추 클릭을 사용 하 여 데이터를 전송할 시점을 선택 하는 사용자를 수 있습니다.

![달력은 이벤트 페이지를 추가 합니다.](images/calendar-form.png)

이러한 유형의 폼 응답 사용자 유연성을 제공합니다. 일반적으로 이러한 유형의 폼 자유 형식 입력된 필드를 더 있고 따라서 더욱 다양 한 응답을 수신 합니다. 유효한 사용자 입력 및 제출에 따라 형식이 올바르게 지정 된 데이터를 보장 하려면 다음 권장 사항을 따릅니다.

- 올바른 컨트롤을 사용 하 여 잘못 된 정보를 제출 하 여 (즉, 사용 하 여 텍스트 상자를 사용 하지 않고는 CalendarDatePicker 달력 날짜에 대 한). 입력 컨트롤 섹션 나중에 폼에서 적절 한 입력된 컨트롤을 선택 하는 방법은 참조 하세요.
- 텍스트 상자 컨트롤을 사용 하는 경우 사용자에 게 사용 하 여 원하는 입력 형식 힌트를 제공 합니다 [PlaceholderText](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox.PlaceholderText) 속성입니다.
- 적절 한 사용자 지정 컨트롤의 값된을 입력 하 여 화상 키보드를 제공 합니다 [InputScope](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.inputscope) 속성입니다.
- Mark 필요한 별표를 사용 하 여 입력 * 레이블에 합니다.
- 필요한 모든 정보 채워질 때까지 전송 단추를 사용 하지 않도록 설정 합니다.
- 제출 시 잘못 된 데이터 인 경우 강조 표시 된 필드 또는 테두리를 사용 하 여 잘못 된 입력을 사용 하 여 컨트롤을 표시 하 고 양식을 다시 제출 해야 할 합니다.
- 실패 한 네트워크 연결과 같은 다른 오류에 대 한 사용자에 게 해당 오류 메시지를 표시 해야 합니다. 


## <a name="layout"></a>레이아웃

사용자 경험을 용이 하 게 사용자는 올바른 입력 수를 확인 하 고 폼 레이아웃을 디자인 하기 위한 다음 권장 사항을 것이 좋습니다. 

### <a name="labels"></a>Labels(레이블)
[레이블](labels.md) 왼쪽 맞춤 하 고 입력된 컨트롤 위에 배치 해야 합니다. 많은 컨트롤에 레이블을 표시 하는 기본 제공 헤더 속성이 있습니다. 컨트롤에 Header 속성이 없거나 컨트롤 그룹에 레이블을 지정하려는 경우에는 [TextBlock](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.TextBlock)을 사용할 수 있습니다.

[내게 필요한 옵션에 대 한 디자인](../accessibility/accessibility.md), 모든 개별 및 그룹 사용자 모두에 대 한 명확 하 고 화면 판독기에 대 한 컨트롤의 레이블을 지정 합니다. 

글꼴 스타일에 대 한 기본값을 사용 하 여 [UWP 형식 진입](../style/typography.md)합니다. 사용 하 여 `TitleTextBlockStyle` 페이지 제목에 대 한 `SubtitleTextBlockStyle` 그룹 머리글 및 `BodyTextBlockStyle` 제어 레이블에 대 한 합니다.

<div class="mx-responsive-img">
<table>
<tr><th>권장 사항</th><th>금지 사항</th></tr>
<tr>
<td><img src="../controls-and-patterns/images/forms-shortform1col.png" alt="form with top labels"></td>
<td><img src="../controls-and-patterns/images/forms-leftlabel-donot1.png" alt="form with left labels don't"></td>
</tr>
</table>
</div>

### <a name="spacing"></a>간격
시각적으로 컨트롤 그룹을 서로 구분을 사용 하 여 [맞춤, 여백 및 안쪽 여백](../layout/alignment-margin-padding.md)합니다. 개별 입력된 컨트롤 높이가 80px 되며 전체 24px 분리 해야 합니다. 입력된 컨트롤의 그룹에는 간격이 48px 떨어져 이어야 합니다.

![양식 그룹](images/forms-groups.png)

### <a name="columns"></a>열
열을 만드는 형태로, 특히 더 큰 화면 크기를 사용 하 여 불필요 한 공백이 줄일 수 있습니다. 그러나 다중 열 폼을 만들려면 원하는 열 수가 입력된 컨트롤 페이지의 수 및 앱 창의 화면 크기에 달라져야 합니다. 다양 한 입력된 컨트롤이 포함 된 화면에 과부하가, 대신 양식에 대 한 여러 페이지를 만드는 것이 좋습니다.  

<div class="mx-responsive-img">
<table>
<tr><th>권장 사항</th><th>금지 사항</th></tr>
<tr>
<td><img src="../controls-and-patterns/images/forms-2cols.png" alt="form with 2 columns"></td>
<td><img src="../controls-and-patterns/images/forms-2cols-bad.png" alt="form with 2 bad columns"></td>
</tr>
<tr><td><img src="../controls-and-patterns/images/forms-3cols.png" alt="form with 3 columns"></td></tr>
</table>

</div>

### <a name="responsive-layout"></a>반응 형 레이아웃
사용자 입력된 필드를 간과 해서는 안 되므로 화면 또는 창 크기가 변경 되 면 forms 크기 조정 해야 합니다. 자세한 내용은 [반응 형 디자인 기술을](../layout/responsive-design.md)합니다. 예를 들어, 다음 화면 크기에 관계 없이 보기에서 폼의 특정 영역을 항상 유지 하는 것이 좋습니다.

![forms 포커스](images/forms-focus2.png)

### <a name="tab-stops"></a>탭 정지
사용자가 키보드를 사용 하 여 사용 하 여 컨트롤을 이동할 [탭 정지](../input/keyboard-interactions.md#tab-stops)합니다. 기본적으로 컨트롤의 탭 순서는 XAML에서 생성 된 순서를 반영 합니다. 기본 동작을 재정의 하려면 변경 된 **IsTabStop** 하거나 **TabIndex** 컨트롤의 속성입니다. 

![폼에서 컨트롤에 탭 포커스](images/forms-focus1.png)

## <a name="input-controls"></a>입력된 컨트롤
입력된 컨트롤은 양식에 정보를 입력할 수 있도록 하는 UI 요소입니다. 폼에 추가할 수 있는 일부 공용 컨트롤 사용 시기에 대 한 정보를 아래 나열 됩니다.

### <a name="text-input"></a>텍스트 입력
컨트롤 | Windows Server Update Services와 함께 | 예
 - | - | -
[TextBox](text-box.md) | 하나 또는 여러 줄의 텍스트 캡처 | 이름, 자유 형식 응답 또는 피드백
[PasswordBox](password-box.md) | 다른 문자에서 개인 데이터를 수집 합니다. | 암호, 주민 등록 번호 (SSN) Pin을 신용 카드 정보 
[AutoSuggestBox](auto-suggest-box.md) | 사용자가 해당 데이터 집합에서 추천 단어 목록을 입력 표시 | 데이터베이스 검색, 메일: 줄을 이전 쿼리
[RichEditBox](rich-edit-box.md) | 서식 있는 텍스트, 하이퍼링크 및 이미지를 사용 하 여 텍스트 파일 편집 | 파일 업로드, 미리 보기 및 앱에서 편집

### <a name="selection"></a>선택
컨트롤 | Windows Server Update Services와 함께 | 예
- | - | - 
| [CheckBox](checkbox.md) | 선택 하거나 하나 이상의 작업 항목을 선택 취소 | 사용 약관에 동의 선택 항목을 추가, 해당 항목 모두 선택
[RadioButton](radio-button.md) | 두 개 이상 선택 항목 중에서 하나의 옵션을 선택 합니다. | 형식, 메서드 등 전달를 선택 합니다.
[ToggleSwitch](toggles.md) | 상호 배타적인 두 옵션 중 하나를 선택합니다 | 설정/해제

> **참고**: 5 개 이상의 선택 항목의 경우에 목록 컨트롤을 사용 합니다.

### <a name="lists"></a>목록
컨트롤 | Windows Server Update Services와 함께 | 예
- | - | -
[ComboBox](https://docs.microsoft.com/windows/uwp/controls-and-patterns/lists.md#drop-down-lists) | Compact 상태에서 시작 하 고 선택 가능한 항목의 목록을 표시 하려면 확장 합니다. | 긴 상태 또는 국가 등의 항목 목록에서 선택
[ListView](https://docs.microsoft.com/windows/uwp/controls-and-patterns/lists#list-views) | 항목을 분류 및 그룹 헤더를 할당, 항목, 레이 콘텐츠를 끌어서 항목을 다시 정렬 | Rank 옵션
[GridView](https://docs.microsoft.com/windows/uwp/controls-and-patterns/lists#grid-views) | 정렬 및 이미지 기반 컬렉션 찾아보기 | 사진, 색 선택, 테마를 표시 합니다.

### <a name="numeric-input"></a>숫자 입력
컨트롤 | Windows Server Update Services와 함께 | 예
- | - | -
[슬라이더](slider.md) | 연속 숫자 값의 범위에서 숫자를 선택 합니다. | 백분율, 볼륨, 재생 속도
[등급](rating.md) | 별표를 사용 하 여 평가 | 고객 의견

### <a name="date-and-time"></a>날짜 및 시간

컨트롤 | Windows Server Update Services와 함께 
- | - 
[CalendarView](calendar-view.md) | 하나의 날짜 또는 항상 표시 되는 달력에서 날짜 범위 선택 
[CalendarDatePicker](calendar-date-picker.md) | 상황에 맞는 일정에서 하나의 날짜를 선택 합니다. 
[DatePicker](date-picker.md) | 때 컨텍스트 정보는 중요 하지 않은 단일 지역화 된 날짜를 선택 합니다.
[TimePicker](time-picker.md) | 단일 시간 값을 선택 합니다.

### <a name="additional-controls"></a>컨트롤 추가 
UWP 컨트롤의 전체 목록은 참조 하세요 [기능별 컨트롤의 인덱스](controls-by-function.md)합니다.

더 복잡 하 고 사용자 지정 UI 컨트롤에 대 한 확인 회사에서 사용할 수 있는 UWP 리소스와 같은 [Telerik](https://www.telerik.com/)를 [SyncFusion](https://www.syncfusion.com/products/uwp)하십시오 [DevExpress](https://www.devexpress.com/Products/NET/Controls/Win10Apps/), [ Infragistics](https://www.infragistics.com/products/universal-windows-platform)하십시오 [ComponentOne](https://www.componentone.com/Studio/Platform/UWP), 및 [ActiPro](https://www.actiprosoftware.com/products/controls/universal)합니다.

## <a name="one-column-form-example"></a>하나의 열 양식 예제
이 예제에서는 못합니다 [마스터/세부 정보](master-details.md) [목록 뷰에서](lists.md) 하 고 [NavigationView](navigationview.md) 컨트롤.
![다른 폼 예제 스크린샷](images/FormExample2.png)
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

## <a name="two-column-form-example"></a>두 열 양식 예제
이 예제에서는 합니다 [피벗](pivot.md) 컨트롤 [못합니다](../style/acrylic.md) 배경, 및 [CommandBar](app-bars.md) 입력된 컨트롤 외에도 합니다.
![Form 예제 스크린샷](images/FormExample.png)
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
![고객 주문 데이터베이스 스크린 샷](images/customerorderform.png) 양식 입력을 연결 하는 방법에는 **Azure** 데이터베이스 및 참조를 완전히 구현 된 양식이 합니다 [고객 주문 데이터베이스](https://github.com/Microsoft/Windows-appsample-customers-orders-database) 앱 샘플.

## <a name="related-topics"></a>관련 항목
- [입력된 컨트롤](controls-and-events-intro.md)
- [Typography](../style/typography.md)

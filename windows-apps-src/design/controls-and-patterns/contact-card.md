---
Description: 단추를 사용하면 즉각적인 작업을 트리거할 수 있습니다.
title: 연락처 카드
ms.date: 03/07/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: kele
design-contact: tbd
dev-contact: tbd
doc-status: not-published
ms.localizationpriority: medium
ms.openlocfilehash: 274481b2a282b025a637f7f6cc54dc0161c3e61d
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82968758"
---
# <a name="contact-card"></a>연락처 카드

연락처 카드는 [연락처](/uwp/api/Windows.ApplicationModel.Contacts.Contact)(Windows가 사용자와 회사를 나타내기 위해 사용하는 메커니즘)에 이름, 전화 번호, 주소 등의 연락처 정보를 표시합니다.  또한 연락처 카드는 사용자가 연락처 정보를 편집할 수 있게 해줍니다. 간략한 연락처 카드를 표시할 것인지, 추가 정보가 포함된 전체 연락처 카드를 표시할 것인지 선택할 수 있습니다.

> **중요 API**: [ShowContactCard 메서드](/uwp/api/windows.applicationmodel.contacts.contactmanager.showcontactcard),  [ShowFullContactCard 메서드](/uwp/api/windows.applicationmodel.contacts.contactmanager.showfullcontactcard), [IsShowContactCardSupported 메서드](/uwp/api/windows.applicationmodel.contacts.contactmanager.IsShowContactCardSupported), [Contact 클래스](/uwp/api/Windows.ApplicationModel.Contacts.Contact)  

다음과 같은 두 가지 방법으로 연락처 카드를 표시할 수 있습니다.  
* 플라이아웃에 표시되는 표준 연락처 카드는 신속 처리가 가능하기 때문에 사용자가 플라이아웃 밖을 클릭하면 바로 사라집니다. 
* 전체 연락처 카드는 더 많은 공간을 차지하고 신속 처리가 불가능하기 때문에 사용자가 **닫기**를 클릭해야만 닫을 수 있습니다. 


<figure>
    <img src="images/contact-card/contact-card-standard.png" alt="The full contact card">
    <figcaption>표준 연락처 카드</figcaption>
</figure>

<figure>
    <img src="images/contact-card/contact-card-full.png" alt="The full contact card">
    <figcaption>전체 연락처 카드</figcaption>
</figure>


## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

연락을 위해 연락처 정보를 표시하고 싶을 때는 연락처 정보를 사용합니다. 연락처의 이름과 사진만 표시하고 싶으면 [인물 사진 컨트롤](person-picture.md)을 사용합니다. 


<!-- TODO: Add examples back when the contact card has been added. -->

<!-- ## Examples

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>If you have the <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> app installed, click here to <a href="xamlcontrolsgallery:/item/Button">open the app and see the Button in action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Get the XAML Controls Gallery app (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Get the source code (GitHub)</a></li>
    </ul>
</td>
</tr>
</table> -->

## <a name="show-a-standard-contact-card"></a>표준 연락처 카드 표시

1. 일반적으로 사용자가 단추든 [인물 사진 컨트롤](person-picture.md)이든 클릭하면 연락처 카드가 표시됩니다. 이 요소를 숨기고 싶지는 않을 것입니다. 요소가 숨겨지지 않게 하려면 요소의 위치와 크기를 설명하는 [Rect](/uwp/api/windows.foundation.rect)를 만들어야 합니다. 

    나중에 사용할 수 있도록 여기에 필요한 유틸리티 기능을 만들어 보겠습니다.
    ```csharp
    // Gets the rectangle of the element 
    public static Rect GetElementRectHelper(FrameworkElement element) 
    { 
        // Passing "null" means set to root element. 
        GeneralTransform elementTransform = element.TransformToVisual(null); 
        Rect rect = elementTransform.TransformBounds(new Rect(0, 0, element.ActualWidth, element.ActualHeight)); 
        return rect; 
    } 

    ```

2. [ContactManager.IsShowContactCardSupported](/uwp/api/windows.applicationmodel.contacts.contactmanager.IsShowContactCardSupported) 메서드를 호출하여 연락처 카드를 표시할 수 있는지 여부를 판단합니다. 지원되지 않는 경우에는 오류 메시지가 표시됩니다 (이 예제에서는 클릭 이벤트에 대한 응답으로 연락처 카드가 표시된다고 가정합니다.)
    ```csharp
    // Contact and Contact Managers are existing classes 
    private void OnUserClickShowContactCard(object sender, RoutedEventArgs e) 
    { 
        if (ContactManager.IsShowContactCardSupported()) 
        { 

    ```

3. 1단계에서 생성한 유틸리티 기능을 사용하여 이벤트를 발생시킨 컨트롤의 경계를 가져옵니다(연락처 정보로는 해결할 수 없기 때문에).

    ```csharp
            Rect selectionRect = GetElementRect((FrameworkElement)sender); 
    ```

4. 표시하고 싶은 [연락처](//docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts.Contact) 개체를 다운로드합니다. 이 예제에서는 간단한 연락처를 만들어 보았지만, 코드로 실제 연락처를 검색해 봐야 합니다. 

    ```csharp
                // Retrieve the contact to display
                var contact = new Contact(); 
                var email = new ContactEmail(); 
                email.Address = "jsmith@contoso.com"; 
                contact.Emails.Add(email); 
    ```
5. [ShowContactCard](/uwp/api/windows.applicationmodel.contacts.contactmanager.showcontactcard) 메서드를 호출하여 연락처 카드를 표시합니다. 

    ```csharp
            ContactManager.ShowFullContactCard(
                contact, selectionRect, Placement.Default); 
        } 
    } 
    ```

다음은 전체 코드 예제입니다.

```csharp
// Gets the rectangle of the element 
public static Rect GetElementRect(FrameworkElement element) 
{ 
    // Passing "null" means set to root element. 
    GeneralTransform elementTransform = element.TransformToVisual(null); 
    Rect rect = elementTransform.TransformBounds(new Rect(0, 0, element.ActualWidth, element.ActualHeight)); 
    return rect; 
} 
 
// Display a contact in response to an event
private void OnUserClickShowContactCard(object sender, RoutedEventArgs e) 
{ 
    if (ContactManager.IsShowContactCardSupported()) 
    { 
        Rect selectionRect = GetElementRect((FrameworkElement)sender);

        // Retrieve the contact to display
        var contact = new Contact(); 
        var email = new ContactEmail(); 
        email.Address = "jsmith@contoso.com"; 
        contact.Emails.Add(email); 
    
        ContactManager.ShowContactCard(
            contact, selectionRect, Placement.Default); 
    } 
} 

```

## <a name="show-a-full-contact-card"></a>전체 연락처 카드 표시

전체 연락처 카드를 표시하려면 [ShowContactCard](/uwp/api/windows.applicationmodel.contacts.contactmanager.showcontactcard)가 아닌 [ShowFullContactCard](/uwp/api/windows.applicationmodel.contacts.contactmanager.showfullcontactcard) 메서드를 호출합니다.

```csharp
private void onUserClickShowContactCard() 
{ 
   
    Contact contact = new Contact(); 
    ContactEmail email = new ContactEmail(); 
    email.Address = "jsmith@hotmail.com"; 
    contact.Emails.Add(email); 
 
 
    // Setting up contact options.     
    FullContactCardOptions fullContactCardOptions = new FullContactCardOptions(); 
 
    // Display full contact card on mouse click.   
    // Launch the People’s App with full contact card  
    fullContactCardOptions.DesiredRemainingView = ViewSizePreference.UseLess; 
     
 
    // Shows the full contact card by launching the People App. 
    ContactManager.ShowFullContactCard(contact, fullContactCardOptions); 
} 

```

## <a name="retrieving-real-contacts"></a>"실제" 연락처 검색

이 문서의 예제에서는 간단한 연락처를 만들어 보았습니다. 실제 앱에서는 아마도 기존 연락처를 검색하고 싶을 것입니다. 이에 대한 지침은 [연락처 및 일정 문서](/windows/uwp/contacts-and-calendar/)를 참조하세요.




## <a name="related-articles"></a>관련된 문서
- [연락처 및 일정](/windows/uwp/contacts-and-calendar/)
- [연락처 카드 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ContactCards)
- [인물 사진 컨트롤](/windows/uwp/controls-and-patterns/person-picture/)

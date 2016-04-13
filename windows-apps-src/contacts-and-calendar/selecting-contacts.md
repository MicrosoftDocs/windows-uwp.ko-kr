---
Windows.ApplicationModel.Contacts 네임스페이스를 통해 연락처를 선택하는 여러 가지 옵션이 있습니다.
연락처 선택
ms.assetid: 35FEDEE6-2B0E-4391-84BA-5E9191D4E442
키워드: 연락처, 선택
키워드: 단일 연락처 선택
키워드: 여러 연락처를 선택하는 방법
키워드: 연락처, 다중 선택
키워드: 특정 연락처 데이터를 선택하는 방법
키워드: 연락처, 특정 데이터 선택
키워드: 연락처, 특정 필드 선택
---

# 연락처 선택

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


[
            **Windows.ApplicationModel.Contacts**](https://msdn.microsoft.com/library/windows/apps/BR225002) 네임스페이스를 통해 연락처를 선택하는 여러 가지 옵션이 있습니다. 여기서는 단일 연락처나 여러 연락처를 선택하는 방법을 설명하고 연락처 선택 기능을 구성하여 앱에 필요한 연락처 정보만 검색하는 방법을 보여 줍니다.

## 연락처 선택 기능 설정

[
            **Windows.ApplicationModel.Contacts.ContactPicker**](https://msdn.microsoft.com/library/windows/apps/BR224913) 인스턴스를 만들고 변수에 할당합니다.

```cs
var contactPicker = new Windows.ApplicationModel.Contacts.ContactPicker();
```

## 선택 모드 설정(옵션)

기본적으로 연락처 선택 기능은 사용자가 선택하는 연락처에 대해 사용 가능한 데이터를 모두 검색합니다. [
            **SelectionMode**](https://msdn.microsoft.com/library/windows/apps/BR224913-selectionmode) 속성을 사용하면 연락처 선택 기능이 앱에 필요한 데이터 필드만 검색하도록 구성할 수 있습니다. 사용 가능한 연락처 데이터의 하위 집합만 필요한 경우에 이것은 연락처 선택 기능을 더 효율적으로 사용하는 방법입니다.

먼저 [**SelectionMode**](https://msdn.microsoft.com/library/windows/apps/BR224913-selectionmode) 속성을 **Fields**로 설정합니다.

```cs
contactPicker.SelectionMode = Windows.ApplicationModel.Contacts.ContactSelectionMode.Fields;
```

그런 다음 [**desiredFieldsWithContactFieldType**](https://msdn.microsoft.com/library/windows/apps/BR224913-desiredfieldswithcontactfieldtype) 속성을 사용하여 연락처 선택 기능에서 검색할 필드를 지정합니다. 이 예제에서는 연락처 선택 기능에서 메일 주소를 검색하도록 구성합니다:

``` cs
contactPicker.DesiredFieldsWithContactFieldType.Add(Windows.ApplicationModel.Contacts.ContactFieldType.Email);
```

## 선택기 시작

```cs
Contact contact = await contactPicker.PickContactAsync();
```

사용자가 하나 이상의 연락처를 선택하도록 하려면 [**pickContactsAsync**](https://msdn.microsoft.com/library/windows/apps/BR224913-pickcontactsasync)를 사용합니다.

```cs
public IList&lt;Contact&gt; contacts;
contacts = await contactPicker.PickContactsAsync();
```

## 연락처 처리

선택 기능이 반환되면 사용자가 연락처를 선택했는지 확인합니다. 선택한 경우 연락처 정보를 처리합니다.

다음 예제에서는 단일 연락처를 처리하는 방법을 보여 줍니다. 여기서는 연락처의 이름을 검색하여 *OutputName*이라는 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) 컨트롤로 복사합니다.

```cs
if (contact != null)
{
    OutputName.Text = contact.DisplayName;
}
else
{
    rootPage.NotifyUser(&quot;No contact was selected.&quot;, NotifyType.ErrorMessage);
}
```

다음 예제에서는 여러 연락처를 처리하는 방법을 보여 줍니다.

```cs
if (contacts != null &amp;&amp; contacts.Count &gt; 0)
{
    foreach (Contact contact in contacts)
    {
        // Do something with the contact information.
    }
}
```

## 완전한 예제(단일 연락처)

이 예제에서는 연락처 선택 기능을 사용하여 메일 주소, 위치 및 전화 번호와 함께 단일 연락처의 이름을 검색합니다.

```cs
// ...
using Windows.ApplicationModel.Contacts;
// ...

private async void PickAContactButton-Click(object sender, RoutedEventArgs e)
{
    ContactPicker contactPicker = new ContactPicker();

    contactPicker.DesiredFieldsWithContactFieldType.Add(ContactFieldType.Email);
    contactPicker.DesiredFieldsWithContactFieldType.Add(ContactFieldType.Address);
    contactPicker.DesiredFieldsWithContactFieldType.Add(ContactFieldType.PhoneNumber);

    Contact contact = await contactPicker.PickContactAsync();

    if (contact != null)
    {
        OutputFields.Visibility = Visibility.Visible;
        OutputEmpty.Visibility = Visibility.Collapsed;

        OutputName.Text = contact.DisplayName;

        AppendContactFieldValues(OutputEmails, contact.Emails);
        AppendContactFieldValues(OutputPhoneNumbers, contact.Phones);
        AppendContactFieldValues(OutputAddresses, contact.Addresses);
    }
    else
    {
        OutputEmpty.Visibility = Visibility.Visible;
        OutputFields.Visibility = Visibility.Collapsed;
    }
}

private void AppendContactFieldValues&lt;T&gt;(TextBlock content, IList&lt;T&gt; fields)
{
    if (fields.Count &gt; 0)
    {
        StringBuilder output = new StringBuilder();

        if (fields[0].GetType() == typeof(ContactEmail))
        {
            foreach (ContactEmail email in fields as IList&lt;ContactEmail&gt;)
            {
                output.AppendFormat(&quot;Email: {0} ({1})\n&quot;, email.Address, email.Kind);
            }
        }
        else if (fields[0].GetType() == typeof(ContactPhone))
        {
            foreach (ContactPhone phone in fields as IList&lt;ContactPhone&gt;)
            {
                output.AppendFormat(&quot;Phone: {0} ({1})\n&quot;, phone.Number, phone.Kind);
            }
        }
        else if (fields[0].GetType() == typeof(ContactAddress))
        {
            List&lt;String&gt; addressParts = null;
            string unstructuredAddress = &quot;&quot;;

            foreach (ContactAddress address in fields as IList&lt;ContactAddress&gt;)
            {
                addressParts = (new List&lt;string&gt; { address.StreetAddress, address.Locality, address.Region, address.PostalCode });
                unstructuredAddress = string.Join(&quot;, &quot;, addressParts.FindAll(s =&gt; !string.IsNullOrEmpty(s)));
                output.AppendFormat(&quot;Address: {0} ({1})\n&quot;, unstructuredAddress, address.Kind);
            }
        }

        content.Visibility = Visibility.Visible;
        content.Text = output.ToString();
    }
    else
    {
        content.Visibility = Visibility.Collapsed;
    }
}
```

## 완전한 예제(여러 연락처)

이 예제에서는 연락처 선택 기능을 사용하여 여러 연락처를 검색한 다음 `OutputContacts`라는 [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) 컨트롤에 연락처를 추가합니다.

```cs
MainPage rootPage = MainPage.Current;
public IList&lt;Contact&gt; contacts;

private async void PickContactsButton-Click(object sender, RoutedEventArgs e)
{
    var contactPicker = new Windows.ApplicationModel.Contacts.ContactPicker();
    contactPicker.CommitButtonText = &quot;Select&quot;;
    contacts = await contactPicker.PickContactsAsync();

    // Clear the ListView.
    OutputContacts.Items.Clear();

    if (contacts.Count &gt; 0)
    {
        OutputContacts.Visibility = Windows.UI.Xaml.Visibility.Visible;
        OutputEmpty.Visibility = Visibility.Collapsed;

        foreach (Contact contact in contacts)
        {
            // Add the contacts to the ListView.
            OutputContacts.Items.Add(new ContactItemAdapter(contact));
        }
    }
    else
    {
        OutputEmpty.Visibility = Visibility.Visible;
    }         
}
```

``` cs
public class ContactItemAdapter
{
    public string Name { get; private set; }
    public string SecondaryText { get; private set; }

    public ContactItemAdapter(Contact contact)
    {
        Name = contact.DisplayName;
        if (contact.Emails.Count &gt; 0)
        {
            SecondaryText = contact.Emails[0].Address;
        }
        else if (contact.Phones.Count &gt; 0)
        {
            SecondaryText = contact.Phones[0].Number;
        }
        else if (contact.Addresses.Count &gt; 0)
        {
            List&lt;string&gt; addressParts = (new List&lt;string&gt; { contact.Addresses[0].StreetAddress, 
              contact.Addresses[0].Locality, contact.Addresses[0].Region, contact.Addresses[0].PostalCode });
              string unstructuredAddress = string.Join(&quot;, &quot;, addressParts.FindAll(s =&gt; !string.IsNullOrEmpty(s)));
            SecondaryText = unstructuredAddress;
        }
    }
}
```

## 요약 및 다음 단계

지금까지 연락처 선택 기능을 사용하여 연락처 정보를 검색하는 방법을 간략히 살펴보았습니다. 연락처 및 연락처 선택 기능을 사용하는 방법에 대한 더 많은 예제를 보려면 GitHub에서 [유니버설 Windows 앱 샘플](http://go.microsoft.com/fwlink/p/?linkid=619979)을 다운로드합니다.



<!--HONumber=Mar16_HO1-->



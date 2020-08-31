---
description: Windows.ApplicationModel.Contacts 네임스페이스를 통해 연락처를 선택하는 여러 가지 옵션이 있습니다.
title: 연락처 선택
ms.assetid: 35FEDEE6-2B0E-4391-84BA-5E9191D4E442
keywords: 연락처를 선택 하 고 단일 연락처 선택을 선택 하 여 여러 연락처 연락처를 선택 하 고, 여러 개의 특정 연락처 데이터 연락처 선택, 특정 데이터 연락처 선택, 특정 필드 선택
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: dcf8fefb82de3cccf3d914ae6003c991107888e1
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154687"
---
# <a name="select-contacts"></a>연락처 선택



[  **Windows.ApplicationModel.Contacts**](/uwp/api/Windows.ApplicationModel.Contacts) 네임스페이스를 통해 연락처를 선택하는 여러 가지 옵션이 있습니다. 여기서는 단일 연락처나 여러 연락처를 선택하는 방법을 설명하고 연락처 선택 기능을 구성하여 앱에 필요한 연락처 정보만 검색하는 방법을 보여 줍니다.

## <a name="set-up-the-contact-picker"></a>연락처 선택 설정

Windows. r e n a l e.. [**연락처 선택기**](/uwp/api/Windows.ApplicationModel.Contacts.ContactPicker) 의 인스턴스를 만들어 변수에 할당 합니다.

```cs
var contactPicker = new Windows.ApplicationModel.Contacts.ContactPicker();
```

## <a name="set-the-selection-mode-optional"></a>선택 모드를 설정 합니다 (선택 사항).

기본적으로 연락처 선택은 사용자가 선택한 연락처에 대해 사용 가능한 모든 데이터를 검색 합니다. [**SelectionMode**](/uwp/api/windows.applicationmodel.contacts.contactpicker.selectionmode) 속성을 사용 하면 연락처 선택기를 구성 하 여 앱에 필요한 데이터 필드만 검색할 수 있습니다. 사용 가능한 연락처 데이터의 하위 집합만 필요한 경우 연락처 선택기를 사용 하는 보다 효율적인 방법입니다.

먼저 [**SelectionMode**](/uwp/api/windows.applicationmodel.contacts.contactpicker.selectionmode) 속성을 **필드**로 설정 합니다.

```cs
contactPicker.SelectionMode = Windows.ApplicationModel.Contacts.ContactSelectionMode.Fields;
```

그런 다음 [**DesiredFieldsWithContactFieldType**](/uwp/api/windows.applicationmodel.contacts.contactpicker.desiredfieldswithcontactfieldtype) 속성을 사용 하 여 연락처 선택기에서 검색할 필드를 지정 합니다. 이 예제에서는 전자 메일 주소를 검색 하도록 연락처 선택기를 구성 합니다.

``` cs
contactPicker.DesiredFieldsWithContactFieldType.Add(Windows.ApplicationModel.Contacts.ContactFieldType.Email);
```

## <a name="launch-the-picker"></a>선택 시작

```cs
Contact contact = await contactPicker.PickContactAsync();
```

사용자가 연락처를 하나 이상 선택 하도록 하려면 [**PickContactsAsync**](/uwp/api/windows.applicationmodel.contacts.contactpicker.pickcontactsasync) 을 사용 합니다.

```cs
public IList<Contact> contacts;
contacts = await contactPicker.PickContactsAsync();
```

## <a name="process-the-contacts"></a>연락처 처리

선택이 반환 되 면 사용자가 연락처를 선택 했는지 여부를 확인 합니다. 그렇다면 연락처 정보를 처리 합니다.

다음 예제에서는 단일 연락처를 처리하는 방법을 보여 줍니다. 여기서 연락처의 이름을 검색 하 고 *OutputName*이라는 [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) 컨트롤에 복사 합니다.

```cs
if (contact != null)
{
    OutputName.Text = contact.DisplayName;
}
else
{
    rootPage.NotifyUser("No contact was selected.", NotifyType.ErrorMessage);
}
```

이 예제에서는 여러 연락처를 처리 하는 방법을 보여 줍니다.

```cs
if (contacts != null && contacts.Count > 0)
{
    foreach (Contact contact in contacts)
    {
        // Do something with the contact information.
    }
}
```

## <a name="complete-example-single-contact"></a>전체 예제 (단일 연락처)

이 예제에서는 연락처 선택기를 사용 하 여 전자 메일 주소, 위치 또는 전화 번호와 함께 단일 연락처의 이름을 검색 합니다.

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

private void AppendContactFieldValues<T>(TextBlock content, IList<T> fields)
{
    if (fields.Count > 0)
    {
        StringBuilder output = new StringBuilder();

        if (fields[0].GetType() == typeof(ContactEmail))
        {
            foreach (ContactEmail email in fields as IList<ContactEmail>)
            {
                output.AppendFormat("Email: {0} ({1})\n", email.Address, email.Kind);
            }
        }
        else if (fields[0].GetType() == typeof(ContactPhone))
        {
            foreach (ContactPhone phone in fields as IList<ContactPhone>)
            {
                output.AppendFormat("Phone: {0} ({1})\n", phone.Number, phone.Kind);
            }
        }
        else if (fields[0].GetType() == typeof(ContactAddress))
        {
            List<String> addressParts = null;
            string unstructuredAddress = "";

            foreach (ContactAddress address in fields as IList<ContactAddress>)
            {
                addressParts = (new List<string> { address.StreetAddress, address.Locality, address.Region, address.PostalCode });
                unstructuredAddress = string.Join(", ", addressParts.FindAll(s => !string.IsNullOrEmpty(s)));
                output.AppendFormat("Address: {0} ({1})\n", unstructuredAddress, address.Kind);
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

## <a name="complete-example-multiple-contacts"></a>전체 예제 (여러 연락처)

이 예제에서는 연락처 선택기를 사용 하 여 여러 연락처를 검색 한 다음 이라는 [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView) 컨트롤에 연락처를 추가 합니다 `OutputContacts` .

```cs
MainPage rootPage = MainPage.Current;
public IList<Contact> contacts;

private async void PickContactsButton-Click(object sender, RoutedEventArgs e)
{
    var contactPicker = new Windows.ApplicationModel.Contacts.ContactPicker();
    contactPicker.CommitButtonText = "Select";
    contacts = await contactPicker.PickContactsAsync();

    // Clear the ListView.
    OutputContacts.Items.Clear();

    if (contacts != null && contacts.Count > 0)
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
        if (contact.Emails.Count > 0)
        {
            SecondaryText = contact.Emails[0].Address;
        }
        else if (contact.Phones.Count > 0)
        {
            SecondaryText = contact.Phones[0].Number;
        }
        else if (contact.Addresses.Count > 0)
        {
            List<string> addressParts = (new List<string> { contact.Addresses[0].StreetAddress,
              contact.Addresses[0].Locality, contact.Addresses[0].Region, contact.Addresses[0].PostalCode });
              string unstructuredAddress = string.Join(", ", addressParts.FindAll(s => !string.IsNullOrEmpty(s)));
            SecondaryText = unstructuredAddress;
        }
    }
}
```

## <a name="summary-and-next-steps"></a>요약 및 다음 단계

이제 연락처 선택기를 사용 하 여 연락처 정보를 검색 하는 방법을 기본적으로 이해 하 고 있습니다. GitHub에서 [유니버설 Windows 앱 샘플](https://github.com/Microsoft/Windows-universal-samples) 을 다운로드 하 여 연락처 및 연락처 선택기를 사용 하는 방법에 대 한 더 많은 예제를 확인 합니다.
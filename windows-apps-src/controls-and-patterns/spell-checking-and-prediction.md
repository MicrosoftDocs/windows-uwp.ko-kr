---
author: Jwmsft
Description: 텍스트를 입력 및 편집하는 동안 맞춤법 검사는 빨간색 물결선으로 강조 표시하여 단어의 맞춤법이 틀렸음을 사용자에게 알리고 맞춤법을 수정하는 방법을 제공합니다.
title: 맞춤법 검사 및 텍스트 자동 완성
ms.assetid: B867C956-5AB2-4207-A8DE-179CE7871180
label: Spell checking and text prediction
template: detail.hbs
---

# 맞춤법 검사에 대한 지침

텍스트를 입력 및 편집하는 동안 맞춤법 검사는 빨간색 물결선으로 강조 표시하여 단어의 맞춤법이 틀렸음을 사용자에게 알리고 맞춤법을 수정하는 방법을 제공합니다.

**중요 API**

-   [**IsSpellCheckEnabled 속성(XAML)**](https://msdn.microsoft.com/library/windows/apps/br209688)


## <span id="checklist_section"></span><span id="CHECKLIST_SECTION"></span>권장 사항


-   맞춤법 검사는 사용자가 텍스트 입력 컨트롤에 단어 또는 문장을 입력할 때 도움을 주기 위해 사용됩니다. 맞춤법 검사는 터치식, 마우스 및 키보드 입력으로 작동합니다.
-   단어가 사전에 없거나 사용자가 맞춤법 검사를 중요시하지 않는 경우에는 맞춤법 검사를 사용하지 마세요. 예를 들어 암호, 전화 번호 또는 이름 입력란에 대해서는 맞춤법 검사를 켜지 마세요. 이러한 컨트롤에서는 맞춤법 검사가 기본적으로 사용되지 않습니다.
-   현재 맞춤법 검사 엔진에서 앱 언어를 지원하지 않는다는 이유만으로 맞춤법 검사를 사용하지 않도록 설정하지 마세요. 맞춤법 검사에서 언어를 지원하지 않는 경우 아무 작업도 하지 않으므로 옵션을 켜진 상태로 두어도 상관없습니다. IME(입력기)를 사용하여 다른 언어를 앱에 입력하는 경우도 있는데, 그 언어가 지원될 수도 있습니다. 예를 들어 일본어 앱을 빌드할 때 맞춤법 검사 엔진이 현재 해당 언어를 인식하지 못하는 경우에도 맞춤법 검사를 끄지 마세요. 사용자가 영어 IME로 전환하여 앱에 영어를 입력할 수도 있습니다. 맞춤법 검사가 활성화되면 영어 맞춤법 검사가 실행됩니다.

## <span id="Additional_usage_guidance"></span><span id="additional_usage_guidance"></span><span id="ADDITIONAL_USAGE_GUIDANCE"></span>추가 사용법 지침


Windows 스토어 앱은 **contentEditable** 속성이 **true**로 설정된 요소, 여러 줄 및 한 줄 텍스트 입력란에 대해 기본 제공 맞춤법 검사 기능을 제공합니다. 다음은 기본 제공 맞춤법 검사를 보여 주는 예제입니다.

![기본 제공 맞춤법 검사](images/spellchecking.png)

자세한 내용은 [**TextBox 클래스**](https://msdn.microsoft.com/library/windows/apps/br209683)를 참조하세요.

텍스트 입력 컨트롤을 이용한 맞춤법 검사는 다음 두 가지 목적에 사용합니다.

-   **오타 자동 수정**

    맞춤법 검사 엔진은 수정에 대한 확신이 있을 경우 철자가 틀린 단어를 자동으로 수정합니다. 예를 들어, 'teh'를 'the'로 자동 수정합니다.

-   **대신할 철자 표시**

    맞춤법 검사 엔진이 수정에 대한 확신이 없으면 철자가 틀린 단어 밑에 빨간 선을 추가하며, 이 단어를 누르거나 마우스 오른쪽 단추로 클릭하면 대신할 철자들이 상황에 맞는 메뉴에 표시됩니다.

JavaScript 컨트롤의 경우 여러 줄 텍스트 입력 컨트롤에 대해서는 맞춤법 검사가 기본적으로 켜져 있고 한 줄 입력 컨트롤에 대해서는 꺼져 있습니다. 한 줄 입력 컨트롤에서는 컨트롤의 **spellcheck** 속성을 **true**로 설정하여 직접 켤 수 있습니다. 컨트롤의 **spellcheck** 속성을 **false**로 설정하여 맞춤법 검사를 사용하지 않도록 설정할 수도 있습니다.

XAML TextBox 컨트롤의 경우 맞춤법 검사가 기본적으로 꺼져 있습니다. **IsSpellCheckEnabled** 속성을 **true**로 설정하면 기능을 켤 수 있습니다.



## <span id="related_topics"></span>관련 문서

* [텍스트 및 텍스트 컨트롤](text-controls.md)
* [텍스트 입력에 대한 지침](https://msdn.microsoft.com/library/windows/apps/hh750315)
* [텍스트 및 입력 체계에 대한 지침](https://msdn.microsoft.com/library/windows/apps/hh700394) 
           **개발자용(XAML)**
* [**TextBox.IsSpellCheckEnabled 속성**](https://msdn.microsoft.com/library/windows/apps/br209688)
* [**TextBox 클래스**](https://msdn.microsoft.com/library/windows/apps/br209683)

 






<!--HONumber=May16_HO2-->



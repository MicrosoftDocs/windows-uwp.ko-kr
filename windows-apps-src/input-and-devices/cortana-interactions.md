---
author: Karl-Bridge-Microsoft
Description: "외부 응용 프로그램을 시작하고 해당 응용 프로그램 내에서 단일 작업을 실행하는 음성 명령으로 Cortana의 기본 기능을 확장하세요."
title: "Cortana 조작"
ms.assetid: 4C11A7CF-DA26-4CA1-A9B9-FE52670101F5
label: Cortana
template: detail.hbs
ms.sourcegitcommit: a2ec5e64b91c9d0e401c48902a18e5496fc987ab
ms.openlocfilehash: d23416ad3344a39c09078b6ba3acc38fa3ba65a0

---

# UWP 앱에서 Cortana 조작




외부 응용 프로그램을 시작하고 해당 응용 프로그램 내에서 단일 작업을 실행하는 음성 명령으로 **Cortana**의 기본 기능을 확장하세요. 


**다른 음성 구성 요소**

-   음성 인식과 텍스트 음성 변환(TTS 또는 음성 합성이라고도 함)을 앱에 직접 통합할 경우 [음성 디자인 지침](speech-interactions.md)을 참조하세요.

> **참고**  
> 음성 명령은 **Cortana**를 통해 설치된 앱에 대한 특정한 의도가 있는 한 번의 말하기로, VCD(음성 명령 정의) 파일에 정의되어 있습니다.

> VCD 파일은 각각 고유한 의도가 있는 음성 명령을 하나 이상 정의합니다.

> 음성 명령 정의는 복잡성에 따라 달라질 수 있습니다. 또한 한 마디의 제한된 말하기부터 동일한 의도를 모두 나타내는 더욱 유연하고 다양한 자연어 표현에 이르기까지 다양한 말하기를 지원할 수 있습니다.


조작의 복잡성에 따라 대상 앱은 포그라운드(포커스가 앱에 있고 **Cortana**가 해제됨)에서 시작되거나 백그라운드(**Cortana**가 포커스를 유지하지만 앱의 결과를 제공함)에서 활성화될 수 있습니다. 예를 들어 추가 컨텍스트 또는 사용자 입력이 필요한 음성 명령은 포그라운드 앱에서 가장 잘 처리할 수 있지만, 기본 명령은 백그라운드 앱을 통해 **Cortana**에서 처리할 수 있습니다.

 

앱의 기본 기능을 통합하고 사용자가 앱을 직접 열지 않고도 대부분의 작업을 수행할 수 있는 중앙 진입점을 제공하면 **Cortana**가 앱과 사용자 간의 연락을 담당하는 역할을 할 수 있습니다. 앱 기능에 대한 이 바로 가기를 제공하고 앱을 전환할 필요를 줄임으로써 사용자의 많은 시간과 노력을 절약할 수 있습니다.


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">문서</th>
<th align="left">설명</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[디자인 지침](cortana-design-guidelines.md)</p></td>
<td align="left"><p>이러한 지침 및 권장 사항에서는 앱에서 **Cortana**를 가장 잘 활용하여 사용자와 상호 작용하고 사용자가 작업을 수행하는 데 도움을 주며 어떻게 모든 작업이 수행되는지를 명확하게 전달할 수 있는 방법에 대해 설명합니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[음성 명령으로 포그라운드 앱 시작](launch-a-foreground-app-with-voice-commands-in-cortana.md)</p></td>
<td align="left"><p><strong>Cortana</strong> 내에서 음성 명령을 사용하여 시스템 기능에 액세스할 수 있을 뿐만 아니라, <strong>Cortana</strong>를 통해 음성 명령을 사용하여 포그라운드 앱을 시작하고 앱에서 실행할 작업 또는 명령을 지정할 수도 있습니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[VCD 구 목록을 동적으로 수정](dynamically-modify-voice-command-definition--vcd--phrase-lists.md)</p></td>
<td align="left"><p>런타임 시 음성 인식 결과를 사용하여 VCD 파일의 지원되는 구 목록(<strong>PhraseList</strong> 요소)에 액세스하여 이를 업데이트 하는 방법을 알아봅니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[음성 명령으로 백그라운드 앱 시작](launch-a-background-app-with-voice-commands-in-cortana.md)</p></td>
<td align="left"><p><strong>Cortana</strong> 내에서 음성 명령을 사용하여 시스템 기능에 액세스하는 것 외에, 앱 내에서 실행할 작업 또는 명령을 지정하는 음성 명령을 사용하여 백그라운드 앱의 기능으로 <strong>Cortana</strong>를 확장할 수도 있습니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[백그라운드 앱 조작](interact-with-a-background-app-in-cortana.md)</p></td>
<td align="left"><p>사용자가 음성 명령을 실행하는 동안 <strong>Cortana</strong> 음성 및 캔버스를 통해 백그라운드 앱을 조작할 수 있는 방법을 알아봅니다.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[백그라운드 앱으로의 딥 링크](deep-link-into-your-app-from-cortana.md)</p></td>
<td align="left"><p>백그라운드 앱 서비스의 딥 링크를 <strong>Cortana</strong>에 제공하여 특정 상태 또는 컨텍스트에서 앱을 포그라운드로 시작합니다.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[자연어 음성 명령 지원](support-natural-language-voice-commands-in-cortana.md)</p></td>
<td align="left"><p>사용자가 명령의 어느 위치에서든 앱 이름을 말할 수 있도록 더 유연하고 자연스러운 음성 명령으로 <strong>Cortana</strong>를 확장하는 방법을 알아봅니다.</p></td>
</tr>
</tbody>
</table>

 

## 관련 문서


* [**VCD 요소 및 특성 v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

**디자이너**
* [음성 디자인 지침](https://msdn.microsoft.com/library/windows/apps/dn596121)
* [Cortana 디자인 지침](https://msdn.microsoft.com/library/windows/apps/dn974233)

**샘플**
* [Cortana 음성 명령 샘플](http://go.microsoft.com/fwlink/p/?LinkID=619899)
 

 







<!--HONumber=Jun16_HO5-->



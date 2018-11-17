---
author: TylerMSFT
title: ms-tonepicker 스키마
description: 이 항목에서는 ms-tonepicker URI 스키마에 대해 설명하고 이를 사용해 톤 선택기를 표시하여 톤을 선택하고, 톤을 저장하고, 톤의 식별 이름을 가져오는 방법을 설명합니다.
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 0c17e4fb-7241-4da9-b457-d6d3a7aefccb
ms.localizationpriority: medium
ms.openlocfilehash: 61967f0aa81c49cb4e81b11bedac84c318be1840
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7162148"
---
# <a name="choose-and-save-tones-using-the-ms-tonepicker-uri-scheme"></a>ms-tonepicker URI 체계를 사용하여 톤 선택 및 저장

이 항목에서는 **ms tonepicker:** URI 스키마를 사용하는 방법을 설명합니다. 이 URI 스키마는 다음 작업에 사용할 수 있습니다.
- 디바이스에서 톤 선택기를 사용할 수 있는지 확인합니다.
- 톤 선택기를 표시하여 사용 가능한 벨소리, 시스템 소리, 문자 알림음 및 알람 소리를 나열하고 사용자가 선택한 소리를 나타내는 톤 토큰을 가져옵니다.
- 사운드 파일 토큰을 입력으로 가져와서 디바이스에 저장하는 톤 보호기를 표시합니다. 저장된 톤은 톤 선택기를 통해 사용할 수 있습니다. 사용자는 톤에 식별 이름을 제공할 수도 있습니다.
- 톤 토큰을 해당 식별 이름으로 변환합니다.

## <a name="ms-tonepicker-uri-scheme-reference"></a>ms tonepicker: URI 스키마 참조

이 URI 스키마는 URI 스키마 문자열을 통해 인수를 전달하지 않지만 대신 [ValueSet](https://msdn.microsoft.com/library/windows/apps/windows.foundation.collections.valueset.aspx)를 통해 인수를 전달합니다. 모든 문자열은 대/소문자를 구분합니다.

아래 섹션은 지정된 작업을 수행하기 위해 전달해야 할 인수를 나타냅니다.

## <a name="task-determine-if-the-tone-picker-is-available-on-the-device"></a>작업: 디바이스에서 톤 선택기를 사용할 수 있는지 확인
```cs
var status = await Launcher.QueryUriSupportAsync(new Uri("ms-tonepicker:"),     
                                     LaunchQuerySupportType.UriForResults,
                                     "Microsoft.Tonepicker_8wekyb3d8bbwe");

if (status != LaunchQuerySupportStatus.Available)
{
    // the tone picker is not available
}
```

## <a name="task-display-the-tone-picker"></a>작업: 톤 선택기 표시

톤 선택기를 표시하기 위해 전달할 수 있는 인수는 다음과 같습니다.

| 매개 변수 | 형식 | 필수 | 가능한 값 | 설명 |
|-----------|------|----------|-------|-------------|
| 액션 | 문자열 | yes | "PickRingtone" | 톤 선택기가 열립니다. |
| CurrentToneFilePath | 문자열 | 아니요 | 기존 톤 토큰입니다. | 톤 선택기에서 현재 톤으로 표시할 톤입니다. 이 값을 설정하지 않으면 목록에서 첫 번째 톤이 기본적으로 선택됩니다.<br>이 값은 엄밀히 말해 파일 경로가 아닙니다. 톤 선택기에서 반환된 `ToneToken` 값에서 `CurrenttoneFilePath`에 적합한 값을 가져올 수 있습니다.  |
| TypeFilter | 문자열 | 아니요 | "Ringtones", "Notifications", "Alarms", "None" | 선택기에 추가할 톤을 선택합니다. 필터가 지정되지 않은 경우에는 모든 톤이 표시됩니다. |

[LaunchUriResults.Result](https://msdn.microsoft.com/library/windows/apps/windows.system.launchuriresult.result.aspx)에 반환되는 값은 다음과 같습니다.

| 반환 값 | 형식 | 가능한 값 | 설명 |
|--------------|------|-------|-------------|
| 결과 | Int32 | 0-성공했습니다. <br>1-취소되었습니다. <br>7-잘못된 매개 변수입니다. <br>8-필터 조건과 일치하는 톤이 없습니다. <br>255-지정한 작업이 구현되지 않았습니다. | 선택기 작업의 결과입니다. |
| ToneToken | 문자열 | 선택한 톤의 토큰입니다. <br>사용자가 선택기에서 **기본값**을 선택할 경우 문자열은 비어 있습니다. | 이 토큰은 알림 메시지 페이로드에 사용되거나 연락처의 벨소리 또는 문자 알림음으로 할당될 수 있습니다. 매개 변수는 **Result**가 0일 경우에만 ValueSet에 반환됩니다. |
| DisplayName | 문자열 | 지정된 톤의 식별 이름입니다. | 선택한 톤을 나타내기 위해 사용자에게 표시할 수 있는 문자열입니다. 매개 변수는 **Result**가 0일 경우에만 ValueSet에 반환됩니다. |


**예: 사용자가 톤을 선택할 수 있도록 톤 선택기 열기**

``` cs
LauncherOptions options = new LauncherOptions();
options.TargetApplicationPackageFamilyName = "Microsoft.Tonepicker_8wekyb3d8bbwe";

ValueSet inputData = new ValueSet() {
    { "Action", "PickRingtone" },
    { "TypeFilter", "Ringtones" } // Show only ringtones
};

LaunchUriResult result = await Launcher.LaunchUriForResultsAsync(new Uri("ms-tonepicker:"), options, inputData);

if (result.Status == LaunchUriStatus.Success)
{
     Int32 resultCode =  (Int32)result.Result["Result"];
     if (resultCode == 0)
     {
         string token = result.Result["ToneToken"] as string;
         string name = result.Result["DisplayName"] as string;
     }
     else
     {
           // handle failure
     }
}
```

## <a name="task-display-the-tone-saver"></a>작업: 톤 보호기 표시

톤 보호기를 표시하기 위해 전달할 수 있는 인수는 다음과 같습니다.

| 매개 변수 | 형식 | 필수 | 가능한 값 | 설명 |
|-----------|------|----------|-------|-------------|
| 액션 | 문자열 | yes | "SaveRingtone" | 벨소리를 저장할 선택기를 엽니다. |
| ToneFileSharingToken | 문자열 | yes | 저장할 벨소리 파일에 대한 [SharedStorageAccessManager](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.sharedstorageaccessmanager.aspx) 파일 공유 토큰입니다. | 특정 사운드 파일을 벨소리로 저장합니다. 파일에 지원되는 콘텐츠 형식은 mpeg 오디오 및 x-ms-wma 오디오입니다. |
| DisplayName | 문자열 | 아니요 | 지정된 톤의 식별 이름입니다. | 지정한 벨소리를 저장할 때 사용할 표시 이름을 설정합니다. |

[LaunchUriResults.Result](https://msdn.microsoft.com/library/windows/apps/windows.system.launchuriresult.result.aspx)에 반환되는 값은 다음과 같습니다.

| 반환 값 | 형식 | 가능한 값 | 설명 |
|--------------|------|-------|-------------|
| 결과 | Int32 | 0-성공했습니다.<br>1-사용자에 의해 취소되었습니다.<br>2-잘못된 파일입니다.<br>3-잘못된 파일 콘텐츠 형식입니다.<br>4-파일이 벨소리 최대 크기(Windows 10에서 1MB)를 초과합니다.<br>5-파일이 40초의 시간 제한을 초과합니다.<br>6-파일이 디지털 권한 관리로 보호됩니다.<br>7-잘못된 매개 변수입니다. | 선택기 작업의 결과입니다. |

**예: 벨소리로 로컬 음악 파일 저장**

``` cs
LauncherOptions options = new LauncherOptions();
options.TargetApplicationPackageFamilyName = "Microsoft.Tonepicker_8wekyb3d8bbwe";

ValueSet inputData = new ValueSet() {
    { "Action", "SaveRingtone" },
    { "ToneFileSharingToken", SharedStorageAccessManager.AddFile(myLocalFile) }
};

LaunchUriResult result = await Launcher.LaunchUriForResultsAsync(new Uri("ms-tonepicker:"), options, inputData);

if (result.Status == LaunchUriStatus.Success)
{
     Int32 resultCode = (Int32)result.Result["Result"];

     if (resultCode == 0)
     {
         // no issues
     }
     else
     {
          switch (resultCode)
          {
             case 2:
              // The specified file was invalid
              break;
              case 3:
              // The specified file's content type is invalid
              break;
              case 4:
              // The specified file was too big
              break;
              case 5:
              // The specified file was too long
              break;
              case 6:
              // The file was protected by DRM
              break;
              case 7:
              // The specified parameter was incorrect
              break;
          }
      }
 }
```

## <a name="task-convert-a-tone-token-to-its-friendly-name"></a>작업: 해당 식별 이름으로 톤 토큰 변환

톤의 식별 이름을 가져오기 위해 전달할 수 있는 인수는 다음과 같습니다.

| 매개 변수 | 형식 | 필수 | 가능한 값 | 설명 |
|-----------|------|----------|-------|-------------|
| 액션 | 문자열 | yes | "GetToneName" | 톤의 식별 이름을 가져올 것인지를 나타냅니다. |
| ToneToken | 문자열 | yes | 톤 토큰 | 표시 이름을 가져올 톤 토큰입니다. |

[LaunchUriResults.Result](https://msdn.microsoft.com/library/windows/apps/windows.system.launchuriresult.result.aspx)에 반환되는 값은 다음과 같습니다.

| 반환 값 | 형식 | 가능한 값 | 설명 |
|--------------|------|-------|-------------|
| 결과 | Int32 | 0-선택기 작업이 성공했습니다.<br>7-잘못된 매개 변수입니다(예: 톤 토큰이 제공되지 않음).<br>9-지정한 토큰의 이름을 읽는 동안 오류가 발생했습니다.<br>10-지정한 톤 토큰을 찾을 수 없습니다. | 선택기 작업의 결과입니다.
| DisplayName | 문자열 | 톤의 식별 이름입니다. | 선택한 톤의 표시 이름을 반환합니다. 이 매개 변수는 **Result**가 0일 경우에만 ValueSet에 반환됩니다. |

**예: Contact.RingToneToken에서 톤 토큰을 검색하고 연락처 카드의 해당 식별 이름 표시**

```cs
using (var connection = new AppServiceConnection())
{
    connection.AppServiceName = "ms-tonepicker-nameprovider";
    connection.PackageFamilyName = "Microsoft.Tonepicker_8wekyb3d8bbwe";
    AppServiceConnectionStatus connectionStatus = await connection.OpenAsync();
    if (connectionStatus == AppServiceConnectionStatus.Success)
    {
        var message = new ValueSet() {
            { "Action", "GetToneName" },
            { "ToneToken", token)
        };
        AppServiceResponse response = await connection.SendMessageAsync(message);
        if (response.Status == AppServiceResponseStatus.Success)
        {
            Int32 resultCode = (Int32)response.Message["Result"];
            if (resultCode == 0)
            {
                string name = response.Message["DisplayName"] as string;
            }
            else
            {
                // handle failure
            }
        }
        else
        {
            // handle failure
        }
    }
}
```

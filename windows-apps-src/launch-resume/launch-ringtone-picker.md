---
title: ms-tonepicker 체계
description: 이 항목에서는 tonepicker URI 체계에 대해 설명 하 고이 체계를 사용 하 여 톤 선택을 표시 하 고 톤을 저장 하며 톤에 대 한 친숙 한 이름을 가져오는 방법을 설명 합니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 0c17e4fb-7241-4da9-b457-d6d3a7aefccb
ms.localizationpriority: medium
ms.openlocfilehash: a57ba895ae77eec3b2c5df2a7ae70864c6f701a1
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167877"
---
# <a name="choose-and-save-tones-using-the-ms-tonepicker-uri-scheme"></a>ms-tonepicker URI 체계를 사용하여 톤 선택 및 저장

이 항목에서는 **tonepicker:** URI 체계를 사용 하는 방법에 대해 설명 합니다. 이 URI 체계를 사용 하 여 다음을 수행할 수 있습니다.
- 장치에서 톤 선택을 사용할 수 있는지 확인 합니다.
- 사용 가능한 벨 소리, 시스템 소리, 텍스트 톤 및 알람 소리를 나열 하려면 톤 선택기를 표시 합니다. 사용자가 선택한 소리를 나타내는 톤 토큰을 가져옵니다.
- 사운드 파일 토큰을 입력으로 사용 하 여 장치에 저장 하는 톤 보호기를 표시 합니다. 저장 된 색조는 톤 선택기를 통해 사용할 수 있습니다. 사용자는 해당 톤에 친숙 한 이름을 지정할 수도 있습니다.
- 톤 토큰을 친숙 한 이름으로 변환 합니다.

## <a name="ms-tonepicker-uri-scheme-reference"></a>tonepicker: URI 체계 참조

이 URI 체계는 URI 체계 문자열을 통해 인수를 전달 하지 않고, 대신 [Valueset](/uwp/api/windows.foundation.collections.valueset)을 통해 인수를 전달 합니다. 모든 문자열은 대/소문자를 구분 합니다.

다음 섹션에서는 지정 된 작업을 수행 하기 위해 전달 해야 하는 인수를 지정 합니다.

## <a name="task-determine-if-the-tone-picker-is-available-on-the-device"></a>작업: 장치에서 톤 선택을 사용할 수 있는지 확인 합니다.
```cs
var status = await Launcher.QueryUriSupportAsync(new Uri("ms-tonepicker:"),     
                                     LaunchQuerySupportType.UriForResults,
                                     "Microsoft.Tonepicker_8wekyb3d8bbwe");

if (status != LaunchQuerySupportStatus.Available)
{
    // the tone picker is not available
}
```

## <a name="task-display-the-tone-picker"></a>작업: 성조 선택기를 표시 합니다.

톤 선택기를 표시 하기 위해 전달할 수 있는 인수는 다음과 같습니다.

| 매개 변수 | 형식 | 필수 | 가능한 값 | Description |
|-----------|------|----------|-------|-------------|
| 작업 | 문자열 | 예 | "PickRingtone" | 톤 선택기를 엽니다. |
| CurrentToneFilePath | 문자열 | no | 기존 톤 토큰입니다. | 톤 선택기에서 현재 톤으로 표시할 톤입니다. 이 값을 설정 하지 않으면 기본적으로 목록의 첫 번째 톤이 선택 됩니다.<br>이는 파일 경로를 엄격히 말하는 것이 아닙니다. `CurrenttoneFilePath`톤 선택에서 반환 된 값에서에 적합 한 값을 가져올 수 있습니다 `ToneToken` .  |
| TypeFilter | 문자열 | no | "벨 소리", "알림", "경보", "없음" | 선택에 추가할 색조를 선택 합니다. 필터를 지정 하지 않으면 모든 색조가 표시 됩니다. |

Launchuriresults에서 반환 된 값 [입니다. 결과](/uwp/api/windows.system.launchuriresult.result):

| 반환 값 | 유형 | 가능한 값 | Description |
|--------------|------|-------|-------------|
| 결과 | Int32 | 0-성공 <br>1-취소 됨. <br>7-잘못 된 매개 변수입니다. <br>8-필터 조건과 일치 하는 톤 없음 <br>255-지정 된 작업이 구현 되지 않았습니다. | 선택 작업의 결과입니다. |
| ToneToken | 문자열 | 선택한 톤의 토큰입니다. <br>사용자가 선택에서 **기본값** 을 선택 하면 문자열이 비어 있습니다. | 이 토큰은 알림 메시지 페이로드에 사용 되거나 연락처의 벨 이나 텍스트 톤으로 할당 될 수 있습니다. 이 매개 변수는 **결과가** 0 인 경우에만 valueset에서 반환 됩니다. |
| DisplayName | 문자열 | 지정 된 단추식 이름입니다. | 선택한 톤을 나타내기 위해 사용자에 게 표시할 수 있는 문자열입니다. 이 매개 변수는 **결과가** 0 인 경우에만 valueset에서 반환 됩니다. |


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

## <a name="task-display-the-tone-saver"></a>작업: 톤 보호기를 표시 합니다.

톤 보호기를 표시 하기 위해 전달할 수 있는 인수는 다음과 같습니다.

| 매개 변수 | 형식 | 필수 | 가능한 값 | Description |
|-----------|------|----------|-------|-------------|
| 작업 | 문자열 | 예 | "SaveRingtone" | 벨 소리를 저장할 선택기를 엽니다. |
| ToneFileSharingToken | 문자열 | 예 | 저장할 벨 소리 파일의 [SharedStorageAccessManager](/uwp/api/windows.applicationmodel.datatransfer.sharedstorageaccessmanager) 파일 공유 토큰입니다. | 특정 사운드 파일을 벨 소리로 저장 합니다. 파일에 대해 지원 되는 콘텐츠 형식은 mpeg audio 및 x-y 오디오입니다. |
| DisplayName | 문자열 | no | 지정 된 단추식 이름입니다. | 지정 된 벨을 저장할 때 사용할 표시 이름을 설정 합니다. |

Launchuriresults에서 반환 된 값 [입니다. 결과](/uwp/api/windows.system.launchuriresult.result):

| 반환 값 | 유형 | 가능한 값 | Description |
|--------------|------|-------|-------------|
| 결과 | Int32 | 0-성공<br>1-사용자가 취소 했습니다.<br>2-잘못 된 파일입니다.<br>3-잘못된 파일 콘텐츠 형식입니다.<br>4-파일이 최대 벨 소리 크기 (Windows 10의 경우 1MB)를 초과 합니다.<br>5-파일이 40 초 길이 제한을 초과 합니다.<br>6-디지털 권한 관리에 의해 파일이 보호 됩니다.<br>7-잘못 된 매개 변수입니다. | 선택 작업의 결과입니다. |

**예: 로컬 음악 파일을 벨 소리로 저장**

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

## <a name="task-convert-a-tone-token-to-its-friendly-name"></a>작업: 톤 토큰을 친숙 한 이름으로 변환 합니다.

톤의 이름을 가져오기 위해 전달할 수 있는 인수는 다음과 같습니다.

| 매개 변수 | 형식 | 필수 | 가능한 값 | Description |
|-----------|------|----------|-------|-------------|
| 작업 | 문자열 | 예 | "GetToneName" | 톤의 이름을 가져오려고 함을 나타냅니다. |
| ToneToken | 문자열 | 예 | 톤 토큰 | 표시 이름을 가져올 톤 토큰입니다. |

Launchuriresults에서 반환 된 값 [입니다. 결과](/uwp/api/windows.system.launchuriresult.result):

| 반환 값 | Type | 가능한 값 | Description |
|--------------|------|-------|-------------|
| 결과 | Int32 | 0-선택 작업이 성공 했습니다.<br>7-잘못 된 매개 변수 (예: 제공 된 ToneToken 없음)<br>9-지정 된 토큰의 이름을 읽는 동안 오류가 발생 했습니다.<br>10-지정 된 톤 토큰을 찾을 수 없습니다. | 선택 작업의 결과입니다.
| DisplayName | 문자열 | 단추식 이름입니다. | 선택한 톤의 표시 이름을 반환합니다. 이 매개 변수는 **결과가** 0 인 경우 valueset 에서만 반환 됩니다. |

**예: RingToneToken에서 톤 토큰을 검색 하 고 연락처 카드에 이름을 표시 합니다.**

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
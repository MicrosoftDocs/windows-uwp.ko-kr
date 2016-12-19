---
author: mcleanbyron
ms.assetid: 37F2C162-4910-4336-BEED-8536C88DCA65
description: "Windows 스토어 제출 API에서 다음 메서드를 사용하여 Windows 개발자 센터 계정에 등록된 앱을 위한 패키지 플라이트를 관리합니다."
title: "Windows 스토어 제출 API를 사용하여 패키지 플라이트 관리"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: de1c23d721cee67d813520e3a23eb553cd90b7e9

---

# Windows 스토어 제출 API를 사용하여 패키지 플라이트 관리




Windows 스토어 제출 API에서 다음 메서드를 사용하여 Windows 개발자 센터 계정에 등록된 앱을 위한 패키지 플라이트를 관리합니다. API 사용을 위한 필수 조건을 비롯하여 Windows 스토어 제출 API에 대한 자세한 내용은 [Windows 스토어 서비스를 사용하여 제출 만들기 및 관리](create-and-manage-submissions-using-windows-store-services.md)를 참조하세요.

>**참고**  이러한 메서드는 Windows 스토어 제출 API를 사용할 수 있는 권한을 가진 Windows 개발자 센터 계정에 대해서만 사용할 수 있습니다. 일부 계정은 이 권한을 사용할 수 없습니다. 이러한 메서드는 패키지 플라이트를 가져오거나, 만들거나, 삭제하는 데만 사용될 수 있습니다. 패키지 플라이트에 대한 제출을 만들려면 [패키지 플라이트 제출 관리](manage-flight-submissions.md)를 참조하세요.

| 메서드        | URI    | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}``` | Windows 개발자 센터 계정에 등록된 앱에 대한 패키지 플라이트의 데이터를 가져옵니다. 자세한 내용은 [패키지 플라이트 가져오기](get-a-flight.md)를 참조하세요. |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights``` | 새 패키지 플라이트를 만듭니다. 자세한 내용은 [패키지 플라이트 만들기](create-a-flight.md)를 참조하세요.|
| DELETE | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}``` | 패키지 플라이트를 삭제합니다. 자세한 내용은 [패키지 플라이트 삭제](delete-a-flight.md)를 참조하세요. |


## 필수 조건

아직 완료하지 않은 경우 이러한 메서드를 사용하기 전에 Windows 스토어 제출 API를 위한 [필수 조건](create-and-manage-submissions-using-windows-store-services.md#prerequisites)을 모두 완료합니다.

## 관련 항목

* [Windows 스토어 서비스를 사용하여 제출 만들기 및 관리](create-and-manage-submissions-using-windows-store-services.md)
* [패키지 플라이트 제출 관리](manage-flight-submissions.md)



<!--HONumber=Aug16_HO5-->



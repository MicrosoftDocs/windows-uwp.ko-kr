---
title: UWP 앱에서 MySQL 데이터베이스 사용
description: UWP 앱에서 MySQL 데이터베이스를 사용합니다.
ms.date: 3/28/2019
ms.topic: article
keywords: windows 10, uwp, MySQL, 데이터베이스
ms.localizationpriority: medium
ms.openlocfilehash: a7708ca082647aef6bbf2261922d2ebd6723923e
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/13/2019
ms.locfileid: "63785482"
---
# <a name="use-a-mysql-database"></a>MySQL 데이터베이스 사용
이 문서는 UWP 앱에서 MySQL 데이터베이스를 사용하도록 설정하는 데 필요한 단계를 포함하고 있습니다. 코드로 데이터베이스와 상호 작용하는 방법을 보여주는 작은 코드 조각도 포함되어 있습니다.

## <a name="set-up-your-solution"></a>솔루션 설정

앱을 MySQL 데이터베이스에 직접 연결하려면 프로젝트 최소 버전의 대상이 Fall Creators Update(빌드 16299)여야 합니다.  UWP 프로젝트의 속성 페이지에서 이에 대한 정보를 찾을 수 있습니다.

![Fall Creators Update에 설정된 대상 및 최소 버전을 보여주는 Visual Studio의 대상 속성 창 이미지](images/min-version-fall-creators.png)

**패키지 관리자 콘솔**을 엽니다(보기 -> 다른 Windows -> 패키지 관리자 콘솔). **Install-Package MySql.Data** 명령을 사용하여 MySQL용 드라이버를 설치합니다. 이렇게 하면 MySQL 데이터베이스에 프로그래밍 방식으로 액세스할 수 있습니다.

## <a name="test-your-connection-using-sample-code"></a>샘플 코드를 사용하여 연결 테스트
다음은 원격 MySQL 데이터베이스에 연결하여 데이터를 읽는 예제입니다. IP 주소, 자격 증명 및 데이터베이스 이름을 사용자 지정해야 합니다.

```csharp
string M_str_sqlcon = "server=10.xxx.xx.xxx;user id=foo;password=bar;database=baz";
MySqlConnection mysqlcon = new MySqlConnection(M_str_sqlcon);
MySqlCommand mysqlcom = new MySqlCommand("select * from table1", mysqlcon);
mysqlcon.Open();
MySqlDataReader mysqlread = mysqlcom.ExecuteReader(CommandBehavior.CloseConnection);
while (mysqlread.Read())
{
    Debug.WriteLine(mysqlread.GetString(0)+":"+mysqlread.GetString(1));
}
mysqlcon.Close();
```
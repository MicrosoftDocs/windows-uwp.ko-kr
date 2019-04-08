---
title: 고객 데이터베이스 응용 프로그램 구조
description: 고객 데이터베이스 자습서의 구조를 검토 및 얼마나를 생성 하는 이유입니다.
keywords: enterprise, 자습서, 고객, 데이터, crud
ms.date: 05/07/2018
ms.topic: article
ms.localizationpriority: med
ms.openlocfilehash: b1f8f8c8a2fd1522d8c304a45514d5257543f222
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656618"
---
# <a name="customer-database-app-structure"></a>고객 데이터베이스 응용 프로그램 구조

복잡 한 기간 업무 앱 경우가 많은 페이지 기능 및 뛰어난 코드 줄 수입니다. 이 인해 것 없어서는 예측 가능한 구조 앱을 디자인 합니다. 엔터프라이즈 앱에 적합 여러 응용 프로그램 디자인 패턴이 있지만 이러한 모든 기반으로 작성 된 대규모 응용 프로그램을 보다 쉽게 이해 하 고 사용 하 여 작업의 목표입니다.

하는 동안 합니다 [고객 데이터베이스 자습서](customer-database-tutorial.md) 단일 페이지 앱을 표시 작업에서 이러한 아이디어를 보여 주기 위해-Model-view-viewmodel (MVVM) 앱 디자인 패턴은 단순 함을 위해 구현 합니다. 이름에서 알 수 있듯이 MVVM 디자인 패턴은 핵심 앱 논리를 세 가지 범주로 구분 합니다.

* 모델은 응용 프로그램의 데이터를 포함 하는 클래스입니다.
* 뷰는 지정 된 페이지의 UI입니다.
* Viewmodel 응용 프로그램 논리를 제공합니다. 이 보기에서 사용자 작업을 처리 및/또는 모델을 사용 하 여 상호 작용을 관리 포함할 수 있습니다.

이 앱에 완벽 하 게 및 architypical MVVM 예가 아니지만, 실행 중인 중요 한 부분의 분리의 기본 원칙 표시지 않습니다. [이 앱을 확인해 보십시오.](https://github.com/Microsoft/windows-tutorials-customer-database)

## <a name="application-structure"></a>응용 프로그램 구조

앱을 연 후 검사 하 여 시작 된 **솔루션 탐색기.** 이전에 UWP 앱을 사용 하 여 했었다면 친숙 한 있습니다 있어야 하지만 폴더의 컬렉션을 볼 수도 표시 되는 내용 중 일부를 보유 하는 앱의 구성 요소입니다.

![솔루션 탐색기의 시작 위치는 앱](images/customer-database-tutorial/solution-explorer.png)

### <a name="views"></a>보기

모든 앱의 UI는 Views 폴더에 정의 됩니다. 하나의 뷰가-즉 이기 때문에 자습서는 단일 페이지 앱 지금 **CustomerListPage**합니다. XAML UI 태그와 코드 숨김 xaml.cs 있기-이 두 파일 하나 보기를 확인 합니다. UI 요소를 추가할 것 **CustomerListPage.xaml**합니다.

> [!NOTE]
> 이 앱에는 MainPage 없는 있는지 확인할 수 있습니다. 에 지정 하기 때문에 이것이 **App.xaml.cs** 앱을 시작 해야 하는 **CustomerListPage** 시작 시.

### <a name="viewmodels"></a>Viewmodel

이 앱에는 하나의 보기에 있는 경우는 두 Viewmodel에 있습니다. 이 이유는 무엇입니까?

**CustomerListPageViewModel.cs** MVVM 패턴의 표준 ViewModel 됩니다. 위치는 기본 논리 앱의 페이지의, 및 페이지를 사용 하 게 됩니다 자습서의 대부분을 사용 하 여는 것입니다. 사용자가 수행한 모든 UI 작업 처리를 위해이 ViewModel에 뷰를 통해 전달 됩니다.

**그러나 CustomerViewModel.cs**합니다 특정 보기를 사용 하 여 연결 되지 않습니다. 대신 개별 고객의 모델에 포함 된 데이터를 사용 하 여 프로그래밍 개념 (속성 편집 되었습니다)을 연결 합니다.

### <a name="models"></a>모델

이 앱에는 앱의 데이터를 저장 하 고 리포지토리와 상호 작용 하기 위한 인터페이스를 제공 하는 세 가지 모델을 포함 합니다. 이러한 값은 앱의 중요 한 부분 이지만 없는에서는 직접 편집 하는 자습서는 합니다.

가장 중요 **Customer.cs**, 자습서에서 사용 하고자 하는 고객 데이터 구조를 설명 하는 합니다.

> [!NOTE]
> 자습서를 무시 합니다 *전자 메일* 및 *Phone* 고객 개체의 속성입니다. 이상의 기능을 제공 하려는 경우 좋은 첫 번째 단계는 표시 되는 내용을 여기에 앱의 UI에 이러한 두 속성을 추가 합니다.

### <a name="repository"></a>리포지토리

리포지토리 폴더 생성 하 고 로컬 SQLite 데이터베이스와 상호 작용 하는 클래스를 포함 합니다. 자습서에서는 SQLite 데이터베이스도 표시 됩니다-됩니다. 코드에 추가 될 수 있지만 **CustomerListPageViewModel.cs** 이러한 클래스에 의해 정의 된 메서드를 호출 하려면 설정 하기 위해 변경할 필요가 없습니다.

SQLite UWP에 대 한 자세한 내용은 [이 문서를 참조 하세요.](../data-access/sqlite-databases.md)합니다.

자습서의 "계속" 섹션을 시도 하면 원격 나머지 데이터베이스에 연결 하는 클래스를 만들어야 하는 것이 이것이입니다. 또한를 구현 하는 **ICustomerRepository** 인터페이스 모델 섹션에 정의 되었지만 SQLite 상응 하는 매우 다르게 표시 됩니다.

### <a name="other-elements"></a>다른 요소

응용 프로그램 시작 동작에 정의 된 UWP 앱에 대 한 일반적인 경우는 **App.xaml.cs** 클래스입니다. 여기에서는 코드의 대부분은 모든 UWP 앱에 대 한 기본 코드입니다. 하지만 몇 가지 작은 변경을 이미 만들었습니다.

* 앱 표시 되도록 지정 했습니다 **CustomerListPage** 시작 합니다.
* 데이터 원본을 사용 하는 것을 포함 하는 리포지토리 개체를 만들었습니다.
* 추가한를 **SQLiteDatabase** 메서드, 로컬 데이터베이스를 초기화 하 고 지정 된 리포지토리로 설정 합니다.

"계속" 섹션을 시도 하면 REST 리포지토리 개체를 초기화 하는 비슷한 메서드를 추가 합니다. 이 문제를 분리 했습니다 고 SQLite 및 REST 작업에 대 한 동일한 정의 된 인터페이스를 사용 하는이 앱에서 SQLite 대신 REST를 사용 하도록 변경 해야 합니다. 기존 코드만 됩니다.

## <a name="next-steps"></a>다음 단계

자습서를 이미 완료 했다고 경우 체크 아웃할 수 있습니다 합니다 [전체 샘플 앱](https://github.com/Microsoft/Windows-appsample-customers-orders-database) 규모가 큰이 기능이 구현 되는 방법 확인.

그렇지 않은 경우 이제 이유 위치는 모든 것을 알면 해야 하는 [자습서로 반환](customer-database-tutorial.md) 와 다루었습니다는 구조를 함께 작동 합니다.
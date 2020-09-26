---
title: Windows Hello
description: 이 문서에서는 Windows 10 운영 체제의 일부로 제공 되는 새로운 Windows Hello 기술에 대해 설명 하 고, 개발자가이 기술을 구현 하 여 UWP (유니버설 Windows 플랫폼) 앱 및 백 엔드 서비스를 보호 하는 방법을 설명 합니다. 기본 자격 증명을 사용 하 여 발생 하는 위협을 완화 하는 데 도움이 되는 이러한 기술의 특정 기능을 강조 하 고, Windows 10 롤아웃의 일부로 이러한 기술을 디자인 하 고 배포 하는 방법을 안내 합니다.
ms.assetid: 0B907160-B344-4237-AF82-F9D47BCEE646
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 보안
ms.localizationpriority: medium
ms.openlocfilehash: ecb35a262f02576f0425460c8e27ce0016a08d1a
ms.sourcegitcommit: 651a6b9769fad1736ab16e2a4e423258889b248e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91366899"
---
# <a name="windows-hello"></a>Windows Hello

이 문서에서는 Windows 10 운영 체제의 일부로 제공 되는 새로운 Windows Hello 기술에 대해 설명 하 고, 개발자가이 기술을 구현 하 여 UWP (유니버설 Windows 플랫폼) 앱 및 백 엔드 서비스를 보호 하는 방법을 설명 합니다. 기본 자격 증명을 사용 하 여 발생 하는 위협을 완화 하는 데 도움이 되는 이러한 기술의 특정 기능을 강조 하 고, Windows 10 롤아웃의 일부로 이러한 기술을 디자인 하 고 배포 하는 방법을 안내 합니다.

이 문서에서는 앱 개발에 중점을 둡니다. Windows Hello의 아키텍처 및 구현 세부 정보에 대 한 자세한 내용은 [TechNet의 Windows Hello 가이드](/windows/keep-secure/microsoft-passport-guide)를 참조 하십시오.

전체 코드 샘플은 [GitHub의 Windows Hello 코드 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MicrosoftPassport)을 참조 하세요.

Windows Hello 및 지원 인증 서비스를 사용 하 여 UWP 앱을 만드는 방법에 대 한 단계별 연습은 [Windows hello 로그인 앱](microsoft-passport-login.md) 및 [windows hello 로그인 서비스](microsoft-passport-login-auth-service.md) 문서를 참조 하세요.

## <a name="1-introduction"></a>1 소개

정보 보안에 대 한 기본적인 가정은 시스템이이를 사용 하는 사용자를 식별할 수 있다는 것입니다. 사용자를 식별 하면 시스템에서 사용자가 적절 하 게 식별 되었는지 (인증으로 알려진 프로세스) 여부를 결정 한 다음 적절 하 게 인증 된 사용자가 수행할 수 있는 작업 (권한 부여)을 결정할 수 있습니다. 전 세계에 배포 된 대부분의 컴퓨터 시스템은 인증 및 권한 부여 결정을 내리는 데 필요한 사용자 자격 증명에 따라 달라 지 며, 이러한 시스템은 보안을 위한 기반으로 재사용 가능한 사용자 생성 암호에 의존 합니다. Oft에서 언급 된 최대 인증에는 "사용자가 알고 있는 항목, 사용자가 보유 하 고 있는 것이 문제를 깔끔하게 강조 표시할 수 있습니다. 재사용 가능한 암호는 자체적으로 모두 인증 요소 이므로 암호를 알고 있는 사용자는 해당 암호를 소유 하는 사용자를 가장할 수 있습니다.

### <a name="11-problems-with-traditional-credentials"></a>1.1 기존 자격 증명과 관련 된 문제

1960s 이전에는 김철수 Corbató와 해당 팀이 암호를 도입 하는 경우 사용자와 관리자는 사용자 인증 및 권한 부여에 대 한 암호 사용을 처리 해야 했습니다. 시간이 지남에 따라 암호 저장 및 사용에 대 한 아트의 상태는 다소 고급 이지만 (예: 보안 해시 및 솔트) 여전히 두 가지 문제에 직면 하 고 있습니다. 암호는 쉽게 복제할 수 있으며 쉽게 도용할 수 있습니다. 또한 구현 오류는 안전 하 게 렌더링할 수 있으며, 사용자는 하드 시간 균형 조정과 보안을 유지 합니다.

#### <a name="111-credential-theft"></a>1.1.1 자격 증명 도난

암호의 가장 큰 위험은 간단 합니다. 공격자가 쉽게 도용할 수 있습니다. 암호를 입력 하거나 처리 하거나 저장 하는 모든 장소는 취약 합니다. 예를 들어 공격자는 응용 프로그램 서버에 대 한 네트워크 트래픽을 도청 하거나, 응용 프로그램 또는 장치에서 맬웨어를 implanting 하거나, 장치에 사용자 키 입력을 기록 하거나, 사용자가 입력 한 문자를 확인 하 여 인증 서버에서 암호나 해시 컬렉션을 도용할 수 있습니다. 가장 일반적인 공격 방법입니다.

또 다른 관련 된 위험은 공격자가 안전 하지 않은 네트워크에서 도청 하 여 유효한 자격 증명을 캡처한 후 나중에 유효한 사용자를 가장 하기 위해 재생 하는 자격 증명 재생의 경우입니다. Kerberos 및 OAuth를 비롯 한 대부분의 인증 프로토콜은 자격 증명 교환 프로세스에 타임 스탬프를 포함 하 여 재생 공격 으로부터 보호 하지만,이 방법은 사용자가 첫 번째 장소에서 티켓을 가져오기 위해 제공 하는 암호가 아니라 인증 시스템에서 발행 하는 토큰만 보호 합니다.

#### <a name="112-credential-reuse"></a>1.1.2 자격 증명 재사용

전자 메일 주소를 사용자 이름으로 사용 하는 일반적인 방법으로 인해 잘못 된 문제가 발생할 수 있습니다. 손상 된 시스템에서 사용자 이름-암호 쌍을 성공적으로 복구한 공격자는 다른 시스템에서 동일한 쌍을 시도할 수 있습니다. 이 기법은 공격자가 손상 된 시스템에서 다른 시스템으로 springboard 수 있도록 하는 경우가 많습니다. 전자 메일 주소를 사용자 이름으로 사용 하면이 가이드의 뒷부분에서 살펴볼 추가 문제가 발생할 수 있습니다.

### <a name="12-solving-credential-problems"></a>1.2 자격 증명 문제 해결

암호가 야기 되는 문제를 해결 하는 것은 어려운 문제입니다. 암호 정책만을 단독으로 수행 하는 것은 아닙니다. 사용자는 암호를 재활용, 공유 또는 기록할 수 있습니다. 사용자 교육은 인증 보안을 위해 중요 하지만 교육용 으로만 문제를 제거 하지는 않습니다.

Windows Hello는 기존 자격 증명을 확인 하 고 생체 인식 또는 PIN 기반 사용자 제스처로 보호 하는 장치 관련 자격 증명을 만들어 암호를 강력한 2 단계 인증 (2FA)으로 바꿉니다. 


## <a name="2-what-is-windows-hello"></a>2 Windows Hello 란?

Windows Hello는 Windows 10에 기본 제공 되는 새로운 생체 인식 로그인 시스템에 Microsoft가 제공 하는 이름입니다. Windows Hello는 운영 체제에 직접 빌드되기 때문에 사용자 장치의 잠금을 해제 하는 데 얼굴 또는 지문 id를 허용 합니다. 사용자가 장치 관련 자격 증명에 액세스 하는 고유한 생체 인식 식별자를 제공할 때 인증이 발생 합니다. 즉, 공격자가 PIN을 사용 하지 않는 한 장치를 도용 하는 공격자가 로그온 할 수 없습니다. Windows 보안 자격 증명 저장소는 장치에서 생체 인식 데이터를 보호 합니다. Windows Hello를 사용 하 여 장치를 잠금 해제 하면 권한 있는 사용자가 모든 Windows 환경, 앱, 데이터, 웹 사이트 및 서비스에 대 한 액세스 권한을 얻게 됩니다.

Windows Hello 인증자를 Hello라고 합니다. Hello는 개별 장치와 특정 사용자의 조합에 대해 고유 합니다. 장치 간에 로밍되지 않으며, 서버 또는 호출 앱과 공유 되지 않으며, 장치에서 쉽게 추출할 수 없습니다. 여러 사용자가 장치를 공유 하는 경우 각 사용자는 자신의 계정을 설정 해야 합니다. 모든 계정은 해당 장치에 대 한 고유한 Hello를 가져옵니다. Hello는 저장 된 자격 증명의 잠금을 해제 하거나 해제 하는 데 사용할 수 있는 토큰으로 간주할 수 있습니다. Hello 자체는 앱 또는 서비스를 인증 하지는 않지만 가능한 자격 증명을 해제 합니다. 즉, Hello는 사용자 자격 증명이 아니지만 인증 프로세스의 두 번째 단계입니다.

### <a name="21-windows-hello-authentication"></a>2.1 Windows Hello 인증

Windows Hello는 장치에서 개별 사용자를 인식할 수 있는 강력한 방법을 제공 하며,이를 통해 사용자와 요청 된 서비스 또는 데이터 항목 간의 경로에서 첫 번째 부분의 주소를 확인할 수 있습니다. 장치에서 사용자를 인식 한 후에도 요청 된 리소스에 대 한 액세스 권한을 부여할지 여부를 결정 하기 전에 사용자를 인증 해야 합니다. Windows Hello는 Windows에 완전히 통합 되 고 재사용 가능한 암호를 특정 장치 및 생체 인식 제스처 또는 PIN의 조합으로 대체 하는 강력한 2FA를 제공 합니다.

그러나 Windows Hello는 기존 2FA 시스템을 대체 하는 것이 아닙니다. 이는 개념적으로 스마트 카드와 유사 합니다. 인증은 문자열 비교 대신 암호화 기본 형식을 사용 하 여 수행 되 고 사용자의 키 자료는 변조 방지 하드웨어 내에서 안전 합니다. Windows Hello에는 스마트 카드 배포에 필요한 추가 인프라 구성 요소가 필요 하지 않습니다. 특히 인증서를 관리 하기 위한 PKI (공개 키 인프라)가 필요 하지 않은 경우 인증서를 관리 하지 않아도 됩니다. Windows Hello는 스마트 카드의 주요 이점 (가상 스마트 카드에 대 한 배포 유연성 및 실제 스마트 카드에 대 한 강력한 보안)을 단점 없이 결합 합니다.

### <a name="22-how-windows-hello-works"></a>2.2 Windows Hello 작동 방법

사용자가 자신의 컴퓨터에서 Windows Hello를 설정 하면 장치에 새로운 공개-개인 키 쌍이 생성 됩니다. TPM ( [신뢰할 수 있는 플랫폼 모듈](/windows/keep-secure/trusted-platform-module-overview) )은이 개인 키를 생성 하 고 보호 합니다. 장치에 TPM 칩이 없는 경우 개인 키는 소프트웨어에 의해 암호화 되 고 보호 됩니다. 또한 TPM 사용 장치는 키가 TPM에 바인딩되어 있음을 증명 하는 데 사용할 수 있는 데이터 블록을 생성 합니다. 이 증명 정보를 솔루션에서 사용 하 여 사용자에 게 다른 권한 부여 수준을 부여할지 여부를 결정할 수 있습니다.

장치에서 Windows Hello를 사용 하도록 설정 하려면 사용자에 게 Windows 설정에서 연결 된 Azure Active Directory 계정 또는 Microsoft 계정이 있어야 합니다.

#### <a name="221-how-keys-are-protected"></a>키를 보호 하는 방법 2.2.1

키 자료가 생성 될 때마다 공격 으로부터 보호 해야 합니다. 이 작업을 수행 하는 가장 강력한 방법은 특수 한 하드웨어를 통하는 것입니다. Hsm (하드웨어 보안 모듈)을 사용 하 여 보안에 중요 한 응용 프로그램에 대 한 키를 생성, 저장 및 처리 하는 방법에 대 한 자세한 기록이 있습니다. 스마트 카드는 TCG(신뢰할 수 있는 컴퓨팅 그룹) TPM 표준과 호환 되는 장치 처럼 특수 한 유형의 HSM입니다. 가능 하면 Windows Hello 구현은 온보드 TPM 하드웨어를 활용 하 여 키를 생성, 저장 및 처리 합니다. 그러나 Windows Hello 및 Windows Hello for Work에는 온보드 TPM이 필요 하지 않습니다.

가능 하면 항상 TPM 하드웨어를 사용 하는 것이 좋습니다. TPM은 암호 대입 공격을 포함 하 여 다양 한 알려져 있고 잠재적인 공격을 방지 합니다. TPM은 계정 잠금 후에도 추가 보호 계층을 제공 합니다. TPM이 키 자료를 잠근 경우 사용자가 PIN을 다시 설정 해야 합니다. PIN을 다시 설정 하면 이전 키 자료로 암호화 된 모든 키 및 인증서가 제거 됩니다.

#### <a name="222-authentication"></a>2.2.2 인증

사용자가 보호 된 키 자료에 액세스 하려는 경우 인증 프로세스는 사용자가 PIN 또는 생체 인식 제스처를 입력 하 여 장치의 잠금을 해제 하는 프로세스를 시작 합니다 .이 프로세스를 "키 해제" 라고도 합니다.

응용 프로그램은 다른 응용 프로그램의 키를 사용할 수 없으며 다른 사용자의 키를 사용할 수 없습니다. 이러한 키는 지정 된 리소스에 대 한 액세스를 검색 하는 id 공급자 또는 IDP 전송 되는 요청에 서명 하는 데 사용 됩니다. 응용 프로그램은 특정 Api를 사용 하 여 특정 작업에 대 한 키 자료가 필요한 작업을 요청할 수 있습니다. 이러한 Api를 통해 액세스 하려면 사용자 제스처를 통해 명시적으로 유효성을 검사 해야 하며, 키 자료가 요청 응용 프로그램에 노출 되지 않습니다. 응용 프로그램은 데이터에 서명 하는 것과 같은 특정 작업을 요청 하 고, Windows Hello 계층은 실제 작업을 처리 하 고 결과를 반환 합니다.

### <a name="23-getting-ready-to-implement-windows-hello"></a>2.3 Windows Hello 구현을 준비 하는 중

Windows Hello가 작동 하는 방식에 대 한 기본적인 내용을 이해 했으므로 응용 프로그램에서 구현 하는 방법을 살펴보겠습니다.

Windows Hello를 사용 하 여 구현할 수 있는 다양 한 시나리오가 있습니다. 예를 들어 장치에서 앱에 로그온 하면 됩니다. 다른 일반적인 시나리오는 서비스에 대해 인증 하는 것입니다. 로그온 이름과 암호를 사용 하는 대신 Windows Hello를 사용 합니다. 다음 장에서는 Windows Hello를 사용 하 여 서비스에 인증 하는 방법과 기존 사용자 이름/암호 시스템에서 Windows Hello 시스템으로 변환 하는 방법을 비롯 하 여 몇 가지 시나리오를 구현 하는 방법을 설명 합니다.

## <a name="3-implementing-windows-hello"></a>3 Windows Hello 구현

이 장에서는 기존 인증 시스템이 없는 최적의 시나리오로 시작 하 고 Windows Hello를 구현 하는 방법을 설명 합니다.

다음 섹션에서는 기존 사용자 이름/암호 시스템에서 마이그레이션하는 방법에 대해 설명 합니다. 그러나 해당 챕터가 더 많은 관심을 가질 수 있는 경우에도이 항목을 검토 하 여 프로세스 및 필요한 코드를 기본적으로 이해 하는 것이 좋습니다.

### <a name="31-enrolling-new-users"></a>3.1 새 사용자 등록

Windows Hello를 사용 하는 새로운 서비스와 새 장치에 등록할 준비가 된 새로운 가상 사용자로 시작 합니다.

첫 번째 단계는 사용자가 Windows Hello를 사용할 수 있는지 확인 하는 것입니다. 앱은 사용자 설정 및 컴퓨터 기능을 확인 하 여 사용자 ID 키를 만들 수 있는지 확인 합니다. 앱이 사용자에 게 Windows Hello를 아직 사용 하도록 설정 하지 않은 것으로 확인 되 면 앱을 사용 하기 전에 사용자에 게이를 설정 하 라는 메시지가 표시 됩니다.

Windows Hello를 사용 하도록 설정 하려면 사용자가 OOBE (첫 실행 경험)를 설정 하는 경우를 제외 하 고 Windows 설정에서 PIN을 설정 하기만 하면 됩니다.

다음 코드 줄에서는 사용자가 Windows Hello에 대해 설정 되었는지 확인 하는 간단한 방법을 보여 줍니다.

```csharp
var keyCredentialAvailable = await KeyCredentialManager.IsSupportedAsync();
if (!keyCredentialAvailable)
{
    // User didn't set up PIN yet
    return;
}
```

다음 단계는 사용자에 게 서비스에 등록 하기 위한 정보를 요청 하는 것입니다. 사용자에 게 이름, 성, 전자 메일 주소 및 고유한 사용자 이름을 묻는 메시지를 표시 하도록 선택할 수 있습니다. 전자 메일 주소를 고유 식별자로 사용할 수 있습니다. 사용자의 경우입니다.

이 시나리오에서는 전자 메일 주소를 사용자의 고유 식별자로 사용 합니다. 사용자가 등록 한 후에는 주소가 유효한 지 확인 하기 위해 유효성 검사 전자 메일을 보내는 것을 고려해 야 합니다. 이렇게 하면 필요한 경우 계정을 다시 설정 하는 메커니즘을 제공 합니다.

사용자가 PIN을 설정 하는 경우 앱에서 사용자의 [**Keycredential**](/uwp/api/Windows.Security.Credentials.KeyCredential)을 만듭니다. 또한 앱은 TPM에서 키가 생성 되는 암호화 증명을 얻기 위한 선택적 키 증명 정보를 가져옵니다. 생성 된 공개 키와 필요에 따라 증명을 백 엔드 서버로 보내 사용 되는 장치를 등록 합니다. 모든 장치에서 생성 되는 모든 키 쌍은 고유 합니다.

[**Keycredential**](/uwp/api/Windows.Security.Credentials.KeyCredential) 을 만드는 코드는 다음과 같습니다.

```csharp
var keyCreationResult = await KeyCredentialManager.RequestCreateAsync(
    AccountId, KeyCredentialCreationOption.ReplaceExisting);
```

[**Requestcreateasync**](/previous-versions/windows/dn973048(v=win.10)) 는 공개 키와 개인 키를 만드는 파트입니다. 장치에 올바른 TPM 칩이 있는 경우 Api는 개인 및 공개 키를 만들고 결과를 저장 하도록 TPM 칩을 요청 합니다. 사용할 수 있는 TPM 칩이 없는 경우 OS가 코드에서 키 쌍을 만듭니다. 앱이 생성 된 개인 키에 직접 액세스할 수 있는 방법은 없습니다. 키 쌍이 생성 되는 부분은 결과 증명 정보 이기도 합니다. 증명에 대 한 자세한 내용은 다음 섹션을 참조 하십시오.

장치에서 키 쌍 및 증명 정보를 만든 후에는 공개 키, 선택적 증명 정보 및 고유 식별자 (예: 전자 메일 주소)를 백 엔드 등록 서비스로 보내고 백 엔드에 저장 해야 합니다.

사용자가 여러 디바이스에서 앱에 액세스할 수 있도록 하려면 백 엔드 서비스에서 동일한 사용자에 대해 여러 개의 키를 저장할 수 있어야 합니다. 모든 키는 모든 장치에 대해 고유 하기 때문에 동일한 사용자에 게 연결 된 모든 키를 저장 합니다. 장치 식별자는 사용자를 인증할 때 서버 파트를 최적화 하는 데 사용 됩니다. 이에 대해서는 다음 장에서 자세히 설명 합니다.

백 엔드에이 정보를 저장 하는 예제 데이터베이스 스키마는 다음과 같습니다.

![Windows Hello 샘플 데이터베이스 스키마](images/passport-db.png)

등록 논리는 다음과 같습니다.

![Windows Hello 등록 논리](images/passport-registration.png)

물론이 간단한 시나리오에 포함 된 것 보다 많은 정보를 제공 하는 정보를 수집 하는 등록 정보에 포함할 수 있습니다. 예를 들어 앱이 은행에 대해 하나의 보안 서비스에 액세스 하는 경우 등록 프로세스의 일부로 id 및 기타 사물 증명을 요청 해야 합니다. 모든 조건이 충족 되 면이 사용자의 공개 키는 백 엔드에 저장 되 고 다음에 사용자가 서비스를 사용할 때의 유효성을 검사 하는 데 사용 됩니다.

```csharp
using System;
using System.Runtime;
using System.Threading.Tasks;
using Windows.Storage.Streams;
using Windows.Security.Credentials;

static async void RegisterUser(string AccountId)
{
    var keyCredentialAvailable = await KeyCredentialManager.IsSupportedAsync();
    if (!keyCredentialAvailable)
    {
        // The user didn't set up a PIN yet
        return;
    }

    var keyCreationResult = await KeyCredentialManager.RequestCreateAsync(AccountId, KeyCredentialCreationOption.ReplaceExisting);
    if (keyCreationResult.Status == KeyCredentialStatus.Success)
    {
        var userKey = keyCreationResult.Credential;
        var publicKey = userKey.RetrievePublicKey();
        var keyAttestationResult = await userKey.GetAttestationAsync();
        IBuffer keyAttestation = null;
        IBuffer certificateChain = null;
        bool keyAttestationIncluded = false;
        bool keyAttestationCanBeRetrievedLater = false;

        keyAttestationResult = await userKey.GetAttestationAsync();
        KeyCredentialAttestationStatus keyAttestationRetryType = 0;

        if (keyAttestationResult.Status == KeyCredentialAttestationStatus.Success)
        {
            keyAttestationIncluded = true;
            keyAttestation = keyAttestationResult.AttestationBuffer;
            certificateChain = keyAttestationResult.CertificateChainBuffer;
        }
        else if (keyAttestationResult.Status == KeyCredentialAttestationStatus.TemporaryFailure)
        {
            keyAttestationRetryType = KeyCredentialAttestationStatus.TemporaryFailure;
            keyAttestationCanBeRetrievedLater = true;
        }
        else if (keyAttestationResult.Status == KeyCredentialAttestationStatus.NotSupported)
        {
            keyAttestationRetryType = KeyCredentialAttestationStatus.NotSupported;
            keyAttestationCanBeRetrievedLater = true;
        }
    }
    else if (keyCreationResult.Status == KeyCredentialStatus.UserCanceled ||
        keyCreationResult.Status == KeyCredentialStatus.UserPrefersPassword)
    {
        // Show error message to the user to get confirmation that user
        // does not want to enroll.
    }
}
```

#### <a name="311-attestation"></a>3.1.1 증명

키 쌍을 만들 때 TPM 칩에 의해 생성 되는 증명 정보를 요청 하는 옵션도 있습니다. 이 선택적 정보를 등록 프로세스의 일부로 서버에 보낼 수 있습니다. TPM 키 증명은 암호화가 키가 TPM 바인딩되어 있음을 증명 하는 프로토콜입니다. 이 유형의 증명을 사용 하 여 특정 암호화 작업이 특정 컴퓨터의 TPM에서 발생 하는 것을 보장할 수 있습니다.

생성 된 RSA 키, 증명 문 및 AIK 인증서를 수신 하면 서버는 다음 조건을 확인 합니다.

- AIK 인증서 서명이 유효 합니다.
- AIK 인증서는 신뢰할 수 있는 루트까지 연결 됩니다.
- AIK 인증서 및 해당 체인은 EKU OID "2.23.133.8.3"에 대해 사용 하도록 설정 됩니다 (이름은 "증명 Id 키 인증서").
- AIK 인증서의 시간이 유효 합니다.
- 체인에서 발급 하는 CA 인증서는 모두 시간이 유효 하며 해지 되지 않습니다.
- 증명 문의 형식이 잘못 되었습니다.
- [**Keyattestation**](/uwp/api/Windows.Security.Cryptography.Certificates.KeyAttestationHelper) blob의 서명은 AIK 공개 키를 사용 합니다.
- [**Keyattestation**](/uwp/api/Windows.Security.Cryptography.Certificates.KeyAttestationHelper) blob에 포함 된 공개 키는 클라이언트가 증명 문과 함께 전송 된 공용 RSA 키와 일치 합니다.

앱은 다음 조건에 따라 사용자에 게 다른 권한 부여 수준을 할당할 수 있습니다. 예를 들어, 이러한 검사 중 하나가 실패 하면 사용자를 등록 하지 않거나 사용자가 수행할 수 있는 작업을 제한할 수 있습니다.

### <a name="32-logging-on-with-windows-hello"></a>3.2 Windows Hello로 로그온

사용자가 시스템에 등록 되 면 앱을 사용할 수 있습니다. 시나리오에 따라 사용자가 앱 사용을 시작 하기 전에 인증 하도록 요청 하거나 백엔드 서비스 사용을 시작한 후 인증 하도록 요청할 수 있습니다.

### <a name="33-force-the-user-to-sign-in-again"></a>3.3 사용자가 다시 로그인 하도록 강제

일부 시나리오에서는 앱에 액세스 하기 전이나 앱 내에서 특정 작업을 수행 하기 전에 사용자가 현재 로그인 한 사용자 임을 증명할 수 있습니다. 예를 들어 뱅킹 앱이 서버에 전송 money 명령을 보내기 전에 로그인 한 장치를 찾은 사람이 아니라 사용자가 트랜잭션을 수행 하려고 시도 하는 것을 확인 하려고 합니다. [**UserConsentVerifier**](/uwp/api/Windows.Security.Credentials.UI.UserConsentVerifier) 클래스를 사용 하 여 사용자가 앱에서 다시 로그인 하도록 강제할 수 있습니다. 다음 코드 줄에서는 사용자가 자격 증명을 입력 하도록 강제 합니다.

다음 코드 줄에서는 사용자가 자격 증명을 입력 하도록 강제 합니다.

```csharp
UserConsentVerificationResult consentResult = await UserConsentVerifier.RequestVerificationAsync("userMessage");
if (consentResult.Equals(UserConsentVerificationResult.Verified))
{
    // continue
}
```

물론 서버에서 챌린지 응답 메커니즘을 사용할 수도 있습니다 .이 경우 사용자가 PIN 코드를 입력 하거나 생체 인식 자격 증명을 입력 해야 합니다. 개발자가 구현 해야 하는 시나리오에 따라 달라 집니다. 이 메커니즘에 대해서는 다음 섹션에서 설명 합니다.

### <a name="34-authentication-at-the-backend"></a>3.4 백 엔드에서 인증

앱이 보호 된 백 엔드 서비스에 액세스 하려고 하면 서비스는 앱에 챌린지를 보냅니다. 앱은 사용자의 개인 키를 사용 하 여 챌린지에 서명 하 고 다시 서버에 보냅니다. 서버는 해당 사용자에 대 한 공개 키를 저장 하므로 표준 crypto Api를 사용 하 여 메시지가 실제로 올바른 개인 키로 서명 되었는지 확인 합니다. 클라이언트에서 서명은 Windows Hello Api를 통해 수행 됩니다. 개발자는 사용자의 개인 키에 대 한 액세스 권한을 갖지 않습니다.

키를 확인 하는 것 외에도, 서비스는 키 증명을 확인 하 고 장치에 키를 저장 하는 방법에 대해 호출 되는 제한이 있는지 여부를 파악할 수 있습니다. 예를 들어 장치에서 TPM을 사용 하 여 키를 보호 하는 경우 TPM 없이 키를 저장 하는 장치 보다 안전 합니다. 예를 들어, 위험을 줄이기 위해 TPM이 사용 되지 않는 경우에만 사용자가 일정 금액을 전송할 수 있도록 백 엔드 논리를 결정할 수 있습니다.

증명은 버전 2.0 이상인 TPM 칩을 사용 하는 장치에만 사용할 수 있습니다. 따라서 모든 장치에서이 정보를 사용 하지 못할 수 있다는 것을 고려해 야 합니다.

클라이언트 워크플로는 다음 차트와 같을 수 있습니다.

![Windows Hello 클라이언트 워크플로](images/passport-client-workflow.png)

앱이 백 엔드에서 서비스를 호출 하면 서버에서 챌린지를 보냅니다. 챌린지는 다음 코드를 사용 하 여 서명 됩니다.

```csharp
var openKeyResult = await KeyCredentialManager.OpenAsync(AccountId);

if (openKeyResult.Status == KeyCredentialStatus.Success)
{
    var userKey = openKeyResult.Credential;
    var publicKey = userKey.RetrievePublicKey();
    var signResult = await userKey.RequestSignAsync(message);

    if (signResult.Status == KeyCredentialStatus.Success)
    {
        return signResult.Result;
    }
    else if (signResult.Status == KeyCredentialStatus.UserPrefersPassword)
    {

    }
}
```

첫 번째 줄 인 [**Keycredentialmanager. OpenAsync**](/uwp/api/windows.security.credentials.keycredentialmanager.openasync)는 OS에 키 핸들을 열도록 요청 합니다. 성공적으로 완료 되 면 [**Keycredential. RequestSignAsync**](/uwp/api/windows.security.credentials.keycredential.requestsignasync) 메서드를 사용 하 여 챌린지 메시지에 서명 하면 Windows Hello를 통해 사용자의 PIN 또는 생체 인식 기능을 요청 하는 OS를 트리거합니다. 개발자가 언제 든 지 사용자의 개인 키에 액세스할 수 있습니다. 이는 모두 Api를 통해 안전 하 게 유지 됩니다.

Api는 개인 키를 사용 하 여 챌린지에 서명 하도록 OS를 요청 합니다. 그러면 시스템에서 사용자에 게 PIN 코드 또는 구성 된 생체 인식 로그온을 요청 합니다. 올바른 정보를 입력 하면 시스템에서 TPM 칩에 암호화 기능을 수행 하 고 챌린지에 서명 하도록 요청할 수 있습니다. 또는 TPM을 사용할 수 없는 경우 대체 (fallback) 소프트웨어 솔루션을 사용 합니다. 클라이언트는 서명 된 챌린지를 서버에 다시 보내야 합니다.

기본 챌린지 – 응답 흐름은 다음 시퀀스 다이어그램에 표시 됩니다.

![Windows Hello 챌린지 응답](images/passport-challenge-response.png)

다음으로 서버는 서명의 유효성을 검사 해야 합니다. 공개 키를 요청 하 고 나중에 유효성 검사를 위해 사용할 서버에 보낼 때이 키는 ASN-1로 인코딩된 publicKeyInfo blob에 있습니다. [GitHub의 Windows Hello 코드 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MicrosoftPassport)을 살펴보면 crypt32.dll 함수를 래핑하여 더 일반적으로 사용 되는 CNG blob로 asn.1로 인코딩된 blob을 변환 하는 도우미 클래스가 있음을 알 수 있습니다. Blob에는 RSA 인 공개 키 알고리즘과 RSA 공개 키가 포함 되어 있습니다.

이 샘플에서 asn.1로 인코딩된 blob을 CNG blob으로 변환 하는 이유는 CNG (/windows/desktop/SecCNG/cng-portal) 및 BCrypt API와 함께 사용할 수 있기 때문입니다. CNG blob을 조회 하는 경우 관련 [BCRYPT_KEY_BLOB 구조가](/windows/desktop/api/bcrypt/ns-bcrypt-_bcrypt_key_blob)표시 됩니다. 이 API surface는 Windows 응용 프로그램의 인증 및 암호화에 사용할 수 있습니다. A s n. 1은 serialize 할 수 있는 데이터 구조를 전달 하기 위해 문서화 된 표준 이며 일반적으로 공개 키 암호화 및 인증서로 사용 됩니다. 이렇게 하면 공개 키 정보가 이런 방식으로 반환 됩니다. 공개 키가 RSA 키입니다. Windows Hello가 데이터에 서명할 때 사용 하는 알고리즘입니다.

CNG blob이 있으면 사용자의 공개 키에 대해 서명 된 챌린지의 유효성을 검사 해야 합니다. 모든 사용자는 자신의 시스템 또는 백 엔드 기술을 사용 하므로이 논리를 구현 하는 일반적인 방법은 없습니다. S h a를 해시 알고리즘으로 사용 하 고 사인 아웃에 대 한 Pkcs1를 사용 하 고 있으므로 클라이언트에서 서명 된 응답의 유효성을 검사할 때 사용 하는 것이 맞는지 확인 합니다. .NET 4.6의 서버에서이 작업을 수행 하는 방법에 대해서는 샘플을 참조 하지만 일반적으로 다음과 같이 표시 됩니다.

```csharp
using (RSACng pubKey = new RSACng(publicKey))
{
    retval = pubKey.VerifyData(originalChallenge, responseSignature, HashAlgorithmName.SHA256, RSASignaturePadding.Pkcs1);
}
```

RSA 키인 저장 된 공개 키를 읽습니다. 공개 키를 사용 하 여 서명 된 챌린지 메시지의 유효성을 검사 하 고이를 체크 아웃 하는 경우 사용자에 게 권한을 부여 합니다. 사용자가 인증 되 면 앱은 일반적인 방법으로 백 엔드 서비스를 호출할 수 있습니다.

전체 코드는 다음과 같을 수 있습니다.

```csharp
using System;
using System.Runtime;
using System.Threading.Tasks;
using Windows.Storage.Streams;
using Windows.Security.Cryptography;
using Windows.Security.Cryptography.Core;
using Windows.Security.Credentials;

static async Task<IBuffer> GetAuthenticationMessageAsync(IBuffer message, String AccountId)
{
    var openKeyResult = await KeyCredentialManager.OpenAsync(AccountId);

    if (openKeyResult.Status == KeyCredentialStatus.Success)
    {
        var userKey = openKeyResult.Credential;
        var publicKey = userKey.RetrievePublicKey();
        var signResult = await userKey.RequestSignAsync(message);
        if (signResult.Status == KeyCredentialStatus.Success)
        {
            return signResult.Result;
        }
        else if (signResult.Status == KeyCredentialStatus.UserCanceled)
        {
            // Launch app-specific flow to handle the scenario 
            return null;
        }
    }
    else if (openKeyResult.Status == KeyCredentialStatus.NotFound)
    {
        // PIN reset has occurred somewhere else and key is lost.
        // Repeat key registration
        return null;
    }
    else
    {
        // Show custom UI because unknown error has happened.
        return null;
    }
}
```

올바른 챌린지를 구현 하는 것은이 문서의 범위를 벗어나지만이 항목은 재생 공격 또는 메시지 가로채기 (man-in-the-middle) 공격과 같은 항목을 방지 하기 위해 보안 메커니즘을 성공적으로 만들기 위해 주의가 필요한 항목입니다.

### <a name="35-enrolling-another-device"></a>3.5 다른 디바이스 등록

오늘날 사용자는 동일한 앱을 사용 하 여 여러 장치를 설치 하는 것이 일반적입니다. 여러 장치에서 Windows Hello를 사용 하는 경우 어떻게 작동 하나요?

Windows Hello를 사용 하는 경우 모든 장치에서 고유한 개인 및 공개 키 집합을 만듭니다. 즉, 사용자가 여러 장치를 사용할 수 있도록 하려면 백 엔드가이 사용자에 게 여러 공개 키를 저장할 수 있어야 합니다. 테이블 구조의 예는 섹션 2.1의 데이터베이스 다이어그램을 참조 하세요.

다른 장치를 등록 하는 것은 처음으로 사용자를 등록 하는 것과 거의 같습니다. 이 새 장치를 등록 하는 사용자는 정말로 자신이 주장 하는 사용자 인지를 알고 있어야 합니다. 현재 사용 되는 2 단계 인증 메커니즘을 사용 하 여이 작업을 수행할 수 있습니다. 안전한 방법으로이를 수행 하는 방법에는 여러 가지가 있습니다. 시나리오에 따라 달라 집니다.

예를 들어, 로그인 이름 및 암호를 계속 사용 하는 경우 사용자를 인증 하 고 SMS 또는 전자 메일과 같은 인증 방법 중 하나를 사용 하도록 요청 하는 데 사용할 수 있습니다. 로그인 이름과 암호가 없으면 이미 등록 된 장치 중 하나를 사용 하 여 해당 장치의 앱에 알림을 보낼 수도 있습니다. MSA authenticator 앱은이에 대 한 예입니다. 간단히 말해서 일반적인 2FA 메커니즘을 사용 하 여 사용자에 대 한 추가 장치를 등록 해야 합니다.

새 장치를 등록 하는 코드는 앱 내에서 처음으로 사용자를 등록 하는 것과 동일 합니다.

```csharp
var keyCreationResult = await KeyCredentialManager.RequestCreateAsync(
    AccountId, KeyCredentialCreationOption.ReplaceExisting);
```

사용자가 등록 된 장치를 보다 쉽게 인식할 수 있도록 등록의 일부로 장치 이름 또는 다른 식별자를 보내도록 선택할 수 있습니다. 예를 들어 장치를 분실 한 경우 사용자가 장치를 등록 취소할 수 있는 백 엔드에서 서비스를 구현 하려는 경우에도 유용 합니다.

### <a name="36-using-multiple-accounts-in-your-app"></a>3.6 앱에서 여러 계정 사용

단일 계정에 대해 여러 장치를 지 원하는 것 외에도 단일 앱에서 여러 계정을 지 원하는 것도 일반적입니다. 예를 들어 앱 내에서 여러 Twitter 계정에 연결 하는 경우를 들 수 있습니다. Windows Hello를 사용 하면 여러 키 쌍을 만들고 앱 내에서 여러 계정을 지원할 수 있습니다.

이 작업을 수행 하는 한 가지 방법은 이전 챕터에서 설명 하는 사용자 이름 또는 고유 식별자를 격리 된 저장소로 유지 하는 것입니다. 따라서 새 계정을 만들 때마다 계정 ID를 격리 된 저장소에 저장 합니다.

앱 UI에서 사용자가 이전에 만든 계정 중 하나를 선택 하거나 새 계정으로 등록할 수 있습니다. 새 계정을 만드는 흐름은 앞에서 설명한 것과 동일 합니다. 계정을 선택 하는 것은 화면에 저장 된 계정을 나열 하는 것입니다. 사용자가 계정을 선택 하면 계정 ID를 사용 하 여 앱에서 사용자에 게 로그온 합니다.

```csharp
var openKeyResult = await KeyCredentialManager.OpenAsync(AccountId);
```

흐름의 나머지 부분은 앞에서 설명한 것과 같습니다. 이 시나리오에서는 동일한 Windows 계정을 사용 하 여 단일 장치에서 사용 되므로 이러한 모든 계정은 동일한 PIN 또는 생체 인식 제스처로 보호 됩니다.

## <a name="4-migrating-an-existing-system-to-windows-hello"></a>4 기존 시스템을 Windows Hello로 마이그레이션

이 짧은 섹션에서는 사용자 이름 및 해시 된 암호를 저장 하는 데이터베이스를 사용 하는 기존 유니버설 Windows 플랫폼 앱 및 백 엔드 시스템에 대해 설명 합니다. 이러한 앱은 앱이 시작 될 때 사용자의 자격 증명을 수집 하 여 백 엔드 시스템이 인증 챌린지를 반환할 때 사용 합니다.

여기서는 Windows Hello 작업을 수행 하기 위해 변경 하거나 교체 해야 하는 부분을 설명 합니다.

이전 장에서 대부분의 기법을 이미 설명 했습니다. 기존 시스템에 Windows Hello를 추가 하려면 코드의 등록 및 인증 부분에 몇 가지 흐름을 추가 해야 합니다.

한 가지 방법은 사용자가 업그레이드할 때 선택할 수 있도록 하는 것입니다. 사용자가 앱에 로그온 하 고 앱 및 OS가 Windows Hello를 지원할 수 있는 것을 감지 하면 사용자가 자격 증명을 업그레이드 하 여 최신의 보안 시스템을 사용할 수 있는지 여부를 사용자에 게 요청할 수 있습니다. 다음 코드를 사용 하 여 사용자가 Windows Hello를 사용할 수 있는지 여부를 확인할 수 있습니다.

```csharp
var keyCredentialAvailable = await KeyCredentialManager.IsSupportedAsync();
```

UI는 다음과 같을 수 있습니다.

![Windows Hello ui](images/passport-ui.png)

사용자가 Windows Hello 사용을 시작 하려는 경우 앞에서 설명한 [**Keycredential**](/uwp/api/Windows.Security.Credentials.KeyCredential) 을 만듭니다. 백 엔드 등록 서버는 공개 키와 선택적 증명 문을 데이터베이스에 추가 합니다. 사용자가 사용자 이름 및 암호를 사용 하 여 이미 인증 되었으므로 서버는 새 자격 증명을 데이터베이스의 현재 사용자 정보에 연결할 수 있습니다. 데이터베이스 모델은 앞에서 설명한 예제와 동일할 수 있습니다.

앱이 사용자 [**Keycredential**](/uwp/api/Windows.Security.Credentials.KeyCredential)을 만들 수 있는 경우 사용자 ID를 격리 된 저장소에 저장 하므로 앱이 다시 시작 되 면 사용자가 목록에서이 계정을 선택할 수 있습니다. 이 시점부터 흐름은 이전 장에 설명 된 예제를 정확히 따릅니다.

전체 Windows Hello 시나리오로 마이그레이션하는 마지막 단계는 앱에서 로그온 이름 및 암호 옵션을 사용 하지 않도록 설정 하 고 저장 된 해시 된 암호를 데이터베이스에서 제거 하는 것입니다.

## <a name="5-summary"></a>5 요약

Windows 10에는 간단 하 게 적용할 수 있는 높은 수준의 보안이 도입 되었습니다. Windows Hello는 사용자를 인식 하 고 적절 한 식별을 회피 하기 위해 적극적으로 노력 하는 새로운 생체 인식 로그인 시스템을 제공 합니다. 그런 다음 신뢰할 수 있는 플랫폼 모듈 외부에서 표시 하거나 사용할 수 없는 여러 가지 키 및 인증서 계층을 제공할 수 있습니다. 또한 증명 id 키 및 인증서를 선택적으로 사용 하 여 추가 보안 계층을 사용할 수 있습니다.

개발자는 이러한 기술의 설계 및 배포에 대해이 지침을 사용 하 여 앱 및 백 엔드 서비스를 보호 하기 위해 Windows 10 롤아웃에 보안 인증을 쉽게 추가할 수 있습니다. 필요한 코드는 최소한의 이해를 돕기 위한 것입니다. Windows 10에서는 많은 작업이 수행 됩니다.

유연 하 게 구현 되는 옵션을 사용 하면 Windows Hello에서 기존 인증 시스템을 교체 하거나 함께 사용할 수 있습니다. 배포 환경은 편리 하 고 경제적입니다. Windows 10 보안을 배포 하는 데 추가 인프라가 필요 하지 않습니다. 운영 체제에 기본 제공 되는 Microsoft Hello를 사용 하 여 Windows 10은 최신 개발자가 직면 한 인증 문제에 대 한 가장 안전한 솔루션을 제공 합니다.

작업이 완료되었습니다. 인터넷을 더욱 안전 하 게 만들 수 있습니다.

## <a name="6-resources"></a>6 리소스

### <a name="61-articles-and-sample-code"></a>6.1 문서 및 샘플 코드

- [Windows Hello 개요](https://support.microsoft.com/help/17215)
- [Windows Hello에 대 한 구현 세부 정보](/windows/keep-secure/microsoft-passport-guide)
- [GitHub의 Windows Hello 코드 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MicrosoftPassport)

### <a name="62-terminology"></a>6.2 용어

| | |
|-|-|
| AIK | 증명 id 키는 비 마이그레이션할 수 있고 키의 속성에 서명 하 고 확인을 위해 신뢰 당사자에 속성 및 서명을 제공 하 여 이러한 암호화 증명 (TPM 키 증명)을 제공 하는 데 사용 됩니다. 결과 시그니처를 "증명 문" 이라고 합니다. 서명 개인 키를 사용 하 여 생성 됩니다 .이 키는 해당 키를 만든 TPM 에서만 사용할 수 있습니다. 신뢰 당사자는 증명 된 키가 진정한 비 마이그레이션할 수 있고을 신뢰 하 고 해당 TPM 외부에서 사용할 수 없습니다. |
| AIK 인증서 | AIK 인증서는 TPM 내에서 AIK의 존재를 증명 하는 데 사용 됩니다. 또한 해당 특정 TPM에서 발생 한 AIK에서 인증 된 다른 키를 증명 하는 데 사용 됩니다. |
| IDP | IDP는 id 공급자입니다. Microsoft 계정에 대 한 Microsoft의 IDP 빌드를 예로 들 수 있습니다. 응용 프로그램이 MSA를 사용 하 여 인증 해야 할 때마다 MSA IDP을 호출할 수 있습니다. |
| PKI | 공개 키 인프라는 일반적으로 조직 자체에서 호스트 되는 환경을 가리키는 데 사용 되며 키 생성, 키 해지 등을 담당 합니다. |
| TPM | 신뢰할 수 있는 플랫폼 모듈을 사용 하 여 TPM 외부에서 개인 키를 표시 하거나 사용할 수 없는 방식으로 암호화 공개/개인 키 쌍을 만들 수 있습니다 (즉, 마이그레이션할 수 있고). |
| TPM 키 증명 | 암호화에서 키가 TPM 바인딩되어 있음을 증명 하는 프로토콜입니다. 이 증명 유형을 사용 하 여 특정 암호화 작업이 특정 컴퓨터의 TPM에서 발생 하는 것을 보장할 수 있습니다. |

## <a name="related-topics"></a>관련 항목

* [Windows Hello 로그인 앱](microsoft-passport-login.md)
* [Windows Hello 로그인 서비스](microsoft-passport-login-auth-service.md)

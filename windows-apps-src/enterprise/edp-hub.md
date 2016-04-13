---
Description: 'EDP(엔터프라이즈 데이터 보호) 기능이 파일, 버퍼, 클립보드, 네트워킹, 백그라운드 작업 및 잠금 상태에서의 데이터 보호와 어떤 관계가 있는지를 보여 주는 전체 개발자 그림을 보여 주는 허브 항목입니다.'
MS-HAID: 'dev\_enterprise.edp\_hub'
MSHAttr: 'PreferredLib:/library/windows/apps'
Search.Product: eADQiWindows 10XVcnh
title: 'EDP(엔터프라이즈 데이터 보호)'
---

# EDP(엔터프라이즈 데이터 보호)

__참고__ Windows 10 버전 1511(빌드 10586) 이전에서는 EDP(엔터프라이즈 데이터 보호) 정책을 적용할 수 없습니다.

EDP(엔터프라이즈 데이터 보호) 기능이 파일, 버퍼, 클립보드, 네트워킹, 백그라운드 작업 및 잠금 상태에서의 데이터 보호와 어떤 관계가 있는지를 보여 주는 전체 개발자 그림을 보여 주는 허브 항목입니다.

최종 사용자 및 관리자 관점에서 EDP에 대한 자세한 내용은 [EDP(엔터프라이즈 데이터 보호) 개요](https://technet.microsoft.com/library/dn985838(v=vs.85).aspx)를 참조하세요.

## EDP란?

EDP는 MDM(모바일 디바이스 관리)을 위한 데스크톱, 노트북, 태블릿 및 휴대폰의 기능 집합입니다. EDP는 엔터프라이즈에서 관리하는 디바이스에서 데이터(엔터프라이즈 파일 및 데이터 Blob)를 처리하는 방식에 대한 엔터프라이즈의 제어를 강화합니다.

-   엔터프라이즈 데이터에는 암호화 태그가 지정됩니다. 이를 "엔터프라이즈 보호된 데이터" 또는 간단히 "보호된 데이터"라고 합니다.
-   관리 엔터프라이즈에서 EDP 정책을 통해 명시적으로 허용하는 앱만 파일 등의 엔터프라이즈 보호된 데이터에 액세스할 수 있습니다.
-   EDP 정책을 통해 명시적으로 허용된 앱만 엔터프라이즈 VPN(가상 사설망)에 액세스할 수 있습니다.
-   또한 앱 제한 정책은 허용된 앱이 엔터프라이즈 데이터를 처리해야 하는 방식을 결정합니다.
-   정책 기반 제한은 Windows 클립보드 또는 공유 계약을 통해 교환되는 엔터프라이즈 콘텐츠에도 적용됩니다.
-   요청 시 관리 엔터프라이즈에서 보호된 콘텐츠에 대한 디바이스 액세스를 해지하여, 기본적으로 개인 데이터는 그대로 두고 엔터프라이즈 데이터를 디바이스에서 지울 수 있습니다.
-   채널 앱은 보호된 데이터를 다운로드하는 앱입니다. 예를 들어 메일 및 파일 동기화 앱이 있습니다.

EDP는 [EFS(파일 시스템 암호화)](http://technet.microsoft.com/library/cc700811.aspx) 및 [Windows 선택 지우기](https://technet.microsoft.com/library/dn486874.aspx)를 개선하여 더 많은 보안 및 유연성 옵션을 제공합니다. 새로운 EDP API를 사용하면 엔터프라이즈 콘텐츠를 보호하고 액세스를 해지하며, 보호된 파일 속성으로 작업하고, 암호화된 데이터를 원시 형태로 액세스하는 앱을 만들 수 있습니다. 파일 및 폴더를 보호하기 위한 새로운 API뿐 아니라 버퍼 및 스트림을 보호하기 위한 API도 도입합니다. 또한 앱이 데이터 보호 정책을 적용해야 하는 엔터프라이즈를 식별하고 나타낼 수 있게 하는 API 집합을 도입합니다.

관리 엔터프라이즈에서 보호된 데이터에 대한 액세스를 제어할 수 있도록 앱 제한 정책은 앱 목록 및 해당 앱에 대한 제한 사항을 정의합니다. 기본적으로 앱은 보호된 데이터에 자동으로 액세스할 수 없습니다. 액세스하려면 허용된 목록에 앱을 추가해야 하며, 허용된 목록에 있는 앱을 허용된 앱이라고 합니다. 관리되는 디바이스에서 Windows는 클립보드 또는 공유 계약을 통해 보호된 데이터에 대한 액세스를 제한하고 감사하여 허용된 목록에 없는 앱의 액세스가 감사되고 사용자 승인이 필요하거나 완전히 차단되도록 할 수 있습니다.

EDP 정책은 관리 엔터프라이즈가 디바이스에 제공하며, 이 때문에 "관리되는 디바이스"가 됩니다. 정책 프로비전은 MDM(모바일 디바이스 관리)에서 사용자 등록을 통해, IT에서 수동 구성을 통해 또는 SCCM(System Center Configuration Manager) 등의 다른 관리 및 정책 전달 메커니즘을 통해 이루어질 수 있습니다.

EDP 파일 보호는 프로비전된 경우 RMS(권한 관리 서비스) 키를 활용합니다. 이러한 키는 디바이스 간에 로밍될 수 있으므로 보호된 데이터의 로밍을 허용하기 때문입니다. RMS 키가 없을 경우 이러한 API는 로컬 선택 지우기 키로 대체되며 로밍 기능을 제한합니다. 암호화된 상태로 로밍되는 데이터는 Microsoft에서 제공하는 플랫폼별 RMS 앱 및 RMS 인식 타사 앱을 통해 하위 수준 Windows 및 타사 디바이스에서 액세스할 수 있습니다.

요약해서 EDP API로 보호된 데이터는 엔터프라이즈에서 관리할 수 있으므로 엔터프라이즈가 해당 데이터를 보호 및 관리하는 데 도움이 되는 방식으로 앱을 빌드할 수 있습니다. 즉, 엔터프라이즈용 앱을 빌드할 수 있습니다. 또한 이 가이드의 나머지 부분은 모두 이 작업을 수행하도록 돕기 위한 것입니다.

## EDP를 위한 컴퓨터 설정


제대로 앱을 개발하고 엔터프라이즈에서 작동 방식을 테스트하려면 컴퓨터 또는 디바이스를 적절한 조건으로 설정하는 것이 좋습니다. 주로 IT 관리자의 도메인에 있는 몇 가지 작업을 수행해야 합니다.

-   개발 컴퓨터를 MDM(모바일 디바이스 관리)에 등록합니다.
-   [AppLocker 구성 서비스 공급자](https://msdn.microsoft.com/library/windows/hardware/dn920019)를 통해 허용된 목록에 앱을 추가합니다.

다음 작업은 앱을 실행하는 관리되는 엔터프라이즈 ID 및 적용된 보호 정책을 인식하고 동적으로 응답할 수 있는 앱을 빌드하는 것입니다. 이는 앱 "인식"을 의미합니다. 정책을 따라 인식되는 앱은 엔터프라이즈 데이터에 액세스할 수 있는 목록에 포함될 가능성이 훨씬 큽니다.

## 엔터프라이즈 인식 앱


앱이 허용된 목록 있으면 보호된 데이터를 읽을 수 있습니다. 또한 앱에서 출력된 모든 데이터는 기본적으로 시스템에서 자동으로 보호됩니다. 이러한 자동 보호는 관리 엔터프라이즈가 어떤 방법으로든 엔터프라이즈 데이터를 제어된 상태로 유지해야 하기 때문입니다. 그러나 이런 방식의 엄격한 앱 제어는 목적을 달성하는 기본 방법일 뿐입니다. 보다 효과적인 방법은 더 강력한 능력과 유연성을 제공할 만큼 자신을 신뢰하도록 시스템에 요청하는 것입니다. 그리고 이 요청을 승인받으려면 앱을 더 스마트하게 만들어야 합니다. 즉, 허용된 목록에 포함되는 것보다 한 단계 더 나아가서 앱을 엔터프라이즈 인식으로 만들고 선언해야 합니다.

데이터가 사용되지 않든, 사용 중이든, 전송 중이든 관계없이 엔터프라이즈 데이터가 자동으로 보호되도록 하는, 앞으로 설명할 기술을 사용하는 경우 앱이 인식된 것입니다. 인식된 앱은 엔터프라이즈 데이터 원본 및 엔터프라이즈 데이터를 인식하고 앱에 도착하면 보호합니다. 또한 인식되면 엔터프라이즈 데이터가 앱을 나갈 때마다 EDP 정책을 인식하고 준수합니다. 여기에는 콘텐츠가 비엔터프라이즈 네트워크 끝점으로 이동할 수 없도록 차단, 로밍을 허용하기 전에 및 잠재적으로(정책 설정에 따라) 암호화된 휴대용 형태로 데이터 래핑, 허용된 목록에 없는 앱에 엔터프라이즈 데이터를 붙여넣기 전에 사용자에게 확인 등이 포함됩니다. 인식 앱으로 설정된 앱은 제한된 **enterpriseDataPolicy** 기능을 선언하여 인식되도록 시스템에 알립니다. 제한된 접근 권한 값으로 작업하는 방법에 대한 자세한 내용은 [특수 및 제한된 접근 권한 값](https://msdn.microsoft.com/library/windows/apps/mt270968#special_and_restricted_capabilities)을 참조하세요.

이상적일 경우, 모든 엔터프라이즈 데이터는 사용하지 않을 때와 전송 중에도 보호된 데이터입니다. 그러나 불가피하게 엔터프라이즈 데이터가 처음 생성되는 시기와 보호되는 시기 사이에 짧은 기간이 있어야 합니다. 또한 엔터프라이즈 데이터가 암호화되지 않고 엔터프라이즈 네트워크 끝점에 존재하는 경우도 있습니다. 인식된 앱은 이러한 데이터를 자동으로 보호할 수 있지만, 허용되었지만 비인식 앱은 시스템에서 보호를 적용해야 합니다.

비인식 앱은 항상 엔터프라이즈 모드로 실행되기 때문입니다. 시스템이 이를 확인합니다. 그러나 인식된 앱은 지정된 시간에 앱에서 사용하는 데이터 종류에 적절하게 엔터프라이즈 모드와 개인 모드 간을 자유롭게 이동합니다. 또한 인식된 앱은 개인 데이터를 준수하고 개인 데이터를 엔터프라이즈 데이터로 태그 지정하지 않습니다. 인식된 앱은 이러한 약속이 유지되기만 하면 엔터프라이즈 데이터와 개인 데이터를 동시에 처리할 수 있습니다. 다음 섹션에서는 코드에서 모드를 전환하는 방법을 보여 줍니다.

## ID가 관리되는지 확인 및 보호 정책 적용 수준 확인

일반적으로 앱은 사서함 메일 주소, 관리되는 도메인 또는 URI 호스트 이름과 같은 외부 리소스에서 엔터프라이즈 ID를 가져옵니다. [
            **ProtectionPolicyManager.GetPrimaryManagedIdentityForNetworkEndpointAsync**](https://msdn.microsoft.com/library/windows/apps/dn706027)를 호출하여 네트워크 끝점 호스트 이름의 관리되는 ID(있는 경우)를 가져올 수 있습니다.

이 코드 예제에서는 엔터프라이즈 ID가 실제로 관리되는지 여부를 확인하는 방법, 현재 적용된 정책 등 인식된 동작의 일반적인 구조를 보여 줍니다.

```CSharp
using Windows.Security.EnterpriseData;

...

string identity = "contoso.com";

if (ProtectionPolicyManager.IsIdentityManaged(identity))
{
    EnforcementLevel enforcementLevel = ProtectionPolicyManager.GetEnforcementLevel();

    // During UI activities or network access, call ProtectionPolicyManager APIs
    // (taking the enforcement level into account) to ensure that the system
    // tags data with the identity as appropriate.

    ProtectionPolicyManager.GetForCurrentView().Identity = identity;
    // The app is now in enterprise mode.

    ProtectionPolicyManager.GetForCurrentView().Identity = string.Empty;
    // The app is back in personal mode.
}
else
{
    // No policy enforcement is done on this identity.
}
```

표시된 것처럼, 먼저 엔터프라이즈의 ID에 대해 EDP 정책이 설정되었는지 여부를 확인합니다. "관리되는"이란 용어는 "EDP 정책에 의해 관리되는"을 줄여서 표현한 것입니다. 특정 ID에 대해 EDP 정책이 설정된 경우 [**ProtectionPolicyManager.IsIdentityManaged**](https://msdn.microsoft.com/library/windows/apps/dn705171)가 해당 ID에 대해 true를 반환합니다. 이 신호에 따라 해당 ID와 함께 EDP API를 사용합니다. 파일 및 버퍼 API는 관리되지 않는 ID에 대해서도 예상대로 작동한다는 점에서 다소 예외적이지만 해당 시나리오는 의미가 없습니다. 디바이스가 관리되면 엔터프라이즈 ID에 대해 관리됩니다. ID가 관리되지 않는 경우 해당 ID로 데이터를 보호하는 것은 의미가 없습니다.

다음 단계는 정책 적용 수준을 확인하고 구현하는 것입니다. 정책 적용 수준을 확인하려면 [**GetEnforcementLevel**](https://msdn.microsoft.com/library/windows/apps/mt608406) 메서드를 호출합니다. 엔터프라이즈 정책이 ID에 적용되는 경우 인식된 앱은 UI 작업 또는 네트워크 액세스 중에 [**ProtectionPolicyManager**](https://msdn.microsoft.com/library/windows/apps/dn705170) API를 호출하여 필요한 경우 시스템이 이 ID로 데이터 전송에 태그를 지정하게 함으로써 시스템의 정책 적용을 지원해야 합니다. 적용 수준을 해석하고 실행하는 방법에 대한 자세한 내용은 이 가이드에 포함되어 있습니다. 이 코드 예제에서는 [**ProtectionPolicyManager.Identity**](https://msdn.microsoft.com/library/windows/apps/dn705785) 값을 각각 엔터프라이즈 ID 또는 빈 문자열로 설정하여 엔터프라이즈 모드로 전환하고 개인 모드로 되돌리는 방법도 보여 줍니다. 엔터프라이즈 모드 시작 및 종료는 관리되는 ID에서만 타당합니다.

## EDP 기능 살펴보기


**파일 및 버퍼 보호.**

-   앱은 엔터프라이즈 ID와 연결된 데이터를 보호하고 컨테이너에 저장하고 지울 수 있습니다.
-   키 관리는 Windows에서 처리합니다. Windows는 디바이스를 사용할 수 있을 경우 엔터프라이즈의 RMS 키를 사용하고, 그렇지 않으면 로컬 선택 지우기 보호로 대체합니다.

**디바이스 정책 관리.**

-   앱은 디바이스를 관리하는 ID(엔터프라이즈 또는 조직)를 쿼리할 수 있습니다.
-   앱은 ID를 해당 데이터와 연결하여 사용자가 실수로 데이터를 공개하지 않도록 보호할 수 있습니다.
-   앱은 엔터프라이즈 소유의 네트워크 끝점 연결(서버, IP 범위)을 확인하고 데이터를 관리되는(즉, MDM에 등록된) ID에 연결하여 네트워크를 통해 엔터프라이즈 리소스를 보호할 수 있습니다.
-   EDP API는 디바이스에 EDP 정책이 정의되어 있는 관리되는 ID에서만 작동합니다. ID가 관리되지 않는 경우 API는 필요에 따라 이 사실을 응용 프로그램에 알립니다.

다음은 EDP API를 드릴하는 항목 및 이러한 특정 기능 영역과 관련된 시나리오에 대한 링크 목록입니다.

## 파일

[EDP를 사용하여 파일 보호](../files/protect-your-enterprise-data-with-edp.md)를 참조하세요.

## 스트림 및 버퍼

[EDP를 사용하여 스트림 및 버퍼 보호](../files/use-edp-to-protect-streams-and-buffers.md)를 참조하세요.

## 클립보드, 공유 및 앱 간에 데이터 교환

[EDP를 사용하여 앱 간에 전송되는 엔터프라이즈 데이터 보호](../app-to-app/use-edp-to-protect-enterprise-data-transferred-between-apps.md)를 참조하세요.

## 네트워킹

[EDP ID를 사용하여 네트워크 연결 태그 지정](../networking/tagging_network_connections_with_edp_identity.md)을 참조하세요.

## DPL(잠금 상태에서 데이터 보호) 및 백그라운드 작업

조직에서 안전한 DPL("잠금 상태에서 데이터 보호") 정책을 관리하도록 선택할 수 있으며, 이 경우 디바이스가 잠기면 보호되는 리소스에 액세스하는 데 필요한 암호화 키가 일시적으로 디바이스 메모리에서 제거됩니다. 이러한 상황에 대해 앱을 준비하려면 이 항목에서 [디바이스 잠금 이벤트 처리 및 메모리에 보호되지 않은 콘텐츠 남김 방지](#handle_lock_events) 섹션을 참조하세요. 또한 파일을 보호하는 데 필요한 백그라운드 작업이 앱에 있는 경우 [엔터프라이즈 데이터를 새 파일에서 보호(백그라운드 작업)](../files/protect-your-enterprise-data-with-edp.md#protect_data_new_file_bg)를 참조하세요.

## UI 정책 적용


이 섹션에서는 사용자에게 속하는 엔터프라이즈 및 개인 사서함을 모두 포함하는 사서함 집합 중 엔터프라이즈 사서함 하나를 현재 표시하는 인식된 메일 앱의 예를 살펴보겠습니다. 엔터프라이즈 사서함의 엔터프라이즈 데이터에서 데이터가 누출되지 않도록 앱은 [**ProtectionPolicyManager.TryApplyProcessUIPolicy**](https://msdn.microsoft.com/library/windows/apps/dn705791)를 호출하여 운영 체제에서 앱의 현재 컨텍스트가 엔터프라이즈 또는 개인인지를 알고 있는지 확인합니다. ID가 엔터프라이즈 정책에 의해 관리되지 않는 경우 API에서 false가 반환됩니다.

```CSharp
using Windows.Security.EnterpriseData;

...

public class Mailbox
{
    public bool HasEnterpriseMail { get { /* implementation */ } }
    public string Identity { get { /* implementation */ } }
}

private void SwitchMailbox(Mailbox targetMailbox)
{
    // Code goes here to perform setup for "targetMailbox".

    if (targetMailbox.HasEnterpriseMail)
    {
        bool result = 
            ProtectionPolicyManager.TryApplyProcessUIPolicy(targetMailbox.Identity);

        // Code goes here to process "result", which indicates whether
        // or not policy enforcement is in place for the identity.
    }
    else
    {
        // For personal mailboxes, we clear policy enforcement (in case
        // it is still set from when we processed an enterprise mailbox).
        ProtectionPolicyManager.ClearProcessUIPolicy();
    }
}
```

## 디바이스 잠금 이벤트 처리 및 메모리에 보호되지 않은 콘텐츠 남김 방지


이 시나리오에서는 엔터프라이즈 메일과 개인 메일을 둘 다 처리하도록 설계된 인식된 메일 앱의 예를 살펴보겠습니다. 안전한 DPL("잠금 상태에서 데이터 보호") 정책을 관리하도록 선택한 조직에서 앱을 실행하는 경우 디바이스가 잠겨 있는 동안 앱이 중요한 데이터를 메모리에서 모두 제거해야 합니다. 이 작업을 위해 앱은 디바이스가 잠기고 잠금 해제(DPL이 적용되는 경우)될 때마다 알림을 받도록 [**ProtectionPolicyManager.ProtectedAccessSuspending**](https://msdn.microsoft.com/library/windows/apps/dn705787) 및 [**ProtectionPolicyManager.ProtectedAccessResumed**](https://msdn.microsoft.com/library/windows/apps/dn705786) 이벤트에 등록합니다.

디바이스에 프로비전된 데이터 보호 키가 일시적으로 제거되기 전에 [**ProtectedAccessSuspending**](https://msdn.microsoft.com/library/windows/apps/dn705787)이 발생합니다. 디바이스가 잠겨 있을 때 이러한 키를 제거하는 이유는 디바이스가 잠겨 있고 해당 소유자의 소유가 아닐 수 있는 동안 암호화된 데이터를 무단으로 액세스할 수 없도록 하기 위한 것입니다. 디바이스 잠금이 해제되어 키를 다시 사용할 수 있게 되면 [**ProtectedAccessResumed**](https://msdn.microsoft.com/library/windows/apps/dn705786)가 발생합니다.

이러한 두 이벤트를 처리하면 앱이 [**DataProtectionManager**](https://msdn.microsoft.com/library/windows/apps/dn706017)를 사용하여 메모리에 있는 중요한 콘텐츠를 모두 보호할 수 있습니다. 또한 보호된 파일에 열려 있는 모든 파일 스트림을 닫아 시스템이 중요한 데이터를 메모리에 캐시하지 않도록 해야 합니다. 이 작업은 여러 가지 방법 중 하나로 수행할 수 있습니다. **StorageFile**의 Open 메서드에서 반환된 파일 스트림을 닫으려면 스트림에서 **Dispose** 메서드를 호출합니다. using 문(C\# 또는 VB)을 사용하여 스트림 사용 범위를 지정할 수 있습니다. 또는 스트림 주위에 **DataReader** 또는 **DataWriter**를 래핑하고 해당 개체와 함께 **Dispose** 메서드 또는 using 문을 사용할 수 있습니다.

**참고**  
DPL 정책이 구성되지 않은 환경에서는 [**ProtectedAccessResumed**](https://msdn.microsoft.com/library/windows/apps/dn705786) 이벤트는 발생하지만 [**ProtectedAccessSuspending**](https://msdn.microsoft.com/library/windows/apps/dn705787) 이벤트는 발생하지 않습니다. 코드에서 이 점을 인식하고, 모든 시스템에서 이벤트가 항상 쌍으로 발생하며 항상 이벤트를 사용하여 디바이스의 잠김/잠금 해제됨 상태를 확인할 수 있다고 가정하지 않도록 주의하세요. 다음 코드 예제에서 현재 표시된 메일의 보호된 상태 및 데이터베이스 파일 스트림의 열린 상태에 대해 아무 것도 가정하지 않는다는 것을 확인할 수 있습니다.

또한 DPL 정책이 구성되지 않은 디바이스의 잠금에서 다시 시작할 경우 [**ProtectedAccessResumedEventArgs.Identities**](https://msdn.microsoft.com/library/windows/apps/dn705772)는 빈 컬렉션입니다.

간단한 설명을 위해 이 코드 예제는 약간 간소화되었으며 엔터프라이즈 메일 처리에 집중합니다. 실제 앱에서는 개인 메일이 보호되지 않은 다른 메일 데이터베이스 파일에 기록되며 잠금 상태에서 개인 메일은 보호하지 않습니다.

```CSharp
using Windows.Security.Cryptography;
using Windows.Security.EnterpriseData;
using Windows.Storage;
using Windows.Storage.Streams;

...

public class DisplayedMail
{
    public string Body { get; set; }
    public IBuffer ProtectedBody { get; set; }
    public bool IsProtected { get; set; }
}

private IOutputStream mailDatabaseStream = null;
private string currentlyDisplayedMailIdentity = "contoso.com";
private DisplayedMail currentlyDisplayedMail = new DisplayedMail()
    { Body = "Lorem ipsum dolor...", ProtectedBody = null, IsProtected = false };

// Gets the app's protected mail database file, then opens and stores a stream on it.
private async void OpenMailDatabase()
{
    // Only attempt to open the database file stream if we know it&#39;s closed.
    if (this.mailDatabaseStream == null)
    {
        StorageFolder appDataStorageFolder = ApplicationData.Current.LocalFolder;
        StorageFile storageFile = await appDataStorageFolder.GetFileAsync("maildb.dat");
        using (IRandomAccessStream randomAccessStream =
            await storageFile.OpenAsync(FileAccessMode.ReadWrite))
        {
            this.mailDatabaseStream = randomAccessStream.GetOutputStreamAt(0);
        }
    }
}

// Called once by the app when starting up.
private void AppSetup()
{
    ProtectionPolicyManager.ProtectedAccessSuspending +=
        this.ProtectionPolicyManager_ProtectedAccessSuspending;
    ProtectionPolicyManager.ProtectedAccessResumed +=
        this.ProtectionPolicyManager_ProtectedAccessResumed;
    this.OpenMailDatabase();
}

// Background work called when the app receives an email.
private async void AppMailReceived(string fauxEmail)
{
    // Only attempt to write to the database file stream if we know it&#39;s open.
    if (this.mailDatabaseStream != null)
    {
        IBuffer emailAsBuffer = CryptographicBuffer.ConvertStringToBinary
            (fauxEmail, BinaryStringEncoding.Utf8);
        await this.mailDatabaseStream.WriteAsync(emailAsBuffer);
        await this.mailDatabaseStream.FlushAsync();
    }
    else
    {
        // Code goes here to handle the case where the device is
        // locked and we can't access the protected mail database.
    }
}

// Called by ProtectionPolicyManager when the device is locked if under DPL.
private async void ProtectionPolicyManager_ProtectedAccessSuspending
    (object sender, ProtectedAccessSuspendingEventArgs e)
{
    if (!new System.Collections.Generic.List<string>(e.Identities).Contains
        (this.currentlyDisplayedMailIdentity))
    {
        // This event is not for our identity. Another will be sent for our identity.
        return;
    }

    // Get suspension deferral.
    Windows.Foundation.Deferral deferral = e.GetDeferral();

    // Protect the displayed mail content.
    if (!this.currentlyDisplayedMail.IsProtected)
    {
        IBuffer mailBodyBuffer = CryptographicBuffer.ConvertStringToBinary
            (this.currentlyDisplayedMail.Body, BinaryStringEncoding.Utf8);
        BufferProtectUnprotectResult result = await DataProtectionManager.ProtectAsync
            (mailBodyBuffer, this.currentlyDisplayedMailIdentity);
        if (result.ProtectionInfo.Status == DataProtectionStatus.Protected)
        {
            // Save the protected version and clear the unprotected version.
            this.currentlyDisplayedMail.ProtectedBody = result.Buffer;
            this.currentlyDisplayedMail.Body = null;
        }
    }

    // Close the mail database file stream to make sure that we have no unprotected
    // content in memory.
    this.mailDatabaseStream.Dispose();
    this.mailDatabaseStream = null;

    // Optionally, code goes here to use e.Deadline to determine whether we have more
    // than 15 seconds left before the suspension deadline. If we do then process any
    // messages queued up for sending while we are still able to access them.

    // All done. Complete deferral.
    deferral.Complete();
}

// Called by ProtectionPolicyManager when the device is unlocked.
private async void ProtectionPolicyManager_ProtectedAccessResumed
    (object sender, ProtectedAccessResumedEventArgs e)
{
    if (!new System.Collections.Generic.List<string>(e.Identities).Contains
        (this.currentlyDisplayedMailIdentity))
    {
        // This event is not for our identity. Another will be sent for our identity.
        return;
    }

    // Unprotect the displayed mail content.
    if (this.currentlyDisplayedMail.IsProtected)
    {
        BufferProtectUnprotectResult result = await DataProtectionManager.UnprotectAsync
            (this.currentlyDisplayedMail.ProtectedBody);
        if (result.ProtectionInfo.Status == DataProtectionStatus.Unprotected)
        {
            // Restore the unprotected version.
            this.currentlyDisplayedMail.Body = CryptographicBuffer.ConvertBinaryToString
                (BinaryStringEncoding.Utf8, result.Buffer);
            this.currentlyDisplayedMail.ProtectedBody = null;
        }
    }

    // Reopen the closed mail database.
    this.OpenMailDatabase();
}
```

## 보호된 콘텐츠가 해지될 때 알림을 받도록 등록


메일 앱이 사용자 디바이스에서 엔터프라이즈 사서함을 설정한 시나리오를 그려 보세요. 일정 시점에서, 그리고 가능한 여러 이유 중 하나로 엔터프라이즈는 엔터프라이즈 보호된 메일과 해당 디바이스의 기타 리소스에 대한 액세스를 해지하려고 합니다. 가능한 해지 이유에는 여러 가지가 있습니다. 특정 엔터프라이즈 정책에서 등록 취소 시 해지를 트리거할 가능성이 크기 때문에 한 시나리오는 사용자가 엔터프라이즈에서 해당 디바이스 등록을 취소한 경우(디바이스를 선물 또는 판매했거나, 다른 디바이스를 사용하려 하거나, 다른 직업으로 이동하거나, 퇴직하는 경우)입니다. 또 다른 시나리오는 가령 디바이스가 분실된 것으로 보고되어 MDM(모바일 디바이스 관리)을 통해 원격으로 IT 관리자가 등록 취소 알림을 보낸 경우입니다.

사례의 구체적인 사항에 관계없이 엔터프라이즈는 더 이상 디바이스에 있을 필요가 없으므로 사용자 디바이스에서 모든 메일을 지우는 요청을 보냅니다. 디바이스의 원격 관리 클라이언트는 엔터프라이즈의 원격 관리 서버에서 요청을 받고 [**ProtectionPolicyManager.RevokeContent**](https://msdn.microsoft.com/library/windows/apps/dn705790)를 호출하여 디바이스에서 해당 엔터프라이즈 ID로 보호된 콘텐츠에 액세스하는 데 필요한 키를 해지합니다.

해지가 발생할 때 앱이 알림을 받아야 하는 경우 [**ProtectionPolicyManager.ProtectedContentRevoked**](https://msdn.microsoft.com/library/windows/apps/dn705788) 이벤트에 등록할 수 있습니다. 앱이 이벤트를 받으면 더 이상 필요하지 않은 엔터프라이즈 사서함과 관련된 메타데이터를 모두 삭제할 수 있습니다.

```CSharp
using Windows.Security.EnterpriseData;

...

private string mailIdentity = "contoso.com";

void MailAppSetup()
{
    ProtectionPolicyManager.ProtectedContentRevoked += ProtectionPolicyManager_ProtectedContentRevoked;
    // Code goes here to set up mailbox for &#39;mailIdentity&#39;.
}

private void ProtectionPolicyManager_ProtectedContentRevoked(object sender, ProtectedContentRevokedEventArgs e)
{
    if (!new System.Collections.Generic.List<string>(e.Identities).Contains
        (this.mailIdentity))
    {
        // This event is not for our identity.
        return;
    }

    // Code goes here to delete any metadata associated with &#39;mailIdentity&#39;.
}
```

## 원격 관리 클라이언트에서 엔터프라이즈 보호된 데이터 지우기 시작


사용자의 개인 디바이스에는 엔터프라이즈 ID로 보호된 여러 엔터프라이즈 파일이 있습니다. 사용자가 디바이스를 분실합니다. 사용자가 디바이스를 분실했다는 알림을 받으면 엔터프라이즈는 해당 데이터가 누출되지 않도록 사용자 디바이스에서 중요한 데이터를 모두 지우는 요청을 보냅니다. 디바이스의 원격 관리 클라이언트는 엔터프라이즈의 원격 관리 서버에서 요청을 받고 [**ProtectionPolicyManager.RevokeContent**](https://msdn.microsoft.com/library/windows/apps/dn705790)를 호출하여 해당 엔터프라이즈 ID로 보호된 콘텐츠에 액세스하는 데 필요한 키를 해지합니다.

```CSharp
Windows.Security.EnterpriseData.ProtectionPolicyManager.RevokeContent("contoso.com");
```

 

 





<!--HONumber=Mar16_HO5-->



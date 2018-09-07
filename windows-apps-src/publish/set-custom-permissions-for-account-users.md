---
author: jnHs
Description: Set roles or custom permissions for account users.
title: 계정 사용자에 대한 역할 또는 사용자 지정 권한 설정
ms.assetid: 99f3aa18-98b4-4919-bd7b-d78356b0bf78
ms.author: wdg-dev-content
ms.date: 07/11/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 사용자 역할, 사용자 권한, 역할 사용자 지정, 사용자 액세스, 권한 사용자 지정, 표준 역할
ms.localizationpriority: medium
ms.openlocfilehash: a4100248857af655f388ad318bb3ae5176aaf046
ms.sourcegitcommit: 00d27738325d6db5b5e481911ae7fac0711b05eb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/07/2018
ms.locfileid: "3663050"
---
# <a name="set-roles-or-custom-permissions-for-account-users"></a>계정 사용자에 대한 역할 또는 사용자 지정 권한 설정

[개발자 센터 계정에 사용자 추가](add-users-groups-and-azure-ad-applications.md)할 때는 계정 내의 액세스 대상을 지정해야 합니다. 이를 위해 전체 계정에 적용되는 [표준 역할](#roles)을 할당하거나 [사용 권한 사용자 지정](#custom)을 통해 적정 수준의 액세스 권한을 제공할 수 있습니다. 사용자 지정 권한 중 일부는 전체 계정에 적용되며, 일부는 한 제품이나 특정 제품들로 제한될 수 있습니다(원할 경우 모든 제품에 권한 부여가 가능).

> [!NOTE] 
> 사용자, 그룹, Azure AD 응용 프로그램 등 무엇을 추가하든 동일한 역할 및 권한을 적용할 수 있습니다.

어떤 역할 또는 권한을 적용할지 결정할 때 다음을 명심해야 합니다. 
-   사용자(그룹 및 Azure AD 응용 프로그램 포함)는 할당된 역할과 관련된 권한으로 전체 개발자 센터 계정에 액세스할 수 있습니다. 단, 특정 앱 및/또는 추가 기능에 적용되도록 [사용 권한 사용자 지정](#custom)을 하고 [제품 수준 사용 권한](#product-level-permissions)을 할당한 경우는 제외합니다.
-   여러 역할을 선택하거나, 원하는 액세스 권한을 부여하도록 사용자 지정 권한을 사용하여 사용자, 그룹 또는 Azure AD 응용 프로그램이 둘 이상의 역할 기능에 액세스하도록 할 수 있습니다.
-   특정 역할이 있는 사용자(또는 사용자 지정 권한 집합)는 다른 역할(또는 권한 집합)이 있는 그룹의 일부가 될 수도 있습니다. 이런 경우 사용자는 그룹과 개별 계정 모두와 연결된 기능 모두에 액세스할 수 있습니다.

> [!TIP]
> 이 항목은 Windows 앱 개발자 프로그램에만 해당합니다. 하드웨어 개발자 프로그램에서 사용자 역할에 대한 정보는 [사용자 역할 관리](https://docs.microsoft.com/windows-hardware/drivers/dashboard/managing-user-roles)를 참조하세요. Windows 데스크톱 응용 프로그램에서 사용자 역할에 대한 정보는 [Windows 데스크톱 응용 프로그램](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#add-and-manage-account-users)을 참조하세요.


<span id="roles" />

## <a name="assign-roles-to-account-users"></a>계정 사용자에게 역할 할당

기본적으로 사용자, 그룹 또는 Azure AD 응용 프로그램을 개발자 센터 계정에 추가할 때 선택할 수 있는 표준 역할 집합이 표시됩니다. 각 역할에는 계정 내에서 특정 기능을 수행하기 위해 특정 사용 권한 집합이 있습니다. 

**사용 권한 사용자 지정**을 선택하여 [사용자 지정 권한](#custom)을 정의하지 않는 한, 계정에 추가하는 각각의 사용자, 그룹 또는 Azure AD 응용 프로그램에 다음 표준 역할 중 하나 이상을 할당해야 합니다. 

> [!NOTE]
> 계정 **소유자**는 Microsoft 계정에 처음으로 해당 계정을 만든 사용자입니다(Azure AD를 통해 추가된 사용자가 아님). 앱 삭제, 모든 계정 사용자 생성 및 편집, 모든 재무 및 계정 설정 변경 기능을 비롯하여 계정에 대한 전체 액세스 권한은 이 계정 소유자에게만 있습니다. 


| 역할                 | 설명              |
|----------------------|--------------------------|
| 관리자              | 세금 및 지급액 설정 변경을 제외하고 계정에 대한 모든 권한이 있습니다. 여기에는 개발자 센터의 사용자를 관리하는 권한이 포함되지만 Azure AD 테넌트에서 사용자를 생성하고 삭제하는 기능은 Azure AD에서 계정의 사용 권한에 따라 다릅니다. 즉, 사용자에게 전역 관리자 역할이 할당되었지만 조직의 Azure AD에서 관리자 권한을 가지고 있지 않은 경우에는 새 사용자를 만들거나 디렉터리에서 사용자를 삭제할 수 없습니다(사용자의 개발자 센터 역할은 변경할 수 있음). <p> 개발자 센터 계정이 여러 개의 Azure AD 테넌트와 연결된 경우 관리자는 해당 테넌트에 대해 전역 관리자 권한이 있는 계정을 가진 사용자와 동일한 테넌트에 로그인하지 않으면 사용자에 대한 전체 정보(이름, 성, 암호 복구 메일, Azure AD 전역 관리자인지 여부 포함)를 볼 수 없습니다. 그러나 개발자 센터 계정과 연결된 모든 테넌트에서 사용자를 추가 및 제거할 수 있습니다. |
| 개발자            | 패키지를 업로드하고 앱 및 추가 기능을 제출할 수 있으며 원격 분석 세부 사항에 대한 [사용 보고서](usage-report.md)를 볼 수 있습니다. [장치 간 환경](https://go.microsoft.com/fwlink/?linkid=874042) 기능에 액세스할 수 있습니다. 재무 정보 또는 계정 설정을 볼 수 없습니다.   |
| 비즈니스 기여자 | [상태](health-report.md) 및 [사용](usage-report.md) 보고서를 볼 수 있습니다. 제품을 만들거나 제출할 수 없고, 계정 설정을 변경하거나 재무 정보를 볼 수 없습니다.   |
| 재무 기여자  | [지급 보고서](payout-summary.md), 재무 정보 및 구입 보고서를 볼 수 있습니다. 앱, 추가 기능 또는 계정 설정을 변경할 수 없습니다.    |
| 마케팅 담당자             | [고객 리뷰에 응답](respond-to-customer-reviews.md)할 수 있고 비재무 [분석 보고서](analytics.md)를 볼 수 있습니다. 앱, 추가 기능 또는 계정 설정을 변경할 수 없습니다.      |

다음 표에서는 이러한 역할 각각(계정 소유자 포함)이 사용할 수 있는 특정 기능 중 일부를 보여 줍니다.

|                                 |    계정 소유자                 |    관리자                       |    개발자                     |    비즈니스 기여자    |    재무 기여자    |    마케팅 담당자                      |
|---------------------------------|----------------------------------|----------------------------------|----------------------------------|----------------------------|---------------------------|----------------------------------|
|    구입 보고서           |    볼 수 있음                      |    볼 수 있음                      |     액세스 권한 없음                    |     액세스 권한 없음              |    볼 수 있음               |    액세스 권한 없음                     |
|    피드백 보고서/응답    |    볼 수 있고 피드백 보낼 수 있음    |    볼 수 있고 피드백 보낼 수 있음    |    볼 수 있고 피드백 보낼 수 있음    |     액세스 권한 없음              |     액세스 권한 없음             |    볼 수 있고 피드백 보낼 수 있음    |
|    상태 보고서                |    볼 수 있음                      |    볼 수 있음                      |    볼 수 있음                      |    볼 수 있음                |     액세스 권한 없음             |    액세스 권한 없음                     |
|    사용 보고서                 |    볼 수 있음                      |    볼 수 있음                      |    볼 수 있음                      |    볼 수 있음                |     액세스 권한 없음             |    액세스 권한 없음                     |
|    지급 계좌               |    업데이트할 수 있음                    |    액세스 권한 없음                     |    액세스 권한 없음                     |    액세스 권한 없음               |    볼 수 있음               |    액세스 권한 없음                     |
|    세금 프로필                  |    업데이트할 수 있음                    |    액세스 권한 없음                     |    액세스 권한 없음                     |    액세스 권한 없음               |    볼 수 있음               |    액세스 권한 없음                     |
|    지급 요약               |    볼 수 있음                      |    액세스 권한 없음                     |    액세스 권한 없음                     |    액세스 권한 없음               |    볼 수 있음               |    액세스 권한 없음                     |

적절한 표준 역할이 없거나 특정 앱 및/또는 추가 기능에 대한 액세스를 제한하려면 아래 설명한 대로 **사용 권한 사용자 지정**을 선택하여 사용자에게 사용자 지정 권한을 부여할 수 있습니다.


<span id="custom" />

## <a name="assign-custom-permissions-to-account-users"></a>계정 사용자에게 사용자 지정 권한 할당

표준 역할 대신 사용자 지정 권한을 할당하려면 사용자 계정을 추가 또는 편집할 때 **역할** 섹션에서 **사용 권한 사용자 지정**을 클릭합니다. 

사용자에 대한 권한을 사용하도록 설정하려면 확인란을 적절한 설정으로 전환합니다. 

![액세스 설정 가이드](images/permission_key.png)

- **액세스 권한 없음**: 사용자가 표시된 권한을 가지고 있지 않습니다.
- **읽기 전용**: 사용자가 표시된 영역과 관련된 기능을 볼 수는 있지만 변경할 수는 없습니다. 
- **읽기/쓰기**: 사용자가 영역과 관련된 내용을 변경할 수도 있고 볼 수도 있습니다.
- **혼합**:이 옵션은 직접 선택할 수 없으며, 해당 권한에 대한 액세스 조합을 허용한 경우 **혼합** 표시기가 나타납니다. 예를 들어, **모든 제품**에 대한 **가격 책정 및 가용성**에 대해 **읽기 전용** 권한을 부여한 다음 특정 제품 하나의 **가격 책정 및 가용성**에 대해 **읽기/쓰기** 권한을 부여하는 경우, **모든 제품**에 대한 **가격 책정 및 가용성** 표시기가 혼합으로 표시됩니다. 일부 제품에는 **액세스 권한 없음**이 부여되었지만 다른 제품에는 **읽기/쓰기** 및/또는 **읽기 전용** 액세스 권한이 부여된 경우에도 동일한 원리가 적용됩니다.

분석 데이터 보기와 관련된 권한 등 일부 권한의 경우 **읽기 전용** 권한만 허용됩니다. 현재 구현에서 일부 권한은 **읽기 전용**과 **읽기/쓰기** 액세스 권한을 구분하지 않습니다. **읽기 전용** 및/또는 **읽기/쓰기** 권한에 의해 부여되는 특정 기능을 파악하려면 각 권한에 대한 세부 정보를 검토하세요.

각 권한에 대한 특정 세부 정보는 아래의 표에서 설명합니다.

## <a name="account-level-permissions"></a>계정 수준 권한

이 섹션의 사용 권한은 특정 제품으로 제한되지 않습니다. 이러한 액세스 권한 중 하나를 부여하면 사용자는 전체 계정에 대해 해당 권한을 갖게 됩니다.

<table>
    <colgroup>
    <col width="20%" />
    <col width="40%" />
    <col width="40%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">사용 권한 이름</th>
    <th align="left">읽기 전용</th>
    <th align="left">읽기/쓰기</th>
    </tr>
    </thead>
    <tbody>
<tr><td align="left">    <b>계정 설정</b>                    </td><td align="left">  <a href="managing-your-profile.md">연락처 정보</a>를 포함하여 <b>계정 설정</b> 섹션의 모든 페이지를 볼 수 있습니다.       </td><td align="left">  <b>계정 설정</b> 섹션의 모든 페이지를 볼 수 있습니다. <a href="managing-your-profile.md">연락처 정보</a> 및 다른 페이지를 변경할 수 있지만, 지급 계좌 또는 세금 프로필은 변경할 수 없습니다(해당 권한을 별도로 부여하지 않는 한).            </td></tr>
<tr><td align="left">    <b>계정 사용자</b>                       </td><td align="left">  <b>사용자</b> 섹션에서 계정에 추가된 사용자를 볼 수 있습니다.          </td><td align="left">  사용자를 계정에 추가하고 <b>사용자</b> 섹션에서 기존 사용자를 변경할 수 있습니다.             </td></tr>
<tr><td align="left">    <b>계정 수준 광고 성과 보고서</b> </td><td align="left">  계정 수준 <a href="advertising-performance-report.md">광고 성과 보고서</a>를 볼 수 있습니다.      </td><td align="left">  해당 없음   </td></tr>
<tr><td align="left">    <b>광고 캠페인</b>                        </td><td align="left">  계정에서 만든 <a href="create-an-ad-campaign-for-your-app.md">광고 캠페인</a>을 볼 수 있습니다.      </td><td align="left">  계정에서 <a href="create-an-ad-campaign-for-your-app.md">광고 캠페인</a>을 만들고 관리하고 볼 수 있습니다.          </td></tr>
<tr><td align="left">    <b>광고 조정</b>                        </td><td align="left">  계정의 모든 제품에 대한 광고 조정 구성을 볼 수 있습니다.    </td><td align="left">  계정의 모든 제품에 대한 광고 조정 구성을 보고 변경할 수 있습니다.        </td></tr>
<tr><td align="left">    <b>광고 조정 보고서</b>                </td><td align="left">  계정의 모든 제품에 대한 <a href="ad-mediation-report.md">광고 조정 보고서</a>를 볼 수 있습니다.    </td><td align="left">  해당 없음    </td></tr>
<tr><td align="left">    <b>광고 성과 보고서</b>              </td><td align="left">  계정의 모든 제품에 대한 <a href="advertising-performance-report.md">광고 성과 보고서</a>를 볼 수 있습니다.       </td><td align="left">  해당 없음         </td></tr>
<tr><td align="left">    <b>광고 단위</b>                            </td><td align="left">  계정에 대해 생성된 <a href="in-app-ads.md">광고 단위</a>를 볼 수 있습니다.    </td><td align="left">  계정에 대한 <a href="in-app-ads.md">광고 단위</a>를 만들고 관리하고 볼 수 있습니다.             </td></tr>
<tr><td align="left">    <b>계열사 광고</b>                       </td><td align="left">  계정의 모든 제품에서 <a href="about-affiliate-ads.md">계열사 광고</a> 사용을 볼 수 있습니다.    </td><td align="left">  계정의 모든 제품에 대한 <a href="about-affiliate-ads.md">계열사 광고</a> 사용을 관리하고 볼 수 있습니다.                </td></tr>
<tr><td align="left">    <b>계열사 성과 보고서</b>      </td><td align="left">  계정의 모든 제품에 대한 <a href="affiliates-performance-report.md">계열사 성과 보고서</a>를 볼 수 있습니다.   </td><td align="left">  해당 없음   </td></tr>
<tr><td align="left">    <b>앱 설치 광고 보고서</b>             </td><td align="left">  <a href="promote-your-app-report.md">광고 캠페인 보고서</a>를 볼 수 있습니다.           </td><td align="left">  해당 없음   </td></tr>
<tr><td align="left">    <b>커뮤니티 광고</b>                       </td><td align="left">  계정의 모든 제품에 대한 무료 <a href="about-community-ads.md">커뮤니티 광고</a> 사용을 볼 수 있습니다.          </td><td align="left">  계정의 모든 제품에 대한 무료 <a href="about-community-ads.md">커뮤니티 광고</a> 사용을 만들고 관리하고 볼 수 있습니다.               </td></tr>
<tr><td align="left">    <b>연락처 정보</b>                        </td><td align="left">  계정 설정 섹션에서 <a href="managing-your-profile.md">연락처 정보</a>를 볼 수 있습니다.        </td><td align="left">  계정 설정 섹션에서 <a href="managing-your-profile.md">연락처 정보</a>를 편집하고 볼 수 있습니다.            </td></tr>
<tr><td align="left">    <b>COPPA 준수</b>                    </td><td align="left">  계정의 모든 제품에 대한 <a href="in-app-ads.md#coppa-compliance">COPPA 준수</a> 선택 사항을 볼 수 있습니다(제품이 13세 이하 어린이를 대상으로 하는지 여부를 나타냄).                                            </td><td align="left">  계정의 모든 제품에 대한 <a href="in-app-ads.md#coppa-compliance">COPPA 준수</a> 선택 사항을 편집하고 볼 수 있습니다(제품이 13세 이하 어린이를 대상으로 하는지 여부를 나타냄).         </td></tr>
<tr><td align="left">    <b>고객 그룹</b>                     </td><td align="left">  <a href="create-customer-groups.md">고객 그룹</a> (고객층 및 알려진된 사용자 그룹)을 볼 수 있습니다.      </td><td align="left">  수 제작, 편집, 하 고 <a href="create-customer-groups.md">고객 그룹</a> (고객층 및 알려진된 사용자 그룹)을 확인 합니다.       </td></tr>
<tr><td align="left">    <b>제품 그룹 관리</b>&nbsp;*                            </td><td align="left">  제품 그룹 만들기 페이지를 볼 수는 있지만 계정에서 제품 그룹을 실제로 만들 수는 없습니다.    </td><td align="left">  제품 그룹을 만들고 편집할 수 있습니다.     </td></tr>
<tr><td align="left">    <b>새 앱</b>                            </td><td align="left">  새 앱 만들기 페이지를 볼 수는 있지만 계정에서 새 앱을 실제로 만들 수는 없습니다.    </td><td align="left">  새 앱 이름을 예약하여 계정에서 <a href="create-your-app-by-reserving-a-name.md">새 앱을 만들</a> 수 있으며, 제출을 만들고 Microsoft Store에 앱을 제출할 수 있습니다.     </td></tr>
<tr><td align="left">    <b>새 번들</b>&nbsp;*                       </td><td align="left">  새 번들 만들기 페이지를 볼 수 있지만 계정에서 실제로 새 번들을 만들 수는 없습니다.     </td><td align="left">  제품의 새 번들을 만들 수 있습니다.          </td></tr>
<tr><td align="left">    <b>파트너 서비스</b>&nbsp;*                  </td><td align="left">  XToken을 검색하는 서비스를 설치하기 위한 인증서를 볼 수 있습니다.     </td><td align="left">  XToken을 검색하는 서비스를 설치하기 위한 인증서를 관리하고 볼 수 있습니다.       </td></tr>
<tr><td align="left">    <b>지급 계좌</b>                      </td><td align="left">  <b>계정 설정</b>에서 <a href="setting-up-your-payout-account-and-tax-forms.md#payout-account">지급 계정 정보</a>를 볼 수 있습니다.     </td><td align="left">  <b>계정 설정</b>에서 <a href="setting-up-your-payout-account-and-tax-forms.md#payout-account">지급 계정 정보</a>를 편집하고 볼 수 있습니다.       </td></tr>
<tr><td align="left">    <b>지급 요약</b>                      </td><td align="left">  지급 보고 정보에 액세스하고 다운로드할 수 있는 <a href="payout-summary.md">지급 요약</a>을 볼 수 있습니다.       </td><td align="left">  지급 보고 정보에 액세스하고 다운로드할 수 있는 <a href="payout-summary.md">지급 요약</a>을 볼 수 있습니다.   </td></tr>
<tr><td align="left">    <b>신뢰 당사자</b>&nbsp;*                   </td><td align="left">  XToken을 검색하는 신뢰 당사자를 볼 수 있습니다.    </td><td align="left">  XToken을 검색하는 신뢰 당사자를 관리하고 볼 수 있습니다.     </td></tr>
<tr><td align="left">    <b>디스크 요청</b>&nbsp;*                   </td><td align="left">  게임 디스크 요청을 볼 수 있습니다.    </td><td align="left">  게임 디스크 요청을 구성하고 볼 수 있습니다.     </td></tr>
<tr><td align="left">    <b>샌드박스</b>&nbsp;*                         </td><td align="left">  <b>샌드박스</b> 페이지에 액세스하여 계정의 샌드박스 및 해당 샌드박스에 적용되는 구성을 볼 수 있습니다. 적절한 제품 수준 사용 권한이 부여되지 않으면 각 샌드박스에 대한 제품과 제출을 볼 수 없습니다. </td><td align="left">  <b>샌드박스</b> 페이지에 액세스하고, 샌드박스 만들기와 삭제 및 구성 관리를 포함하여 계정의 샌드박스를 보고 관리할 수 있습니다. 적절한 제품 수준 사용 권한이 부여되지 않으면 각 샌드박스에 대한 제품과 제출을 볼 수 없습니다.    </td></tr>
<tr><td align="left">    <b>Microsoft Store 영업 이벤트</b>&nbsp;*                            </td><td align="left">  해당 없음    </td><td align="left">  자동으로 Microsoft Store 영업 이벤트에서 제품을 포함하는 옵션을 구성할 수 있습니다.     </td></tr>
<tr><td align="left">    <b>세금 프로필</b>                         </td><td align="left">  <b>계정 설정</b>의 <a href="setting-up-your-payout-account-and-tax-forms.md#tax-forms">세금 프로필 정보 및 양식</a>을 볼 수 있습니다.     </td><td align="left">  <b>계정 설정</b>에서 세금 양식을 작성하고 <a href="setting-up-your-payout-account-and-tax-forms.md#tax-forms">세금 프로필 정보</a>를 업데이트할 수 있습니다.     </td></tr>
<tr><td align="left">    <b>테스트 계정</b>&nbsp;*                     </td><td align="left">  Xbox Live 구성을 테스트하기 위한 계정을 볼 수 있습니다.      </td><td align="left">  Xbox Live 구성을 테스트하기 위한 계정을 만들고 관리하고 볼 수 있습니다.      </td></tr>
<tr><td align="left">    <b>Xbox 장치</b>                        </td><td align="left">  <b>계정 설정</b> 섹션에서 계정에 대해 사용하도록 설정된 Xbox 개발 콘솔을 볼 수 있습니다.       </td><td align="left">  <b>계정 설정</b> 섹션에서 계정에 대해 사용하도록 설정된 Xbox 개발 콘솔을 추가하고 제거하고 볼 수 있습니다.     </td></tr>
    </tbody>
    </table>

별표(*)로 표시된 \* 사용 권한은 모든 계정에서 사용할 수 없는 기능에 대한 액세스를 허용합니다. 계정이 이러한 기능을 사용하도록 설정되지 않은 경우, 해당 사용 권한을 선택해도 아무런 영향이 없습니다.   


## <a name="product-level-permissions"></a>제품 수준 사용 권한

이 섹션의 사용 권한을 계정의 모든 제품에 부여할 수 있습니다. 또는 하나 이상의 특정 제품에만 권한을 허용하도록 사용자 지정할 수도 있습니다. 

제품 수준 사용 권한은 **분석**, **수익 창출**, **게시** 및 **Xbox Live**의 4개 범주로 그룹화됩니다. 각 범주에서 개별 사용 권한을 보려면 각 범주를 확장할 수 있습니다. 하나의 이상의 특정 제품에 대해 **모든 사용 권한**을 허용하는 옵션이 있습니다.

계정의 모든 제품에 대해 사용 권한을 부여하려면 **모든 제품**이라고 표시된 행에서 해당 권한을 선택합니다(**읽기 전용** 또는 **읽기/쓰기**를 표시하도록 확인란을 전환하여). 
 
> [!TIP]
> **모든 제품**에 대한 선택 사항은 현재 계정의 모든 제품은 물론이고 해당 계정에서 생성된 추가 제품에도 적용됩니다. 사용 권한이 향후 제품에 적용되지 않도록 하려면 **모든 제품**을 선택하지 않고 모든 제품을 개별적으로 선택합니다.

**모든 제품** 행 아래에서 계정의 각 제품이 별도의 행에 나열됩니다. 특정 제품에 대해서만 사용 권한을 부여하려면 해당 제품의 행에서 해당 권한을 선택합니다.

각 추가 기능은 **모든 추가 기능**과 함께 상위 제품 아래 별도의 행에 나열됩니다. **모든 추가 기능**에 대한 선택 사항은 해당 제품의 모든 현재 추가 기능은 물론 앞으로 생성될 추가 기능에도 적용됩니다.

일부 사용 권한은 추가 기능에 대해 설정할 수 없습니다. 이는 해당 권한이 추가 기능에 적용되지 않기 때문이거나(예: **고객 의견** 권한), 상위 제품 수준에서 부여된 권한이 해당 제품의 모든 추가 기능에 적용되기 때문입니다(예: **홍보 코드**). 그러나 추가 기능에 대해 사용 가능한 권한은 별도로 설정해야 합니다. 추가 기능은 상위 제품에 대해 선택한 항목을 상속하지 않습니다. 예를 들어 사용자가 추가 기능에 대해 가격 책정 및 가용성을 선택할 수 있도록 하려면, 상위 제품에 대해 **가격 책정 및 가용성** 권한을 부여했는지와 상관없이 추가 기능에 대해(또는 **모든 추가 기능**에 대해) **가격 책정 및 가용성**을 사용하도록 설정해야 합니다. 


### <a name="analytics"></a>분석

<table>
    <thead>
    <tr class="header">
    <th align="left">사용 권한&nbsp;이름</th>
    <th align="left">읽기&nbsp;전용</th>
    <th align="left">읽기/쓰기</th>
    <th align="left">읽기만(추가&nbsp;기능)&nbsp; </th>
    <th align="left">읽기-쓰기(추가&nbsp;기능)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    <b>구입</b>     </td><td>    제품에 대한 <a href="acquisitions-report.md">구입</a> 및 <a href="add-on-acquisitions-report.md">추가 기능 구입</a> 보고서를 볼 수 있습니다.        </td><td>    해당 없음    </td><td>    해당 없음 (상위 제품에 대 한 설정에 **추가 기능 구입** 보고서 포함)        </td><td>    해당 없음                         </td></tr>
    <tr><td align="left">    <b>사용</b> </td><td>    제품에 대한 <a href="usage-report.md">사용 보고서</a>를 볼 수 있습니다.     </td><td>    해당 없음       </td><td>    해당 없음     </td><td>    해당 없음         </td></tr>
    <tr><td align="left">    <b>상태</b> </td><td>    제품에 대한 <a href="health-report.md">상태 보고서</a>를 볼 수 있습니다.    </td><td>    해당 없음     </td><td>    해당 없음     </td><td>    해당 없음         </td></tr>
    <tr><td align="left">    <b>고객 피드백</b>    </td><td>    제품에 대한 <a href="reviews-report.md">리뷰</a> 및 <a href="feedback-report.md">피드백</a> 보고서를 볼 수 있습니다.       </td><td>    해당 없음(피드백이나 리뷰에 응답하려면 <b>고객에게 문의</b> 권한을 부여받아야 함)   </td><td>    해당 없음     </td><td>    해당 없음         </td></tr>
    <tr><td align="left">    <b>Xbox 분석</b> </td><td>    제품에 대 한 [Xbox 분석 보고서](xbox-analytics-report.md) 를 볼 수 있습니다.    </td><td>    해당 없음   </td><td>    해당 없음       </td><td>    해당 없음          </td></tr>
    <tr><td align="left">    <b>실시간</b>   </td><td>    제품에 대한 실시간 보고서를 볼 수 있습니다. (주: 이 보고서는 <a href="dev-center-insider-program.md">개발자 센터 참가자 프로그램</a>을 통해서만 제공되고 있습니다.)      </td><td>    해당 없음   </td><td>    해당 없음     </td><td>    해당 없음                 </td></tr>
    </tbody>
    </table>

### <a name="monetization"></a>수익 창출

<table>
    <thead>
    <tr class="header">
    <th align="left">사용 권한&nbsp;이름</th>
    <th align="left">읽기&nbsp;전용</th>
    <th align="left">읽기/쓰기</th>
    <th align="left">읽기만(추가&nbsp;기능)&nbsp; </th>
    <th align="left">Read&#8209;write&nbsp;(Add&#8209;on)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    <b>홍보 코드</b>     </td><td>    제품 및 추가 기능에 대한 <a href="generate-promotional-codes.md">홍보 코드</a> 주문과 사용 정보를 볼 수 있습니다.         </td><td>    제품 및 추가 기능에 대한 <a href="generate-promotional-codes.md">홍보 코드</a> 주문을 보고 관리하고 만들 수 있으며, 사용 정보를 볼 수 있습니다.          </td><td>    해당 없음(상위 제품에 대한 설정이 모든 추가 기능에 적용됨)     </td><td>    해당 없음(상위 제품에 대한 설정이 모든 추가 기능에 적용됨)     </td></tr>
    <tr><td align="left">    <b>대상 제품</b>     </td><td>    제품에 대한 <a href="use-targeted-offers-to-maximize-engagement-and-conversions.md">대상 제품</a>을 볼 수 있습니다.         </td><td>    제품에 대한 <a href="use-targeted-offers-to-maximize-engagement-and-conversions.md">대상 제품</a>을 확인하고 관리하고 만들 수 있습니다.          </td><td>    해당 없음     </td><td>    해당 없음      </td></tr>
    <tr><td align="left">    <b>고객에게 문의</b>  </td><td>    <b>고객 의견</b> 권한도 부여된 경우에 한해 <a href="respond-to-customer-feedback.md">고객 의견에 대한 응답</a> 및 <a href="respond-to-customer-reviews.md">고객 리뷰에 대한 응답</a>을 볼 수 있습니다. 제품에 대해 생성된 <a href="send-push-notifications-to-your-apps-customers.md">대상이 지정된 알림</a>을 볼 수 있습니다.    </td><td>    <b>고객</b> 의견 권한도 부여 된 아니라 <a href="respond-to-customer-feedback.md">고객 피드백에 응답</a> 하 고 <a href="respond-to-customer-reviews.md">고객 리뷰에 응답할</a>수 있습니다. 제품에 대해 <a href="send-push-notifications-to-your-apps-customers.md">대상이 지정된 알림을 만들고 보낼</a> 수도 있습니다.                   </td><td>    해당 없음         </td><td>    해당 없음                          </td></tr>
    <tr><td align="left">    <b>실험</b></td><td>    제품에 대한 <a href="../monetize/run-app-experiments-with-a-b-testing.md">실험(A/B 테스트)</a> 및 실험 데이터를 볼 수 있습니다.   </td><td>    제품에 대한 <a href="../monetize/run-app-experiments-with-a-b-testing.md">실험(A/B 테스트)</a>을 만들고 관리하고 볼 수 있으며, 실험 데이터를 볼 수 있습니다.     </td><td>    해당 없음  </td><td>    해당 없음                 </td></tr>
    <tr><td align="left">    <b>Microsoft Store 영업 이벤트</b>&nbsp;*</td><td>    제품에 대한 영업 이벤트 상태를 볼 수 있습니다.   </td><td>    영업 이벤트에 제품을 추가하고 할인을 구성할 수 있습니다.      </td><td>    제품에 대한 영업 이벤트 상태를 볼 수 있습니다.   </td><td>    영업 이벤트에 제품을 추가하고 할인을 구성할 수 있습니다.      </td></tr>
    </tbody>
    </table>

### <a name="publishing"></a>게시 

<table>
    <thead>
    <tr class="header">
    <th align="left">사용 권한&nbsp;이름</th>
    <th align="left">읽기&nbsp;전용</th>
    <th align="left">읽기/쓰기</th>
    <th align="left">읽기만(추가&nbsp;기능)&nbsp; </th>
    <th align="left">읽기-쓰기(추가&nbsp;기능)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    <b>가격 책정 및 가용성</b>  </td><td>    제품 제출의 <a href="set-app-pricing-and-availability.md">가격 책정 및 가용성</a> 페이지를 볼 수 있습니다.     </td><td>    제품 제출의 <a href="set-app-pricing-and-availability.md">가격 책정 및 가용성</a> 페이지를 보고 편집할 수 있습니다. </td><td>    추가 기능 제출의 <a href="set-add-on-pricing-and-availability.md">가격 책정 및 가용성</a> 페이지를 볼 수 있습니다.   </td><td>    추가 기능 제출의 <a href="set-add-on-pricing-and-availability.md">가격 책정 및 가용성</a> 페이지를 보고 편집할 수 있습니다.          </td></tr>
    <tr><td align="left">    <b>속성</b>   </td><td>    제품 제출의 <a href="enter-app-properties.md">속성</a> 페이지를 볼 수 있습니다.      </td><td>    제품 제출의 <a href="enter-app-properties.md">속성</a> 페이지를 보고 편집할 수 있습니다.       </td><td>    추가 기능 제출의 <a href="enter-add-on-properties.md">속성</a> 페이지를 볼 수 있습니다.     </td><td>    추가 기능 제출의 <a href="enter-add-on-properties.md">속성</a> 페이지를 보고 편집할 수 있습니다.               </td></tr>
    <tr><td align="left">    <b>연령별 등급</b>    </td><td>    제품 제출의 <a href="age-ratings.md">연령별 등급</a> 페이지를 볼 수 있습니다.       </td><td>    제품 제출의 <a href="age-ratings.md">연령별 등급</a> 페이지를 보고 편집할 수 있습니다.    </td><td>    추가 기능 제출의 연령별 등급 페이지를 볼 수 있습니다.          </td><td>     추가 기능 제출의 연령별 등급 페이지를 보고 편집할 수 있습니다.       </td></tr>
    <tr><td align="left">    <b>패키지</b>        </td><td>    제품 제출의 <a href="upload-app-packages.md">패키지</a> 페이지를 볼 수 있습니다.  </td><td>    제품 제출의 <a href="upload-app-packages.md">패키지</a> 페이지를 보고 편집할 수 있습니다(패키지 업로드 포함).     </td><td>   추가 기능 제출의 장치 패밀리 타기팅 및 패키지(해당되는 경우)를 볼 수 있습니다.   </td><td>     추가 기능 제출의 장치 패밀리 타기팅을 보고 편집할 수 있습니다(해당되는 경우 패키지 업로드 포함).             </td></tr>
    <tr><td align="left">    <b>Store 목록</b>  </td><td>    제품 제출의 <a href="create-app-store-listings.md">Store 목록 페이지</a>를 볼 수 있습니다.  </td><td>    제품 제출의 <a href="create-app-store-listings.md">Store 목록 페이지</a>를 보고 편집할 수 있으며, 서로 다른 언어에 대한 새로운 Store 목록을 추가할 수 있습니다.     </td><td>    추가 기능 제출의 <a href="create-add-on-store-listings.md">Store 목록 페이지</a>를 볼 수 있습니다.            </td><td>    추가 기능 제출의 <a href="create-add-on-store-listings.md">Store 목록 페이지</a>를 보고 편집할 수 있으며, 서로 다른 언어에 대한 Store 목록을 추가할 수 있습니다.                 </td></tr>
    <tr><td align="left">    <b>Microsoft Store 제출</b>     </td><td>    이 사용 권한이 읽기 전용으로 설정된 경우 액세스 권한 없음이 허용됩니다.           </td><td>    제품을 Microsoft Store에 제출하고 인증 보고서를 볼 수 있습니다. 새 제출 및 업데이트된 제출이 포함됩니다. </td><td>이 사용 권한이 읽기 전용으로 설정된 경우 액세스 권한 없음이 허용됩니다.     </td><td>    추가 기능을 Microsoft Store에 제출하고 인증 보고서를 볼 수 있습니다. 새 제출 및 업데이트된 제출이 포함됩니다.</td></tr>
    <tr><td align="left">    <b>새 제출 만들기</b>       </td><td>    이 사용 권한이 읽기 전용으로 설정된 경우 액세스 권한 없음이 허용됩니다.        </td><td>    제품의 새 <a href="app-submissions.md">제출</a>을 만들 수 있습니다.  </td><td>    이 사용 권한이 읽기 전용으로 설정된 경우 액세스 권한 없음이 허용됩니다.   </td><td>    추가 기능의 새 <a href="add-on-submissions.md">제출</a>을 만들 수 있습니다.        </td></tr>
    <tr><td align="left">    <b>새 추가 기능</b>    </td><td>    이 사용 권한이 읽기 전용으로 설정된 경우 액세스 권한 없음이 허용됩니다. </td><td>    제품의 새 <a href="set-your-add-on-product-id.md">추가 기능</a>을 만들 수 있습니다. </td><td>    해당 없음    </td><td>    해당 없음        </td></tr>
    <tr><td align="left">    <b>이름 예약</b>   </td><td>    제품의 <a href="manage-app-names.md">앱 이름 관리</a> 페이지를 볼 수 있습니다.</td><td>    제품의 <a href="manage-app-names.md">앱 이름 관리</a> 페이지를 보고 편집할 수 있습니다(추가 이름 예약 및 예약된 이름 삭제 포함). </td><td>   추가 기능의 예약된 이름을 볼 수 있습니다.    </td><td>   추가 기능의 예약된 이름을 보고 편집할 수 있습니다.          </td></tr>
    </tbody>
    </table>

### <a name="xbox-live-"></a>Xbox Live \*

<table>
    <thead>
    <tr class="header">
    <th align="left">사용 권한&nbsp;이름</th>
    <th align="left">읽기&nbsp;전용</th>
    <th align="left">읽기/쓰기</th>
    <th align="left">읽기만(추가&nbsp;기능)&nbsp; </th>
    <th align="left">Read&#8209;write&nbsp;(Add&#8209;on)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    <b>앱 채널</b>&nbsp;*</td><td>    해당 없음  </td><td>    OneGuide를 통해 볼 수 있도록 홍보 비디오 채널을 Xbox 콘솔에 게시할 수 있습니다.  </td><td>  해당 없음 </td><td> 해당 없음 </td></tr>
    <tr><td align="left">    <b>서비스 구성</b>&nbsp;*    </td><td>    도전 과제, 멀티플레이어, 순위표 및 제품에 대한 기타 Xbox Live 구성과 관련된 설정을 볼 수 있습니다.  </td><td>    도전 과제, 멀티플레이어, 순위표 및 제품에 대한 기타 Xbox Live 구성과 관련된 설정을 보고 편집할 수 있습니다.  </td><td>    해당 없음     </td><td>    해당 없음                      </td></tr>
</tbody>
</table>

별표(*)로 표시된 \* 사용 권한은 모든 계정에서 사용할 수 없는 기능에 대한 액세스를 허용합니다. 계정이 이러한 기능을 사용하도록 설정되지 않은 경우, 해당 사용 권한을 선택해도 아무런 영향이 없습니다.  

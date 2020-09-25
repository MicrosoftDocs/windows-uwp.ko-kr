---
title: 계정 사용자에 대 한 역할 또는 사용자 지정 권한 설정
description: 파트너 센터 계정에 사용자를 추가할 때 표준 역할 또는 사용자 지정 권한을 설정 하는 방법에 대해 알아봅니다.
ms.assetid: 99f3aa18-98b4-4919-bd7b-d78356b0bf78
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, 사용자 역할, 사용자 권한, 사용자 지정 역할, 사용자 액세스, 사용자 지정 권한, 표준 역할
ms.localizationpriority: medium
ms.openlocfilehash: 6aa88bd5af2e878fa702c3faff2d2677a23f33f2
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91219756"
---
# <a name="set-roles-or-custom-permissions-for-account-users"></a>계정 사용자에 대 한 역할 또는 사용자 지정 권한 설정

[파트너 센터 계정에 사용자를 추가](add-users-groups-and-azure-ad-applications.md)하는 경우 계정 내에 있는 액세스 권한을 지정 해야 합니다. 이렇게 하려면 전체 계정에 적용 되는 [표준 역할](#roles) 을 할당 하거나 해당 [권한을 사용자 지정](#custom) 하 여 적절 한 수준의 액세스를 제공할 수 있습니다. 사용자 지정 권한 중 일부는 전체 계정에 적용 되며 일부는 하나 이상의 특정 제품으로 제한 될 수 있습니다 (원하는 경우 모든 제품에 부여 됨).

> [!NOTE] 
> 사용자, 그룹 또는 Azure AD 응용 프로그램을 추가 하는지 여부에 관계 없이 동일한 역할 및 사용 권한을 적용할 수 있습니다.

적용할 역할 또는 사용 권한을 결정할 때 다음 사항에 유의 하세요. 
-   사용자 (그룹 및 Azure AD 응용 프로그램 포함)는 [사용 권한을 사용자 지정](#custom) 하 고 [제품 수준 권한을](#product-level-permissions) 할당 하 여 특정 앱 및/또는 추가 기능만 사용할 수 있도록 하는 경우를 제외 하 고는 할당 된 역할에 연결 된 권한으로 전체 파트너 센터 계정에 액세스할 수 있습니다.
-   여러 역할을 선택 하거나 사용자 지정 권한을 사용 하 여 원하는 액세스 권한을 부여 하 여 사용자, 그룹 또는 Azure AD 응용 프로그램이 둘 이상의 역할 기능에 액세스할 수 있도록 허용할 수 있습니다.
-   특정 역할 (또는 사용자 지정 권한 집합)이 있는 사용자는 다른 역할 (또는 권한 집합)을 가진 그룹의 일부일 수도 있습니다. 이 경우 사용자는 그룹과 개별 계정 모두에 연결 된 모든 기능에 액세스할 수 있습니다.

> [!TIP]
> 이 항목은 [파트너 센터](https://partner.microsoft.com/dashboard)의 Windows 앱 개발자 프로그램에만 적용 됩니다. 하드웨어 개발자 프로그램의 사용자 역할에 대 한 정보는 [사용자 역할 관리](/windows-hardware/drivers/dashboard/managing-user-roles)를 참조 하세요. Windows 데스크톱 응용 프로그램 프로그램의 사용자 역할에 대 한 정보는 [Windows 데스크톱 응용 프로그램 프로그램](/windows/desktop/appxpkg/windows-desktop-application-program#add-and-manage-account-users)을 참조 하세요.


<span id="roles" />

## <a name="assign-roles-to-account-users"></a>계정 사용자에 게 역할 할당

기본적으로 파트너 센터 계정에 사용자, 그룹 또는 Azure AD 응용 프로그램을 추가할 때 선택할 수 있는 표준 역할 집합이 제공 됩니다. 각 역할에는 계정 내에서 특정 기능을 수행 하기 위해 특정 권한 집합이 있습니다. 

**권한 사용자**지정을 선택 하 여 [사용자 지정 권한을](#custom) 정의 하도록 선택 하지 않은 경우 계정에 추가 하는 각 사용자, 그룹 또는 Azure AD 응용 프로그램에는 다음 표준 역할 중 하나 이상이 할당 되어야 합니다. 

> [!NOTE]
> 계정의 **소유자** 는 Azure AD를 통해 추가 된 사용자가 아닌 Microsoft 계정을 사용 하 여 처음 만든 사람입니다. 이 계정 소유자는 앱을 삭제 하 고, 모든 계정 사용자를 만들고 편집 하 고, 모든 금융 및 계정 설정을 변경 하는 기능을 포함 하 여 계정에 대 한 완전 한 액세스 권한을 가진 유일한 사람입니다. 


| 역할                 | 설명              |
|----------------------|--------------------------|
| Manager              | 세금 및 지급 설정 변경을 제외 하 고 계정에 대 한 완전 한 액세스 권한이 있습니다. 파트너 센터에서 사용자를 관리 하는 작업이 포함 되지만, Azure AD 테 넌 트에서 사용자를 만들고 삭제 하는 기능은 Azure AD의 계정 권한에 따라 달라 집니다. 즉, 사용자에 게 관리자 역할이 할당 되어 있지만 조직의 Azure AD에서 전역 관리자 권한이 없는 경우, 사용자는 새 사용자를 만들거나 디렉터리에서 사용자를 삭제할 수 없습니다 (사용자의 파트너 센터 역할을 변경할 수 있지만). <p> 파트너 센터 계정이 둘 이상의 Azure AD 테 넌 트와 연결 된 경우 관리자는 해당 테 넌 트에 대 한 전역 관리자 권한이 있는 계정으로 해당 사용자와 동일한 테 넌 트에 로그인 하지 않는 한 사용자에 대 한 전체 세부 정보 (이름, 성, 암호 복구 전자 메일 및 Azure AD 전역 관리자 인지 여부)를 볼 수 없습니다. 그러나 파트너 센터 계정과 연결 된 모든 테 넌 트에서 사용자를 추가 하 고 제거할 수 있습니다. |
| 개발자            | 는 패키지를 업로드 하 고 앱과 추가 기능을 제출 하며, 원격 분석 세부 정보에 대 한 [사용 현황 보고서](usage-report.md) 를 볼 수 있습니다. [장치 간 환경](https://developer.microsoft.com/windows/project-rome) 기능에 액세스할 수 있습니다. 재무 정보 또는 계정 설정을 볼 수 없습니다.   |
| 비즈니스 참여자 | [상태](health-report.md) 및 [사용](usage-report.md) 보고서를 볼 수 있습니다. 제품을 만들거나 제출 하거나, 계정 설정을 변경 하거나, 재무 정보를 볼 수 없습니다.   |
| 재무 참여자  | [지급 보고서](payout-summary.md), 재무 정보 및 획득 보고서를 볼 수 있습니다. 앱, 추가 기능 또는 계정 설정을 변경할 수 없습니다.    |
| 마케터             | [고객 리뷰에 응답](respond-to-customer-reviews.md) 하 고 비 재무 [분석 보고서](analytics.md)를 볼 수 있습니다. 앱, 추가 기능 또는 계정 설정을 변경할 수 없습니다.      |

아래 표에는 이러한 각 역할과 계정 소유자에 게 제공 되는 특정 기능 중 일부가 나와 있습니다.

|                                                       |    계정 소유자                 |    Manager                       |    Developer                     |    비즈니스 참여자    |    재무 참여자    |    마케터                      |
|-------------------------------------------------------|----------------------------------|----------------------------------|----------------------------------|----------------------------|---------------------------|----------------------------------|
|    취득 보고서 (거의 실시간 데이터 포함) |    보기 가능                      |    보기 가능                      |    액세스 권한 없음                     |    액세스 권한 없음               |    보기 가능               |    액세스 권한 없음                     |
|    사용자 의견 보고서/응답                          |    사용자 의견을 보고 보낼 수 있음    |    사용자 의견을 보고 보낼 수 있음    |    사용자 의견을 보고 보낼 수 있음    |    액세스 권한 없음               |    액세스 권한 없음              |    사용자 의견을 보고 보낼 수 있음    |
|    상태 보고서 (거의 실시간 데이터 포함)      |    보기 가능                      |    보기 가능                      |    보기 가능                      |    보기 가능                |    액세스 권한 없음              |    액세스 권한 없음                     |
|    사용량 보고서                                       |    보기 가능                      |    보기 가능                      |    보기 가능                      |    보기 가능                |    액세스 권한 없음              |    액세스 권한 없음                     |
|    지급 계정                                     |    업데이트할 수 있음                    |    액세스 권한 없음                     |    액세스 권한 없음                     |    액세스 권한 없음               |    업데이트할 수 있음             |    액세스 권한 없음                     |
|    세금 프로필                                        |    업데이트할 수 있음                    |    액세스 권한 없음                     |    액세스 권한 없음                     |    액세스 권한 없음               |    업데이트할 수 있음             |    액세스 권한 없음                     |
|    지급 요약                                     |    보기 가능                      |    액세스 권한 없음                     |    액세스 권한 없음                     |    액세스 권한 없음               |    보기 가능               |    액세스 권한 없음                     |

표준 역할이 적절 하지 않거나 특정 앱 및/또는 추가 기능에 대 한 액세스를 제한 하려는 경우 아래에 설명 된 대로 **권한 사용자**지정을 선택 하 여 사용자에 게 사용자 지정 권한을 부여할 수 있습니다.


<span id="custom" />

## <a name="assign-custom-permissions-to-account-users"></a>계정 사용자에 게 사용자 지정 권한 할당

표준 역할이 아닌 사용자 지정 권한을 할당 하려면 사용자 계정을 추가 하거나 편집할 때 **역할** 섹션에서 **사용 권한 사용자 지정** 을 클릭 합니다. 

사용자에 대 한 사용 권한을 설정 하려면 상자를 적절 한 설정으로 전환 합니다. 

![액세스 설정에 대 한 가이드](images/permission_key.png)

- **액세스 권한 없음**: 사용자에 게 표시 된 권한이 없습니다.
- **읽기 전용**: 사용자는 표시 된 영역과 관련 된 기능을 볼 수 있지만 변경할 수는 없습니다. 
- **읽기/쓰기**: 사용자가 영역과 관련 된 변경을 수행 하 고 볼 수 있는 액세스 권한을 가집니다.
- **혼합**:이 옵션을 직접 선택할 수는 없지만 해당 사용 권한에 대 한 액세스 조합이 허용 된 경우 **혼합** 표시기가 표시 됩니다. 예를 들어 **모든 제품**에 대 한 **가격 책정 및 가용성** 에 대 한 **읽기** 전용 액세스 권한을 부여 하 고 특정 제품의 **가격 책정 및 가용성** 에 대 한 **읽기/쓰기** 액세스 권한을 부여 하는 경우 **모든 제품** 의 **가격 책정 및 가용성** 표시기는 혼합으로 표시 됩니다. 일부 제품에 사용 권한에 대 한 **액세스** 권한이 없는 경우와 **읽기/쓰기** 및/또는 **읽기 전용** 액세스 권한이 있는 경우에도 마찬가지입니다.

분석 데이터 보기와 관련 된 사용 권한의 경우 **읽기** 전용 액세스만 허용할 수 있습니다. 현재 구현에서 일부 권한은 **읽기** 전용 및 **읽기/쓰기** 액세스를 구분 하지 않습니다. 각 사용 권한에 대 한 세부 정보를 검토 하 여 **읽기 전용** 및/또는 **읽기/쓰기** 액세스로 부여 되는 특정 기능을 이해 합니다.

각 사용 권한에 대 한 구체적인 세부 정보는 아래 표에 설명 되어 있습니다.

## <a name="account-level-permissions"></a>계정 수준 권한

이 섹션의 권한은 특정 제품으로 제한 될 수 없습니다. 이러한 사용 권한 중 하나에 대 한 액세스 권한을 부여 하면 사용자가 전체 계정에 대해 해당 권한을 가질 수 있습니다.

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
<tr><td align="left">    <b>계정 설정</b>                    </td><td align="left">  <a href="/windows/uwp/publish/manage-account-settings-and-profile">연락처 정보</a>를 포함 하 여 <b>계정 설정</b> 섹션에서 모든 페이지를 볼 수 있습니다.       </td><td align="left">  <b>계정 설정</b> 섹션에서 모든 페이지를 볼 수 있습니다. <a href="/windows/uwp/publish/manage-account-settings-and-profile">연락처 정보</a> 및 다른 페이지를 변경할 수 있지만 해당 권한이 별도로 부여 되지 않는 한 지급 계정 또는 세금 프로필은 변경할 수 없습니다.            </td></tr>
<tr><td align="left">    <b>계정 사용자</b>                       </td><td align="left">  <b>사용자</b> 섹션에서 계정에 추가 된 사용자를 볼 수 있습니다.          </td><td align="left">  계정에 사용자를 추가 하 고 <b>사용자</b> 섹션에서 기존 사용자를 변경할 수 있습니다.             </td></tr>
<tr><td align="left">    <b>계정 수준 ad 성능 보고서</b> </td><td align="left">  계정 수준 <a href="advertising-performance-report.md">광고 성능 보고서</a>를 볼 수 있습니다.      </td><td align="left">  N/A   </td></tr>
<tr><td align="left">    <b>광고 캠페인</b>                        </td><td align="left">  계정에서 만든 <a href="create-an-ad-campaign-for-your-app.md">ad 캠페인</a> 을 볼 수 있습니다.      </td><td align="left">  계정에 생성 된 <a href="create-an-ad-campaign-for-your-app.md">광고 캠페인</a> 을 만들고 관리 하 고 볼 수 있습니다.          </td></tr>
<tr><td align="left">    <b>Ad 중재</b>                        </td><td align="left">  계정의 모든 제품에 대 한 ad 중재 구성을 볼 수 있습니다.    </td><td align="left">  계정의 모든 제품에 대 한 ad 중재 구성을 확인 하 고 변경할 수 있습니다.        </td></tr>
<tr><td align="left">    <b>광고 중재 보고서</b>                </td><td align="left">  계정에 있는 모든 제품에 대 한 <a href="/windows/uwp/publish/advertising-performance-report">광고 중재 보고서</a> 를 볼 수 있습니다.    </td><td align="left">  N/A    </td></tr>
<tr><td align="left">    <b>Ad 성능 보고서</b>              </td><td align="left">  계정에 있는 모든 제품에 대 한 <a href="advertising-performance-report.md">광고 성능 보고서</a> 를 볼 수 있습니다.       </td><td align="left">  N/A         </td></tr>
<tr><td align="left">    <b>Ad 단위</b>                            </td><td align="left">  계정에 대해 만들어진 <a href="in-app-ads.md">ad 단위</a> 를 볼 수 있습니다.    </td><td align="left">  계정에 대 한 <a href="in-app-ads.md">ad 단위</a> 를 만들고 관리 하 고 볼 수 있습니다.             </td></tr>
<tr><td align="left">    <b>관련 광고</b>                       </td><td align="left">  계정의 모든 제품에서 관련 <a href="/windows/uwp/publish/in-app-ads">ad</a> 사용량을 볼 수 있습니다.    </td><td align="left">  계정의 모든 제품에 대 한 관련 <a href="/windows/uwp/publish/in-app-ads">광고</a> 사용을 관리 하 고 볼 수 있습니다.                </td></tr>
<tr><td align="left">    <b>계열사 성능 보고서</b>      </td><td align="left">  계정의 모든 제품에 대 한 <a href="/windows/uwp/publish/advertising-performance-report">계열사 성능 보고서</a> 를 볼 수 있습니다.   </td><td align="left">  N/A   </td></tr>
<tr><td align="left">    <b>앱 설치 광고 보고서</b>             </td><td align="left">  <a href="/windows/uwp/publish/ad-campaign-report">광고 캠페인 보고서</a>를 볼 수 있습니다.           </td><td align="left">  N/A   </td></tr>
<tr><td align="left">    <b>커뮤니티 광고</b>                       </td><td align="left">  계정의 모든 제품에 대 한 무료 <a href="about-community-ads.md">커뮤니티 ad</a> 사용량을 볼 수 있습니다.          </td><td align="left">  계정의 모든 제품에 대 한 무료 <a href="about-community-ads.md">커뮤니티 ad</a> 사용량을 만들고, 관리 하 고, 볼 수 있습니다.               </td></tr>
<tr><td align="left">    <b>연락처 정보</b>                        </td><td align="left">  계정 설정 섹션에서 <a href="/windows/uwp/publish/manage-account-settings-and-profile">연락처 정보</a> 를 볼 수 있습니다.        </td><td align="left">  계정 설정 섹션에서 <a href="/windows/uwp/publish/manage-account-settings-and-profile">연락처 정보</a> 를 편집 하 고 볼 수 있습니다.            </td></tr>
<tr><td align="left">    <b>COPPA 규정 준수</b>                    </td><td align="left">  계정에 있는 모든 제품에 대해 제품이 13 세 미만의 어린이를 대상으로 하는지 여부를 나타내는 <a href="in-app-ads.md#coppa-compliance">COPPA 준수</a> 선택 항목을 볼 수 있습니다.                                            </td><td align="left">  계정에 있는 모든 제품에 대 한 <a href="in-app-ads.md#coppa-compliance">COPPA 준수</a>  선택 항목을 편집 하 고 볼 수 있습니다 (제품이 13 세 미만의 어린이를 대상으로 하는지 여부를 나타냄).         </td></tr>
<tr><td align="left">    <b>고객 그룹</b>                     </td><td align="left">  <a href="create-customer-groups.md">고객 그룹</a> (세그먼트 및 알려진 사용자 그룹)을 볼 수 있습니다.      </td><td align="left">  <a href="create-customer-groups.md">고객 그룹</a> (세그먼트 및 알려진 사용자 그룹)을 만들고, 편집 하 고, 볼 수 있습니다.       </td></tr>
<tr><td align="left">    <b>제품 그룹 관리</b>&nbsp;*                            </td><td align="left">  새 제품 그룹 만들기 페이지를 볼 수 있지만 실제로 새 제품 그룹을 만들 수는 없습니다.    </td><td align="left">  제품 그룹을 만들고 편집할 수 있습니다.     </td></tr>
<tr><td align="left">    <b>새 앱</b>                            </td><td align="left">  새 앱 만들기 페이지를 볼 수 있지만 실제로 계정에 새 앱을 만들 수는 없습니다.    </td><td align="left">  새 앱 이름을 예약 하 여 계정에 <a href="create-your-app-by-reserving-a-name.md">새 앱을 만들</a> 수 있으며, 제출을 만들고 스토어에 앱을 제출할 수 있습니다.     </td></tr>
<tr><td align="left">    <b>새 번들</b>&nbsp;*                       </td><td align="left">  새 번들 만들기 페이지를 볼 수 있지만 실제로 계정에 새 번들을 만들 수는 없습니다.     </td><td align="left">  제품의 새 번들을 만들 수 있습니다.          </td></tr>
<tr><td align="left">    <b>파트너 서비스</b>&nbsp;*                  </td><td align="left">  XToken을 검색하는 서비스를 설치하기 위한 인증서를 볼 수 있습니다.     </td><td align="left">  는 서비스에 설치 하 여 XTokens을 검색 하는 인증서를 관리 하 고 볼 수 있습니다.       </td></tr>
<tr><td align="left">    <b>지급 계정</b>                      </td><td align="left">  <b>계정 설정</b>에서 <a href="setting-up-your-payout-account-and-tax-forms.md#payout-account">지급 계정 정보</a> 를 볼 수 있습니다.     </td><td align="left">  <b>계정 설정</b>에서 <a href="setting-up-your-payout-account-and-tax-forms.md#payout-account">지급 계정 정보</a> 를 편집 하 고 볼 수 있습니다.       </td></tr>
<tr><td align="left">    <b>지급 요약</b>                      </td><td align="left">  <a href="payout-summary.md">지급 요약</a> 을 확인 하 여 지급 보고 정보에 액세스 하 고 다운로드할 수 있습니다.       </td><td align="left">  <a href="payout-summary.md">지급 요약</a> 을 확인 하 여 지급 보고 정보에 액세스 하 고 다운로드할 수 있습니다.   </td></tr>
<tr><td align="left">    <b>신뢰 당사자</b>&nbsp;*                   </td><td align="left">  신뢰 당사자가 XTokens를 검색할 수 있습니다.    </td><td align="left">  는 신뢰 당사자를 관리 하 고 확인 하 여 XTokens를 검색할 수 있습니다.     </td></tr>
<tr><td align="left">    <b>샌드박스</b>&nbsp;*                         </td><td align="left">  는 <b>샌드박스</b> 페이지에 액세스 하 고 계정 및 해당 샌드박스에 대해 적용 가능한 모든 구성에서 샌드박스를 볼 수 있습니다. 적절 한 제품 수준 사용 권한이 부여 되지 않은 경우 각 샌드박스에 대 한 제품 및 제출을 볼 수 없습니다. </td><td align="left">  <b>샌드박스 페이지에</b> 액세스 하 고, 샌드박스를 만들고 삭제 하 고 구성을 관리 하는 등 계정에서 샌드박스를 보고 관리할 수 있습니다. 적절 한 제품 수준 사용 권한이 부여 되지 않은 경우 각 샌드박스에 대 한 제품 및 제출을 볼 수 없습니다.    </td></tr>
<tr><td align="left">    <b>매장 판매 이벤트</b>&nbsp;*                            </td><td align="left">  N/A    </td><td align="left">  매장 판매 이벤트에 제품을 자동으로 포함 하는 옵션을 구성할 수 있습니다.     </td></tr>
<tr><td align="left">    <b>세금 프로필</b>                         </td><td align="left">  <b>계정 설정</b>에서 <a href="setting-up-your-payout-account-and-tax-forms.md#tax-forms">세금 프로필 정보 및 양식을</a> 볼 수 있습니다.     </td><td align="left">  <b>계정 설정</b>에서 세금 양식을 작성 하 고 <a href="setting-up-your-payout-account-and-tax-forms.md#tax-forms">세금 프로필 정보</a> 를 업데이트할 수 있습니다.     </td></tr>
<tr><td align="left">    <b>테스트 계정</b>&nbsp;*                     </td><td align="left">  Xbox Live 구성을 테스트 하기 위한 계정을 볼 수 있습니다.      </td><td align="left">  Xbox Live 구성을 테스트 하기 위한 계정을 만들고, 관리 하 고, 볼 수 있습니다.      </td></tr>
<tr><td align="left">    <b>Xbox 장치</b>                        </td><td align="left">  계정 <b>설정</b> 섹션에서 계정에 대해 사용 하도록 설정 된 Xbox development 콘솔을 볼 수 있습니다.       </td><td align="left">  <b>계정 설정</b> 섹션에서 계정에 대해 사용 하도록 설정 된 Xbox development 콘솔을 추가, 제거 및 볼 수 있습니다.     </td></tr>
    </tbody>
    </table>

\* 별표 (*)로 표시 된 사용 권한은 모든 계정에서 사용할 수 없는 기능에 대 한 액세스 권한을 부여 합니다. 이러한 기능에 대해 계정을 사용 하도록 설정 하지 않은 경우에는 이러한 사용 권한에 대 한 선택 항목이 적용 되지 않습니다.   


## <a name="product-level-permissions"></a>제품 수준 권한

이 섹션의 권한은 계정의 모든 제품에 부여 하거나, 하나 이상의 특정 제품에 대 한 사용 권한만 허용 하도록 사용자 지정할 수 있습니다. 

제품 수준 권한은 **분석**, **수익 화**, **게시**및 **Xbox Live**의 네 가지 범주로 그룹화 됩니다. 이러한 각 범주를 확장 하 여 각 범주의 개별 사용 권한을 볼 수 있습니다. 또한 하나 이상의 특정 제품에 대 한 **모든 사용 권한을** 설정 하는 옵션도 있습니다.

계정에 있는 모든 제품에 대 한 사용 권한을 부여 하려면 **모든 제품**으로 표시 된 행에 **읽기** 전용 또는 **읽기/쓰기**를 표시 하도록 상자를 설정/해제 하 여 해당 사용 권한을 선택 합니다. 
 
> [!TIP]
> **모든 제품** 에 대 한 선택 항목은 현재 계정에 있는 모든 제품 및 해당 계정에 생성 된 이후 제품에 적용 됩니다. 사용 권한이 향후 제품에 적용 되지 않도록 하려면 **모든**제품을 선택 하는 대신 개별적으로 모든 제품을 선택 합니다.

**모든 제품** 행 아래에 계정에 있는 각 제품이 별도의 행에 표시 됩니다. 특정 제품에 대 한 사용 권한만 부여 하려면 해당 제품에 대 한 행에서 해당 권한을 선택 합니다.

각 추가 기능은 부모 제품 아래에 있는 별도의 행과 함께 **모든 추가 기능** 행에 나열 됩니다. **모든 추가 기능** 에 대해 선택 된 항목은 해당 제품의 모든 현재 추가 기능과 해당 제품에 대해 만들어진 이후의 추가 기능에 모두 적용 됩니다.

일부 권한은 추가 기능에 대해 설정할 수 없습니다. 이는 추가 기능 (예: **고객 피드백** 권한)에 적용 되지 않거나, 부모 제품 수준에서 부여 된 권한이 해당 제품의 모든 추가 기능 (예: **판촉 코드**)에 적용 되기 때문입니다. 그러나 추가 기능에 사용할 수 있는 모든 사용 권한은 별도로 설정 해야 합니다. 추가 기능은 부모 제품에 대해 선택한 선택 항목을 상속 하지 않습니다. 예를 들어 사용자가 추가 기능에 대 한 가격 책정 및 가용성을 선택할 수 있게 하려는 경우 부모 제품에 대 한 **가격 책정 및 가용성** 권한을 부여 했는지 여부에 관계 없이 추가 기능에 대 한 **가격 책정** 및 가용성 권한 (또는 **모든 추가 기능**)을 사용 하도록 설정 해야 합니다. 


### <a name="analytics"></a>분석

<table>
    <thead>
    <tr class="header">
    <th align="left">권한 &nbsp; 이름</th>
    <th align="left">읽기 &nbsp; 전용</th>
    <th align="left">읽기/쓰기</th>
    <th align="left">읽기 &nbsp; 전용 &nbsp; (추가&#8209;) </th>
    <th align="left">읽기&#8209;쓰기 &nbsp; (추가&#8209;)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    <b>인수</b> (거의 실시간 데이터 포함) </td><td>    제품에 대 한 획득 및 <a href="add-on-acquisitions-report.md">추가 기능 구매</a> <a href="acquisitions-report.md">보고서를 볼</a> 수 있습니다.        </td><td>    N/A    </td><td>    해당 없음 (부모 제품에 대 한 설정에 **추가 기능 추가** 보고서 포함)        </td><td>    N/A                         </td></tr>
    <tr><td align="left">    <b>보려면</b> </td><td>    제품에 대 한 <a href="usage-report.md">사용 현황 보고서</a> 를 볼 수 있습니다.     </td><td>    N/A       </td><td>    N/A     </td><td>    N/A         </td></tr>
    <tr><td align="left">    <b>상태</b> (거의 실시간 데이터 포함) </td><td>    제품에 대 한 <a href="health-report.md">상태 보고서</a> 를 볼 수 있습니다.    </td><td>    N/A     </td><td>    N/A     </td><td>    N/A         </td></tr>
    <tr><td align="left">    <b>고객 의견</b>    </td><td>    제품에 대 한 <a href="reviews-report.md">검토</a> 및 <a href="feedback-report.md">피드백</a> 보고서를 볼 수 있습니다.       </td><td>    해당 없음 (사용자 의견 또는 리뷰에 응답 하려면 <b>고객 지원</b> 권한이 부여 되어야 함)   </td><td>    N/A     </td><td>    N/A         </td></tr>
    <tr><td align="left">    <b>Xbox 분석</b> </td><td>    제품에 대 한 <a href="xbox-analytics-report.md">Xbox 분석 보고서</a> 를 볼 수 있습니다.    </td><td>    N/A   </td><td>    N/A       </td><td>    N/A          </td></tr>
    </tbody>
    </table>

### <a name="monetization"></a>수익 창출

<table>
    <thead>
    <tr class="header">
    <th align="left">권한 &nbsp; 이름</th>
    <th align="left">읽기 &nbsp; 전용</th>
    <th align="left">읽기/쓰기</th>
    <th align="left">읽기 &nbsp; 전용 &nbsp; (추가&#8209;) </th>
    <th align="left">읽기&#8209;쓰기 &nbsp; (추가&#8209;)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    <b>판촉 코드</b>     </td><td>    제품 및 해당 추가 기능에 대 한 <a href="generate-promotional-codes.md">판촉 코드</a> 주문 및 사용 정보를 보고 사용 정보를 볼 수 있습니다.         </td><td>    제품 및 해당 추가 기능에 대 한 <a href="generate-promotional-codes.md">판촉 코드</a> 주문을 보고 관리 하 고 만들 수 있으며 사용 정보를 볼 수 있습니다.          </td><td>    해당 없음 (부모 제품의 설정이 모든 추가 기능에 적용 됩니다.)     </td><td>    해당 없음 (부모 제품의 설정이 모든 추가 기능에 적용 됩니다.)     </td></tr>
    <tr><td align="left">    <b>대상 제안</b>     </td><td>    제품에 대 한 <a href="use-targeted-offers-to-maximize-engagement-and-conversions.md">대상</a> 제품을 볼 수 있습니다.         </td><td>    제품에 대 한 <a href="use-targeted-offers-to-maximize-engagement-and-conversions.md">대상</a> 제품을 보고 관리 하 고 만들 수 있습니다.          </td><td>    N/A     </td><td>    N/A      </td></tr>
    <tr><td align="left">    <b>고객 지원</b>  </td><td>    고객 <b>피드백</b> 권한이 부여 되는 한 고객 <a href="respond-to-customer-feedback.md">의견</a> 및 고객 <a href="respond-to-customer-reviews.md">리뷰에</a>대 한 응답을 볼 수 있습니다. 제품에 대해 만들어진 <a href="send-push-notifications-to-your-apps-customers.md">대상 알림도</a> 볼 수도 있습니다.    </td><td>    <b>고객 피드백 권한이 부여</b> 된 경우에는 고객 <a href="respond-to-customer-feedback.md">피드백에 응답</a> 하 고 <a href="respond-to-customer-reviews.md">고객 리뷰에 응답할</a>수 있습니다. 또한 제품에 대 한 <a href="send-push-notifications-to-your-apps-customers.md">대상 알림을 만들고 보낼</a> 수 있습니다.                   </td><td>    N/A         </td><td>    N/A                          </td></tr>
    <tr><td align="left">    <b>실험</b></td><td>    <a href="../monetize/run-app-experiments-with-a-b-testing.md">실험 (A/B 테스트)</a> 을 보고 제품의 실험 데이터를 볼 수 있습니다.   </td><td>    제품에 대 한 <a href="../monetize/run-app-experiments-with-a-b-testing.md">실험 (A/B 테스트)</a> 을 생성, 관리 및 확인 하 고 실험 데이터를 볼 수 있습니다.     </td><td>    N/A  </td><td>    N/A                 </td></tr>
    <tr><td align="left">    <b>매장 판매 이벤트</b>&nbsp;*</td><td>    제품에 대 한 판매 이벤트 상태를 볼 수 있습니다.   </td><td>    판매 이벤트에 제품을 추가 하 고 할인을 구성할 수 있습니다.      </td><td>    제품에 대 한 판매 이벤트 상태를 볼 수 있습니다.   </td><td>    판매 이벤트에 제품을 추가 하 고 할인을 구성할 수 있습니다.      </td></tr>
    </tbody>
    </table>

### <a name="publishing"></a>게시 

<table>
    <thead>
    <tr class="header">
    <th align="left">권한 &nbsp; 이름</th>
    <th align="left">읽기 &nbsp; 전용</th>
    <th align="left">읽기/쓰기</th>
    <th align="left">읽기 &nbsp; 전용 &nbsp; (추가&#8209;) </th>
    <th align="left">읽기&#8209;쓰기 &nbsp; (추가&#8209;)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    <b>제품 설정</b>  </td><td>    제품의 제품 설정 페이지를 볼 수 있습니다.     </td><td>    제품의 제품 설치 페이지를 보고 편집할 수 있습니다. </td><td>    추가 기능의 제품 설정 페이지를 볼 수 있습니다.   </td><td>    제품 설정 페이지 추가 기능을 보고 편집할 수 있습니다.          </td></tr>
    <tr><td align="left">    <b>가격 책정 및 가용성</b>  </td><td>    제품의 <a href="set-app-pricing-and-availability.md">가격 책정 및 가용성</a> 페이지를 볼 수 있습니다.     </td><td>    제품의 <a href="set-app-pricing-and-availability.md">가격 책정 및 가용성</a> 페이지를 보고 편집할 수 있습니다. </td><td>    추가 기능에 대 한 <a href="set-add-on-pricing-and-availability.md">가격 책정 및 가용성</a> 페이지를 볼 수 있습니다.   </td><td>    추가 기능에 대 한 <a href="set-add-on-pricing-and-availability.md">가격 책정 및 가용성</a> 페이지를 보고 편집할 수 있습니다.          </td></tr>
    <tr><td align="left">    <b>정보의</b>   </td><td>    제품의 <a href="enter-app-properties.md">속성</a> 페이지를 볼 수 있습니다.      </td><td>    제품의 <a href="enter-app-properties.md">속성</a> 페이지를 보고 편집할 수 있습니다.       </td><td>    추가 기능의 <a href="enter-add-on-properties.md">속성</a> 페이지를 볼 수 있습니다.     </td><td>    추가 기능의 <a href="enter-add-on-properties.md">속성</a> 페이지를 보고 편집할 수 있습니다.               </td></tr>
    <tr><td align="left">    <b>연령별 등급</b>    </td><td>    제품의 <a href="age-ratings.md">연령 등급</a> 페이지를 볼 수 있습니다.       </td><td>    제품의 <a href="age-ratings.md">연령 등급</a> 페이지를 보고 편집할 수 있습니다.    </td><td>    추가 기능의 수명 등급 페이지를 볼 수 있습니다.          </td><td>     추가 기능의 수명 등급 페이지를 보고 편집할 수 있습니다.       </td></tr>
    <tr><td align="left">    <b>검색할</b>        </td><td>    제품의 <a href="upload-app-packages.md">패키지</a> 페이지를 볼 수 있습니다.  </td><td>    패키지 업로드를 포함 하 여 제품의 <a href="upload-app-packages.md">패키지</a> 페이지를 보고 편집할 수 있습니다.     </td><td>   Addons의 <a href="upload-app-packages.md">패키지</a> 페이지를 볼 수 있습니다 (해당 하는 경우).   </td><td>     Addons의 <a href="upload-app-packages.md">패키지</a> 페이지를 보고 편집할 수 있습니다 (해당 하는 경우).             </td></tr>
    <tr><td align="left">    <b>저장소 목록</b>  </td><td>    제품의 <a href="create-app-store-listings.md">스토어 목록 페이지</a> 를 볼 수 있습니다.  </td><td>    제품의 <a href="create-app-store-listings.md">스토어 목록 페이지</a> 를 보고 편집할 수 있으며, 다른 언어에 대 한 새 스토어 목록을 추가할 수 있습니다.     </td><td>    추가 기능의 <a href="create-add-on-store-listings.md">스토어 목록 페이지</a> 를 볼 수 있습니다.            </td><td>    추가 기능의 <a href="create-add-on-store-listings.md">스토어 목록 페이지</a> 를 보고 편집할 수 있으며 다양 한 언어에 대 한 매장 목록을 추가할 수 있습니다.                 </td></tr>
    <tr><td align="left">    <b>스토어 제출</b>     </td><td>    이 권한이 읽기 전용으로 설정 된 경우에는 액세스 권한이 부여 되지 않습니다.           </td><td>    제품을 스토어에 제출 하 고 인증 보고서를 볼 수 있습니다. 에는 새로운 전송과 업데이트 된 제출이 모두 포함 됩니다. </td><td>이 권한이 읽기 전용으로 설정 된 경우에는 액세스 권한이 부여 되지 않습니다.     </td><td>    저장소에 추가 기능을 제출 하 고 인증 보고서를 볼 수 있습니다. 에는 새로운 전송과 업데이트 된 제출이 모두 포함 됩니다.</td></tr>
    <tr><td align="left">    <b>새 제출 만들기</b>       </td><td>    이 권한이 읽기 전용으로 설정 된 경우에는 액세스 권한이 부여 되지 않습니다.        </td><td>    제품에 대 한 새 <a href="app-submissions.md">제출을</a> 만들 수 있습니다.  </td><td>    이 권한이 읽기 전용으로 설정 된 경우에는 액세스 권한이 부여 되지 않습니다.   </td><td>    추가 기능에 대 한 새 <a href="add-on-submissions.md">제출을</a> 만들 수 있습니다.        </td></tr>
    <tr><td align="left">    <b>새 추가 기능</b>    </td><td>    이 권한이 읽기 전용으로 설정 된 경우에는 액세스 권한이 부여 되지 않습니다. </td><td>    제품에 대 한 <a href="set-your-add-on-product-id.md">새 추가 기능을 만들</a> 수 있습니다. </td><td>    N/A    </td><td>    N/A        </td></tr>
    <tr><td align="left">    <b>이름 예약</b>   </td><td>    제품에 대 한 <a href="manage-app-names.md">앱 이름 관리</a> 페이지를 볼 수 있습니다.</td><td>    추가 이름을 예약 하 고 예약 된 이름을 삭제 하는 등 제품에 대 한 <a href="manage-app-names.md">앱 이름 관리</a> 페이지를 보고 편집할 수 있습니다. </td><td>   추가 기능에 대 한 예약 된 이름을 볼 수 있습니다.    </td><td>   추가 기능에 대 한 예약 된 이름을 보고 편집할 수 있습니다.          </td></tr>
    <tr><td align="left">    <b>디스크 요청</b>   </td><td>    요청 페이지에서 디스크를 볼 수 있습니다. </td><td>    디스크 요청을 만들 수 있습니다. </td><td>   N/A    </td><td>   N/A          </td></tr>
    <tr><td align="left">    <b>디스크 로열티 </b>   </td><td>    로열티 페이지에서 디스크를 볼 수 있습니다.</td><td>    로열티 디스크를 만들 수 있습니다. </td><td>   N/A    </td><td>   N/A          </td></tr>
    </tbody>
    </table>

### <a name="xbox-live-"></a>Xbox Live \*

<table>
    <thead>
    <tr class="header">
    <th align="left">권한 &nbsp; 이름</th>
    <th align="left">읽기 &nbsp; 전용</th>
    <th align="left">읽기/쓰기</th>
    <th align="left">읽기 &nbsp; 전용 &nbsp; (추가&#8209;) </th>
    <th align="left">읽기&#8209;쓰기 &nbsp; (추가&#8209;)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    <b>신뢰 당사자</b>&nbsp;*</td><td>    계정의 신뢰 당사자 페이지를 볼 수 있습니다.   </td><td>    계정의 신뢰 당사자 페이지를 보고 편집할 수 있습니다.    </td><td>    N/A    </td><td>    N/A                      </td></tr>
    <tr><td align="left">    <b>파트너 서비스</b>&nbsp;*</td><td>    계정의 웹 서비스 페이지를 볼 수 있습니다.  </td><td>    계정의 웹 서비스 페이지를 보고 편집할 수 있습니다.      </td><td>    N/A    </td><td>    N/A                      </td></tr>
    <tr><td align="left">    <b>Xbox 테스트 계정</b>&nbsp;*</td><td>    계정의 Xbox 테스트 계정 페이지를 볼 수 있습니다.  </td><td>    계정의 Xbox 테스트 계정 페이지를 보고 편집할 수 있습니다.    </td><td>    N/A    </td><td>    N/A                      </td></tr>
    <tr><td align="left">    <b>샌드박스 당 Xbox 테스트 계정</b>&nbsp;*</td><td>    Xbox 테스트 계정 페이지에서 계정의 지정 된 샌드박스에 대해서만 볼 수 있습니다.  </td><td>    Xbox 테스트를 보고 편집할 수 있습니다.   <tr><td align="left">    <b>계정의 지정 된 샌드박스에 대 한 계정 페이지    </td><td>    N/A    </td><td>    N/A                      </td></tr>
    <tr><td align="left">    <b>Xbox 장치</b>&nbsp;*</td><td>    계정의 Xbox one development 콘솔 페이지를 볼 수 있습니다.  </td><td>    계정의 Xbox one development 콘솔 페이지를 보고 편집할 수 있습니다.    </td><td>    N/A    </td><td>    N/A                      </td></tr>
    <tr><td align="left">    <b>샌드박스 별 Xbox 장치</b>&nbsp;*</td><td>    Xbox one development 콘솔 페이지에서 지정 된 계정 샌드박스에 대해 볼 수 있습니다.  </td><td>    Xbox one development 콘솔 페이지를 보고 편집할 수 있습니다.    </td><td>    N/A    </td><td>    N/A                      </td></tr>
    <tr><td align="left">    <b>앱 채널</b>&nbsp;*</td><td>    N/A  </td><td>    는 OneGuide를 통해 볼 수 있도록 비디오 채널을 Xbox 콘솔에 게시할 수 있습니다.    </td><td>    N/A    </td><td>    N/A                      </td></tr>
    <tr><td align="left">    <b>서비스 구성</b>&nbsp;*</td><td>    제품의 Xbox Live 서비스 구성 페이지를 볼 수 있습니다.  </td><td>    제품의 Xbox Live 서비스 구성 페이지를 보고 편집할 수 있습니다.    </td><td>    N/A    </td><td>    N/A                      </td></tr>
    <tr><td align="left">    <b>도구 액세스</b>&nbsp;*</td><td>    제품에서 Xbox Live tools를 실행 하 여 데이터만 볼 수 있습니다.  </td><td>    제품에서 Xbox Live tools를 실행 하 여 데이터를 보고 편집할 수 있습니다.    </td><td>    N/A    </td><td>    N/A                      </td></tr>
</tbody>
</table>

\* 별표 (*)로 표시 된 사용 권한은 모든 계정에서 사용할 수 없는 기능에 대 한 액세스 권한을 부여 합니다. 이러한 기능에 대해 계정을 사용 하도록 설정 하지 않은 경우에는 이러한 사용 권한에 대 한 선택 항목이 적용 되지 않습니다.
---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 05/04/2020
ms.author: glenga
ms.openlocfilehash: 37a61e270f95ffdc420536046e0ce1eda1e489b4
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85391491"
---
| 리소스 |[사용 계획](../articles/azure-functions/functions-scale.md#consumption-plan)|[프리미엄 계획](../articles/azure-functions/functions-scale.md#premium-plan)|[전용 계획](../articles/azure-functions/functions-scale.md#app-service-plan)|[ASE](../articles/app-service/environment/intro.md)| [Kubernetes](../articles/aks/quotas-skus-regions.md) |
| --- | --- | --- | --- | --- | --- |
|기본 [제한 시간](../articles/azure-functions/functions-scale.md#timeout) (분) |5 | 30 |30<sup>1</sup> | 30 | 30 |
|최대 [제한](../articles/azure-functions/functions-scale.md#timeout) 시간 (분) |10 | 무제한<sup>7</sup> | 제한 없음<sup>2</sup> | unbounded | unbounded |
| 최대 아웃 바운드 연결 (인스턴스당) | 600 활성 (총 1200) | unbounded | unbounded | unbounded | unbounded |
| 최대 요청 크기 (MB)<sup>3</sup> | 100 | 100 | 100 | 100 | 클러스터에 종속 |
| 최대 쿼리 문자열 길이<sup>3</sup> | 4096 | 4096 | 4096 | 4096 | 클러스터에 종속 |
| 최대 요청 URL 길이<sup>3</sup> | 8192 | 8192 | 8192 | 8192 | 클러스터에 종속 |
|인스턴스당 [Acu](../articles/virtual-machines/windows/acu.md) | 100 | 210-840 | 100-840 | 210-250<sup>8</sup> | [AKS 가격 책정](https://azure.microsoft.com/pricing/details/container-service/) |
| 최대 메모리 (인스턴스당 GB) | 1.5 | 3.5-14 | 1.75 g-14 | 3.5-14 | 모든 노드가 지원 됩니다. |
| 계획 당 함수 앱 |100 |100 |무제한<sup>4</sup> | unbounded | unbounded |
| [App Service 계획](../articles/app-service/overview-hosting-plans.md) | 100/ [지역](https://azure.microsoft.com/global-infrastructure/regions/) |리소스 그룹당 100 |리소스 그룹당 100 | - | - |
| 스토리지<sup>5</sup> |5TB |250GB |50-1000 GB | 1TB | 해당 없음 |
| 앱당 사용자 지정 도메인</a> |500<sup>6</sup> |500 |500 | 500 | 해당 없음 |
| 사용자 지정 도메인 [SSL 지원](../articles/app-service/configure-ssl-bindings.md) |제한 없는 SNI SSL 연결 포함 | 제한 없는 SNI SSL 및 1 개의 IP SSL 연결 포함 |제한 없는 SNI SSL 및 1 개의 IP SSL 연결 포함 | 제한 없는 SNI SSL 및 1 개의 IP SSL 연결 포함 | 해당 없음 |

<sup>1</sup> 기본적으로 App Service 계획의 함수 1.x 런타임에 대 한 제한 시간은 제한 되지 않습니다.  
<sup>2</sup> App Service 계획을 [Always On](../articles/azure-functions/functions-scale.md#always-on)으로 설정 해야 합니다. 표준 [요금](https://azure.microsoft.com/pricing/details/app-service/)으로 지불 합니다.  
<sup>3</sup> 이러한 제한은 [호스트에 설정](https://github.com/Azure/azure-functions-host/blob/dev/src/WebJobs.Script.WebHost/web.config)됩니다.  
<sup>4</sup> 사용할 수 있는 실제 함수 앱 수는 앱의 작업, 컴퓨터 인스턴스의 크기 및 해당 리소스 사용률에 따라 달라 집니다.  
<sup>5</sup> 저장소 제한은 동일한 App Service 계획의 모든 앱에서 임시 저장소의 전체 콘텐츠 크기입니다. 소비 계획은 임시 저장소에 대 한 Azure Files를 사용 합니다.  
<sup>6</sup> 함수 앱이 [소비 계획](../articles/azure-functions/functions-scale.md#consumption-plan)에서 호스트 되는 경우에는 CNAME 옵션만 지원 됩니다. [프리미엄 계획](../articles/azure-functions/functions-scale.md#premium-plan) 또는 [App Service 계획](../articles/azure-functions/functions-scale.md#app-service-plan)의 함수 앱의 경우 CNAME 또는 a 레코드를 사용 하 여 사용자 지정 도메인을 매핑할 수 있습니다.  
<sup>7</sup> 최대 60 분까지 보장 됩니다.  
<sup>8</sup> 작업자는 고객 앱을 호스트 하는 역할입니다. 작업자는 3 가지 고정 크기 (vCPU/3.5 g b RAM)로 사용할 수 있습니다. VCPU/7GB RAM 2 개 VCPU/14gb RAM 4 개

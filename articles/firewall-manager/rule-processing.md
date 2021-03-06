---
title: Azure 방화벽 관리자 규칙 처리 논리
description: Azure Firewall 규칙 처리 논리 알아보기
services: firewall-manager
author: vhorne
ms.service: firewall-manager
ms.topic: conceptual
ms.date: 06/30/2020
ms.author: victorh
ms.openlocfilehash: 9184bf7baa85420e067edb4c0aafccb7e6711225
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86512183"
---
# <a name="azure-firewall-rule-processing-logic"></a>Azure Firewall 규칙 처리 논리

Azure Firewall에는 NAT 규칙, 네트워크 규칙 및 애플리케이션 규칙이 있습니다. 규칙은 규칙 유형에 따라 처리됩니다.

## <a name="network-rules-and-applications-rules"></a>네트워크 규칙 및 애플리케이션 규칙

네트워크 규칙이 가장 먼저 적용되고, 다음으로 애플리케이션 규칙이 적용됩니다. 규칙은 종료됩니다. 따라서 네트워크 규칙에 일치 하는 항목이 있으면 응용 프로그램 규칙이 처리 되지 않습니다.  네트워크 규칙이 일치 하지 않고 패킷 프로토콜이 HTTP/HTTPS 인 경우 해당 패킷은 응용 프로그램 규칙에 의해 평가 됩니다. 여전히 일치하는 항목이 없으면 인프라 규칙 컬렉션과 비교하여 패킷이 평가됩니다. 여전히 일치 하는 항목이 없는 경우에는 기본적으로 패킷이 거부 됩니다.

## <a name="nat-rules"></a>NAT 규칙

[자습서: Azure Portal을 사용하여 Azure Firewall DNAT를 통해 인바운드 트래픽 필터링](../firewall/tutorial-firewall-dnat.md)에 설명된 대로 DNAT(Destination Network Address Translation)를 구성하여 인바운드 연결을 사용할 수 있습니다. DNAT 규칙이 가장 먼저 적용됩니다. 일치 항목이 발견되면 변환된 트래픽을 허용하도록 암시적인 해당 네트워크 규칙이 추가됩니다. 변환된 트래픽을 일치시키는 거부 규칙을 사용하여 네트워크 규칙 컬렉션을 명시적으로 추가함으로써 이 동작을 재정의할 수 있습니다. 이러한 연결에는 애플리케이션 규칙이 적용되지 않습니다.

## <a name="inherited-rules"></a>상속 된 규칙

부모 정책에서 상속 된 네트워크 규칙 컬렉션은 항상 새 정책의 일부로 정의 된 네트워크 규칙 컬렉션 보다 우선 순위가 지정 됩니다. 애플리케이션 규칙 컬렉션에도 동일한 논리가 적용됩니다. 그러나 네트워크 규칙 컬렉션은 상속과 관계없이 항상 애플리케이션 규칙 컬렉션보다 먼저 처리됩니다.

기본적으로 정책은 부모 정책 위협 인텔리전스 모드를 상속 합니다. 정책 설정 페이지에서 위협 인텔리전스 모드를 다른 값으로 설정 하 여이를 재정의할 수 있습니다. 더 엄격한 값으로만 재정의할 수 있습니다. 예를 들어 부모 정책이 *경고만*으로 설정 된 경우이 로컬 정책을 *경고 및 거부*로 구성할 수 있지만 해제할 수는 없습니다.

## <a name="next-steps"></a>다음 단계

- [Azure 방화벽 관리자에 대 한 자세한 정보](overview.md)
---
title: Always On VPN 터널 구성
titleSuffix: Azure VPN Gateway
description: 사용자 로그인, 네트워크 상태 변경, 장치 화면 활성 등의 트리거를 기반으로 VPN 연결을 유지 관리 하는 Always On를 구성 하는 방법에 대해 알아봅니다.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 03/12/2020
ms.author: cherylmc
ms.openlocfilehash: 24043576fca4910631ccddb3924303dd642c6842
ms.sourcegitcommit: 4f1c7df04a03856a756856a75e033d90757bb635
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87927064"
---
# <a name="configure-an-always-on-vpn-device-tunnel"></a>Always On VPN 디바이스 터널 구성

[!INCLUDE [intro](../../includes/vpn-gateway-vwan-always-on-intro.md)]

## <a name="configure-the-gateway"></a>게이트웨이 구성

[지점 및 사이트 간 vpn 연결 구성](vpn-gateway-howto-point-to-site-resource-manager-portal.md) 문서를 사용 하 여 IKEv2 및 인증서 기반 인증을 사용 하도록 VPN gateway를 구성 합니다.

## <a name="configure-the-device-tunnel"></a>장치 터널 구성

[!INCLUDE [device tunnel](../../includes/vpn-gateway-vwan-always-on-device.md)]

## <a name="to-remove-a-profile"></a>프로필을 제거 하려면

프로필을 제거 하려면 다음 명령을 실행 합니다.

![정리](./media/vpn-gateway-howto-always-on-device-tunnel/cleanup.png)

## <a name="next-steps"></a>다음 단계

문제를 해결 [하려면 Azure 지점 및 사이트 간 연결 문제](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md) 를 참조 하세요.

---
title: Azure CLI 스크립트 샘플 - 운영 체제 디스크 탑재
description: Azure CLI 스크립트 샘플 - 운영 체제 디스크 탑재
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: cynthn
ms.custom: mvc, devx-track-azurecli
ms.openlocfilehash: 735b16ad054edfea7cf0ad2e1e5b9c76b664e6c3
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87479641"
---
# <a name="troubleshoot-a-vms-operating-system-disk"></a>VM 운영 체제 디스크 문제 해결

이 스크립트는 실패했거나 문제가 있는 가상 컴퓨터의 운영 체제 디스크를 두 번째 가상 컴퓨터에 대한 데이터 디스크로 탑재합니다. 디스크 문제를 해결하거나 데이터를 복구할 때 유용할 수 있습니다.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>샘플 스크립트

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/mount-os-disk/mount-os-disk.sh "Quick Create VM")]

## <a name="script-explanation"></a>스크립트 설명

이 스크립트는 다음 명령을 사용하여 리소스 그룹, 가상 컴퓨터 및 모든 관련된 리소스를 만듭니다. 테이블에 있는 각 명령은 명령에 해당하는 문서에 연결됩니다.

| 명령 | 메모 |
|---|---|
| [az vm show](/cli/azure/vm) | 가상 머신 목록을 반환합니다. 이 경우 가상 컴퓨터 운영 체제 디스크를 반환하는 데 쿼리 옵션이 사용됩니다. 그러면 이 값이 변수 이름 'uri'에 추가됩니다. |
| [az vm delete](/cli/azure/vm) | 가상 머신을 삭제합니다. |
| [az vm create](/cli/azure/vm) | 가상 머신을 만듭니다.  |
| [az vm disk attach](/cli/azure/vm/disk) | 디스크를 가상 머신에 연결합니다. |
| [az vm list-ip-addresses](/cli/azure/vm) | 가상 컴퓨터의 IP 주소를 반환합니다. |

## <a name="next-steps"></a>다음 단계

Azure CLI에 대한 자세한 내용은 [Azure CLI 설명서](/cli/azure)를 참조하세요.

추가 가상 머신 CLI 스크립트 샘플은 [Azure Linux VM 설명서](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)에서 확인할 수 있습니다.

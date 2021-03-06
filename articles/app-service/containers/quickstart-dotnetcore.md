---
title: '빠른 시작: Linux ASP.NET Core 앱 실행'
description: 첫 번째 ASP.NET Core 앱을 App Service의 Linux 컨테이너에 배포하여 Azure App Service에서 Linux 앱을 시작하세요.
keywords: azure app service, 웹앱, dotnet, core, linux, oss
ms.assetid: c02959e6-7220-496a-a417-9b2147638e2e
ms.tgt_pltfrm: linux
ms.topic: quickstart
ms.date: 04/22/2020
ms.custom: mvc, cli-validate, seodec18
ms.openlocfilehash: 1eeb5bbd4b10ef660a50f40d6c1300b0ca214561
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82206676"
---
# <a name="create-an-aspnet-core-app-in-app-service-on-linux"></a>Linux의 App Service에서 ASP.NET Core 앱 만들기

> [!NOTE]
> 이 문서에서는 Linux의 App Service에 앱을 배포합니다. _Windows_의 App Service에 배포하려면 [Azure에서 ASP.NET Core 앱 만들기](../app-service-web-get-started-dotnet.md)를 참조하세요.
>

[Linux의 App Service](app-service-linux-intro.md)는 Linux 운영 체제를 기반으로 확장성이 높은 자체 패치 웹 호스팅 서비스를 제공합니다. 이 빠른 시작에서는 Linux의 App Service에서 [.NET Core](https://docs.microsoft.com/aspnet/core/) 앱을 만드는 방법을 보여줍니다. [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli)를 사용하여 앱을 만들고 Git을 사용하여 앱에 .NET Core 코드를 배포합니다.

![Azure에서 실행되는 샘플 앱](media/quickstart-dotnetcore/dotnet-browse-azure.png)

Mac, Windows 또는 Linux 컴퓨터를 사용하여 이 문서의 단계를 수행하면 됩니다.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>사전 요구 사항

이 빠른 시작을 완료하려면 다음이 필요합니다.

* <a href="https://git-scm.com/" target="_blank">Git 설치</a>
* <a href="https://dotnet.microsoft.com/download/dotnet-core/3.1" target="_blank">최신 .NET Core 3.1 SDK 설치</a>

## <a name="create-the-app-locally"></a>로컬로 앱 만들기

컴퓨터의 터미널 창에서 `hellodotnetcore`라는 디렉터리를 만들고 현재 디렉터리를 이 디렉터리로 변경합니다.

```bash
mkdir hellodotnetcore
cd hellodotnetcore
```

.NET Core 앱을 만듭니다.

```bash
dotnet new web
```

## <a name="run-the-app-locally"></a>로컬에서 앱 실행하기

애플리케이션을 로컬로 실행하여 Azure에 애플리케이션을 배포할 때 표시되는 모양을 확인합니다. 

NuGet 패키지를 복원하고 앱을 실행합니다.

```bash
dotnet run
```

웹 브라우저를 열고 `http://localhost:5000`의 앱으로 이동합니다.

이 페이지에 표시된 샘플 앱에서 **Hello World** 메시지가 표시됩니다.

![브라우저를 통한 테스트](media/quickstart-dotnetcore/dotnet-browse-local.png)

터미널 창에서 **Ctrl+C**를 눌러 웹 서버를 종료합니다. .NET Core 프로젝트에 대해 Git 리포지토리를 초기화합니다.

```bash
git init
git add .
git commit -m "first commit"
```

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user.md)]

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-linux.md)]

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux.md)]

## <a name="create-a-web-app"></a>웹앱 만들기

[!INCLUDE [Create web app](../../../includes/app-service-web-create-web-app-dotnetcore-linux-no-h.md)]

새로 만든 앱으로 이동합니다. _&lt;app-name>_ 을 앱 이름으로 바꿉니다.

```bash
https://<app-name>.azurewebsites.net
```

새로운 앱은 다음과 같아야 합니다.

![빈 앱 페이지](media/quickstart-dotnetcore/dotnet-browse-created.png)

[!INCLUDE [Push to Azure](../../../includes/app-service-web-git-push-to-azure.md)] 

<pre>
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 285 bytes | 95.00 KiB/s, done.
Total 3 (delta 2), reused 0 (delta 0), pack-reused 0
remote: Deploy Async
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id 'd6b54472f7'.
remote: Repository path is /home/site/repository
remote: Running oryx build...
remote: Build orchestrated by Microsoft Oryx, https://github.com/Microsoft/Oryx
remote: You can report issues at https://github.com/Microsoft/Oryx/issues
remote:
remote: Oryx Version      : 0.2.20200114.13, Commit: 204922f30f8e8d41f5241b8c218425ef89106d1d, ReleaseTagName: 20200114.13
remote: Build Operation ID: |imoMY2y77/s=.40ca2a87_
remote: Repository Commit : d6b54472f7e8e9fd885ffafaa64522e74cf370e1
.
.
.
remote: Deployment successful.
remote: Deployment Logs : 'https://&lt;app-name&gt;.scm.azurewebsites.net/newui/jsonviewer?view_url=/api/deployments/d6b54472f7e8e9fd885ffafaa64522e74cf370e1/log'
To https://&lt;app-name&gt;.scm.azurewebsites.net:443/&lt;app-name&gt;.git
   d87e6ca..d6b5447  master -> master
</pre>

## <a name="browse-to-the-app"></a>앱으로 이동

웹 브라우저를 사용하여 배포된 애플리케이션으로 이동합니다.

```bash
http://<app_name>.azurewebsites.net
```

.NET Core 샘플 코드가 기본 제공 이미지가 있는 Linux의 App Service에서 실행됩니다.

![Azure에서 실행되는 샘플 앱](media/quickstart-dotnetcore/dotnet-browse-azure.png)

**축하합니다.** Linux의 App Service에 첫 번째 .NET Core 앱을 배포했습니다.

## <a name="update-and-redeploy-the-code"></a>코드 업데이트 및 다시 배포

로컬 디렉터리에서 _Startup.cs_ 파일을 엽니다. 메서드 호출 `context.Response.WriteAsync`의 텍스트를 약간 변경합니다.

```csharp
await context.Response.WriteAsync("Hello Azure!");
```

Git에서 변경 내용을 커밋한 다음 Azure에 코드 변경 내용을 푸시합니다.

```bash
git commit -am "updated output"
git push azure master
```

배포가 완료되면 **앱으로 이동** 단계에서 열린 브라우저 창으로 다시 전환하고 새로 고침을 누릅니다.

![Azure에서 실행되는 업데이트된 샘플 앱](media/quickstart-dotnetcore/dotnet-browse-azure-updated.png)

## <a name="manage-your-new-azure-app"></a>새 Azure 앱 관리

만든 앱을 관리하려면 <a href="https://portal.azure.com" target="_blank">Azure Portal</a>로 이동합니다.

왼쪽 메뉴에서 **App Services**를 클릭한 다음, Azure 앱의 이름을 클릭합니다.

![Azure 앱에 대한 포털 탐색](./media/quickstart-dotnetcore/portal-app-service-list.png)

앱의 [개요] 페이지가 표시됩니다. 여기에서 찾아보기, 중지, 시작, 다시 시작, 삭제와 같은 기본 관리 작업을 수행할 수 있습니다. 

![Azure Portal의 App Service 페이지](media/quickstart-dotnetcore/portal-app-overview.png)

왼쪽 메뉴에는 앱을 구성할 수 있는 여러 페이지가 표시됩니다. 

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [자습서: SQL Database를 사용하는 ASP.NET Core 앱](tutorial-dotnetcore-sqldb-app.md)

> [!div class="nextstepaction"]
> [ASP.NET Core 앱 구성](configure-language-dotnetcore.md)

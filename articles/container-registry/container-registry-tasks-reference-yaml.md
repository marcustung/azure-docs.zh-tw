---
title: Azure Container Registry 工作參考 - YAML
description: 適用於以 YAML 為「ACR 工作」定義工作的參考，包括工作屬性、步驟類型、步驟屬性及內建變數。
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 03/28/2019
ms.author: danlep
ms.openlocfilehash: b2398e7db7ed91dee8d85c0c50058bb15b9f4c7e
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/03/2019
ms.locfileid: "58894127"
---
# <a name="acr-tasks-reference-yaml"></a>ACR 工作參考：YAML

「ACR 工作」中的多步驟工作定義提供一個著重於建置、測試及修補容器並以容器為中心的計算原始物件。 本文章涵蓋命令、 參數、 屬性和定義您的多步驟工作的 YAML 檔案的語法。

此文章包含適用於為「ACR 工作」建立多步驟工作 YAML 檔案的參考。 如果您想要「ACR 工作」的簡介，請參閱 [ACR 工作概觀](container-registry-tasks-overview.md)。

## <a name="acr-taskyaml-file-format"></a>acr-task.yaml 檔案格式

「ACR 工作」支援採用標準 YAML 語法的多步驟工作宣告。 您可以定義工作的步驟於 YAML 檔案。 您可以再執行工作以手動方式傳遞至檔案[執行 az acr] [ az-acr-run]命令。 或者，您也可以使用檔案來建立一個具有工作[az acr 工作建立][ az-acr-task-create] Git 認可或基底映像更新會自動觸發的。 雖然此文章以 `acr-task.yaml` 作為包含步驟的檔案，但「ACR 工作」支援任何使用[支援的副檔名](#supported-task-filename-extensions)的有效檔案名稱。

最上層 `acr-task.yaml` 原始物件是**工作屬性** **步驟類型** 及 **步驟屬性**：

* [工作屬性](#task-properties)會套用至整個工作執行的所有步驟。 有數個全域工作屬性，包括：
  * `version`
  * `stepTimeout`
  * `workingDirectory`
* [工作步驟類型](#task-step-types)代表可在工作中執行的動作類型。 步驟類型有三種：
  * `build`
  * `push`
  * `cmd`
* [工作步驟屬性](#task-step-properties)是套用至個別步驟的參數。 步驟屬性有數個，包括：
  * `startDelay`
  * `timeout`
  * `when`
  * ...等等。

接下來是 `acr-task.yaml` 檔案的基底格式 (包括一些常見的步驟屬性)。 雖然這並未完整呈現所有可用的步驟屬性或步驟類型使用方式，但提供一個基本檔案格式的快速概觀。

```yml
version: # acr-task.yaml format version.
stepTimeout: # Seconds each step may take.
steps: # A collection of image or container actions.
  - build: # Equivalent to "docker build," but in a multi-tenant environment
  - push: # Push a newly built or retagged image to a registry.
    when: # Step property that defines either parallel or dependent step execution.
  - cmd: # Executes a container, supports specifying an [ENTRYPOINT] and parameters.
    startDelay: # Step property that specifies the number of seconds to wait before starting execution.
```

### <a name="supported-task-filename-extensions"></a>支援的工作副檔名

「ACR 工作」保留了數個將視為工作檔案來處理的副檔名，包括 `.yaml`。 所有「未」包含在下列清單中的副檔名都會被「ACR 工作」視為 Dockerfile：.yaml、.yml、.toml、.json、.sh、.bash、.zsh、.ps1、.ps、.cmd、.bat、.ts、.js、.php、.py、.rb、.lua

YAML 是「ACR 工作」目前唯一支援的檔案格式。 其他副檔名則保留供可能的未來支援使用。

## <a name="run-the-sample-tasks"></a>執行範例工作

此文章接下來的各節中會參考數個範例工作檔案。 這些範例工作位於公用 GitHub 存放庫 [Azure-Samples/acr-tasks][acr-tasks]中。 您可以使用 Azure CLI 命令 [az acr run][az-acr-run]來執行這些工作。 範例命令會類似於：

```azurecli
az acr run -f build-push-hello-world.yaml https://github.com/Azure-Samples/acr-tasks.git
```

範例命令的格式會假設您已在 Azure CLI 中設定預設登錄，因此它們會省略 `--registry` 參數。 若要設定預設登錄，請使用 [az configure][az-configure] 命令搭配 `--defaults` 參數，這會接受 `acr=REGISTRY_NAME` 值。

例如，為 Azure CLI 設定一個名為 "myregistry" 的預設登錄：

```azurecli
az configure --defaults acr=myregistry
```

## <a name="task-properties"></a>工作屬性

工作屬性通常會出現在頂端`acr-task.yaml`檔案，並會完整執行工作步驟的過程中套用的全域屬性。 這些全域屬性中有些可在個別步驟中被覆寫。

| 屬性 | 類型 | 選用 | 描述 | 支援覆寫 | 預設值 |
| -------- | ---- | -------- | ----------- | ------------------ | ------------- |
| `version` | 字串 | 是 | 「ACR 工作」服務所剖析的 `acr-task.yaml` 檔案版本。 在「ACR 工作」努力維持回溯相容性的同時，此值則可讓「ACR 工作」在已定義的版本內維持相容性。 如果未指定，則會預設為最新版本。 | 否 | None |
| `stepTimeout` | 整數 (秒) | 是 | 步驟的可執行秒數上限。 如果工作指定的屬性，它會設定預設`timeout`屬性的所有步驟。 如果`timeout`屬性會指定上一個步驟時，它會覆寫工作所提供的屬性。 | 是 | 600 (10 分鐘) |
| `workingDirectory` | 字串 | 是 | 在執行階段容器的工作目錄。 如果工作指定的屬性，它會設定預設`workingDirectory`屬性的所有步驟。 如果指定的步驟，它會覆寫工作所提供的屬性。 | 是 | `$HOME` |
| `env` | [字串, 字串, ...] | 是 |  中的字串陣列`key=value`定義工作的環境變數的格式。 如果工作指定的屬性，它會設定預設`env`屬性的所有步驟。 如果指定的步驟，它會覆寫繼承自工作的任何環境變數。 | None |
| `secrets` | [secret, secret, ...] | 是 | 陣列[祕密](#secret)物件。 | None |
| `networks` | [網路，網路，...] | 是 | 陣列[網路](#network)物件。 | None |

### <a name="secret"></a>secret

密碼的物件具有下列屬性。

| 屬性 | 類型 | 選用 | 描述 | 預設值 |
| -------- | ---- | -------- | ----------- | ------- |
| `id` | 字串 | 否 | 密碼的識別項。 | None |
| `akv` | 字串 | 是 | Azure Key Vault (AKV) 祕密 URL。 | None |
| `clientID` | 字串 | 是 | 使用者指派的用戶端識別碼會管理適用於 Azure 資源的身分識別。 | None |

### <a name="network"></a>網路

網路物件具有下列屬性。

| 屬性 | 類型 | 選用 | 描述 | 預設值 |
| -------- | ---- | -------- | ----------- | ------- | 
| `name` | 字串 | 否 | 網路的名稱。 | None |
| `driver` | 字串 | 是 | 若要管理的網路驅動程式。 | None |
| `ipv6` | 布林 | 是 | 是否已啟用 IPv6 網路功能。 | `false` |
| `skipCreation` | 布林 | 是 | 是否略過建立網路。 | `false` |
| `isDefault` | 布林 | 是 | 網路是否提供 Azure Container Registry 的預設網路 | `false` |

## <a name="task-step-types"></a>工作步驟類型

「ACR 工作」支援三種步驟類型。 每一種步驟類型都支援數個屬性，詳述於每一種步驟類型的小節中。

| 步驟類型 | 描述 |
| --------- | ----------- |
| [`build`](#build) | 使用熟悉的 `docker build` 語法來建置容器映像。 |
| [`push`](#push) | 執行新建置或重新標記之映像的 `docker push` 以推送至容器登錄。 支援 Azure Container Registry、其他私人登錄，以及公用 Docker Hub。 |
| [`cmd`](#cmd) | 以命令方式執行容器，並將參數傳遞給容器的 `[ENTRYPOINT]`。 `cmd`步驟類型支援的參數，例如`env`， `detach`，和其他熟悉`docker run`命令選項，啟用單元和並行容器執行功能測試。 |

## <a name="build"></a>build

建置容器映像。 `build` 步驟類型代表一種在雲端中以第一級原始物件形式執行 `docker build` 的多租用戶、安全方法。

### <a name="syntax-build"></a>語法：build

```yml
version: v1.0.0
steps:
  - [build]: -t [imageName]:[tag] -f [Dockerfile] [context]
    [property]: [value]
```

`build` 步驟類型支援下表中的參數。 `build` 步驟類型也支援 [docker build](https://docs.docker.com/engine/reference/commandline/build/) 命令的所有建置選項，例如以 `--build-arg` 設定建置時間變數。

| 參數 | 描述 | 選用 |
| --------- | ----------- | :-------: |
| `-t` &#124; `--image` | 定義所建置映像的完整 `image:tag`。<br /><br />由於映像可能會用於內部工作驗證 (例如功能測試)，因此並非所有映像都需要 `push` 來推送至登錄。 不過，若要在工作執行內將某個映像執行個體化，則該映像確實需要一個可供參考的名稱。<br /><br />不同於`az acr build`，執行中的 ACR 工作並不提供預設的推入行為。 使用「ACR 工作」時，預設案例會能夠建置、驗證，然後推送映像。 如需了解如何視需要推送所建置的映像，請參閱 [push](#push)。 | 是 |
| `-f` &#124; `--file` | 指定傳遞給 `docker build` 的 Dockerfile。 如果未指定，則會假設使用內容根目錄中的預設 Dockerfile。 若要指定的 Dockerfile，傳遞內容的根目錄的相對檔案名稱。 | 是 |
| `context` | 傳遞給 `docker build` 的根目錄。 每個工作的根目錄都會設定為共用的 [workingDirectory](#task-step-properties)，並且包含相關 Git 複製目錄的根目錄。 | 否 |

### <a name="properties-build"></a>屬性：build

`build` 步驟類型支援下列屬性。 尋找在這些屬性的詳細資料[工作步驟屬性](#task-step-properties)一節。

| | | |
| -------- | ---- | -------- |
| `detach` | 布林 | 選用 |
| `disableWorkingDirectoryOverride` | 布林 | 選用 |
| `entryPoint` | 字串 | 選用 |
| `env` | [字串, 字串, ...] | 選用 |
| `expose` | [字串, 字串, ...] | 選用 |
| `id` | 字串 | 選用 |
| `ignoreErrors` | 布林 | 選用 |
| `isolation` | 字串 | 選用 |
| `keep` | 布林 | 選用 |
| `network` | 物件 | 選用 |
| `ports` | [字串, 字串, ...] | 選用 |
| `pull` | 布林 | 選用 |
| `repeat` | int | 選用 |
| `retries` | int | 選用 |
| `retryDelay` | 整數 (秒) | 選用 |
| `secret` | 物件 | 選用 |
| `startDelay` | 整數 (秒) | 選用 |
| `timeout` | 整數 (秒) | 選用 |
| `when` | [字串, 字串, ...] | 選用 |
| `workingDirectory` | 字串 | 選用 |

### <a name="examples-build"></a>範例：build

#### <a name="build-image---context-in-root"></a>建置映像 - 根目錄中的內容

```azurecli
az acr run -f build-hello-world.yaml https://github.com/AzureCR/acr-tasks-sample.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/build-hello-world.yaml -->
[!code-yml[task](~/acr-tasks/build-hello-world.yaml)]

#### <a name="build-image---context-in-subdirectory"></a>建置映像 - 子目錄中的內容

```yml
version: v1.0.0
steps:
  - build: -t {{.Run.Registry}}/hello-world -f hello-world.dockerfile ./subDirectory
```

## <a name="push"></a>push

將一或多個已建置或重新標記的映像推送至登錄。 支援推送至私人登錄 (例如 Azure Container Registry) 或公用 Docker Hub。

### <a name="syntax-push"></a>語法：push

`push` 步驟類型支援映像集合。 YAML 集合語法支援內嵌和巢狀格式。 推送單一映像通常是以內嵌語法表示：

```yml
version: v1.0.0
steps:
  # Inline YAML collection syntax
  - push: ["{{.Run.Registry}}/hello-world:{{.Run.ID}}"]
```

為了提升可讀性，推送多個映像時，請使用巢狀語法：

```yml
version: v1.0.0
steps:
  # Nested YAML collection syntax
  - push:
    - {{.Run.Registry}}/hello-world:{{.Run.ID}}
    - {{.Run.Registry}}/hello-world:latest
```

### <a name="properties-push"></a>屬性：push

`push` 步驟類型支援下列屬性。 尋找在這些屬性的詳細資料[工作步驟屬性](#task-step-properties)一節。

| | | |
| -------- | ---- | -------- |
| `env` | [字串, 字串, ...] | 選用 |
| `id` | 字串 | 選用 |
| `ignoreErrors` | 布林 | 選用 |
| `startDelay` | 整數 (秒) | 選用 |
| `timeout` | 整數 (秒) | 選用 |
| `when` | [字串, 字串, ...] | 選用 |

### <a name="examples-push"></a>範例：push

#### <a name="push-multiple-images"></a>推送多個映像

```azurecli
az acr run -f build-push-hello-world.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/build-push-hello-world.yaml -->
[!code-yml[task](~/acr-tasks/build-push-hello-world.yaml)]

#### <a name="build-push-and-run"></a>建置、推送及執行

```azurecli
az acr run -f build-run-hello-world.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/build-run-hello-world.yaml -->
[!code-yml[task](~/acr-tasks/build-run-hello-world.yaml)]

## <a name="cmd"></a>cmd

`cmd` 步驟類型會執行容器。

### <a name="syntax-cmd"></a>語法：cmd

```yml
version: v1.0.0
steps:
  - [cmd]: [containerImage]:[tag (optional)] [cmdParameters to the image]
```

### <a name="properties-cmd"></a>屬性：cmd

`cmd` 步驟類型支援下列屬性：

| | | |
| -------- | ---- | -------- |
| `detach` | 布林 | 選用 |
| `disableWorkingDirectoryOverride` | 布林 | 選用 |
| `entryPoint` | 字串 | 選用 |
| `env` | [字串, 字串, ...] | 選用 |
| `expose` | [字串, 字串, ...] | 選用 |
| `id` | 字串 | 選用 |
| `ignoreErrors` | 布林 | 選用 |
| `isolation` | 字串 | 選用 |
| `keep` | 布林 | 選用 |
| `network` | 物件 | 選用 |
| `ports` | [字串, 字串, ...] | 選用 |
| `pull` | 布林 | 選用 |
| `repeat` | int | 選用 |
| `retries` | int | 選用 |
| `retryDelay` | 整數 (秒) | 選用 |
| `secret` | 物件 | 選用 |
| `startDelay` | 整數 (秒) | 選用 |
| `timeout` | 整數 (秒) | 選用 |
| `when` | [字串, 字串, ...] | 選用 |
| `workingDirectory` | 字串 | 選用 |

您可以在此文章的[工作步驟屬性](#task-step-properties)一節中，找到這些屬性的詳細資料。

### <a name="examples-cmd"></a>範例：cmd

#### <a name="run-hello-world-image"></a>執行 hello-world 映像

此命令會執行 `hello-world.yaml` 工作檔案以參考 Docker Hub 上的 [hello-world](https://hub.docker.com/_/hello-world/) 映像。

```azurecli
az acr run -f hello-world.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/hello-world.yaml -->
[!code-yml[task](~/acr-tasks/hello-world.yaml)]

#### <a name="run-bash-image-and-echo-hello-world"></a>執行 bash 映像並回應 "hello world"

此命令會執行 `bash-echo.yaml` 工作檔案以參考 Docker Hub 上的 [bash](https://hub.docker.com/_/bash/) 映像。

```azurecli
az acr run -f bash-echo.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/bash-echo.yaml -->
[!code-yml[task](~/acr-tasks/bash-echo.yaml)]

#### <a name="run-specific-bash-image-tag"></a>執行特定 bash 映像標記

若要執行特定的映像版本，請在 `cmd`中指定標記。

此命令會執行 `bash-echo-3.yaml` 工作檔案以參考 Docker Hub 上的 [bash:3.0](https://hub.docker.com/_/bash/) 映像。

```azurecli
az acr run -f bash-echo-3.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/bash-echo-3.yaml -->
[!code-yml[task](~/acr-tasks/bash-echo-3.yaml)]

#### <a name="run-custom-images"></a>執行自訂映像

`cmd` 步驟類型會使用標準 `docker run` 格式來參考映像。 映像的開頭如果不是登錄，系統會假設其產生自 docker.io。 上述範例可同樣表示為：

```yml
version: v1.0.0
steps:
  - cmd: docker.io/bash:3.0 echo hello world
```

使用標準`docker run`映像參考慣例`cmd`可以從任何私人登錄或公用 Docker Hub 執行映像。 如果您要參考「ACR 工作」執行所在之相同登錄中的映像，則不需要指定任何登錄認證。

* 執行從 Azure container registry 的映像

    以您的登錄名稱取代 `[myregistry]`：

    ```yml
    version: v1.0.0
    steps:
        - cmd: [myregistry].azurecr.io/bash:3.0 echo hello world
    ```

* 使用 Run 變數將登錄參考一般化

    您可以不將登錄名稱以硬式編碼寫在 `acr-task.yaml` 檔案中，而是使用 [Run 變數](#run-variables)來提升其可攜性。 `Run.Registry` 變數會在執行階段擴充成工作執行所在的登錄名稱。

    若要將前述工作一般化，讓它能夠在任何 Azure 容器登錄中運作，請在映像名稱中參考 [Run.Registry](#runregistry) 變數：

    ```yml
    version: v1.0.0
    steps:
      - cmd: {{.Run.Registry}}/bash:3.0 echo hello world
    ```

## <a name="task-step-properties"></a>工作步驟屬性

每個步驟類型都支援數個適用於其類型的屬性。 下表定義所有可用的步驟屬性。 並非所有步驟類型都支援所有屬性。 若要查看每個步驟類型可使用這當中哪些屬性，請參閱 [cmd](#cmd)、[build](#build)及 [push](#push) 步驟類型參考小節。

| 屬性 | 類型 | 選用 | 描述 | 預設值 |
| -------- | ---- | -------- | ----------- | ------- |
| `detach` | 布林 | 是 | 執行時是否應將容器中斷連結。 | `false` |
| `disableWorkingDirectoryOverride` | 布林 | 是 | 是否要停用`workingDirectory`覆寫功能。 搭配使用此`workingDirectory`有容器的工作目錄的完整控制權。 | `false` |
| `entryPoint` | 字串 | 是 | 覆寫步驟容器的 `[ENTRYPOINT]`。 | None |
| `env` | [字串, 字串, ...] | 是 | `key=value` 格式的字串陣列，用來定義步驟的環境變數。 | None |
| `expose` | [字串, 字串, ...] | 是 | 從容器所公開的連接埠的陣列。 |  None |
| [`id`](#example-id) | 字串 | 是 | 可唯一識別工作內的步驟。 工作內的其他步驟可以參考步驟的 `id`，例如使用 `when`進行相依性檢查。<br /><br />`id` 同時也是執行中容器的名稱。 舉例來說，在工作內其他容器中執行的程序可以參考 `id` 作為其 DNS 主機名稱，或藉由 docker logs [id] 來存取它。 | `acb_step_%d`其中`%d`是由上而下的 YAML 檔案中的步驟 0 為基底的索引 |
| `ignoreErrors` | 布林 | 是 | 是否要將步驟標記為成功，不論是否在容器執行期間發生錯誤。 | `false` |
| `isolation` | 字串 | 是 | 容器的隔離等級。 | `default` |
| `keep` | 布林 | 是 | 在執行後是否應保留步驟的容器。 | `false` |
| `network` | 物件 | 是 | 識別容器執行所在的網路。 | None |
| `ports` | [字串, 字串, ...] | 是 | 發行從容器至主機的連接埠的陣列。 |  None |
| `pull` | 布林 | 是 | 是否要強制提取容器，然後再執行它以防止任何快取行為。 | `false` |
| `privileged` | 布林 | 是 | 是否要在特權模式下執行容器。 | `false` |
| `repeat` | int | 是 | 若要重複執行容器的重試次數。 | 0 |
| `retries` | int | 是 | 如果容器無法執行嘗試的重試次數。 如果容器的結束代碼為非零，只會嘗試重試。 | 0 |
| `retryDelay` | 整數 (秒) | 是 | 以秒為單位的容器執行的重試之間的延遲。 | 0 |
| `secret` | 物件 | 是 | 識別 Azure Key Vault 祕密或適用於 Azure 資源的受管理身分識別。 | None |
| `startDelay` | 整數 (秒) | 是 | 若要延遲的容器執行的秒數。 | 0 |
| `timeout` | 整數 (秒) | 是 | 終止步驟前可允許步驟執行的秒數上限。 | 600 |
| [`when`](#example-when) | [字串, 字串, ...] | 是 | 設定步驟與工作內一或多個其他步驟的相依性。 | None |
| `user` | 字串 | 是 | 使用者名稱或容器的 UID | None |
| `workingDirectory` | 字串 | 是 | 設定步驟的工作目錄。 「ACR 工作」預設會建立根目錄作為工作目錄。 不過，如果您的組建含有數個步驟，則可藉由指定相同的工作目錄，讓較前面的步驟與較後面的步驟共用成品。 | `$HOME` |

### <a name="examples-task-step-properties"></a>範例：工作步驟屬性

#### <a name="example-id"></a>範例：id

建置兩個映像，將功能測試映像執行個體化。 每個步驟都會由唯一 `id` 識別，工作中的其他步驟會在其 `when` 屬性中參考此 id。

```azurecli
az acr run -f when-parallel-dependent.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/when-parallel-dependent.yaml -->
[!code-yml[task](~/acr-tasks/when-parallel-dependent.yaml)]

#### <a name="example-when"></a>範例：when

`when` 屬性會指定步驟與工作內其他步驟的相依性。 它支援兩個參數值：

* `when: ["-"]` -表示不會相依於其他步驟。 指定 `when: ["-"]` 的步驟將立即開始執行，並可啟用並行步驟執行。
* `when: ["id1", "id2"]` -指出的步驟，是取決於使用的步驟`id`"id1 」 和`id`"id2 」。 此步驟必須等到 "id1" 和 "id2" 步驟都完成後，才會執行。

如果步驟中未指定 `when`，則該步驟會取決於 `acr-task.yaml` 檔案中的前一個步驟是否完成。

不含 `when` 的循序步驟執行：

```azurecli
az acr run -f when-sequential-default.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/when-sequential-default.yaml -->
[!code-yml[task](~/acr-tasks/when-sequential-default.yaml)]

含有 `when` 的循序步驟執行：

```azurecli
az acr run -f when-sequential-id.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/when-sequential-id.yaml -->
[!code-yml[task](~/acr-tasks/when-sequential-id.yaml)]

平行的映像組建中：

```azurecli
az acr run -f when-parallel.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/when-parallel.yaml -->
[!code-yml[task](~/acr-tasks/when-parallel.yaml)]

平行映像建置和相依測試：

```azurecli
az acr run -f when-parallel-dependent.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/when-parallel-dependent.yaml -->
[!code-yml[task](~/acr-tasks/when-parallel-dependent.yaml)]

## <a name="run-variables"></a>Run 變數

「ACR 工作」包含一組預設變數，可供工作步驟在執行時使用。 使用 `{{.Run.VariableName}}` 格式即可存取這些變數，其中 `VariableName` 是下列其中之一：

* `Run.ID`
* `Run.Registry`
* `Run.Date`

### <a name="run46id"></a>Run&#46;ID

每個「執行」(透過 `az acr run`) 或透過 `az acr task create` 建立之以觸發程序為基礎的工作執行都具有唯一識別碼。 此識別碼代表目前正在執行的「執行」。

通常用來唯一標記某個映像：

```yml
version: v1.0.0
steps:
    - build: -t {{.Run.Registry}}/hello-world:{{.Run.ID}} .
```

### <a name="runregistry"></a>Run.Registry

登錄的完整伺服器名稱。 通常用來一般參考工作執行所在的登錄。

```yml
version: v1.0.0
steps:
  - build: -t {{.Run.Registry}}/hello-world:{{.Run.ID}} .
```

### <a name="rundate"></a>Run.Date

執行開始的目前 UTC 時間。

## <a name="next-steps"></a>後續步驟

如需多步驟工作的概觀，請參閱[執行 ACR 工作中的多步驟建置、測試及修補工作](container-registry-tasks-multi-step.md)。

如需了解單一步驟建置，請參閱 [ACR 工作概觀](container-registry-tasks-overview.md)。

<!-- IMAGES -->

<!-- LINKS - External -->
[acr-tasks]: https://github.com/Azure-Samples/acr-tasks

<!-- LINKS - Internal -->
[az-acr-run]: /cli/azure/acr#az-acr-run
[az-acr-task-create]: /cli/azure/acr/task#az-acr-task-create
[az-configure]: /cli/azure/reference-index#az-configure

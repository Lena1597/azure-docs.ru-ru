---
title: DevOps для конвейера приема данных
titleSuffix: Azure Machine Learning
description: Узнайте, как применять DevOps методики к реализации конвейера приема данных, используемой для подготовки данных для обучения модели.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: iefedore
author: eedorenko
manager: davete
ms.reviewer: larryfr
ms.date: 01/30/2020
ms.openlocfilehash: 49b384e9e2d9b77179a0154bf2d96524c064c2ca
ms.sourcegitcommit: fa6fe765e08aa2e015f2f8dbc2445664d63cc591
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/01/2020
ms.locfileid: "76960975"
---
# <a name="devops-for-a-data-ingestion-pipeline"></a>DevOps для конвейера приема данных

В большинстве случаев решение приема данных представляет собой композицию скриптов, вызовов служб и конвейера, управляющего всеми действиями. Из этой статьи вы узнаете, как применять DevOps методики к жизненному циклу разработки общего конвейера приема данных. Конвейер готовит данные для обучения модели Машинное обучение.

## <a name="the-solution"></a>Решение

Рассмотрим следующий рабочий процесс приема данных:

![прием данных — конвейер](media/how-to-cicd-data-ingestion/data-ingestion-pipeline.png)

При таком подходе обучающие данные хранятся в хранилище BLOB-объектов Azure. Конвейер фабрики данных Azure извлекает данные из входного контейнера больших двоичных объектов, преобразует их и сохраняет в выходном контейнере больших двоичных объектов. Этот контейнер служит [хранилищем данных](https://docs.microsoft.com/azure/machine-learning/concept-data#access-data-in-storage) для службы машинное обучение Azure. После подготовки данных конвейер фабрики данных вызывает конвейер обучения Машинное обучение для обучения модели. В этом конкретном примере преобразование данных выполняется с помощью записной книжки Python, работающей в кластере Azure Databricks. 

## <a name="what-we-are-building"></a>Что мы создаем

Как и в случае с любым программным решением, над ним работает группа (например, инженеры по работе с данными). 

![cicd — прием данных](media/how-to-cicd-data-ingestion/cicd-data-ingestion.png)

Они совместно работают и совместно используют одни и те же ресурсы Azure, такие как фабрика данных Azure, Azure Databricks, учетная запись хранения Azure и т. д. Коллекция этих ресурсов является средой разработки. Специалисты по данным вносят вклад в одну базу исходного кода. Процесс непрерывной интеграции выполняет сборку кода, проверяет его с помощью тестов качества кода, модульных тестов и создает артефакты, такие как протестированный код и шаблоны Azure Resource Manager. Процесс непрерывной поставки развертывает артефакты в нижестоящих средах. В этой статье показано, как автоматизировать процессы CI и CD с помощью [Azure pipelines](https://azure.microsoft.com/services/devops/pipelines/).

## <a name="source-control-management"></a>Управление системой управления версиями

Члены группы работают несколько разными способами для совместной работы над исходным кодом записной книжки Python и исходным кодом фабрики данных Azure. Однако в обоих случаях код хранится в репозитории системы управления версиями (например, Azure DevOps, GitHub, GitLab), а совместное взаимодействие обычно основано на модели ветвления (например, [гитфлов](https://datasift.github.io/gitflow/IntroducingGitFlow.html)).

### <a name="python-notebook-source-code"></a>Исходный код записной книжки Python

Специалисты по работе с данными работают с исходным кодом для записной книжки Python локально в интегрированной среде разработки (например, [Visual Studio Code](https://code.visualstudio.com)) или непосредственно в рабочей области "кирпичи данных". Последний дает возможность отлаживать код в среде разработки. В любом случае код будет объединен в репозиторий после применения политики ветвления. Настоятельно рекомендуется хранить код в `.py` файлах, а не в формате записной книжки `.ipynb` Jupyter. Он повышает удобочитаемость кода и включает автоматическую проверку качества кода в процессе непрерывной интеграции.

### <a name="azure-data-factory-source-code"></a>Исходный код фабрики данных Azure

Исходный код конвейеров фабрики данных Azure — это коллекция JSON-файлов, созданных рабочей областью. Обычно инженеры по работе с данными работают с визуальным конструктором в рабочей области фабрики данных Azure, а не непосредственно с файлами исходного кода. Настройте рабочую область с репозиторием системы управления версиями, как описано в [документации по фабрике данных Azure](https://docs.microsoft.com/azure/data-factory/source-control#author-with-azure-repos-git-integration). В такой конфигурации разработчики данных могут совместно работать над исходным кодом, который следует за предпочтительным рабочим процессом ветвления.    

## <a name="continuous-integration-ci"></a>Непрерывная интеграция (CI)

Основная цель процесса непрерывной интеграции состоит в том, чтобы собрать совместную работу группы из исходного кода и подготовить ее к развертыванию в нижестоящих средах. Как и в случае с управлением исходным кодом, этот процесс отличается для записных книжек Python и конвейеров фабрики данных Azure. 

### <a name="python-notebook-ci"></a>Элемент конфигурации записной книжки Python

Процесс CI для записных книжек Python получает код из ветви совместной работы (например, с ***главной*** или ***разработкой***) и выполняет следующие действия:
* Linting кода
* Модульное тестирование
* Сохранение кода как артефакта

В следующем фрагменте кода показано, как реализовать эти шаги в конвейере Azure DevOps ***YAML*** :

```yaml
steps:
- script: |
   flake8 --output-file=$(Build.BinariesDirectory)/lint-testresults.xml --format junit-xml  
  workingDirectory: '$(Build.SourcesDirectory)'
  displayName: 'Run flake8 (code style analysis)'  
  
- script: |
   python -m pytest --junitxml=$(Build.BinariesDirectory)/unit-testresults.xml $(Build.SourcesDirectory)
  displayName: 'Run unit tests'

- task: PublishTestResults@2
  condition: succeededOrFailed()
  inputs:
    testResultsFiles: '$(Build.BinariesDirectory)/*-testresults.xml'
    testRunTitle: 'Linting & Unit tests'
    failTaskOnFailedTests: true
  displayName: 'Publish linting and unit test results'

- publish: $(Build.SourcesDirectory)
    artifact: di-notebooks

```

Конвейер использует ***flake8*** для работы с кодом Python linting. Он выполняет модульные тесты, определенные в исходном коде, и публикует linting и результаты тестов, чтобы они были доступны на экране выполнения конвейера Azure:

![linting — модульные тесты](media/how-to-cicd-data-ingestion/linting-unit-tests.png)

Если linting и модульное тестирование выполнены успешно, конвейер скопирует исходный код в репозиторий артефактов, который будет использоваться последующими шагами развертывания.

### <a name="azure-data-factory-ci"></a>CI фабрики данных Azure

Процесс непрерывной интеграции для конвейера фабрики данных Azure является узким местом в полной истории CI/CD для конвейера приема данных. ***Непрерывная*** Интеграция отсутствует. Развертываемый артефакт фабрики данных Azure — это коллекция шаблонов Azure Resource Manager. Единственный способ создать эти шаблоны — нажать кнопку ***опубликовать*** в рабочей области фабрика данных Azure. Здесь нет автоматизации.
Специалисты по работе с данными объединяют исходный код из их компонентов в ветвь совместной работы, например ***master*** или ***Разработка***. Затем пользователь с предоставленными разрешениями нажмет кнопку ***Publish (опубликовать*** ), чтобы создать Azure Resource Manager шаблоны из исходного кода в ветви совместной работы. При нажатии кнопки Рабочая область проверяет конвейеры (Подумайте о linting и модульном тестировании), создает шаблоны Azure Resource Manager (Подумайте об этом как на стадии сборки) и сохраняет созданные шаблоны в техническом ветви ***adf_publish*** в том же репозитории кода (с учетом артефактов публикации). Эта ветвь создается автоматически рабочей областью фабрики данных Azure. Этот процесс описан в [документации по фабрике данных Azure](https://docs.microsoft.com/azure/data-factory/continuous-integration-deployment).

Важно убедиться, что созданные шаблоны Azure Resource Manager не зависят от среды. Это означает, что все значения, которые могут отличаться между средами, являются параметризованным. Фабрика данных Azure достаточно интеллектуальна для предоставления большинства таких значений в качестве параметров. Например, в следующем шаблоне свойства подключения к рабочей области Машинное обучение Azure предоставляются в виде параметров:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "factoryName": {
            "value": "devops-ds-adf"
        },
        "AzureMLService_servicePrincipalKey": {
            "value": ""
        },
        "AzureMLService_properties_typeProperties_subscriptionId": {
            "value": "0fe1c235-5cfa-4152-17d7-5dff45a8d4ba"
        },
        "AzureMLService_properties_typeProperties_resourceGroupName": {
            "value": "devops-ds-rg"
        },
        "AzureMLService_properties_typeProperties_servicePrincipalId": {
            "value": "6e35e589-3b22-4edb-89d0-2ab7fc08d488"
        },
        "AzureMLService_properties_typeProperties_tenant": {
            "value": "72f988bf-86f1-41af-912b-2d7cd611db47"
        }
    }
}
```

Тем не менее вам может потребоваться предоставить пользовательские свойства, которые не обрабатываются рабочей областью фабрики данных Azure по умолчанию. В сценарии этой статьи конвейер фабрики данных Azure вызывает записную книжку Python, обрабатывающую данные. Записная книжка принимает параметр с именем входного файла данных.

```Python
import pandas as pd
import numpy as np

data_file_name = getArgument("data_file_name")
data = pd.read_csv(data_file_name)

labels = np.array(data['target'])
...
```

Это имя отличается для сред ***разработки***, ***QA***, ***UAT***и ***производственных*** . В сложном конвейере с несколькими действиями может быть несколько пользовательских свойств. Рекомендуется объединить все эти значения в одном месте и определить их как ***переменные***конвейера:

![ADF-переменные](media/how-to-cicd-data-ingestion/adf-variables.png)

Действия конвейера могут ссылаться на переменные конвейера при их фактическом использовании:

![ADF-Notebook-параметры](media/how-to-cicd-data-ingestion/adf-notebook-parameters.png)

В рабочей области фабрики данных Azure ***не*** предоставляются переменные конвейера по умолчанию в качестве параметров шаблонов Azure Resource Manager. В рабочей области используется [шаблон параметризации по умолчанию](https://docs.microsoft.com/azure/data-factory/continuous-integration-deployment#default-parameterization-template) , определяющий, какие свойства конвейера должны быть предоставлены как параметры шаблона Azure Resource Manager. Чтобы добавить в список переменные конвейера, обновите раздел "Microsoft. DataTemplate/фабрики/конвейеры" в [шаблоне параметризации по умолчанию](https://docs.microsoft.com/azure/data-factory/continuous-integration-deployment#default-parameterization-template) , используя следующий фрагмент кода, и поместите результирующий JSON-файл в корень исходной папки:

```json
"Microsoft.DataFactory/factories/pipelines": {
        "properties": {
            "variables": {
                "*": {
                    "defaultValue": "="
                }
            }
        }
    }
```

При этом Рабочая область фабрики данных Azure будет добавлять переменные в список параметров при нажатии кнопки " ***опубликовать*** ":

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "factoryName": {
            "value": "devops-ds-adf"
        },
        ...
        "data-ingestion-pipeline_properties_variables_data_file_name_defaultValue": {
            "value": "driver_prediction_train.csv"
        }        
    }
}
```

Значения в файле JSON являются значениями по умолчанию, настроенными в определении конвейера. Они должны быть переопределены значениями целевой среды при развертывании шаблона Azure Resource Manager.

## <a name="continuous-delivery-cd"></a>Непрерывная доставка

Процесс непрерывной поставки принимает артефакты и развертывает их в первую целевую среду. Это гарантирует, что решение работает, выполняя тесты. В случае успешного выполнения он переходит к следующей среде. Конвейер Azure на компакт-диске состоит из нескольких этапов, представляющих собой среды. На каждом этапе содержатся [развертывания](https://docs.microsoft.com/azure/devops/pipelines/process/deployment-jobs?view=azure-devops) и [задания](https://docs.microsoft.com/azure/devops/pipelines/process/phases?view=azure-devops&tabs=yaml) , выполняющие следующие действия.
* Развертывание записной книжки Python в Azure Databricks рабочей области
* Развертывание конвейера фабрики данных Azure 
* Запуск конвейера
* Проверка результата приема данных

Этапы конвейера можно настроить с помощью [утверждений](https://docs.microsoft.com/azure/devops/pipelines/process/approvals?view=azure-devops&tabs=check-pass) и [шлюзов](https://docs.microsoft.com/azure/devops/pipelines/release/approvals/gates?view=azure-devops) , которые обеспечивают дополнительный контроль над тем, как процесс развертывания проходит через цепочку сред.

### <a name="deploy-a-python-notebook"></a>Развертывание записной книжки Python

В следующем фрагменте кода определяется [развертывание](https://docs.microsoft.com/azure/devops/pipelines/process/deployment-jobs?view=azure-devops) конвейера Azure, которое копирует записную книжку Python в кластер кирпичей.

```yaml
- stage: 'Deploy_to_QA'
  displayName: 'Deploy to QA'
  variables:
  - group: devops-ds-qa-vg
  jobs:
  - deployment: "Deploy_to_Databricks"
    displayName: 'Deploy to Databricks'
    timeoutInMinutes: 0
    environment: qa
    strategy:
      runOnce:
        deploy:
          steps:
            - task: UsePythonVersion@0
              inputs:
                versionSpec: '3.x'
                addToPath: true
                architecture: 'x64'
              displayName: 'Use Python3'

            - task: configuredatabricks@0
              inputs:
                url: '$(DATABRICKS_URL)'
                token: '$(DATABRICKS_TOKEN)'
              displayName: 'Configure Databricks CLI'    

            - task: deploynotebooks@0
              inputs:
                notebooksFolderPath: '$(Pipeline.Workspace)/di-notebooks'
                workspaceFolder: '/Shared/devops-ds'
              displayName: 'Deploy (copy) data processing notebook to the Databricks cluster'       
```            

Артефакты, созданные CI, автоматически копируются в агент развертывания и доступны в папке ***$ (конвейер. Workspace)*** . В этом случае задача развертывания относится к артефакту ***di-Notebooks*** , содержащему записную книжку Python. В этом [развертывании](https://docs.microsoft.com/azure/devops/pipelines/process/deployment-jobs?view=azure-devops) для копирования файлов записной книжки в рабочую область "модули обработки" используется [расширение Azure DevOps для модуля](https://marketplace.visualstudio.com/items?itemName=riserrad.azdo-databricks) обработки.
На этапе ***Deploy_to_QA*** содержится ссылка на группу переменных ***devops-DS-QA-VG*** , определенную в проекте Azure devops. Действия на этом этапе относятся к переменным из этой группы переменных (например, $ (DATABRICKS_URL), $ (DATABRICKS_TOKEN)). Идея состоит в том, что следующий этап (например, ***Deploy_to_UAT***) будет действовать с теми же именами переменных, которые определены в собственной группе переменных с областью действия UAT.

### <a name="deploy-an-azure-data-factory-pipeline"></a>Развертывание конвейера фабрики данных Azure

Развертываемый артефакт фабрики данных Azure является шаблоном Azure Resource Manager. Поэтому он будет развертываться вместе с задачей ***развертывания группы ресурсов Azure*** , как показано в следующем фрагменте кода:

```yaml
  - deployment: "Deploy_to_ADF"
    displayName: 'Deploy to ADF'
    timeoutInMinutes: 0
    environment: qa
    strategy:
      runOnce:
        deploy:
          steps:
            - task: AzureResourceGroupDeployment@2
              displayName: 'Deploy ADF resources'
              inputs:
                azureSubscription: $(AZURE_RM_CONNECTION)
                resourceGroupName: $(RESOURCE_GROUP)
                location: $(LOCATION)
                csmFile: '$(Pipeline.Workspace)/adf-pipelines/ARMTemplateForFactory.json'
                csmParametersFile: '$(Pipeline.Workspace)/adf-pipelines/ARMTemplateParametersForFactory.json'
                overrideParameters: -data-ingestion-pipeline_properties_variables_data_file_name_defaultValue "$(DATA_FILE_NAME)"
```
Значение параметра filename файла данных берется из переменной $ (DATA_FILE_NAME), определенной в группе переменных этапа контроля качества. Аналогично, все параметры, определенные в ***армтемплатефорфактори. JSON*** , можно переопределить. Если это не так, используются значения по умолчанию.

### <a name="run-the-pipeline-and-check-the-data-ingestion-result"></a>Запуск конвейера и проверка результата приема данных

Следующий шаг — убедиться, что развернутое решение работает. Следующее определение задания выполняет конвейер фабрики данных Azure с помощью [скрипта PowerShell](https://github.com/microsoft/DataOps/tree/master/adf/utils) и выполняет записную книжку Python в кластере Azure Databricks. Записная книжка проверяет, правильно ли были приняты данные, и проверяет файл данных результатов с помощью имени $ (bin_FILE_NAME).

```yaml
  - job: "Integration_test_job"
    displayName: "Integration test job"
    dependsOn: [Deploy_to_Databricks, Deploy_to_ADF]
    pool:
      vmImage: 'ubuntu-latest'
    timeoutInMinutes: 0
    steps:
    - task: AzurePowerShell@4
      displayName: 'Execute ADF Pipeline'
      inputs:
        azureSubscription: $(AZURE_RM_CONNECTION)
        ScriptPath: '$(Build.SourcesDirectory)/adf/utils/Invoke-ADFPipeline.ps1'
        ScriptArguments: '-ResourceGroupName $(RESOURCE_GROUP) -DataFactoryName $(DATA_FACTORY_NAME) -PipelineName $(PIPELINE_NAME)'
        azurePowerShellVersion: LatestVersion
    - task: UsePythonVersion@0
      inputs:
        versionSpec: '3.x'
        addToPath: true
        architecture: 'x64'
      displayName: 'Use Python3'

    - task: configuredatabricks@0
      inputs:
        url: '$(DATABRICKS_URL)'
        token: '$(DATABRICKS_TOKEN)'
      displayName: 'Configure Databricks CLI'    

    - task: executenotebook@0
      inputs:
        notebookPath: '/Shared/devops-ds/test-data-ingestion'
        existingClusterId: '$(DATABRICKS_CLUSTER_ID)'
        executionParams: '{"bin_file_name":"$(bin_FILE_NAME)"}'
      displayName: 'Test data ingestion'

    - task: waitexecution@0
      displayName: 'Wait until the testing is done'
```

Последняя задача в задании проверяет результат выполнения записной книжки. Если она возвращает ошибку, она устанавливает для состояния выполнения конвейера значение Failed.

## <a name="putting-pieces-together"></a>Совместное размещение элементов

Результатом этой статьи является конвейер Azure CI/CD, который состоит из следующих этапов:
* CI
* Развертывание в QA
    * Развертывание в модулях и развертывание в ADF
    * Интеграционный тест

Он содержит несколько этапов ***развертывания*** , равных количеству требуемых целевых сред. Каждый этап ***развертывания*** содержит два [развертывания](https://docs.microsoft.com/azure/devops/pipelines/process/deployment-jobs?view=azure-devops) , которые выполняются параллельно, и [Задание](https://docs.microsoft.com/azure/devops/pipelines/process/phases?view=azure-devops&tabs=yaml) , которое запускается после развертывания для тестирования решения в среде.

Пример реализации конвейера собираются в следующем фрагменте кода ***YAML*** :

```yaml
variables:
- group: devops-ds-vg

stages:
- stage: 'CI'
  displayName: 'CI'
  jobs:
  - job: "CI_Job"
    displayName: "CI Job"
    pool:
      vmImage: 'ubuntu-latest'
    timeoutInMinutes: 0
    steps:
    - task: UsePythonVersion@0
      inputs:
        versionSpec: '3.x'
        addToPath: true
        architecture: 'x64'
      displayName: 'Use Python3'
    - script: pip install --upgrade flake8 flake8_formatter_junit_xml
      displayName: 'Install flake8'
    - checkout: self
    - script: |
       flake8 --output-file=$(Build.BinariesDirectory)/lint-testresults.xml --format junit-xml  
    workingDirectory: '$(Build.SourcesDirectory)'
    displayName: 'Run flake8 (code style analysis)'  
    - script: |
       python -m pytest --junitxml=$(Build.BinariesDirectory)/unit-testresults.xml $(Build.SourcesDirectory)
    displayName: 'Run unit tests'
    - task: PublishTestResults@2
    condition: succeededOrFailed()
    inputs:
        testResultsFiles: '$(Build.BinariesDirectory)/*-testresults.xml'
        testRunTitle: 'Linting & Unit tests'
        failTaskOnFailedTests: true
    displayName: 'Publish linting and unit test results'    

    # The CI stage produces two artifacts (notebooks and ADF pipelines).
    # The pipelines Azure Resource Manager templates are stored in a technical branch "adf_publish"
    - publish: $(Build.SourcesDirectory)/$(Build.Repository.Name)/code/dataingestion
      artifact: di-notebooks
    - checkout: git://${{variables['System.TeamProject']}}@adf_publish    
    - publish: $(Build.SourcesDirectory)/$(Build.Repository.Name)/devops-ds-adf
      artifact: adf-pipelines

- stage: 'Deploy_to_QA'
  displayName: 'Deploy to QA'
  variables:
  - group: devops-ds-qa-vg
  jobs:
  - deployment: "Deploy_to_Databricks"
    displayName: 'Deploy to Databricks'
    timeoutInMinutes: 0
    environment: qa
    strategy:
      runOnce:
        deploy:
          steps:
            - task: UsePythonVersion@0
              inputs:
                versionSpec: '3.x'
                addToPath: true
                architecture: 'x64'
              displayName: 'Use Python3'

            - task: configuredatabricks@0
              inputs:
                url: '$(DATABRICKS_URL)'
                token: '$(DATABRICKS_TOKEN)'
              displayName: 'Configure Databricks CLI'    

            - task: deploynotebooks@0
              inputs:
                notebooksFolderPath: '$(Pipeline.Workspace)/di-notebooks'
                workspaceFolder: '/Shared/devops-ds'
              displayName: 'Deploy (copy) data processing notebook to the Databricks cluster'             
  - deployment: "Deploy_to_ADF"
    displayName: 'Deploy to ADF'
    timeoutInMinutes: 0
    environment: qa
    strategy:
      runOnce:
        deploy:
          steps:
            - task: AzureResourceGroupDeployment@2
              displayName: 'Deploy ADF resources'
              inputs:
                azureSubscription: $(AZURE_RM_CONNECTION)
                resourceGroupName: $(RESOURCE_GROUP)
                location: $(LOCATION)
                csmFile: '$(Pipeline.Workspace)/adf-pipelines/ARMTemplateForFactory.json'
                csmParametersFile: '$(Pipeline.Workspace)/adf-pipelines/ARMTemplateParametersForFactory.json'
                overrideParameters: -data-ingestion-pipeline_properties_variables_data_file_name_defaultValue "$(DATA_FILE_NAME)"
  - job: "Integration_test_job"
    displayName: "Integration test job"
    dependsOn: [Deploy_to_Databricks, Deploy_to_ADF]
    pool:
      vmImage: 'ubuntu-latest'
    timeoutInMinutes: 0
    steps:
    - task: AzurePowerShell@4
      displayName: 'Execute ADF Pipeline'
      inputs:
        azureSubscription: $(AZURE_RM_CONNECTION)
        ScriptPath: '$(Build.SourcesDirectory)/adf/utils/Invoke-ADFPipeline.ps1'
        ScriptArguments: '-ResourceGroupName $(RESOURCE_GROUP) -DataFactoryName $(DATA_FACTORY_NAME) -PipelineName $(PIPELINE_NAME)'
        azurePowerShellVersion: LatestVersion
    - task: UsePythonVersion@0
      inputs:
        versionSpec: '3.x'
        addToPath: true
        architecture: 'x64'
      displayName: 'Use Python3'

    - task: configuredatabricks@0
      inputs:
        url: '$(DATABRICKS_URL)'
        token: '$(DATABRICKS_TOKEN)'
      displayName: 'Configure Databricks CLI'    

    - task: executenotebook@0
      inputs:
        notebookPath: '/Shared/devops-ds/test-data-ingestion'
        existingClusterId: '$(DATABRICKS_CLUSTER_ID)'
        executionParams: '{"bin_file_name":"$(bin_FILE_NAME)"}'
      displayName: 'Test data ingestion'

    - task: waitexecution@0
      displayName: 'Wait until the testing is done'                

```

## <a name="next-steps"></a>Дальнейшие действия

* [Система управления версиями в фабрике данных Azure](https://docs.microsoft.com/azure/data-factory/source-control)
* [Непрерывная интеграция и доставка в фабрике данных Azure](https://docs.microsoft.com/azure/data-factory/continuous-integration-deployment)
* [DevOps для Azure Databricks](https://marketplace.visualstudio.com/items?itemName=riserrad.azdo-databricks)

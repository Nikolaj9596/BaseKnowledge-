#kubernetes  #ci/cd
# Добавляем объект Image Pull Policy GitLab

| Чтобы GitLab Runner мог деплоить приложение в кластер Kubernetes, 
| необходимо дать ему соответствующие права внутри конкретного namespace. 
| Для этого потребуется создать несколько RBAC-объектов:

```bash
# создать сервисную учетную запись (УЗ) командой:
kubectl create sa deploy -n $KUBE_NS

# предоставить ей права на создание объектов внутри namespace example_namespace

kubectl create rolebinding deploy \
  -n $KUBE_NS \
  --clusterrole edit \
  --serviceaccount $KUBE_NS:deploy
```
  
| Токен созданной УЗ необходимо прописать в GitLab-переменных. 
| С помощью этого токена GitLab Runner сможет получать доступ к кластеру Kubernetes. 
| Для получения токена воспользуйтесь командой:

```bash
kubectl get secret -n $KUBE_NS \
  $(kubectl get sa -n  $KUBE_NS deploy \
    -o jsonpath='{.secrets[].name}') \
  -o jsonpath='{.data.token}'
```

Создаем переменную `K8S_CI_TOKEN` и ее заполняем ее результатов выполнеения последнего действия (без `%` в конце строки):
file:/homepage/developers-wiki/devops/razvorachivaniedjangpproektanakubernetes/2022-09-2316-20.png

Скопируйте токен в буфер обмена и вернитесь в интерфейс GitLab. В пункте меню «Settings» — «CI/CD» нужно нажать на кнопку «Expand» в секции «Variables» и затем на кнопку «Add variable».

В открывшейся форме требуется указать имя переменной, которое будет впоследствии использоваться в секции deploy манифеста gitlab-ci.yml — K8S_CI_TOKEN. В качестве значения переменной нужно использовать скопированный ранее токен сервисной УЗ. Также следует снять флажок «Protect variable», который разрешает доступ к переменным исключительно из ветки master. И нужно установить флажок «Mask variable» — в таком случае переменная будет закрытой, то есть ее значение не будет отображаться внутри логов CI/CD.

Последнее, что необходимо сделать перед запуском деплоя, — добавить в Kubernetes возможность авторизации в GitLab, чтобы получать из Docker Registry формируемый образ и запускать на его основе приложение. Для этого нужно добавить Deploy token.

В GitLab необходимо выбрать пункт меню «Repository» и нажать на кнопку «Expand» в секции «Deploy tokens».

В открывшейся форме необходимо ввести имя токена `k8s-pull-token` и установить флажок «read_registry», который будет означать доступ к образам только на чтение.

Сформированные имя пользователя и пароль нужно скопировать в следующую команду, после чего выполнить ее в консоли. Команда добавит секрет с типом `docker-registry` и именем `example-image-pull`. С помощью секрета Kubernetes сможет получать формируемые образы из GitLab.

```bash
kubectl create secret docker-registry $KUBE_IMAGE_PULL \ 
  --docker-server registry.gitlab.com \
  --docker-email 'admin@mycompany.com' \
  --docker-username '<gitlab_token_name>' \
  --docker-password '<gitlab_token_password>' \
  --namespace $KUBE_NS
```

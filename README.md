# Домашнее задание к занятию «Базовые объекты K8S» - Морозов Александр

### Цель задания

В тестовой среде для работы с Kubernetes, установленной в предыдущем ДЗ, необходимо развернуть Pod с приложением и подключиться к нему со своего локального компьютера. 

------

### Чеклист готовности к домашнему заданию

1. Установленное k8s-решение (например, MicroK8S).
2. Установленный локальный kubectl.
3. Редактор YAML-файлов с подключенным Git-репозиторием.

------

### Инструменты и дополнительные материалы, которые пригодятся для выполнения задания

1. Описание [Pod](https://kubernetes.io/docs/concepts/workloads/pods/) и примеры манифестов.
2. Описание [Service](https://kubernetes.io/docs/concepts/services-networking/service/).

------

### Задание 1. Создать Pod с именем hello-world

1. Создать манифест (yaml-конфигурацию) Pod.
2. Использовать image - gcr.io/kubernetes-e2e-test-images/echoserver:2.2.
3. Подключиться локально к Pod с помощью `kubectl port-forward` и вывести значение (curl или в браузере).

Ответ:
1. Подготавливаем манифест hello-world.yaml
```
apiVersion: v1
kind: Pod
metadata:
  name: hello-world
spec:
  containers:
  - name: hello-world
    image: gcr.io/kubernetes-e2e-test-images/echoserver:2.2
    ports:
    - containerPort: 8080
```
2. Запускаем pod Hellow-world
![alt text](https://github.com/Mars12121/kuber-homeworks_1.2/blob/main/img/1.png)

3. Подключаемся локально к pod с помощью `kubectl port-forward` и проверяем доступность URL. Так как на ВМ не установлен GUI и IP K8S не опубликован в интернет, проверям на локальной ВМ через curl

![alt text](https://github.com/Mars12121/kuber-homeworks_1.2/blob/main/img/2.png)

![alt text](https://github.com/Mars12121/kuber-homeworks_1.2/blob/main/img/3.png)

------

### Задание 2. Создать Service и подключить его к Pod

1. Создать Pod с именем netology-web.
2. Использовать image — gcr.io/kubernetes-e2e-test-images/echoserver:2.2.
3. Создать Service с именем netology-svc и подключить к netology-web.
4. Подключиться локально к Service с помощью `kubectl port-forward` и вывести значение (curl или в браузере).

Ответ:
1. Создаем манифест Pod netology-web.yaml
```
apiVersion: v1
kind: Pod
metadata:
  name: netology-web
  labels:
    app: web
  namespace: default
spec:
  containers:
  - name: netology-web
    imagePullPolicy: IfNotPresent
    image: gcr.io/kubernetes-e2e-test-images/echoserver:2.2
    ports:
    - containerPort: 8080
```

2. Создаем манифест Service netology-svc.yaml
```
apiVersion: v1
kind: Service
metadata:
  name: netology-svc
spec:
  selector:
    app: web
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
```

3. Запускаем манифест Pod и Service
![alt text](https://github.com/Mars12121/kuber-homeworks_1.2/blob/main/img/4.png)

4. Подключаемся локально к pod с помощью `kubectl port-forward` и проверяем доступность URL. Так как на ВМ не установлен GUI и IP K8S не опубликован в интернет, проверям на локальной ВМ через curl
![alt text](https://github.com/Mars12121/kuber-homeworks_1.2/blob/main/img/5.png)
![alt text](https://github.com/Mars12121/kuber-homeworks_1.2/blob/main/img/6.png)

------

### Правила приёма работы

1. Домашняя работа оформляется в своем Git-репозитории в файле README.md. Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.
2. Файл README.md должен содержать скриншоты вывода команд `kubectl get pods`, а также скриншот результата подключения.
3. Репозиторий должен содержать файлы манифестов и ссылки на них в файле README.md.

------

### Критерии оценки
Зачёт — выполнены все задания, ответы даны в развернутой форме, приложены соответствующие скриншоты и файлы проекта, в выполненных заданиях нет противоречий и нарушения логики.

На доработку — задание выполнено частично или не выполнено, в логике выполнения заданий есть противоречия, существенные недостатки.
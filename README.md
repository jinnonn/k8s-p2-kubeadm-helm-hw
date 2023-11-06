# Домашнее задание к занятию «Kubernetes. Часть 2»
---

### Задание 1

**Выполните действия:**

1. Создайте свой кластер с помощью kubeadm.
1. Установите любой понравившийся CNI плагин.
1. Добейтесь стабильной работы кластера.

В качестве ответа пришлите скриншот результата выполнения команды `kubectl get po -n kube-system`.

---

### Решение 1

![task1](https://github.com/jinnonn/k8s-p2-kubeadm-helm-hw/blob/main/изображение_2023-11-06_012003529.png)

---

### Задание 2

Есть файл с деплоем:

```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis
spec:
  selector:
    matchLabels:
      app: redis
  replicas: 1
  template:
    metadata:
      labels:
        app: redis
    spec:
      containers:
      - name: master
        image: bitnami/redis
        env:
         - name: REDIS_PASSWORD
           value: password123
        ports:
        - containerPort: 6379
```
**Выполните действия:**

1. Создайте Helm Charts.
1. Добавьте в него сервис.
1. Вынесите все нужные, на ваш взгляд, параметры в `values.yaml`.
1. Запустите чарт в своём кластере и добейтесь его стабильной работы.

В качестве ответа пришлите вывод команды `helm get manifest <имя_релиза>`.

---

### Решение 2
![task2](https://github.com/jinnonn/k8s-p2-kubeadm-helm-hw/blob/main/изображение_2023-11-06_045407560.png)

---
## Дополнительные задания* (со звёздочкой)

Их выполнение необязательное и не влияет на получение зачёта по домашнему заданию. Можете их решить, если хотите лучше разобраться в материале.

---

### Задание 3*

1. Изучите [документацию](https://kubernetes.io/docs/concepts/storage/volumes/#hostpath) по подключению volume типа hostPath.
1. Дополните деплоймент в чарте подключением этого volume.
1. Запишите что-нибудь в файл на сервере, подключившись к поду с помощью `kubectl exec`, и проверьте правильность подключения volume.

В качестве ответа пришлите получившийся yaml-файл.

---

### Решение 3*

```
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ .Release.Name }}-hw
spec:
  selector:
    matchLabels:
      app: redis
  replicas: {{ .Values.replicas }}
  template:
    metadata:
      labels:
        app: redis
    spec:
      containers:
      - name: {{ .Release.Name }}-master
        image: bitnami/redis
        env:
         - name: {{ .Values.name_of_password | quote }}
           value: {{ .Values.value_of_password | quote }}
        volumeMounts:
        - mountPath: /task3-netology
          name: task3-volume
        ports:
        - containerPort: {{ .Values.port }}
      volumes:
      - name: task3-volume
        hostPath:
          path: /task3
          type: DirectoryOrCreate
```

### Домашнее задание к занятию «Сетевое взаимодействие в K8S. Часть 2»
## Задание 1. Создать Deployment приложений backend и frontend
1. Создать Deployment приложения frontend из образа nginx с количеством реплик 3 шт.
2. Создать Deployment приложения backend из образа multitool.
3. Добавить Service, которые обеспечат доступ к обоим приложениям внутри кластера.
4. Продемонстрировать, что приложения видят друг друга с помощью Service.
5. Предоставить манифесты Deployment и Service в решении, а также скриншоты или вывод команды п.4.
## Задание 2. Создать Ingress и обеспечить доступ к приложениям снаружи кластера
1. Включить Ingress-controller в MicroK8S.
2. Создать Ingress, обеспечивающий доступ снаружи по IP-адресу кластера MicroK8S так, чтобы при запросе только по адресу открывался frontend а при добавлении /api - backend.
3. Продемонстрировать доступ с помощью браузера или curl с локального компьютера.
4. Предоставить манифесты и скриншоты или вывод команды п.2.

## Решение задания 1. Создать Deployment приложений backend и frontend
Для выполнения задания создаем отдельный Namespace. Пишем манифест Deployment приложения frontend из образа nginx с количеством реплик 3 шт:

![img1_1](https://github.com/user-attachments/assets/7183733d-430f-4443-aafa-c41c8b8d7ab2)

Пишем манифест Deployment приложения backend из образа multitool:

![img1_2](https://github.com/user-attachments/assets/a63bc827-5c6b-4445-bdd1-197f49ab0a1d)

Проверим созданные поды:

![img1_3](https://github.com/user-attachments/assets/58afac2a-742a-4a79-9f1e-f328dbe3ff7d)

Поды созданы, количество реплик соответствуют заданию.

Пишем манифест Service, который обеспечит доступ к обоим приложениям внутри кластера:

![img1_4](https://github.com/user-attachments/assets/1b598e35-1941-4121-ad9a-a8f288554123)

Так как имена приложений в Deployments разные, для связи сервиса с деплойментами буду использовать selector типа component.

Применяем сервис и проверяем его состояние:

![img1_4](https://github.com/user-attachments/assets/198c9e84-320b-4543-8f7c-6835bb01895f)

Используя curl, проверим, видят ли приложения друг друга через созданный сервис из пода backend:

![img1_5](https://github.com/user-attachments/assets/13369f92-d86d-45f6-a40c-ec2572727d56)

![img1_6](https://github.com/user-attachments/assets/14f9d7c9-22a2-4248-9128-629c4d85637b)

Приложения видят друг друга.

Ссылка на манифест Deployment c frontend - https://github.com/slava1005/1_5/blob/main/manifest/deploy_front.yaml

Ссылка на манифест Deployment c frontend - https://github.com/slava1005/1_5/blob/main/manifest/deploy_back.yaml

Ссылка на манифест Service - https://github.com/slava1005/1_5/blob/main/manifest/service.yaml

## Решение задания 2. Создать Ingress и обеспечить доступ к приложениям снаружи кластера

Включим Ingress-controller в MicroK8S. Проверим его состояние:

![img1_8](https://github.com/user-attachments/assets/112789d4-5471-4e31-ac46-03f66e024764)

Ingress-controller запущен.

Пишем манифест Ingress, обеспечивающий доступ снаружи по IP-адресу кластера MicroK8S так, чтобы при запросе только по адресу открывался frontend а при добавлении /api - backend.
Применим манифест и проверим результат:

![img1_9](https://github.com/user-attachments/assets/3cfd05d7-2b84-419c-9aae-b80495d2918e)

Ingress создан.

Для проверки доступа с помощью браузера или curl с локального компьютера, добавим в DNS соответствующую запись так, чтобы примененный в Ingress адрес myingress.com ссылался на IP адрес кластера MicroK8S.
Проверим доступ к приложениям через Ingress:

![img1_10_1_cr](https://github.com/user-attachments/assets/1388415e-fd92-4c30-b764-8edd30998829)

При обращении к ```http://myingress.com/``` получаем ответ от Nginx, при обращении к ```http://myingress.com/api``` получаем ответ от Multitool.

Ссылка на манифест Ingress - https://github.com/slava1005/1_5/blob/main/manifest/ingress.yaml

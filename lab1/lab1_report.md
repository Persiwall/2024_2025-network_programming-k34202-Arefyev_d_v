# Лабораторная работа №1 "Установка CHR и Ansible, настройка VPN"
---
University: [ITMO University](https://itmo.ru/ru/)

Faculty: [FICT](https://fict.itmo.ru)

Course: [Network programming](https://github.com/itmo-ict-faculty/network-programming)

Year: 2024/2025

Group: K34202

Author: Arefyev Dmitriy Vladimirovich

Lab: Lab1

Date of create: 28.09.2024

Date of finished: 30.09.2024

---

## Цель работы

Целью данной работы является развертывание виртуальной машины на базе платформы Microsoft Azure с установленной системой контроля конфигураций Ansible и установка CHR в VirtualBox

## Ход работы

### Создание виртуальной машины в Yandex Cloud

Был создан аккаунт в Yandex Cloud

![image](https://github.com/Persiwall/2024_2025-network_programming-k34202-Arefyev_d_v/blob/main/lab1/pictures/yandex_cloud.png "Основная страница Yandex Compute Cloud")

Была выбрана конфигурация с минимально возможными характеристиками. Для того, чтобы завершить создание машины, потребовалось сгенерировать на локальной машине ssh-ключ

![](https://github.com/Persiwall/2024_2025-network_programming-k34202-Arefyev_d_v/blob/main/lab1/pictures/Снимок%20экрана%20от%202024-09-28%2015-18-01.png "Поле для введения ключа")

После этого виртуальная машина была успешно создана и запущена

![](https://github.com/Persiwall/2024_2025-network_programming-k34202-Arefyev_d_v/blob/main/lab1/pictures/Снимок%20экрана%20от%202024-09-28%2015-23-50.png "Созданная виртульная машина")

### Создание локальной виртуальной машины с CHR

После того, как была поднята виртуальная машина в облаке, я занялся локальной виртуальной машиной на VirtualBox. 

Для начала в VirtualBox начнём создание машины на Linux, выбрав подтип Other Linux

![](https://github.com/Persiwall/2024_2025-network_programming-k34202-Arefyev_d_v/blob/main/lab1/pictures/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%20%D0%BE%D1%82%202024-09-28%2014-40-51.png "Создание виртуалки")

Далее при выборе виртуального жесткого диска укажем использование уже существующего диска, а конкретно - vdi образа CHR, скаченного с официального сайта Mikrotik

![](https://github.com/Persiwall/2024_2025-network_programming-k34202-Arefyev_d_v/blob/main/lab1/pictures/Снимок%20экрана%20от%202024-09-28%2014-42-29.png "Выбор диска")

Конфигурация машины была закончена, но при попытке запуска я столкнулся с ошибкой драйвера ядра VirtualBox

![](https://github.com/Persiwall/2024_2025-network_programming-k34202-Arefyev_d_v/blob/main/lab1/pictures/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%20%D0%BE%D1%82%202024-09-28%2014-52-33.png "Ошибка драйева")

Чтобы решить проблему, я вручную переустановил модули VirtualBox следующими командами:

```
sudo apt update
sudo apt install --reinstall virtualbox-dkms virtualbox
```

Это помогло, теперь машина запускалась, но сразу же вылетала, выдавая ошибку "Implementation of the USB 2.0, controller not found". Я решил проблему ручным отключением поддержки USB 2.0 в настройках виртуальной машины

![](https://github.com/Persiwall/2024_2025-network_programming-k34202-Arefyev_d_v/blob/main/lab1/pictures/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%20%D0%BE%D1%82%202024-09-28%2015-07-13.png "Отключение USB")

После этого машина наконец-то успешно заработала

![](https://github.com/Persiwall/2024_2025-network_programming-k34202-Arefyev_d_v/blob/main/lab1/pictures/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%20%D0%BE%D1%82%202024-09-28%2015-09-43.png "Успешный запуск локальной ВМ")

### Установка OpenVPN-AS на облачную ВМ

После успешного создания локальной ВМ, я вернулся к облачной ВМ, чтобы создать на ней Open VPN сервер (я выбрал OpenVPN, пойдя по пути наименшего сопротивления, не желая сталкиваться с проблемами Wireguard в РФ)

Итак, сначала я со своего рабочего устройства подключился к ВМ через ssh

![](https://github.com/Persiwall/2024_2025-network_programming-k34202-Arefyev_d_v/blob/main/lab1/pictures/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%20%D0%BE%D1%82%202024-09-28%2015-28-39.png "Подключение к ВМ")

Далее на ВМ был установлен Python и Ansible 

![](https://github.com/Persiwall/2024_2025-network_programming-k34202-Arefyev_d_v/blob/main/lab1/pictures/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%20%D0%BE%D1%82%202024-09-28%2015-33-41.png "Установленный Ansible")

Перед началом установки OpenVPN сервера я настроил дату и время, потому что это рекомендовали сделать на официальном сайте OpenVPN, на инструкцию с которого я оглядывался при выполнении работы

![](https://github.com/Persiwall/2024_2025-network_programming-k34202-Arefyev_d_v/blob/main/lab1/pictures/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%20%D0%BE%D1%82%202024-09-28%2015-57-26.png "Настройка даты и времени")

Далее набором команд с вышеописанного сайта был создан VPN сервер

```
apt update && apt -y install ca-certificates wget net-tools gnupg
wget https://as-repository.openvpn.net/as-repo-public.asc -qO /etc/apt/trusted.gpg.d/as-repository.asc
echo "deb [arch=amd64 signed-by=/etc/apt/trusted.gpg.d/as-repository.asc] http://as-repository.openvpn.net/as/debian jammy main">/etc/apt/sources.list.d/openvpn-as-repo.list
apt update && apt -y install openvpn-as
```

![](https://github.com/Persiwall/2024_2025-network_programming-k34202-Arefyev_d_v/blob/main/lab1/pictures/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%20%D0%BE%D1%82%202024-09-28%2016-36-51.png "Создание сервера")

Для дальнейшей настройки я посмотрел внешний IP-адрес своей облачной ВМ и перешёл по ссылке: https://84.201.148.232:943/admin , зайдя в систему под логином openvpn и паролем, который был указан при создания сервера

![](https://github.com/Persiwall/2024_2025-network_programming-k34202-Arefyev_d_v/blob/main/lab1/pictures/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%20%D0%BE%D1%82%202024-09-28%2016-47-55.png "Настройка сервера")

В протоколах был выбрал только TCP, а номер порта изменен на 443

![](https://github.com/Persiwall/2024_2025-network_programming-k34202-Arefyev_d_v/blob/main/lab1/pictures/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%20%D0%BE%D1%82%202024-09-28%2016-50-21.png "Дальнейшая настройка сервера")

В настройках VPN отключаем TLS

![image](https://github.com/Persiwall/2024_2025-network_programming-k34202-Arefyev_d_v/blob/main/lab1/pictures/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%20%D0%BE%D1%82%202024-09-28%2016-54-26.png "TLS")


Далее был создан профиль Persiwall с возможностью свободного подключения(скриншот почему-то забыл сделать)
https://github.com/Persiwall/2024_2025-network_programming-k34202-Arefyev_d_v/blob/main/lab1/pictures/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%20%D0%BE%D1%82%202024-09-28%2017-54-08.png
### Подключение локальной ВМ к серверу

Была скачена программа WibBox. 

![](https://github.com/Persiwall/2024_2025-network_programming-k34202-Arefyev_d_v/blob/main/lab1/pictures/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%20%D0%BE%D1%82%202024-09-28%2017-00-09.png "WinBox")

В ней, зайдя в раздел neighbours, была выбрана нужна локальная ВМ. В настойках самой ВМ тип подключения был изменён с NAT на Сетевой мост

![](https://github.com/Persiwall/2024_2025-network_programming-k34202-Arefyev_d_v/blob/main/lab1/pictures/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%20%D0%BE%D1%82%202024-09-28%2017-02-38.png "Настройка сети")

После этого подключение через WinBox прошло успешно

![](https://github.com/Persiwall/2024_2025-network_programming-k34202-Arefyev_d_v/blob/main/lab1/pictures/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%20%D0%BE%D1%82%202024-09-28%2017-05-58.png "Подключение к ВМ через WinBox")

После создания профиля на сайте openvpn-as был скачен файл с сертификатами. Этот файл был загружен в локальную ВМ через WinBox, после чего был произведён импорт самих сертификатов

![](https://github.com/Persiwall/2024_2025-network_programming-k34202-Arefyev_d_v/blob/main/lab1/pictures/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%20%D0%BE%D1%82%202024-09-28%2017-19-00.png "Импорт сертификатов")

В настройках интерфейсов был создан новый интерфейс, в котором был указан публичный адрес сервера, порт 443 и юзернейм persiwall 

![](https://github.com/Persiwall/2024_2025-network_programming-k34202-Arefyev_d_v/blob/main/lab1/pictures/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%20%D0%BE%D1%82%202024-09-28%2017-24-35.png "Создание интерфейса")

# Вывод

В результате выполнения работы я научился создавать ВМ в облаке и локальные ВМ на CHR, а также познакомился с функионалом WinBox и настройкой openvpn-as

Тоннель успешно работает. Докажем это пингом на сервер с локальной ВМ

![](https://github.com/Persiwall/2024_2025-network_programming-k34202-Arefyev_d_v/blob/main/lab1/pictures/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%20%D0%BE%D1%82%202024-09-28%2017-54-08.png "Пинг сервера")

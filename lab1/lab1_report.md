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

![](https://github.com/Persiwall/2024_2025-network_programming-k34202-Arefyev_d_v/blob/main/lab1/pictures/yandex_cloud.png "Основная страница Yandex Compute Cloud")

Была выбрана конфигурация с минимально возможными характеристиками. Для того, чтобы завершить создание машины, потребовалось сгенерировать на локальной машине ssh-ключ

![](https://github.com/Persiwall/2024_2025-network_programming-k34202-Arefyev_d_v/blob/main/lab1/pictures/Снимок%20экрана%20от%202024-09-28%2015-18-01.png "Поле для введения ключа")

После этого виртуальная машина была успешно создана и запущена

![](https://github.com/Persiwall/2024_2025-network_programming-k34202-Arefyev_d_v/blob/main/lab1/pictures/Снимок%20экрана%20от%202024-09-28%2015-23-50.png "Созданная виртульная машина")

### Создание локальной виртуальной машины с CHR

После создания виртуальной машины в облаке, 

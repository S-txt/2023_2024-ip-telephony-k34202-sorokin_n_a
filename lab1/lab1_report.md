#### University: [ITMO University](https://itmo.ru/ru/)
#### Faculty: [FICT](https://fict.itmo.ru)
#### Course: [IP-telephony](https://github.com/itmo-ict-faculty/ip-telephony)
#### Year: 2023/2024
#### Group: K34202
#### Author: Sorokin Nikita
#### Lab: Lab1
#### Date of create: 25.02.2024
#### Date of finished: __.02.2024

# Лабораторная работа №1
# "Базовая настройка IP-телефонов в среде Сisco packet tracer"

**Цель работы** - изучить рабочую среду Cisco Packet Tracer, ознакомиться с интерфейсами основных устройств, типами кабелей, научиться собирать топологию; изучить построение сети IP-телефонии с помощью маршрутизатора, коммутатора и IP телефонов Cisco 7960 в среде Packet tracer.

**Ход работы:**

## Часть 1

В Cisco Packet Tracer была собрана следующая схема.

![Схема часть 1](https://github.com/s-txt/2023_2024-ip-telephony-k34202-sorokin_n_a/blob/main/lab1/src/1.png)

Всем устройствам в сети был присвоен ip адрес в диапазоне 192.168.10.1-10

![Связь 1 -> 7](https://github.com/s-txt/2023_2024-ip-telephony-k34202-sorokin_n_a/blob/main/lab1/src/2.png)

## Часть 2.

Была собрана схема #2

![Схема часть 2](https://github.com/s-txt/2023_2024-ip-telephony-k34202-sorokin_n_a/blob/main/lab1/src/3.png)

### Настройка маршрутизатора
1. Изменим имя маршрутизатора
```
hostname CMERoter
```
2. Настроим интерфейс Fast Ethernet

```
int fa0/0.1
ip address 192.168.1.1 255.255.255.0
no shutdown
```

3. Настроим DHCP для голоса и данных.
```
ip dhcp pool ipphones
network 192.168.1.0 255.255.255.0
default-router 192.168.1.1
option 150 ip 192.168.1.1
```
5. Настроим сервисы телефонии.

```
telephony-service 
max-dn
max-dn 10
max-ephones 10
ip source-address 192.168.0.1 port 3100
auto assign 1 to 19
```

```
ephone-dn 1
number 001
ephone-dn 2
number 002
```


### Настройка коммутатора

1. Настроим vlan

```
ephone-dn 2
number 002
```

### Тестирование

После проведение настроек и поключения питания к телефонам, они получат адрес и можно будет провести звонок:


![Тест звона](https://github.com/s-txt/2023_2024-ip-telephony-k34202-sorokin_n_a/blob/main/lab1/src/4.png)

## Вывод 
Была изучена среда Cisco Packet Tracer, построены и настроены две схемы связи. Была построена и проверена сеть IP-телефонии.

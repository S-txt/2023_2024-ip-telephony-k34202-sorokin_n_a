#### University: [ITMO University](https://itmo.ru/ru/)
#### Faculty: [FICT](https://fict.itmo.ru)
#### Course: [IP-telephony](https://github.com/itmo-ict-faculty/ip-telephony)
#### Year: 2023/2024
#### Group: K34202
#### Author: Sorokin Nikita
#### Lab: Lab2
#### Date of create: 25.02.2024
#### Date of finished: __.02.2024

# Лабораторная работа №2
# "Конфигурация voip в среде Сisco packet tracer"

**Цель работы** - изучить построение сети IP-телефонии с помощью маршрутизатора Cisco 2811, коммутатора Cisco catalyst 3560 и IP телефонов Cisco 7960.

## **Ход работы:**

### Часть 1

В Cisco Packet Tracer была собрана следующая схема.

![Схема часть 1](https://github.com/s-txt/2023_2024-ip-telephony-k34202-sorokin_n_a/blob/main/lab2/src/1.png)

### Настройка маршрутизатора
1. Изменим имя маршрутизатора
```
hostname CMERoter
```
2. Отключим DNS и настроим авторизацию
```
no ip domain-lookup
line vty 0 4
password 12345
login
logging synchronous
exit
line console 0
password 12345
login
logging synchronous
exit
```

3. Настроим интерфейс FastEthernet.
```
int fa0/0
no shutdown
ip address 192.168.1.1 255.255.255.0
```
4. Настроим DHCP
```
ip dhcp pool ipphones
network 192.168.1.0 255.255.255.0
default-router 192.168.1.1
option 150 ip 192.168.1.1
```

5. Настроим сервисы телефонии.

```
telephony-service
max-dn 5
max-ephones 5
ip source-address 192.168.10.1 port 2000
auto assign 1 to 5

```

```
ephone-dn 1
number 101
exit
ephone-dn 2
number 102
exit
ephone-dn 3
number 103
exit
```


### Настройка коммутатора

1. Настроим vlan

```
interface range FastEthernet 0/1-4
switchport mode access
switchport voice vlan 1
```

### Тестирование

После проведение настроек и поключения питания к телефонам, они получат адрес и можно будет провести звонок:


![Тест звона](https://github.com/s-txt/2023_2024-ip-telephony-k34202-sorokin_n_a/blob/main/lab2/src/2.png)

### Часть 2

Была дополнена схема:

![Схема часть 2](https://github.com/s-txt/2023_2024-ip-telephony-k34202-sorokin_n_a/blob/main/lab2/src/3.png)

### Настройка маршрутизатора
1. Изменим DHCP и настроим интерфейсы
```
ip dhcp pool data
network 192.168.10.0 255.255.255.0
default-router 192.168.10.1
exit
ip dhcp pool voice
network 192.168.20.0 255.255.255.0
default-router 192.168.20.1
option 150 ip 192.168.20.1
exit
int fa0/0.10
ip add 192.168.10.1 255.255.255.0
encapsulation dot1Q 10
ip add 192.168.10.1 255.255.255.0
no shutdown
exit
int fa0/0.20
encapsulation dot1Q 20
ip add 192.168.20.1 255.255.255.0
no shutdown
exit
```
### Настройка коммутатора
1. Созадим vlan и настроим интерфейсы
```
vlan 10
name data
exit
vlan 20
name voice
exit
int fa0/1
switchport mode trunk
exit
interface range fa0/2-4
switchport mode access
switchport access vlan 10
switchport voice vlan 20
```

### Тестирование

Проверим что подключенные компьютеры получили ip-адрес через dhcp

![Тест DHCP](https://github.com/s-txt/2023_2024-ip-telephony-k34202-sorokin_n_a/blob/main/lab2/src/4.png)

Проверим связность компьютеров

![Тест ping](https://github.com/s-txt/2023_2024-ip-telephony-k34202-sorokin_n_a/blob/main/lab2/src/5.png)

Попробуем провести звонок

![Тест звонка](https://github.com/s-txt/2023_2024-ip-telephony-k34202-sorokin_n_a/blob/main/lab2/src/6.png)

## Вывод 
Были получены навыки построения сети IP-телефонии и были реализованы две схемы связи - с использованием исключительно ip телефонов и с использованием компьютеров и ip телефонов.



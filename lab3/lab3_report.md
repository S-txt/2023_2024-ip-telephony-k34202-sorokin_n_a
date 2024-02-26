#### University: [ITMO University](https://itmo.ru/ru/)
#### Faculty: [FICT](https://fict.itmo.ru)
#### Course: [IP-telephony](https://github.com/itmo-ict-faculty/ip-telephony)
#### Year: 2023/2024
#### Group: K34202
#### Author: Sorokin Nikita
#### Lab: Lab2
#### Date of create: 26.02.2024
#### Date of finished: __.02.2024

# Лабораторная работа №3
# "Использование Asterisk в качестве SIP proxy"

**Цель работы** - Изучить программный комплекс Asterisk. Настройка Asterisk для локальных звонков.

##**Ход работы:**

### Конфигурация Asterisk
1. Установим Asterisk
```
sudo apt install asterisk
```
2. Настроим файлы конфигураций

**sip.conf**
```
[general]
context=internal
allowguest=no
allowoverlap=no
bindport=5060
bindaddr=0.0.0.0
srvlookup=no
disallow=all
allow=ulaw
alwaysauthreject=yes
canreinvite=no
nat=yes
session-timers=refuse
localnet=192.168.31.0/255.255.255.0

[1001]
type=friend
host=dynamic
secret=1001
context=internal

[1002]
type=friend
host=dynamic
secret=1002
context=internal
```

**extensions.conf**
```
exten => 1001,1,Answer()
exten => 1001,2,Dial(SIP/1001,60)
exten => 1001,3,Playback(vm-nobodyavail)
exten => 1001,4,VoiceMail(1001@main)
exten => 1001,5,Hangup()

exten => 1002,1,Answer()
exten => 1002,2,Dial(SIP/1002,60)
exten => 1002,3,Playback(vm-nobodyavail)
exten => 1002,4,VoiceMail(1002@main)
exten => 1002,5,Hangup()

exten => 2001,1,VoicemailMain(1001@main)
exten => 2001,2,Hangup()

exten => 2002,1,VoicemailMain(1002@main)
exten => 2002,2,Hangup()
```

**voicemail.conf**
```
1001 => 1001
1002 => 1002
```

2. Зайдем в CLI asterisk и перезагрузим его
```
sudo astrisk -r
relad
```
3. Проверим созданный подключения:
```
sip show peer
```

На текущий момент в списке не должно быть активных подключений поскольку мы только зарешистрировали пользователей

### Zoiper

В качетсве soft телефона был выбран zoiper

1. Скачаем и установим zoiper с оффициального сайта
2. После запуска конфигурируем новый аккаунт:
username: 1001
password: 1001
server: <current pc ip>

После проверки доступности у нас появиться новый настроенный аккаунт подключенный к sip серверу

![Подключенный аккаунт](https://github.com/s-txt/2023_2024-ip-telephony-k34202-sorokin_n_a/blob/main/lab3/src/1.png)

Проверим в CLI Asterisk подключенного пользователя

![Активное подключение](https://github.com/s-txt/2023_2024-ip-telephony-k34202-sorokin_n_a/blob/main/lab3/src/2.png)

Проведя аналогичные действия на другом устройстве в той же локальной сети, можно будет провести звонок.

##Вывод 
Были получены навыки в конфигурации SIP сервера в Asterisk и подключение SIP клиентов в Zoiper 


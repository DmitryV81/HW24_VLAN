Vagrant-стенд c VLAN и LACP

Описание домашнего задания

в Office1 в тестовой подсети появляется сервера с доп интерфесами и адресами

в internal сети testLAN: 

- testClient1 - 10.10.10.254

- testClient2 - 10.10.10.254

- testServer1- 10.10.10.1 

- testServer2- 10.10.10.1

Равести вланами:

testClient1 <-> testServer1

testClient2 <-> testServer2

Между centralRouter и inetRouter "пробросить" 2 линка (общая inernal сеть) и объединить их в бонд, проверить работу c отключением интерфейсов

Ход работы:

В Vagrantfile описано создание 7 виртуальных машин (5 на CentOS 8 Stream, 2 на Ubuntu 20.04)

Схема стенда:

![Stend Scheme](https://github.com/DmitryV81/HW24_VLAN/blob/main/screenshot/structure.png)

С помощью ansible конфигурируем сеть ВМ testClient1, testClient2, testServer1, testServer2

Проверяем:

```
[vagrant@testClient1 ~]$ ping 10.10.10.1
PING 10.10.10.1 (10.10.10.1) 56(84) bytes of data.
64 bytes from 10.10.10.1: icmp_seq=1 ttl=64 time=0.660 ms
64 bytes from 10.10.10.1: icmp_seq=2 ttl=64 time=0.712 ms
64 bytes from 10.10.10.1: icmp_seq=3 ttl=64 time=0.703 ms
```

```
vagrant@testClient2:~$ ping 10.10.10.1
PING 10.10.10.1 (10.10.10.1) 56(84) bytes of data.
64 bytes from 10.10.10.1: icmp_seq=1 ttl=64 time=1.21 ms
64 bytes from 10.10.10.1: icmp_seq=2 ttl=64 time=0.701 ms
64 bytes from 10.10.10.1: icmp_seq=3 ttl=64 time=0.724 ms
64 bytes from 10.10.10.1: icmp_seq=4 ttl=64 time=0.678 ms
```

testClient1 "видит" testServer1

testClient2 "видит" testServer2

Далее с ansible настраиваем бондинг между inetRouter и centralRouter

При запущенном ping с inetRouter до centralRouter отключаем один из интерфейсов в бондинге (интерфейс eth1) на centralRouter, ping не пропадает, связь не потеряна.

![Test LACP](https://github.com/DmitryV81/HW24_VLAN/blob/main/screenshot/screenshot.png)

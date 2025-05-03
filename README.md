# Домашнее задание:  VLAN'ы,LACP_Строим бонды и вланы


Vagrantfile создает 7 VM: 5 VM на centos/stream9 v.20250331.0 и 2 VM на Ubuntu 22.04.

Playbook provision.yml устанавливает на VM необхоимай софт.
Дальнейшая настрока VM для выполнения домашнего задания произодится с помощью ansible-файла vlan_workbook.yml


Проверка создания VLAN1:

![Image alt](https://github.com/AlexndrVakulenko/homework25/blob/main/01_check_VLAN1.png)

Проверка создания VLAN2:

![Image alt](https://github.com/AlexndrVakulenko/homework25/blob/main/02_check_VLAN2.png)

Проверка создания bond -интерфеса между inetRouter и centralRouter:

![Image alt](https://github.com/AlexndrVakulenko/homework25/blob/main/03_check_ping_bond0.png)

В районе выделенного ping был отключен инрейфейс eth1 на centralRouter командой
*ip link set down eth1*

Как видно, ping не прервался

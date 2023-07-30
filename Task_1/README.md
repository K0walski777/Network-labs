# Задание 1

## Исходные данные

В здании три этажа. На каждом этаже располагается по 3 компьютера. В целях экономии были установлены только коммутаторы уровня доступа, на каждом этаже. Для отказоустойчивости сеть "закольцована", чтобы в случае неисправности одного из линков связь восстанавливалась.

Схема сети

![Исходная схема сети](https://github.com/K0walski777/Network-labs/blob/4308cdc547249ebb59af4baa98484f8e4a4fb103/Task_1/img/task1_img1.png)

Задача
1. Реализовать схему в Cisco Packet Tracer.
2. Сегментировать сеть на три VLAN-а (2-й, 3-й и 4-й). Компьютеры одного VLAN-а с разных этажей должны взаимодействовать друг с другом (т.е. должен быть успешный ping).
3. PC1.1 может осуществить успешный ping до PC2.1 и PC3.1.
4. PC1.2 может осуществить успешный ping до PC2.2 и PC3.2.
5. PC1.3 может осуществить успешный ping до PC2.3 и PC3.3.
6. При отключении любого линка между коммутаторами связь должна восстанавливаться.

## Решение

### Схема сети

Реализованная схема сети в Cisco Packet Tracer.

![Реализованная схема сети](https://github.com/K0walski777/Network-labs/blob/4308cdc547249ebb59af4baa98484f8e4a4fb103/Task_1/img/task1_img2.png)

### Настройка коммутаторов

Для всех трех коммутаторов настройки идентичны:

```sh
Switch> enable
Switch# configure terminal
Switch(config)# vlan 2
Switch(config-vlan)# name PC-x.1
Switch(config-vlan)# exit
Switch(config)# vlan 3
Switch(config-vlan)# name PC-x.2
Switch(config-vlan)# exit
Switch(config)# vlan 4
Switch(config-vlan)# name PC-x.3
Switch(config-vlan)# exit
Switch(config)# interface FastEthernet0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 2
Switch(config-if)# description PC2.1
Switch(config-if)# exit
Switch(config)# interface FastEthernet0/2
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 3
Switch(config-if)# description PC2.2
Switch(config-if)# exit
Switch(config)# interface FastEthernet0/3
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 4
Switch(config-if)# description PC2.3
Switch(config-if)# exit
Switch(config)# interface GigabitEthernet0/1
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport trunk allowed vlan 2-4
Switch(config-if)# exit
Switch(config)# interface GigabitEthernet0/2
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport trunk allowed vlan 2-4
Switch(config-if)# exit
```
В описании интерфесов изменяется только номер компьютера, подключенныый к соответствующему порту на коммутаторе.
При желании можно изменить ```hostname``` каждого коммутатора. Для коммутатора, расположенном на первом этаже, установлено имя ```1-layer```, коммутатор на втором этаже получил имя ```2-layer```, коммутатор на третьем этаже соответственно ```3-layer```.

```sh
Switch(config)# hostname x-layer
```
где ```x``` - номер этажа, на котором установлен коммутатор.

Заключительным этапом настройки коммутаторов является сохранение конфигурации:

```sh
1-layer# copy running-config startup-config
```

### Настройка IP-адресации на компьютерах

После настройки коммутаторов, необходимо настроить IP-адресацию на компьютерах.

| Имя компьютера | IP-адрес | Маска подсети |
| ------ | ------ | ------ |
| PC1.1 | 192.168.0.10 | 255.255.255.0 |
| PC1.2 | 192.168.0.11 | 255.255.255.0 |
| PC1.3 | 192.168.0.12 | 255.255.255.0 |
| PC2.1 | 192.168.0.13 | 255.255.255.0 |
| PC2.2 | 192.168.0.14 | 255.255.255.0 |
| PC2.3 | 192.168.0.15 | 255.255.255.0 |
| PC3.1 | 192.168.0.16 | 255.255.255.0 |
| PC3.2 | 192.168.0.17 | 255.255.255.0 |
| PC3.3 | 192.168.0.18 | 255.255.255.0 |

### Проверка доступности узлов

Компьютеры, входящие в один VLAN успешно осуществляют ping между собой. В свою очередь ping не успешен при попытке "пропинговать" компьютер, находящийся в другом VLAN.

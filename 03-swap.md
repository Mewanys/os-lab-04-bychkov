# Задание 3. Настройка подкачки

1. `free -h` и `swapon --show` - использование RAM и swap и список активных swap-устройств.
```
               total        used        free      shared  buff/cache   available
Mem:           3,8Gi       498Mi       3,1Gi       760Ki       435Mi       3,3Gi
Swap:          1,1Gi          0B       1,1Gi
```
уже есть swap-раздел /dev/sda5 на 1.1G
```
NAME      TYPE      SIZE USED PRIO
/dev/sda5 partition 1,1G   0B   -2
```
приоритет -2, не используется.

2. `sudo fallocate -l 512M /swapfile`
`sudo chmod 600 /swapfile`
`sudo mkswap /swapfile`
`sudo swapon /swapfile` - создание файла подкачки 512М, права только root, разметка как `swap`, и включение.

```
user@debian:~$ sudo swapon --show
NAME      TYPE      SIZE USED PRIO
/dev/sda5 partition 1,1G   0B   -2
/swapfile file      512M   0B   -3
user@debian:~$ free -h
               total        used        free      shared  buff/cache   available
Mem:           3,8Gi       495Mi       3,1Gi       760Ki       436Mi       3,3Gi
Swap:          1,6Gi          0B       1,6Gi
```
теперь два swap-устройства. Суммарный swap вырос с 1.1Gi до 1.6Gi. Приоритет файла -3.


3. `cat /proc/sys/vm/swappiness` и `sudo sysctl vm.swappiness=10` - просмотр текущего swappiness и понижаем его до 10.
```
user@debian:~$ cat /proc/sys/vm/swappiness 
60
user@debian:~$ sudo sysctl vm.swappiness=10
vm.swappiness = 10
user@debian:~$ cat /proc/sys/vm/swappiness
10
```
Значение изменилось с 60 на 10, до перезагрузки (для постоянного в /etc/sysctl.d/)


4. `sudo swapoff /swapfile` и `sudo rm -f /swapfile` - отключение и удаление файла подкачки.
```
               total        used        free      shared  buff/cache   available
Mem:           3,8Gi       494Mi       3,1Gi       760Ki       436Mi       3,3Gi
Swap:          1,1Gi          0B       1,1Gi
```
swap вернулся к 1.1Gi


### В отчёт ( 03-swap.md ):
что делает каждая из команд fallocate , mkswap , swapon:
- fallocate -l 512M /swapfile — создаёт файл заданного размера (резервирует блоки);
- mkswap /swapfile — размечает файл как область подкачки (пишет заголовок swap);
- swapon /swapfile — активирует область, ядро начинает использовать её для выгрузки страниц;

что такое swappiness и что означает значение 10 против 60:
> насколько агрессивно ядро вытесняет анонимные страницы в swap вместо освобождения page cache
ответьте: почему для сервера базы данных swap иногда рекомендуют вовсе отключать?:
> СУБД сама управляет кешами и ожидает предсказуемое время доступа к данным, риск OOM-killer при нехватке RAM. 
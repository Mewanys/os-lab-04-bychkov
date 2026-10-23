# Задание 3. Настройка подкачки

1. `free -h` и `swapon --show`
```
               total        used        free      shared  buff/cache   available
Mem:           3,8Gi       498Mi       3,1Gi       760Ki       435Mi       3,3Gi
Swap:          1,1Gi          0B       1,1Gi
```

```
NAME      TYPE      SIZE USED PRIO
/dev/sda5 partition 1,1G   0B   -2
```

2. ` sudo fallocate -l 512M /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile `

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

3. `cat /proc/sys/vm/swappiness` и `sudo sysctl vm.swappiness=10`
```
user@debian:~$ cat /proc/sys/vm/swappiness 
60
user@debian:~$ sudo sysctl vm.swappiness=10
vm.swappiness = 10
user@debian:~$ cat /proc/sys/vm/swappiness
10
```

4. `sudo swapoff /swapfile` и `sudo rm -f /swapfile`
```
               total        used        free      shared  buff/cache   available
Mem:           3,8Gi       494Mi       3,1Gi       760Ki       436Mi       3,3Gi
Swap:          1,1Gi          0B       1,1Gi
```



В отчёт ( 03-swap.md ):
вывод free -h до и после подключения swap;
что делает каждая из команд fallocate , mkswap , swapon ;
что такое swappiness и что означает значение 10 против 60;
ответьте: почему для сервера базы данных swap иногда рекомендуют
вовсе отключать?
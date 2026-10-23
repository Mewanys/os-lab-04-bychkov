# Задание 5. Пароль на виду

1. `bash -c 'exec -a "db-backup --password=SuperSecret123" sleep 300' &`
```
[1] 1403
```

2. `ps aux | grep -- --password`
```
$ ps aux | grep -- --password
user        1403  0.0  0.0   6012  2304 pts/0    S    04:10   0:00 db-backup --password=SuperSecret123 300
user2       1430  0.0  0.0   6676  2444 pts/1    S+   04:12   0:00 grep -- --password
```

3. `cat /proc/$(pgrep -f SuperSecret)/environ`
```
cat: /proc/1403/environ: Отказано в доступе
```


В отчет (05-parol.md):
Скриншот, где student2 видит чужой пароль через ps ;
скриншот Permission denied при попытке прочитать environ ;
объясните, почему cmdline доступен всем, а environ — нет;
сформулируйте правило: как правильно передавать пароль программе
(переменная окружения, файл конфигурации с правами 600,
интерактивный ввод) и почему не аргументом
# Задача 4. Нехватка памяти и OOM

1. Создание файла
```
  GNU nano 8.4                                        /home/user/eat.py *                                                
chunks = []
mb = 0
try: 
    while True:
        chunk = bytearray(10*1024*1024)
        for i in range(0, len(chunk), 4096):
            chunk[i] = 1
        chunks.append(chunk)
        mb += 10
```

2. `( ulimit -v 307200; python3 ~/eat.py )`
```
100 МБ
200 МБ
MemoryError на  280 МБ
```
Запуск скрипта без лимита и проверка на OOM Killer
```
3700 МБ
3800 МБ
3900 МБ
4000 МБ
4100 МБ
4200 МБ
4300 МБ
4400 МБ
4500 МБ
4600 МБ
Убито
```

3. `sudo dmesg | grep -i 'out of memory' | tail -5`
```
[sudo] пароль для user: 
[ 1573.887437] Out of memory: Killed process 1350 (python3) total-vm:4801060kB, anon-rss:3682640kB, file-rss:44kB, shmem-rss:0kB, UID:1000 pgtables:9428kB oom_score_adj:0
```

В отчёт ( 04-oom.md ):
на каком объёме процесс упал с MemoryError ;
что делает ulimit -v и почему этот способ безопаснее запуска без лимита;
как ядро выбирает жертву, когда память кончается у всей системы (по
лекции);
что такое oom_score и где его посмотреть ( cat /proc/PID/oom_score ).
# Задание 2. RSS против VSZ

1. `ps -eo pid,user,vsz,rss,comm --sort=-rss | head -10` - VSZ и RSS всех процессов, по убыванию реального расхода
```
    PID USER        VSZ   RSS COMMAND
   1013 root     1960260 98536 dockerd
   1380 65532    1315944 60144 mikan-node
    744 root     1813368 50360 containerd
   1379 65532    1348872 48640 mikan
    385 root      51032 19052 systemd-journal
   1329 root     1268604 17068 containerd-shim
   1331 root     1268348 16948 containerd-shim
      1 root      23536 14188 systemd
   1017 root      19896 12968 sshd-session
```

2. `grep -E 'VmSize|VmRSS' /proc/1233/status`
```
VmSize:     9076 kB
VmRSS:      5868 kB
```

3. `python3 -c 'import mmap, time; m = mmap.mmap(-1, 500*1024*1024); time.sleep(30)' & ps -o pid,vsz,rss,comm -C python3`
```
[2] 4240
    PID    VSZ   RSS COMMAND
   4240 529080  8684 python3
[1]   Завершён        python3 -c 'import mmap, time; m = mmap.mmap(-1, 500*1024*1024); time.sleep(30)'
```

4.`python3 -c 'import mmap, time
m = mmap.mmap(-1, 500*1024*1024)
for i in range(0, len(m), 4096): m[i] = 1
time.sleep(30)' &
ps -o pid,vsz,rss,comm -C python3`
```
[1] 4377
    PID    VSZ   RSS COMMAND
   4377  16804  7304 python3
user@debian:~$ grep VmRSS /proc/4377/status
VmRSS:    520696 kB
```

В отчёт ( 02-rss-vsz.md ):
таблица из ps с VSZ и RSS;
VSZ и RSS обоих python-процессов: того, что память только зарезервировал
через mmap, и того, что записал в каждую страницу;
объясните разницу: почему у первого RSS маленький, хотя VSZ большой;
как это связано с принципом «выделено ≠ занято» из лекции?

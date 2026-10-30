# Задание 2. RSS против VSZ

1. `ps -eo pid,user,vsz,rss,comm --sort=-rss | head -10` - вывод PID, пользователя, виртуальный размер (VSZ), резидентный размер (RSS) и имя процесса, сортируем по убыванию RSS
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
у dockerd VSZ 1.96 ГБ, а RSS 96 МБ - VSZ в разы больше RSS

2. `grep -E 'VmSize|VmRSS' /proc/1233/status` - смотрим те же величины для конкретного процесса (PID 1233) из `/proc`.
```
VmSize:     9076 kB
VmRSS:      5868 kB
```
VmSize 9076 kB, VmRSS 5868 kB - совпадает с итогом pmap

3. `python3 -c 'import mmap, time; m = mmap.mmap(-1, 500*1024*1024); time.sleep(30)' & ps -o pid,vsz,rss,comm -C python3` - процесс резервирует 500 МБ анонимной памяти через mmap, но не пишет в неё
```
[2] 4240
    PID    VSZ   RSS COMMAND
   4240 529080  8684 python3
[1]   Завершён        python3 -c 'import mmap, time; m = mmap.mmap(-1, 500*1024*1024); time.sleep(30)'
```
VSZ 529 МБ (500 МБ резерва + базовый python), RSS всего 8.7 МБ — физически страницы не выделены, пока в них не записали.

4. 
```
python3 -c 'import mmap, time
m = mmap.mmap(-1, 500*1024*1024)
for i in range(0, len(m), 4096): m[i] = 1
time.sleep(30)' &
ps -o pid,vsz,rss,comm -C python3
``` 
- тот же резерв 500 МБ, но теперь в каждую страницу 
```
[1] 4377
    PID    VSZ   RSS COMMAND
   4377  16804  7304 python3
user@debian:~$ grep VmRSS /proc/4377/status
VmRSS:    520696 kB
```
grep VmRSS показывает реальное значение VmRSS 508 МБ, RSS вырос с 8 до 508 МБ.

### В отчёт ( 02-rss-vsz.md ):
- объясните разницу: почему у первого RSS маленький, хотя VSZ большой:
> mmap лишь резервирует виртуальное адресное пространство, создаются записи в таблицах страниц, но физические страницы не выделяются.
- как это связано с принципом «выделено ≠ занято» из лекции?
> VSZ — это сколько виртуальной памяти процесс зарезервировал (может адресовать), RSS — сколько физической памяти реально занято его страницами/
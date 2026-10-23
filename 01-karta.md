# Задание 1. Карта памяти процесса

1. `echo $$` - номер оболочки
```
1233
```

2.  `cat /proc/$$/maps | head -20` - Карта памяти: области кода, данных, стека, библеотек
```
558c92039000-558c92068000 r--p 00000000 08:01 131183                     /usr/bin/bash
558c92068000-558c92131000 r-xp 0002f000 08:01 131183                     /usr/bin/bash
558c92131000-558c92169000 r--p 000f8000 08:01 131183                     /usr/bin/bash
558c92169000-558c9216d000 r--p 00130000 08:01 131183                     /usr/bin/bash
558c9216d000-558c92176000 rw-p 00134000 08:01 131183                     /usr/bin/bash
558c92176000-558c92181000 rw-p 00000000 00:00 0 
558cab316000-558cab52b000 rw-p 00000000 00:00 0                          [heap]
7f7f1a600000-7f7f1a8ec000 r--p 00000000 08:01 148490                     /usr/lib/locale/locale-archive
7f7f1a907000-7f7f1a90e000 r--s 00000000 08:01 163219                     /usr/lib/x86_64-linux-gnu/gconv/gconv-modules.cache
7f7f1a90e000-7f7f1a910000 rw-p 00000000 00:00 0 
7f7f1a910000-7f7f1a938000 r--p 00000000 08:01 163230                     /usr/lib/x86_64-linux-gnu/libc.so.6
7f7f1a938000-7f7f1aa9b000 r-xp 00028000 08:01 163230                     /usr/lib/x86_64-linux-gnu/libc.so.6
7f7f1aa9b000-7f7f1aaf1000 r--p 0018b000 08:01 163230                     /usr/lib/x86_64-linux-gnu/libc.so.6
7f7f1aaf1000-7f7f1aaf5000 r--p 001e0000 08:01 163230                     /usr/lib/x86_64-linux-gnu/libc.so.6
7f7f1aaf5000-7f7f1aaf7000 rw-p 001e4000 08:01 163230                     /usr/lib/x86_64-linux-gnu/libc.so.6
7f7f1aaf7000-7f7f1ab04000 rw-p 00000000 00:00 0 
7f7f1ab04000-7f7f1ab13000 r--p 00000000 08:01 130578                     /usr/lib/x86_64-linux-gnu/libtinfo.so.6.5
7f7f1ab13000-7f7f1ab27000 r-xp 0000f000 08:01 130578                     /usr/lib/x86_64-linux-gnu/libtinfo.so.6.5
7f7f1ab27000-7f7f1ab35000 r--p 00023000 08:01 130578                     /usr/lib/x86_64-linux-gnu/libtinfo.so.6.5
7f7f1ab35000-7f7f1ab39000 r--p 00031000 08:01 130578                     /usr/lib/x86_64-linux-gnu/libtinfo.so.6.5
```

3. `pmap $$` - то же в читаемом виде, с размерами
```
1233:   -bash
0000558c92039000    188K r---- bash
0000558c92068000    804K r-x-- bash
0000558c92131000    224K r---- bash
0000558c92169000     16K r---- bash
0000558c9216d000     36K rw--- bash
0000558c92176000     44K rw---   [ anon ]
0000558cab316000   2132K rw---   [ anon ]
00007f7f1a600000   2992K r---- locale-archive
00007f7f1a907000     28K r--s- gconv-modules.cache
00007f7f1a90e000      8K rw---   [ anon ]
00007f7f1a910000    160K r---- libc.so.6
00007f7f1a938000   1420K r-x-- libc.so.6
00007f7f1aa9b000    344K r---- libc.so.6
00007f7f1aaf1000     16K r---- libc.so.6
00007f7f1aaf5000      8K rw--- libc.so.6
00007f7f1aaf7000     52K rw---   [ anon ]
00007f7f1ab04000     60K r---- libtinfo.so.6.5
00007f7f1ab13000     80K r-x-- libtinfo.so.6.5
00007f7f1ab27000     56K r---- libtinfo.so.6.5
00007f7f1ab35000     16K r---- libtinfo.so.6.5
00007f7f1ab39000      4K rw--- libtinfo.so.6.5
00007f7f1ab3e000      8K rw---   [ anon ]
00007f7f1ab40000     16K r----   [ anon ]
00007f7f1ab44000      8K r-x--   [ anon ]
00007f7f1ab46000      4K r---- ld-linux-x86-64.so.2
00007f7f1ab47000    160K r-x-- ld-linux-x86-64.so.2
00007f7f1ab6f000     44K r---- ld-linux-x86-64.so.2
00007f7f1ab7a000      8K r---- ld-linux-x86-64.so.2
00007f7f1ab7c000      4K rw--- ld-linux-x86-64.so.2
00007f7f1ab7d000      4K rw---   [ anon ]
00007ffd5c07c000    132K rw---   [ stack ]
 total             9076K
```

4. `pmap $$ | tail -1` - Итоговый размер
```
total             9076K
```

5. `grep -E 'stack|libc' /proc/$$/maps`
```
7f7f1a910000-7f7f1a938000 r--p 00000000 08:01 163230                     /usr/lib/x86_64-linux-gnu/libc.so.6
7f7f1a938000-7f7f1aa9b000 r-xp 00028000 08:01 163230                     /usr/lib/x86_64-linux-gnu/libc.so.6
7f7f1aa9b000-7f7f1aaf1000 r--p 0018b000 08:01 163230                     /usr/lib/x86_64-linux-gnu/libc.so.6
7f7f1aaf1000-7f7f1aaf5000 r--p 001e0000 08:01 163230                     /usr/lib/x86_64-linux-gnu/libc.so.6
7f7f1aaf5000-7f7f1aaf7000 rw-p 001e4000 08:01 163230                     /usr/lib/x86_64-linux-gnu/libc.so.6
7ffd5c07c000-7ffd5c09d000 rw-p 00000000 00:00 0                          [stack]
```

В отчёт ( 01-karta.md ):
- фрагмент pmap $$ с пояснением, какие области видны;
- где в памяти стек, где библиотеки, где код оболочки;
- какой общий размер показал pmap ;
- ответьте: почему одна и та же библиотека libc присутствует в памяти десятков процессов, но в ОЗУ лежит в одном экземпляре?
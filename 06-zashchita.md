# Задание 6. Защита важной службы от OOM


1. `sleep 600 &
PID=$!
cat /proc/$PID/oom_score
cat /proc/$PID/oom_score_adj`
```
[2] 1443
[1]   Завершено      bash -c 'exec -a "db-backup --password=SuperSecret123" sleep 300'
666
0
```
2. `sudo bash -c "echo -500 > /proc/$PID/oom_score_adj"
cat /proc/$PID/oom_score_adj
cat /proc/$PID/oom_score`
```
-500
334
```

В отчёт ( 06-zashchita.md ):
oom_score и oom_score_adj до и после изменения;
как изменилась оценка после понижения oom_score_adj ;
зачем понадобился sudo ;
приведите пример: какой службе на реальном сервере вы понизили бы
oom_score_adj и почему (подсказка: та, потеря которой дороже всего).
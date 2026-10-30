# Задание 6. Защита важной службы от OOM


1. 
```
sleep 600 & 
PID=$!
cat /proc/$PID/oom_score
cat /proc/$PID/oom_score_adj
```
- Запускает важную службу 
```
[2] 1443
[1]   Завершено      bash -c 'exec -a "db-backup --password=SuperSecret123" sleep 300'
666
0
```
oom_score = 666, базовая оценка по потреблению памяти
2. `sudo bash -c "echo -500 > /proc/$PID/oom_score_adj"` 
`cat /proc/$PID/oom_score_adj`
`cat /proc/$PID/oom_score`- 
```
-500
334
```
oom_score_adj стал −500; oom_score снизился с 666 до 334 - процесс стал заметно менее вероятной жертвой OOM killer.

### В отчёт ( 06-zashchita.md ):

- зачем понадобился sudo ;
> файл /proc/PID/oom_score_adj принадлежит процессу/root; запись в него требует прав root
- приведите пример: какой службе на реальном сервере вы понизили бы oom_score_adj и почему (подсказка: та, потеря которой дороже всего):
> Потеря БД или ssh дороже всего: при OOM лучше убить менее критичный процесс, чем базу или доступ к серверу.
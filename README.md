# Workshop Docker

Docker — программное обеспечение для автоматизации развёртывания и управления приложениями в средах с поддержкой контейнеризации, контейнеризатор приложений. Позволяет «упаковать» приложение со всем его окружением и зависимостями в контейнер, который может быть развёрнут на любой Linux-системе, а также предоставляет набор команд для управления этими контейнерами. 

Есть два основных варианта работы с Docker - cli и desktop

Для более комфортной работы с cli также желательно иметь базовые навыки работы с терминалом. Основные команды - cd, ls, pwd



### Запуск контейнеров
Для запуска контейнера используется команда 

```docker run <название образа>```

Для примера возьмем postgres

```
docker run postgres
```
Докер автоматически загрузит образ из https://hub.docker.com/


```
docker run -e POSTGRES_PASSWORD=password postgres
```
В терминале видим ошибку - не хватает переменной окружения. Сразу же дается подсказка, как добавить переменную окружения с помощью ключа -e
```
docker run -d -e POSTGRES_PASSWORD=password postgres
```

Но как к нему подключиться? Для подключения не из докера необходимо пробросить порт во вне с помощью ключа -p. Первым указывается порт на хосте, вторым порт внутри контейнера
```
docker run -d -e POSTGRES_PASSWORD=password -p 5432:5432 postgres
```

Докер контейнеры могут общаться между собой внутри сети самого докера по названиям контейнера в качестве доменного имени, а могут и через сеть хост операционной системы `host.docker.internal`


После остановки контейнера они остаются. Для того, чтобы контейнер удалялся можно добавить ключ --rm
```
docker run -d --rm -e POSTGRES_PASSWORD=password -p 5432:5432 postgres
```


Теперь наш контейнер удалился вместе со всеми данными. Для того, чтобы сохранять данные для переиспользования, можно добавить ключ -v и указать путь на хосте и путь внутри контейнера через двоеточие
```
docker run -d --rm -e POSTGRES_PASSWORD=password -p 5432:5432 -v C:\Users\Artemiy\Documents\pg_data:/var/lib/postgresql/data postgres
```

Посмотреть логи можно с помощью команды
```
docker logs
```

```
docker run -d --rm -e POSTGRES_PASSWORD=password -p 5432:5432 -v C:\Users\Artemiy\Documents\pg_data:/var/lib/postgresql/data postgres:13
```

Для просмотра информации о содержимом образа необходимо выполнить команду 
```
docker image inspect postgres
```
```
docker image inspect <название образа>
```


### Docker compose

Для запуска используется команда
```
docker compose up 
```
Запуск выполняется для ямла из текущей директории


Для запуска в detach режиме необходимо добавить ключ -d
```
docker compose up -d 
```

Для остановки и удаления контейнеров используется команда
```
docker compose down
```

Для остановки без удаления используется команда
```
docker compose stop
```

иногда надо чистить вольюмы, для этого ключик -v
```
docker compose down -v
```



###

Запуск контейнера из 1 спринта https://practicum.yandex.ru/learn/data-engineer/courses/dd73421d-bf74-4f2d-9b5f-c45d7d258b00/sprints/125752/topics/3f0a5098-425a-40be-9fff-a1df6bcb5532/lessons/e229854f-9e81-40fc-bb2e-314e17116d42/
```
docker run --rm -d -p 7010:8000 --name de-sprint-1-server-local cr.yandex/crp1r8pht0n0gl25aug1/de-sprint-1-v2:latest 
```

Подключим volume
Важно, чтобы подключенная директория, была пустой
```
docker run -d -v /home/<username>/de_lessons/sprint1:/s1-lessons --rm -p 7010:8000 --name=de-sprint-1-server-local cr.yandex/crp1r8pht0n0gl25aug1/de-sprint-1-v2:latest
```
Если директория, которую мы укажем, была не пустой - все файлы, которые в ней должны были быть в образе, как при запуске без вольюма - не появятся

### Очистка ресурсов

```
docker system prune
```

```
docker volume prune
docker volume prune --all
docker system prune --volumes
docker system prune --all
```

```
docker image prune -a
```

В Docker Desktop очистка находится под кнопкой Troubleshoot с жучком рядом с настройками

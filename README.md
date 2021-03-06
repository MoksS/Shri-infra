# Инфраструктура

Версия Node js - 12.16.1

## Установка и запуск build сервера 

**Предварительно необходимо записать токен в _/server/server-conf.json_**

```
  cd server
  npm ci
  npm start
```

## Установка и запуск build агента
```
  cd agent
  npm ci
  npm start
```

## Требования

- [x] Сервер должен максимально утилизировать имеющихся агентов.
  
  Сервер через определенное количество времени проверят наличие новых билдов, и если они есть, добавляет их в очередь. также отдельно через определенно количество времени идет проверка на количество свободных агентов и билдов, дальше происходит распределение в порядке очереди.

- [x] Сервер должен корректно обрабатывать ситуацию, когда агент прекратил работать между сборками.
  Когда сервер отправляет запрос агенту на сборку, и он не отвечает, то агент удаляется из списка агентов, а билд передается следующему агенту.
  По моей логике если агент не отвечает, то он упал и был перезапущен каким нибудь демоном, после чего заново зарегистрировался.

- [x] Сервер должен корректно обрабатывать ситуацию, когда агент прекратил работать в процессе выполнения сборки.

  В определенно количество времени (можно задать в настройках в самом коде, да знаю, надо было вынести в конфиг), у меня проходит проверка по всем работающим агентам, если какой-то агент, не отвечал продолжительное время, то он будет удален из списка агентов, а сборка передается другому агенту

- [x] Агент должен корректно обрабатывать ситуацию, когда при старте не смог соединиться с сервером.

  Если агент не смог зарегистрироваться,  то он еще 2 раза с периодом в 2 секунды(никто не мешает поставить значение больше) отправляет запрос к серверу, в случае неудачи, агент прекращает свою работу 

- [x] Агент должен корректно обрабатывать ситуацию, когда при отправке результатов сборки не смог соединиться с сервером.

  Так же пробует несколько раз отправить результат сборки на сервер, в случае неудаче, агент прекращает свою работу. Билд сервер позже сам перезапустит сборку.

- [x] Изменение настроек

  При добавлении билдов в очередь, если меняется id настроек, то настройки обновляются, так как при обновлении настроек наши прошлы билды чистятся в базе данных api Яндекса, то и я удаляю старую очередь и начинаю с новой.

- [x] Сервер не может получить первоначальный конфиг с api Яндекса

  Так же пробует несколько попыток, в случае неудачи, сервер прекращает работу

## To Do

1) **Разобраться с докером и настроить его**

2) **Добавить тесты, сейчас не сделаны потому что я тупой), из-за того что тупой, не успеваю  в дедлайн, не успеваю в дедлайн -> забиваю на тесты**

3) **Прорефакторить приложение, навести чистоту в коде**

4) **Перенести очередь, а так же список агентов на локальное хранилище, а не в оперативной памяти, например воспользоваться библиотекой [Lowdb](https://www.npmjs.com/package/lowdb)**

5) **Разобраться с безопасностью приложения, я не валидирую поступающие команды, с докером конечно было бы лучше, хотя бы изолировал среду**

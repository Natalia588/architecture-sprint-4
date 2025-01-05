
# Выбор и настройка мониторинга в системе

## Мотивация

Мониторинг просто необходим компании, чтобы узнавать о проблемах с системой раньше пользователей.
В настоящий момент о проблемах сообщают клиенты и представители бизнеса, 
что ставит компании в заведомо невыигрышное положение.

## Выбор подхода к мониторингу

Предлагаю использовать метод RED. Он измеряет показатели, которые важны для конечных пользователей, что наиболее критично в нашем случае. 
Этот метод подойдет как для API, так и для запросов к базам данных и очередей.

## Список метрик

Рассмотрим список необходимых метрик по частям.

### Shop API

- **Number of requests (RPS) for internet shop API**
    
    Показывает количество запросов (в секунду) к Shop API. 
    Резкий рост этого показателя может указывать на DDoS атаку.


- **Number of requests (RPS) per user for internet shop API**

    Показывает количество запросов (в секунду), отправляемых одним пользователем.
    Слишком высокая активность одного пользователя может быть подозрительной.


- **CPU % for shop API**

    Загруженность процессора конкретного инстанса API. 
    Критическое значение - более 80-90% в течении длительного времени.


- **Memory Utilisation for shop API**

    Использование оперативной памяти.
    Постоянно возрастающее значение может указывать на утечки памяти.


- **Response time (latency) for shop API**

    Среднее время ответа API.
    Критический показатель зависит от требований (SLA).


- **Memory Utilisation for shop db instance**

    Использование оперативной памяти базой данных.
    Высокое значение (более 80-90%) может указывать на проблемы.


- **Number of connections for shop db instance**

    Количество активных подключений к базе данных.
    Критический показатель - близкий к максимальному значению для определенной базы данных.
    В этом случае стоит настроить пул соединений или оптимизировать приложение, уменьшив количество подключений.


- **Size of shop db instance**

    Размер хранилища, используемого базой данных.
    Критический показатель - более 80-90%, в этом случае необходимо увеличивать объем хранилища.


- **Number of HTTP 200 for shop API**

    Количество успешных ответов. 
    Уменьшение этого показателя может указывать на сбои.


- **Number of HTTP 500 for shop API**

    Количество ошибок на стороне сервера.
    Необходимо найти причину сбоя и устранить.


- **Number of simultanious sessions for shop API**

    Количество одновременных сеансов API.
    Критический показатель зависит от пропускной способности.


- **Kb tranferred (received) for shop API**

  Количество данных, принятых API.
  Слишком большие показатели могут свидетельствовать об аномалиях, необходимо настроить rate limiting, мониторить загрузку.


- **Kb provided (sent) for shop API**

  Количество данных, отправленных клиентам от API.
  Слишком большие показатели могут перегрузить сеть, нужно подумать о сжатии.


### CRM API

- Number of requests (RPS) for CRM API
- Number of requests (RPS) per user for CRM API
- CPU % for CRM API
- Memory Utilisation for CRM API
- Response time (latency) for CRM API
- Number of HTTP 200 for CRM API
- Number of HTTP 500 for CRM API
- Number of simultanious sessions for CRM API
- Kb tranferred (received) for CRM API
- Kb provided (sent) for CRM API

### MES API

- Number of requests (RPS) for MES API
- Number of requests (RPS) per user for MES API
- CPU % for MES API
- Memory Utilisation for MES API
- Memory Utilisation for MES db instance
- Number of connections for MES db instance
- Response time (latency) for MES API
- Size of MES db instance
- Number of HTTP 200 for MES API
- Number of HTTP 500 for MES API
- Number of simultanious sessions for MES API
- Kb tranferred (received) for MES API
- Kb provided (sent) for MES API

### S3 storage

- Size of S3 storage
- Memory Utilisation for db instance
- Number of connections for db instance

### Rabbit MQ

- Number of dead-letter-exchange letters in RabbitMQ

    Количество сообщений, попавших в Dead Letter Queue (DLQ), потому что они не были обработаны.
    Критический показатель - быстрое и значительное увеличение.
    Нужно проанализировать причину - неверный формат сообщений, истекший TTL, переполнение очереди.


- Number of message in flight in RabbitMQ

    Количество сообщений, которые RabbitMQ отправил, но не получил подтверждения от Consumer об их получении.
    Увеличение этого показателя указывает на проблемы, нужно оптимизировать Consumers.


## План действий

- Необходимо настроить инструмент для сбора показателей, например Prometeus.
- Добавить необходимые метрики в приложения - это высокоуровневую задачу стоит разделить по компонентам.
- Настроить инструмент для визуализации собранных показателей, например Grafana.
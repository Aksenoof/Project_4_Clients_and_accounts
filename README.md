# Проект №4 Клиенты и счета.

## Цель проекта сформировать 3 витрины для дальнейшего анализа счетов клиентов.

Из полученных данных по клиентам, счетам и операциям приложение формирует 3 витрины и сохраняет каждую в отдельный parquet.
Витрины формируются и сохраняются согласно ключу партиции, который можно указать в приложении.

### В прокете используется технология spark + scalla. 

Так как это связка дает быструю обработку больших данных, простоту преобразования и трансформацию данных,
сохраняя баланс между продуктивностью и производительностью.

## Реализация проекта.

Для выполнения поставленной задачи проекта:

1. Установить [Spakr](https://spark.apache.org/downloads.html) локально.

2. Установить [Docker](https://docs.docker.com/get-docker/) в зависимости от oперационной системы.

3. Развернуть контейнер с Postgres
 ```bash
docker-compose up
````
Для администрирования Postgres
 - установить [DBeaver](https://dbeaver.com/) 
 
В DBeaver создаем и загружаем в постгрес из Comment_filter.sql таблицу списков.

4. Открываем проект Clients and accounts в IntelliJ IDEA.

Для правильной работы приложения в IntelliJ IDEA изменяем пути нахождения файлов данных по клиентам, счетам и операциям,
а так же прописываем пути сохранения витрин на вашем хосте.

После выполнения приложения получаем три каталога с сохраненными витринами согласно ключу партиции: 
 - corporate_payments,
 - corporate_account, 
 - corporate_info.















  

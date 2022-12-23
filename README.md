# Проект №4 Клиенты и счета.

## Задание

Необходимо сформировать три витрины на следующие даты: 2020-11-01, 2020-11-02, 2020-11-03, 2020-11-04.

### 1. Витрина _corporate_payments_.
 Строится по каждому уникальному счету (AccountDB  и AccountCR) из таблицы Operation. Ключ партиции CutoffDt

AccountId - ИД счета,
ClientId - Ид клиента счета,
PaymentAmt - Сумма операций по счету, где счет клиента указан в дебете проводки,
EnrollementAmt - Сумма операций по счету, где счет клиента указан в  кредите проводки,
TaxAmt - Сумму операций, где счет клиента указан в дебете, и счет кредита 40702,
ClearAmt - Сумма операций, где счет клиента указан в кредите, и счет дебета 40802,
CarsAmt - Сумма операций, где счет клиента указан в дебете проводки и назначение платежа не содержит слов по маскам Списка 1,
FoodAmt - Сумма операций, где счет клиента указан в кредите проводки и назначение платежа содержит слова по Маскам Списка 2,
FLAmt - Сумма операций с физ. лицами. Счет клиента указан в дебете проводки, а клиент в кредите проводки – ФЛ.,
CutoffDt - Дата операции.

### 2. Витрина _corporate_account_.
 Строится по каждому уникальному счету из таблицы Operation на заданную дату расчета. Ключ партиции CutoffDt  

AccountID - ИД счета,
AccountNum - Номер счета,
DateOpen - Дата открытия счета,
ClientId - ИД клиента,
ClientName - Наименование клиента,
TotalAmt - Общая сумма оборотов по счету. Считается как сумма PaymentAmt и EnrollementAmt,
CutoffDt - Дата операции.     

### 3. Витрина _corporate_info_.
 Строится по каждому уникальному клиенту из таблицы Operation. Ключ партиции CutoffDt 

ClientId - ИД клиента (PK),
ClientName - Наименование клиента,
Type - Тип клиента (ФЛ, ЮЛ),
Form - Организационно-правовая форма (ООО, ИП и т.п.),
RegisterDate - Дата регистрации клиента,
TotalAmt - Сумма операций по всем счетам клиент (Считается как сумма corporate_account.total_amt по всем счетам),
CutoffDt - Дата операции.

## Источники данных.
Данны 4 csv файла:

### 1. Таблица клиентов 10 000 записей - Clients.csv
ClientId - ИД клиента (PK),
ClientName - Наименование клиента,
Type - Тип клиента (ФЛ, ЮЛ),
Form - Организационно-правовая форма (ООО, ИП и т.п.),
RegisterDate - Дата регистрации клиента.

### 2. Таблица счетов – 20 000 записей - Account.csv
AccountId - ИД  счета (PK),
AccountNum - Двадцатизначный номер счета,
ClientId - ИД клиента владельца счета (FK),
DateOpen - Дата открытия счета.

### 3. Операции по счетам – 100 000 записей - Operation.csv
AccountDB - Счет дебета проводки (FK),
AccountCR - Счет кредита проводки (FK),
DateOp - Дата операции,
Amount - Сумма операции,
Currency - Валюта операции,
Comment - Назначение платежа.

### 4. Курсы валют по отношению к рублю - Rate.csv
Currency - Валюта,
Rate - Курс,
RateDate - Дата курса.

### 5. Списки масок:
Список 1:
%а/м%, %а\м%, %автомобиль %, %автомобили %, %транспорт%, %трансп%средс%, %легков%, %тягач%, %вин%, %vin%,%viн:%, %fоrd%, %форд%,%кiа%, %кия%, %киа%%мiтsuвisнi%, %мицубиси%, %нissан%, %ниссан%, %sсанiа%, %вмw%, %бмв%, %аudi%, %ауди%, %jеер%, %джип%, %vоlvо%, %вольво%, %тоyота%, %тойота%, %тоиота%, %нyuнdаi%, %хендай%, %rенаulт%, %рено%, %реugеот%, %пежо%, %lаdа%, %лада%, %dатsuн%, %додж%, %меrсеdеs%, %мерседес%, %vоlкswаgен%, %фольксваген%, %sкоdа%, %шкода%, %самосвал%, %rover%, %ровер%

Список 2:
%сою%, %соя%, %зерно%, %кукуруз%, %масло%, %молок%, %молоч%, %мясн%, %мясо%, %овощ%, %подсолн%, %пшениц%, %рис%, %с/х%прод%, %с/х%товар%, %с\х%прод%, %с\х%товар%, %сахар%, %сельск%прод%, %сельск%товар%, %сельхоз%прод%, %сельхоз%товар%, %семен%, %семечк%, %сено%, %соев%, %фрукт%, %яиц%, %ячмен%, %картоф%, %томат%, %говя%, %свин%, %курин%, %куриц%, %рыб%, %алко%, %чаи%, %кофе%, %чипс%, %напит%, %бакале%, %конфет%, %колбас%, %морож%, %с/м%, %с\м%, %консерв%, %пищев%, %питан%, %сыр%, %макарон%, %лосос%, %треск%, %саир%, % филе%, % хек%, %хлеб%, %какао%, %кондитер%, %пиво%, %ликер%

Суммы должны быть в национальной валюте, для перевода использовать самый актуальный курс из таблицы курсов.
Таблица списков должна загружаться из постгреса в спарк (локально).
По итогу должно получиться 3 витрины с 4 днями.
Каждая витрина – один паркет.

## Технологический стек – scala.
Использовал  Spark, postgres, IntelliJ IDEA CE.
IntelliJ IDEA CE, Spark установлены локально.
Postgres уставлен в докере.

## Реализации проекта

## Postgres
1. Развернул контейнер с Postgres в докере использовал docker-compose.yml (postgres)
2. Создал таблицу масок файл - Comment_filter.sql в postgres
3. Подготовил общий список масок файл - список_масок.csv
4. Загрузил в созданую таблицу postgres

## IntelliJ IDEA CE
1. Создание сессии Spark.
2. Создание сессии для чтения данных из postgres.
3. Считывание csv файлов в dataframe.
4. Создание таблицы операций с конвертируемой суммой операции в национальную валюту по актуальному курсу и ключу партиции CutoffDt.
5. Сборка таблицы операций по ИД счету (AccountId) по таблице счетов и счету дебета проводки (AccountDB) по таблице операций.
6. Сборка таблицы для нахождения суммы операций по счету, где счет клиента указан в дебете проводки - PaymentAmt.
7. Сборка таблицы операций по ИД счету (AccountId) по таблице счетов и счету кредита проводки (AccountCR) по таблице операций.
8. Сборка таблицы для нахождения суммы операций по счету, где счет клиента указан в кредите проводки - EnrollementAmt.
9. Сборка таблицы для нахождения даты операции CutoffDt.
10. Считываем из postgres таблицу масок и конвертируем в список масок 1 и список масок 2.
11. Создание функций проверки назначения платежа по маскам списка 1 и списка 2.
12. Сборка таблицы для нахождения суммы операций, где счет клиента указан в дебете проводки и назначение платежа не содержит слов по маскам Списка 1 - CarsAmt.
13. Сборка таблицы для нахождения суммы операций, где счет клиента указан в кредите проводки и назначение платежа содержит слова по маскам Списка 2 - FoodAmt. 
14. Сборка сборной таблицы для расчета оставшихся параметров.
15. Сборка ветрин: _corporate_payments_, _corporate_info_, _corporate_info_.
16. Сохранениe каждой ветрины в паркет согласно ключу партиции CutoffDti.
    

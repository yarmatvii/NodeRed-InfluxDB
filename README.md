# NodeRed+InfluxDB
 Laboratory work (4th year bachelor). Object visualization and management in the Node-RED environment. Storing data in the InfluxDB. Reading and processing archived data from InfluxDB using Node-RED.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
**1 Завдання 1**

## **1.1 Постановка**

Розробити візуалізацію роботи контуру керування та управління ним у середовищі Node-RED:

- Змінювати режим роботи регулятора – «ручний / авто»

- Технологічні змінні відображати у вигляді графіків;

- Відображати та задавати параметри регулятора;

## **1.2 Вирішення**

При виконанні лабораторної роботи було записано всі булеві змінні конфігурації в перший (0) holding register

![screenshot](https://github.com/yarmatvii/NodeRed-InfluxDB/blob/main/img/Picture1.png?raw=true)

Рисунок 1. Зчитування конфігурацій контролера з регістрів у CodeSys

У середовищі візуального програмування node-red створено фрагмент коду для зчитування конфігурацій системи та контролера. Функція SystemState створює об'єкт у глобальному контексті з усіма даними про систему. Функція getConfigстворює массив у глобальному контексті з конфігураціями контролера які можна зберігати у 1омурегістрі. (Рис.2). Кожен елемент масиву відповідає порядкому значенню у регістрі. Приклад реалізації даної функції див Рис.3.

![](RackMultipart20230428-1-rrta77_html_2ec0eddbdcae1b60.png)

Рисунок 2. Зчитування налаштувань контроллера та системи у node-red.

![](RackMultipart20230428-1-rrta77_html_f36de952227069be.png)

Рисунок 3. Реалізаціяфункції getConfig

Для зміни параметрів роботи системи створимо візуалізацію (див. Рис.4)

![](RackMultipart20230428-1-rrta77_html_37bc4c70910b1ece.png)

Рисунок 4. Реалізація візуалізації у середовищі node-red

Для запису булевого значення потрібно змінити відповідний біт у конфігурації контролера, після чого потрібно його перетворити у число і записати у перший регістр. Розглянемо встановлення ручного та автоматичного режиму:

![](RackMultipart20230428-1-rrta77_html_4946e390ad8dd595.png)

Рисунок 5. Реалізація функції manualMode

![](RackMultipart20230428-1-rrta77_html_ee9bfac50feb7d68.png)

Рисунок 6. Реалізація функції toNumber

## **1.3 Верифікація**

![](RackMultipart20230428-1-rrta77_html_6b0604058ce3beee.png)

Рисунок 7. Візуалізація до завдання 1

![](RackMultipart20230428-1-rrta77_html_d52f3fc315e44af7.png)

Рисунок 8. Робота в CodeSys

На рисунку 7 показано UI для встановлення режиму роботи регулятора - "ручний/авто" та поля для задання значень параметрів регулятора. Також можна здійснювати відкриття та закриття клапана ВМ та бачити результати роботи на графіку процесу.

**2 Завдання 2**

## **2.1 Постановка**

Забезпечити зберігання даних про роботу розробленого раніше

контуру керування в базі даних часових рядів InfluxDB

- Розробити структуру зберігання даних з полями та тегами;

- Налаштувати зберігання, параметри доступу до бази даних

- Тривалість зберігання даних – 1 місяць

## **2.2 Вирішення**

Щоб забезпечити зберігання інформації про роботу в базі даних часових рядів InfluxDb, потрібно створити структуру для зберігання даних з відповідними полями та тегами безпосередньо в системі управління базами даних InfluxDb. Перш за все, необхідно створити Bucket, у якому будуть зберігатись дані з відповідними полями та тегами.

![](RackMultipart20230428-1-rrta77_html_6a99b33653b2b606.png)

Рисунок 9. Створений Bucket

Виконаємо налаштування бази даних та встановимо тривалість зберігання даних - один місяць.

![](RackMultipart20230428-1-rrta77_html_6f2dc1501f2723ca.png)

Рисунок 10. Налаштування бази даних

Налаштуємо зберігання. Для цього визначимо параметри зберігання до бази даних, генеруючи API Token для підключення з середовища Node-Red.

![](RackMultipart20230428-1-rrta77_html_bad0f167d58561f0.png)

Рисунок 11.Створений API Token

Для роботи з базою даних InfluxDb в середовищі Node-Red необхідно підключити модуль node-red-contrib-stackhero-influxdb-v2

![](RackMultipart20230428-1-rrta77_html_2950ac414627acc9.png)

Рисунок 12. Підключення модуля node-red-contrib-stackhero-influxdb-v2

Для того, щоб записувати real-time дані у БДвикористаємо алгоритм, що складається з блоків для зчитування даних з flow змінної, форматування даних у JSON та запиус в базу даних:

![](RackMultipart20230428-1-rrta77_html_13e7d156ee6a0b83.png)

Рисунок 13. Алгоритм запису даних у БД

У нодіtoDbFormatвизначаємоформатування вхідних даних у JSON який будемо передавати в базу даних:

![](RackMultipart20230428-1-rrta77_html_8bf1976f61426af.png)

Рисунок 14.Форматування вхідних даних у JSON

У ноді запису у БД задаємо параметри конфігурації для підключення до бази даних InfluxDb.

![](RackMultipart20230428-1-rrta77_html_feadbf8326169255.png)

Рисунок 15. Конфігурація підключення до Bucket в базі даних

## **2.3 Верифікація**

Результат роботи створення бази даних InfluxDb та підключення до неї через середовище Node-Red:

![](RackMultipart20230428-1-rrta77_html_39cb1ae329f4eaec.png)

Рисунок 16. Дані в базі отримані з Node-Red.

**3 Завдання 3**

## **3.1 Постановка**

Візуалізувати динаміку роботи контуру керування на графіках Node-RED, використовуючи архівні дані з InfluxDB (зчитування та обробка архівних даних):

- Зчитати архівні дані про роботу системи керування з БД InfluxDB за

обраний час: останню хвилину, останні 5 хвилин, останні 15 хвилин,

вказаний користувачем час;

- Візуалізувати показники технологічних змінних вбудованими

засобами Node-RED або іншими бібліотеками графіків;

- Зміну виходу об'єкта (датчика) та уставки регулятора відобразити на одному графіку;

## **3.2 Вирішення**

Покажемо динаміку роботи контуру керування на графіках Node-Red, використовуючи архівні дані з InfluxDb.

Реалізуємо наступний алгоритм для зчитування архівних даних про роботу системи керування з БД InfluxDB за обраний час: останню хвилину, останні 5 хвилин, останні 15 хвилин, вказаний користувачем час.

![](RackMultipart20230428-1-rrta77_html_d8b90c3055ce57c1.png)

Рисунок 17. Алгоритм для зчитування даних з БД

На рисунку 17 відбувається зчитування даних за останню хвилину, останні 5 хвилин, останні 15 хвилин та час вказаний користувачем з нашого UI для Node-Red. Після цього дані форматуються у JSON для подальшої передачі їх в базу даних. Нижче наведено запит до бази даних який читає дані за відповідний проміжок часу:

![](RackMultipart20230428-1-rrta77_html_1e22d7a09e1ae66e.png)

Рисунок 18.Запит для читання даних з БД

Таким чином виконана візуалізація показників технологічних змінних за допомогою вбудованих засобів Node-RED. Після того як ми отримали потрібні дані потрібно виконати обробку корисного навантаження та відобразити зміну виходу об'єкта та уставки на одному графіку.

![](RackMultipart20230428-1-rrta77_html_1a1ecd513a57813a.png)

Рисунок 19.Алгоритм для передачі даних для виводу об'єкта та уставки на одному графіку

## **3.3 Верифікація**

Виконавши дії описані в попередньому пункті отримуємо результат у вигляді інтерфейсу для взаємодії з застосунком, де ми можемо вказати потрібний час роботи системи керування і відповідно побачимо графік виходу об'єкту та уставки регулятора на одному графіку.

![](RackMultipart20230428-1-rrta77_html_7e4282b4b265e306.png)

Рисунок 20. Інтерфейсвиводу архівних даних на графік

# **Висновки**

В даній лабораторній роботі була поставлена задача розробити візуалізацію роботи контору керування та управління ним у середовищі Node-red. За допомогою візуалізації можна змінювати технологічні параметри регулятора та керува ти ВМ у ручному режимі. Також потрібно було забезпечити зберігання даних про роботу розробленого раніше контуру керування в базі даних часових рядів InfluxDB. Для цього було розроблено структуру з полями та тегами для зберігання даних, з налаштуванням зберігання та параметрів доступу до бази даних, із тривалістю зберігання даних протягом 1 місяця.

Для візуалізації динаміки роботи контуру керування на графіках Node-RED використали dashboard, зчитували та візуально відображали дані про роботу системи керування з БД InfluxDB за вибраний період часу, і відображали показники технологічних змінних на графіках Node-RED.

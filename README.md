# Задание на курсовую работу
---

1. Анализ предметной области. Постановка задачи.

    1.1. Описание предметной области и функции решаемых задач.

    1.2. Перечень входных данных.

    1.3. Перечень выходных данных

    1.4. Ограничения предметной области (если таковые имеются).

    1.5. Взаимодействие с другими программами.

3. Инфологическая (концептуальная) модель базы данных.

    2.1. Выделение информационных объектов.

    2.2. Определение атрибутов объектов.

    2.3. Определение отношений и мощности отношений между объектами.

    2.4. Построение концептуальной модели.

4. Логическая структура БД.

5. Физическая структура базы данных.

6. Реализация проекта в среде конкретной СУБД.

    5.1. Создание таблиц.

    5.2. Создание запросов.

    5.3. Разработка интерфейса.

    5.4. Назначение прав доступа.

    5.5. Создание индексов.

    5.6. Разработка стратегии резервного копирования базы данных


# 1. Анализ предметной области. Постановка задачи
---
### 1.1. Описание предметной области и функции решаемых задач
Коммерческий учебный центр предоставляет услуги по дополнительному образованию: курсы, тренинги, мастер-классы. Учебный процесс предполагает взаимодействие следующих сторон:
- слушателей (учеников),
- преподавателей,
- администраторов центра,
- учебных групп и курсов.

Основные функции:
- Ведение учёта клиентов (слушателей),
- Формирование учебных групп,
- Назначение преподавателей на курсы,
- Учёт расписания и посещаемости,
- Учёт оплаты за обучение,
- Формирование отчетов для анализа эффективности деятельности.

### 1.2. Перечень входных данных
- Персональные данные слушателей (ФИО, телефон, email),
- Данные преподавателей (ФИО, специализация),
- Курсы (название, описание, длительность, стоимость),
- Расписание занятий (дата, время, аудитория),
- Информация об оплате (дата, сумма, способ оплаты),
- Группы (состав, назначенные курсы и преподаватели).
    
### 1.3. Перечень выходных данных
- Список слушателей по курсам,
- Расписание занятий,
- Список преподавателей с назначенными курсами,
- Отчёт по оплате и задолженностям,
- Аналитические отчеты: популярность курсов, посещаемость.

### 1.4. Ограничения предметной области
- Один слушатель может быть записан на несколько курсов одновременно.
- Преподаватель может вести несколько курсов, но не одновременно по времени.
- Оплата может вноситься частями.
- В одной группе — ограниченное число слушателей (например, не более 15).

### 1.5. Взаимодействие с другими программами
- Возможность экспорта отчётов в формат Excel/PDF.
- Интеграция с почтовыми сервисами для уведомлений.
- Возможность синхронизации с календарями (Google Calendar, Outlook).

# 2. Инфологическая (концептуальная) модель базы данных
### 2.1. Выделение информационных объектов
На основе анализа предметной области можно выделить следующие основные информационные объекты:
- **Слушатель** – пользователь, записывающийся на обучение.
- **Преподаватель** – сотрудник, ведущий один или несколько курсов.
- **Курс** – учебная программа с определённой продолжительностью и стоимостью.
- **Группа** – совокупность слушателей, обучающихся по курсу.
- **Занятие** – конкретная дата/время проведения курса.
- **Оплата** – информация о платеже за обучение.
- **Регистрация на курс** – связывает слушателя с конкретным курсом/группой.

### 2.2. Определение атрибутов объектов
#### Слушатель:
- ID_слушателя (PK)
- ФИО
- Телефон
- Email
- Дата регистрации
#### Преподаватель:
- ID_преподавателя (PK)
- ФИО
- Телефон
- Email
- Специализация
#### Курс:
- ID_курса (PK)
- Название
- Описание
- Длительность (в часах)
- Стоимость
#### Группа:
- ID_группы (PK)
- Название группы
- ID_курса (FK)
- ID_преподавателя (FK)
- Максимальное количество слушателей
#### Занятие:
- ID_занятия (PK)
- ID_группы (FK)
- Дата
- Время начала
- Время окончания
- Аудитория
#### Оплата:
- ID_платежа (PK)
- ID_слушателя (FK)
- Дата оплаты
- Сумма
- Способ оплаты (наличные, карта и т.д.)
#### Регистрация:
- ID_регистрации (PK)
- ID_слушателя (FK)
- ID_группы (FK)
- Дата регистрации

### 2.3. Определение отношений и мощности отношений между объектами
- Один **слушатель** может быть зарегистрирован в несколько **групп** (1:M).
- Каждая **группа** связана с одним **курсом** и одним **преподавателем** (M:1).
- Один **преподаватель** может вести много **групп** (1:M).
- Один **курс** может преподаваться в нескольких **группах** (1:M).
- Каждая **группа** может иметь несколько **занятий** (1:M).
- Один **слушатель** может иметь несколько **оплат** (1:M).
- Каждое **занятие** относится к одной **группе** (M:1).

### 2.4. Построение концептуальной модели (ER-диаграммы)
![image](https://github.com/lenka228n/gavgav/blob/main/lenka.png)

# 3. Логическая структура БД
Логическая структура — это реализация концептуальной модели в терминах реляционной модели. Ниже — таблицы с первичными и внешними ключами:

#### Таблица: Слушатель
    CREATE TABLE listener (
        id_listener SERIAL PRIMARY KEY,
        full_name VARCHAR(100),
        phone VARCHAR(20),
        email VARCHAR(100),
        registration_date DATE
    );
#### Таблица: Преподаватель
    CREATE TABLE teacher (
        id_teacher SERIAL PRIMARY KEY,
        full_name VARCHAR(100),
        phone VARCHAR(20),
        email VARCHAR(100),
        specialization VARCHAR(100)
    );
#### Таблица: Курс
    CREATE TABLE course (
        id_course SERIAL PRIMARY KEY,
        name VARCHAR(100),
        description TEXT,
        duration INT,
        price DECIMAL(10, 2)
    );
#### Таблица: Группа
    CREATE TABLE group_table (
        id_group SERIAL PRIMARY KEY,
        group_name VARCHAR(50),
        id_course INT,
        id_teacher INT,
        max_students INT,
        FOREIGN KEY (id_course) REFERENCES course(id_course),
        FOREIGN KEY (id_teacher) REFERENCES teacher(id_teacher)
    );
#### Таблица: Занятие
    CREATE TABLE lesson (
        id_lesson SERIAL PRIMARY KEY,
        id_group INT,
        lesson_date DATE,
        start_time TIME,
        end_time TIME,
        classroom VARCHAR(50),
        FOREIGN KEY (id_group) REFERENCES group_table(id_group)
    );
#### Таблица: Регистрация
    CREATE TABLE registration (
        id_registration SERIAL PRIMARY KEY,
        id_listener INT,
        id_group INT,
        registration_date DATE,
        FOREIGN KEY (id_listener) REFERENCES listener(id_listener),
        FOREIGN KEY (id_group) REFERENCES group_table(id_group)
    );
#### Таблица: Оплата
    CREATE TABLE payment (
        id_payment SERIAL PRIMARY KEY,
        id_listener INT,
        payment_date DATE,
        amount DECIMAL(10,2),
        payment_method VARCHAR(50),
        FOREIGN KEY (id_listener) REFERENCES listener(id_listener)
    );

# 4. Физическая структура базы данных
Физическая структура зависит от используемой СУБД. В рамках проекта используется DBeaver (возможно, с PostgreSQL или MySQL). Ключевые моменты:

- Индексы создаются по основным ключам и внешним ключам для ускорения JOIN-операций.

- Типы данных выбраны оптимально по размеру.

- Используются ограничения целостности:

  - `PRIMARY KEY`, `FOREIGN KEY`, `NOT NULL`.

- Применяются текстовые типы (`VARCHAR`, `TEXT`) для хранения строк.

- Числовые и денежные типы (`INT`, `DECIMAL`) — для ID и сумм.

# 5. Реализация проекта в среде конкретной СУБД
### 5.1. Создание таблиц
    CREATE TABLE listener (
        id_listener SERIAL PRIMARY KEY,
        full_name VARCHAR(100),
        phone VARCHAR(20),
        email VARCHAR(100),
        registration_date DATE
    );
    
    CREATE TABLE teacher (
        id_teacher SERIAL PRIMARY KEY,
        full_name VARCHAR(100),
        phone VARCHAR(20),
        email VARCHAR(100),
        specialization VARCHAR(100)
    );
    
    CREATE TABLE course (
        id_course SERIAL PRIMARY KEY,
        name VARCHAR(100),
        description TEXT,
        duration INT,
        price DECIMAL(10, 2)
    );
    
    CREATE TABLE group_table (
        id_group SERIAL PRIMARY KEY,
        group_name VARCHAR(50),
        id_course INT,
        id_teacher INT,
        max_students INT,
        FOREIGN KEY (id_course) REFERENCES course(id_course),
        FOREIGN KEY (id_teacher) REFERENCES teacher(id_teacher)
    );
    
    CREATE TABLE lesson (
        id_lesson SERIAL PRIMARY KEY,
        id_group INT,
        lesson_date DATE,
        start_time TIME,
        end_time TIME,
        classroom VARCHAR(50),
        FOREIGN KEY (id_group) REFERENCES group_table(id_group)
    );
    
    CREATE TABLE registration (
        id_registration SERIAL PRIMARY KEY,
        id_listener INT,
        id_group INT,
        registration_date DATE,
        FOREIGN KEY (id_listener) REFERENCES listener(id_listener),
        FOREIGN KEY (id_group) REFERENCES group_table(id_group)
    );
    
    CREATE TABLE payment (
        id_payment SERIAL PRIMARY KEY,
        id_listener INT,
        payment_date DATE,
        amount DECIMAL(10,2),
        payment_method VARCHAR(50),
        FOREIGN KEY (id_listener) REFERENCES listener(id_listener)
    );

### Вставки в таблицу
    INSERT INTO listener (full_name, phone, email) VALUES
    ('Ivanov Ivan Ivanovich', '89101234567', 'ivanov@mail.ru'),
    ('Petrova Anna Sergeevna', '89107654321', 'petrova@mail.ru'),
    ('Sidorov Aleksei Petrovich', '89103456789', 'sidorov@mail.ru'),
    ('Kuznetsova Maria Igorevna', '89105678901', 'kuznetsova@mail.ru'),
    ('Orlov Nikolai Andreevich', '89107890123', 'orlov@mail.ru');
    
    INSERT INTO teacher (full_name, phone, email, specialization) VALUES
    ('Smirnov Andrei Pavlovich', '89201231231', 'smirnov@mail.ru', 'Programming'),
    ('Egorova Tatiana Nikolaevna', '89204564564', 'egorova@mail.ru', 'Design'),
    ('Novikov Oleg Viktorovich', '89207897897', 'novikov@mail.ru', 'Marketing'),
    ('Vasilieva Inna Sergeevna', '89206786786', 'vasilieva@mail.ru', 'Accounting'),
    ('Makarov Ilya Denisovich', '89205675675', 'makarov@mail.ru', 'Data Analytics');
    
    INSERT INTO course (name, description, duration, price) VALUES
    ('Python for Beginners', 'Python basics, syntax, data types', 40, 15000.00),
    ('Basics of Web Design', 'Working with Figma, UI/UX principles', 36, 18000.00),
    ('Internet Marketing', 'SEO, SMM, contextual advertising', 30, 20000.00),
    ('1C:Accounting', 'Accounting in 1C', 50, 22000.00),
    ('Power BI and Data Visualization', 'BI tools, reports, dashboards', 45, 25000.00);
    
    INSERT INTO group_table (group_name, id_course, id_teacher, max_students) VALUES
    ('Python-1', 1, 1, 10),
    ('WebDesign-A', 2, 2, 12),
    ('Marketing-B', 3, 3, 8),
    ('Buh-Group', 4, 4, 10),
    ('PowerBI-5', 5, 5, 15);
    
    INSERT INTO lesson (id_group, lesson_date, start_time, end_time, classroom) VALUES
    (1, '2025-04-20', '10:00', '12:00', '101'),
    (2, '2025-04-21', '14:00', '16:00', '202'),
    (3, '2025-04-22', '16:00', '18:00', '303'),
    (4, '2025-04-23', '09:00', '11:00', '404'),
    (5, '2025-04-24', '11:00', '13:00', '505');
    
    INSERT INTO registration (id_listener, id_group) VALUES
    (1, 1),
    (2, 2),
    (3, 3),
    (4, 4),
    (5, 5);
    
    INSERT INTO payment (id_listener, payment_date, amount, payment_method) VALUES
    (1, '2025-04-01', 15000.00, 'Card'),
    (2, '2025-04-02', 18000.00, 'Cash'),
    (3, '2025-04-03', 20000.00, 'Sberbank Online'),
    (4, '2025-04-04', 22000.00, 'Card'),
    (5, '2025-04-05', 25000.00, 'Cash');


### 5.2. Создание запросов
Примеры типовых SQL-запросов:
#### • ㅤСписок слушателей по курсу:
    SELECT Слушатель.ФИО, Курс.Название
    FROM Регистрация
    JOIN Слушатель ON Регистрация.ID_слушателя = Слушатель.ID_слушателя
    JOIN Группа ON Регистрация.ID_группы = Группа.ID_группы
    JOIN Курс ON Группа.ID_курса = Курс.ID_курса;
#### • ㅤСумма оплат по каждому слушателю:
    SELECT Слушатель.ФИО, SUM(Оплата.Сумма) AS Общая_сумма
    FROM Оплата
    JOIN Слушатель ON Оплата.ID_слушателя = Слушатель.ID_слушателя
    GROUP BY Слушатель.ФИО;
#### • ㅤРасписание занятий конкретной группы:
    SELECT Занятие.Дата, Занятие.Время_начала, Занятие.Аудитория
    FROM Занятие
    WHERE ID_группы = 1
    ORDER BY Занятие.Дата, Занятие.Время_начала;
    
### 5.3. Разработка интерфейса
![image](https://github.com/lenka228n/gavgav/blob/main/foto/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%20%D0%BE%D1%82%202025-04-24%2012-19-45.png)
![image](https://github.com/lenka228n/gavgav/blob/main/foto/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%20%D0%BE%D1%82%202025-04-24%2012-19-48.png)
![image](https://github.com/lenka228n/gavgav/blob/main/foto/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%20%D0%BE%D1%82%202025-04-24%2012-19-50.png)
![image](https://github.com/lenka228n/gavgav/blob/main/foto/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%20%D0%BE%D1%82%202025-04-24%2012-19-55.png)
![image](https://github.com/lenka228n/gavgav/blob/main/foto/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%20%D0%BE%D1%82%202025-04-24%2012-19-57.png)
![image](https://github.com/lenka228n/gavgav/blob/main/foto/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%20%D0%BE%D1%82%202025-04-24%2012-20-02.png)
![image](https://github.com/lenka228n/gavgav/blob/main/foto/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%20%D0%BE%D1%82%202025-04-24%2012-20-06.png)
![image](https://github.com/lenka228n/gavgav/blob/main/foto/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%20%D0%BE%D1%82%202025-04-24%2012-20-08.png)
![image](https://github.com/lenka228n/gavgav/blob/main/foto/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%20%D0%BE%D1%82%202025-04-24%2012-20-11.png)
![image](https://github.com/lenka228n/gavgav/blob/main/foto/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%20%D0%BE%D1%82%202025-04-24%2012-20-14.png)

## Разработка стратегии резервного копирования базы данных

Для защиты данных от сбоев и потерь необходимо разработать стратегию регулярного резервного копирования. Для PostgreSQL основными методами являются:

- **Ежедневное логическое резервное копирование с помощью pg_dump** — позволяет создавать бэкап базы данных в формате SQL. Эти резервные копии можно хранить на удаленном сервере или в облаке.

Пример ежедневного резервного копирования:
```
pg_dump -U inventorybd  -F c -b -v -f "/backups/backup_$(date +\%Y\%m\%d).backup" inventorybd 
```
- **Полное физическое резервное копирование раз в неделю с использованием pg_basebackup** — особенно полезно для больших объемов данных, так как обеспечивает быстрое восстановление базы данных.

Пример команды:
```
pg_basebackup -U inventorybd  -D /path/to/backup -Ft -z -P
```
- **Проверка и тестирование восстановлений** — резервное копирование должно регулярно тестироваться на восстановление, чтобы убедиться в работоспособности резервных копий. Это критически важно для обеспечения постоянной доступности и надежности данных.

Стратегия резервного копирования должна включать хранение нескольких копий данных на случай различных инцидентов, таких как сбой оборудования или ошибка администратора.

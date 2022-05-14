МІНІСТЕРСТВО ОСВІТИ І НАУКИ УКРАЇНИ

Національний Технічний Університет України

«Київський Політехнічний Інститут»

Факультет інформатики та обчислювальної техніки

Кафедра обчислювальної техніки





**Лабораторна робота №5**

з дисципліни «Організація баз даних»

**на тему: «Реалізація бази даних засобами MySQL»**




**Виконали:**

студенти групи ІО-з01

Кузьменко Ю.А. Ермоленко Т.Г.


**Перевірив:**

Доцент 

Кандидат технічних наук

Болдак О.А







**Київ – 2022**



**МЕТА:** навчитися встановлювати та налаштовувати системи розробки баз даних MySQL та Oracle SQL Developer.

**ДОВІДКА**

**Встановлення та налаштування серверу баз даних**

Сервер БД обслуговує базу даних і відповідає за цілісність і збереження даних, а також забезпечує операції введення-виведення при доступі клієнта до інформації.

Архітектура клієнт-сервера складається з клієнтів і серверів. Основна ідея полягає в тому, щоб розміщувати сервери на потужних машинах, а додаткам, що використовують мовні компоненти СУБД, забезпечити доступ до них з менш потужних машин-клієнтів за допомогою зовнішніх інтерфейсів.

Більшість СУБД використовують мову SQL (Structured Query Language - мова структурованих запитів), так як вона зручна для опису логічних підмножин БД.

Призначення SQL:

- Створення БД і таблиці з повним описом їх структури;
- Виконання основних операцій маніпулювання даними (такі як вставка, модифікація і видалення даних з таблиць);
- Виконання простих і складних запитів.

Одна з ключових особливостей мови SQL полягає в тому, що з її допомогою формуються запити, що описують яку інформацію з бази даних необхідно отримати, а шляхи вирішення цього завдання програма визначає сама.

**MySQL**

MySQL - вільна система управління базами даних (СКБД). MySQL є власністю компанії Oracle Corporation, що отримала її разом з поглинанням Sun Microsystems, що здійснює розробку і підтримку програми. Розповсюджується під GNU General Public License або під власною комерційною ліцензією.

Гнучкість СУБД MySQL забезпечується підтримкою великої кількості типів таблиць: користувачі можуть вибрати як таблиці типу MyISAM, що підтримують повнотекстовий пошук, так і таблиці InnoDB, що підтримують транзакції на рівні окремих записів. Більш того, СУБД MySQL поставляється із спеціальним типом таблиць EXAMPLE, що демонструє принципи створення нових типів таблиць. Завдяки відкритій архітектурі і GPL-ліцензуванню, в СУБД MySQL постійно з'являються нові типи таблиць.

MySQL портована на велику кількість платформ: AIX, BSDi, FreeBSD, HP-UX, Linux, Mac OS X, NetBSD, OpenBSD, OS / 2 Warp, SGI IRIX, Solaris, SunOS, SCO OpenServer, SCO UnixWare, Tru64, Windows 95, Windows 98, Windows NT, Windows 2000, Windows XP, Windows Server 2003, WinCE, Windows Vista і Windows 7.

MySQL має API для мов Delphi, C, C + +, Ейфель, Java, Лісп, Perl, PHP, PureBasic, Python, Ruby, Smalltalk, Компонентний Паскаль і Tcl бібліотеки для мов платформи. NET

**Завдання**

Вивчити основи мови структурних запитів SQL, недоліки та переваги мови, цілі створення, стандарт-ревізії, сумісність мови.

Детально вивчити основні команди SQL та їх особливості. Вільно володіти такими командами Select, Insert, Update.

Створити базу даних. Виконати зміну бази по варіанту.

**ВИКОНАННЯ ЗАВДАНЬ**

**База даних для системи управління проектами**

create database Priority;

create table Priority.Dashboard(

Id int primary  key,
 
TasksCount int,
 
ProjectTeamLead varchar (50)
 
);
 
create table Priority.TaskColumn(

ColumnID int primary key,

ColumnName varchar (50),

TaskCount int,

ProjectID int,

foreign key (ProjectID) references Dashboard(Id)

);
 
create table Priority.UserInfo(

UserId int primary key,
 
Username char(50),
 
ProjectId int,
 
Email varchar (50),
 
Position varchar (50),
 
foreign key (ProjectId) references Dashboard(Id)
 
);
  
create table Priority.Task(

TaskId int primary key,
  
TaskName varchar(50),
  
TaskInformation varchar (256),
  
PartisipantID int,
  
CreatorID int ,
  
Period DateTime,
  
Cooment varchar (256),
  
ColumnID int,
  
ProjectID int,
  
foreign key (CreatorID) references UserInfo(UserId),
  
foreign key (PartisipantID) references UserInfo(UserId),
  
foreign key (ProjectID) references Dashboard(Id),
  
foreign key (ColumnID) references TaskColumn(ColumnID)
  
);
  
-- change a datatype of the table's column

USE priority;

ALTER TABLE Dashboard MODIFY Id BIGINTEGER;

ALTER TABLE TaskColumn MODIFY ColumnID BIGINTEGER;

ALTER TABLE Task MODIFY TaskId BIGINTEGER;

-- how to delete the table

DROP table Priority.Dashboard;

DROP table Priority.TaskColumn;

DROP table Priority.UserInfo;

DROP table Priority.Task;

-- to insert multiple values into the table

INSERT INTO Priority.Dashboard(Id, TasksCount, ProjectTeamLead)

values(1, 1, 'Andry Melnik'),

(2, 2, 'Mary Klymuk'),

(3, 3, 'Volodymyr Borsuk');

INSERT INTO Priority.TaskColumn(ColumnID, ColumnName, TaskCount, ProjectID)

values (1,'Need to do', 1, 1), 

(2, 'Need to check', 1, 2), 

(3,'Ready tasks', 1, 3);

INSERT INTO Priority.UserInfo(UserId, Username, ProjectId, Email, Position)

values(1,  'Anna Sivyuk', 1, 'annasyvuk23@gmail.com', 'QA'),

(2, 'Olga Chaika', 2, 'olgachai1993@ukr.net', 'QA'),

(3, 'Maxim Chalikh', 2, 'chalikhmax@ukr.net', 'System Administration'),

(4, 'Christina Pustovit', 3, 'pustovitchris87@gmail.com', 'Python Developer'),

(5, 'Artem Lozovy', 3, 'loz_artem@gmail.com', 'DevOps');

-- to update data in the table column

use Priority

SELECT *

FROM TaskColumn

WHERE ColumnName=3;

UPDATE TaskColumn

SET ColumnName = 'Completed Tasks'

WHERE ProjectID=3;

-- Task

INSERT INTO Priority.Task(TaskId, TaskName, TaskInformation, PartisipantID, CreatorID, Period, Cooment, ColumnID, ProjectID)

values(1, 'Notifications', 'Set up notification', 5, 1, '2022/05/20', 'Configure application error notifications', 1, 3),

(2, 'Test', 'Do non-functional testing', 1, 3, '2022/05/20', 'Conduct non-functional testing of the project using load testing, stress testing, stability or reliability testing, volume testing', 2, 1),

(3, 'Test', 'Do functional testing', 1, 1, '2022/05/18', 'Conduct functional testing of the project using functional testing, security and access control testing, interoperability ', 2, 2),

(4, 'Develop a discount scheme', 'Develop a mechanism for setting discounts taking into account the specifics of the enterprise', 4, 2, '2022/05/22', 'Create a "supplier product groups" information register and develop a "discount management" processing', 1, 2 );


-- to show tasks of every user in database (if every user made only one order)

use priority;

SELECT *

FROM Task t

JOIN UserInfo ui

ON t.PartisipantID = ui.UserId

![изображение](https://user-images.githubusercontent.com/97729953/168430171-92809e95-5fe9-4641-acf9-4b346f822381.png)

**Результат**

![изображение](https://user-images.githubusercontent.com/97729953/168430210-989e1401-13aa-473e-af64-8014019dbb8e.png)

**ВИСНОВКИ**

Навчилися встановлювати та налаштовувати системи розробки баз даних MySQL та Oracle SQL Developer а також створювати бази даних.

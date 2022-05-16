МІНІСТЕРСТВО ОСВІТИ І НАУКИ УКРАЇНИ

Національний Технічний Університет України

«Київський Політехнічний Інститут»

Факультет інформатики та обчислювальної техніки

Кафедра обчислювальної техніки





**Лабораторна робота №6**

з дисципліни «Організація баз даних»

**на тему: «Реалізація об’єктно-реляційного відображення»**




**Виконали:**

студенти 2-го курсу ФІОТ

групи ІО-з01

Кузьменко Юлія, Єрмоленко Тимур


**Перевірив:**

Доцент 

Кандидат технічних наук

Болдак О.А







**Київ – 2022**



**МЕТА:** Отримання навичок зі створення DAO-інфраструктури для доступу до MySQL баз даних.

**ДОВІДКА**

***DAO*** - об'єкт що надає абстрактний інтерфейс до деяких видів баз даних чи механізмів персистентності реалізуючи певні операції без розкриття деталей бази даних. Він надає відображення від програмних викликів до рівня персистентності. Така ізоляція розділює запити до даних в термінах предметної області та їх реалізацію засобами СКБД.

Персистентність - здатність стану існувати довше, ніж процес, що створив його. Без цієї можливості, стан може існувати лише в оперативній пам'яті і втрачається, коли оперативна пам'ять вимикається, наприклад, при вимкненні комп'ютера.


***Active Record*** - це шаблон проектування, що використовується при реалізації доступу до реляційних баз даних. Вперше згадується Мартіном Фаулером в книжці Patterns of Enterprise Application Architecture. Цей шаблон є підвидом DAO, але навідміну від нього, він надає CRUD API кожному окремому об'єкту, який репрезентує окремий запис в таблиці БД.

**Завдання**

1. Ознайомитись з призначенням шаблону Data Access Object
1. Створити базу даних у MySQL
1. Створити bean-класи по одному для кожної таблиці, які складаються з опису одного рядка таблиці.
1. Розробити DAO-інфраструктуру для роботи з базою даних.
1. Розробити програму для тестування можливостей DAO, яка створює деякі об’єкти в кожній таблиці та шукає існуючи їх по деяким ознакам.
1. Запустити програму та продивитись результати її роботи у MySQL.

ВИКОНАННЯ ЗАВДАНЬ

Створена база у MySQL

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

**DAO у цьому випадку грає роль сервер Node.js**

**Запит у базу даних код продемонстрований нижче**

const express = require("express");

const app = express();

const mysql = require('mysql');

const pool = mysql.createPool({

host : 'localhost',

user : 'Timur',

password : 'password1',

database : 'Priority'

});

app.get("/",(req,res) => {

pool.getConnection((err, connection) => {

if(err) throw err;

console.log('connected as id ' + connection.threadId);

connection.query('SELECT * from Priority.Task LIMIT 1', (err, rows) => {

connection.release(); // return the connection to pool

if(err) throw err;

console.log('The data from tasks table are: \n', rows);

res.send(rows)

});

});

});

app.get("/Task/:id", (req, res) => {

pool.getConnection((err, connection) => {

if(err) throw err;

connection.query(SELECT * from  Priority.Task WHERE TaskId = ${req.params.id}, (err, rows) => {

connection.release(); // return the connection to pool

if(err) throw err;

res.send(rows[0])

});

});

})

app.get("/Task", (req, res) => {

pool.getConnection((err, connection) => {

if(err) throw err;

connection.query(SELECT * from Priority.Task, (err, rows) => {

connection.release(); // return the connection to pool

if(err) throw err;

res.send(rows)

});

});

})

app.listen(3000, () => {

console.log('Server is running at port 3000');

});

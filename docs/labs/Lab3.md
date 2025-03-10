МІНІСТЕРСТВО ОСВІТИ І НАУКИ УКРАЇНИ

«Київський політехнічний інститут імені Ігоря Сікорського»

Факультет інформатики та обчислювальної техніки

Кафедра обчислювальної техніки 


**Лабораторна робота №3**

з дисципліни «Організація баз даних»

**на тему: «Розробка моделі бізнес-об’єктів»**


**Виконали:**

студенти групи ІО-з01

Кузьменко Ю.А. Ермоленко Т.Г.

**Перевірив:**

Болдак О.А.





**Київ 2022**

**Мета:** Зрозуміти сутність бізнес-моделювання. Отримати навички розробки моделі бізнес-об’єктів та словника предметної області.

**Довідка**

***Бізнес-моделювання (ділове моделювання)*** - діяльність з формування моделей організацій, що включає опис ділових об'єктів (підрозділів, посад, ресурсів, ролей, процесів, операцій, інформаційних систем, носіїв інформації і т.д.) і вказівка зв'язків між ними. Вимоги до моделей, що формуються та їх відповідне утримання визначаються цілями моделювання.

Бізнес-моделюванням також називають дисципліну і окремий підпроцес в процесі розробки програмного забезпечення, в якому описується діяльність компанії та визначаються вимоги до системи - ті підпроцеси та операції, що підлягають автоматизації розробки в інформаційній системі.

***Бізнес-об’єкт*** представляє значну і постійну частину інформації, керованої бізнес-акторами і виконавцями. Бізнес-об’єкти пасивні, тобто, вони не ініціюють взаємодії самі по собі. Бізнес-об’єкт може бути використаний в безлічі різних реалізацій бізнес-прецедентів і зазвичай реагує на будь-яку одиничну взаємодію. Бізнес-об’єкти забезпечують основу для поділу інформації (потоку документів) серед виконавців, що беруть участь в різних реалізаціях бізнес-прецедентів.

Бізнес-об’єкти являють абстракцію важливої постійної інформації в бізнес-системі. Будь-яка частина інформації, яка є властивістю чого-небудь ще, ймовірно не є бізнес-об’єктом в дійсності.

Бізнес-аналітик відповідає за ідентифікацію та опис бізнес-об’єктів, а також за визначення впливу організаційних змін на інформацію, створену і необхідну бізнес-системою. Бізнес-об’єкти також використовуються системними аналітиками та дизайнерами при описі системних прецедентів та ідентифікації програмних об’єктів відповідно.

Метою моделювання бізнес-об’єктів та їх станів є використання моделей бізнес-об’єктів та їх станів при проектуванні переліку вхідних / вихідних сигналів і даних, інтерфейсу користувача, баз даних (БД), класів, які реалізують функції системи.

Бізнес-об’єкт (business entity), представляє абстракцію суті реального світу. Прикладами бізнес-об’єктів можуть бути заявка на кредитування, договір, угода і т.д.

Модель з описом бізнес-об’єктів повинна будуватися на основі опису бізнес-процесів. Бізнес-об’єкти повинні моделюватися в розбивці по бізнес-процесам. При створенні програмної системи моделюватися повинні тільки бізнес-об’єкти, пов'язані з діяльностями, що підлягають автоматизації.

Для створення опису документів / бізнес-об’єктів використовується компоненти діаграми класів / функцій:

- пакет (package);
- бізнес сутність (business entity);
- асоціативний зв'язок (association);
- зв'язок агрегація (agregation);
- зв'язок композиція (composition);
- зв'язок спадкування або батько нащадок (generalization).

***Пакет*** використовується для групування бізнес об’єктів.

**Завдання**

1.Вивчити основні концепції бізнес моделювання. Вільно володіти термінологією: бізнес-об’єкт, словник предметної області, їх сутність та призначення.

2.Виділити з Use CASE запити зацікавлених осіб.

3.Виділити об'єкти, над якими відбуваються дії, та їхні атрибути.

4.Визначити співвідношення між вище згаданими компонентами

5.Зробити розбивку на пакети, якщо це необхідно.

6.Побудувати діаграму бізнес-об’єктів.

7.Розробити словник предметної області

**ВИКОНАННЯ ЗАВДАНЬ**

Взаємодія користувача з сервісом

![photo_2022-05-09_17-51-46](https://user-images.githubusercontent.com/97729272/167437065-617ec415-fb1a-4283-91d2-0fb2953cc136.jpg)


Якщо брати за модель взаємовідносини між усіма акторами бізнес-моделі можна побачити, що обмін інформацією відбувається наступним чином:

Користувач → сервіс

Користувач публікує завдання на сервісі, а сервіс в свою чергу зберігає дані про завдання та відображає їх користувачу.

Обмін інформацією керівника з виконавцем

![photo_2022-05-09_17-51-46 (2)](https://user-images.githubusercontent.com/97729272/167437257-1afcdfb0-b449-4da7-b304-1688f88522ec.jpg)

Керівник → сервіс → виконавець


Керівник додає завдання на сервіс. Сервіс зберігає завдання та відображає його виконавцю. Виконавець в свою чергу бачить додане завдання та виконує його. Після виконання завдань зберігає і таким чином керівник може прослідкувати виконання, активність та додані коментарі. 

Об’єкти над якими відбуваються дії

Окремо слід виділити об’єкти системи управління проектами над якими відбуваються дії в процесі створення завдань, або дощок, які мають окремі інструменти, що потрібні користувачу для додавання інформації.

Співвідношення між інструментами для додавання завдань у системі управління проектами.

![photo_2022-05-09_17-51-47](https://user-images.githubusercontent.com/97729272/167437440-d744245b-d00f-4b51-a993-ab687e09093e.jpg)

На діаграмі позначено дії додавання списків та створення завдань. 

Після додавання користувача до дошки, йому на електронну пошту надходить лист з повідомленням, що його додали до списку з завданням. Дошка з’являться в його особистому кабінеті. 
 
 ![photo_2022-05-09_17-51-48](https://user-images.githubusercontent.com/97729272/167437498-dee813b8-3f79-453c-814d-06a1b73e22d4.jpg)

## **Словник предметної області**

***Дошка*** - це сторінка, де розміщені колонки.

***Колонки*** - місце для розміщення карток з завданнями.

***Картка*** - це поле для розміщення завдань.

***Опис*** - докладний коментар, що саме потрібно зробити у завданні.

***Учасники*** - користувачі, яким відображається список із завданнями, які потрібно виконати. 

***Дата*** - час, за який завдання має бути зробленим.

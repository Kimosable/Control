1)
Создаем файл «домашние животные»
majestic@gb-linux:~$ cat > home_animals
dogs,cats,hamster
Создаем файл «вьючные животные»
majestic@gb-linux:~$ cat > pack_animal
Horses,camel,donkey
Объединяем в один файл, назовём его «животные»
majestic@gb-linux:~$ cat home_animals pack_animal > animals
Просмотрим содержимое
majestic@gb-linux:~$ cat animals
Переименуем файл в «друзья человека»
majestic@gb-linux:~$ mv animals people_friends

2)
Создаём новую директорию
majestic@gb-linux:~$ mkdir homework
Перемещаем файл в новую директорию
majestic@gb-linux:~$ mv people_friends ~/homework

3)Подключить дополнительный репозиторий MySQL. Установить любой пакет из этого репозитория. 

majestic@gb-linux:~$ sudo wget https://dev.mysql.com/get/mysql-apt-config_0.8.23-1_all.deb

4)Установить и удалить deb-пакет с помощью dpkg
majestic@gb-linux:~$ sudo dpkg -i mysql-apt-config_0.8.23-1_all.deb
majestic@gb-linux:~$ sudo apt-get update
majestic@gb-linux:~$ sudo apt-get install mysql-server

# 7 В подключенном MySQL репозитории создать базу данных “Друзья человека”
CREATE DATABASE people_friends;
# 8 Создать таблицы с иерархией из диаграммы в БД
USE people_friends;
CREATE TABLE animal_classes
(
    Id INT AUTO_INCREMENT PRIMARY KEY, 
    Class_name VARCHAR(20)
);

INSERT INTO animal_classes (Class_name)
VALUES ('вьючные'),
('домашние');  

CREATE TABLE pack_animals
(
      Id INT AUTO_INCREMENT PRIMARY KEY,
    Genus_name VARCHAR (20),
    Class_id INT,
    FOREIGN KEY (Class_id) REFERENCES animal_classes (Id) ON DELETE CASCADE ON UPDATE CASCADE
);

INSERT INTO pack_animals (Genus_name, Class_id)
VALUES ('Лошади', 1),
('Ослы', 1),  
('Верблюды', 1); 
    
CREATE TABLE home_animals
(
      Id INT AUTO_INCREMENT PRIMARY KEY,
    Genus_name VARCHAR (20),
    Class_id INT,
    FOREIGN KEY (Class_id) REFERENCES animal_classes (Id) ON DELETE CASCADE ON UPDATE CASCADE
);

INSERT INTO home_animals (Genus_name, Class_id)
VALUES ('Кошки', 2),
('Собаки', 2),  
('Хомяки', 2); 

CREATE TABLE cats 
(       
    Id INT AUTO_INCREMENT PRIMARY KEY, 
    Name VARCHAR(20), 
    Birthday DATE,
    Commands VARCHAR(50),
    Genus_id int,
    Foreign KEY (Genus_id) REFERENCES home_animals (Id) ON DELETE CASCADE ON UPDATE CASCADE
);
# 9 Заполнить низкоуровневые таблицы именами(животных), командами которые они выполняют и датами рождения.

INSERT INTO cats (Name, Birthday, Commands, Genus_id)
VALUES ('Васька', '2012-03-11', 'Сюда', 1),
('Тося', '2012-09-22', "Нельзя!", 1),  
('Мурка', '2014-12-01', "кс-кс", 1); 

CREATE TABLE dogs 
(       
    Id INT AUTO_INCREMENT PRIMARY KEY, 
    Name VARCHAR(20), 
    Birthday DATE,
    Commands VARCHAR(50),
    Genus_id int,
    Foreign KEY (Genus_id) REFERENCES home_animals (Id) ON DELETE CASCADE ON UPDATE CASCADE
);
INSERT INTO dogs (Name, Birthday, Commands, Genus_id)
VALUES ('Шарик', '2021-01-03', 'ко мне, лежать, лапу, голос', 2),
('Брабус', '2021-09-11', "сидеть, лежать, лапу", 2),  
('Чарли', '2018-09-01', "сидеть, лежать, лапу, след, фас", 2), 
('Бобик', '2020-12-20', "сидеть, лежать, фу, место", 2);

CREATE TABLE hamsters 
(       
    Id INT AUTO_INCREMENT PRIMARY KEY, 
    Name VARCHAR(20), 
    Birthday DATE,
    Commands VARCHAR(50),
    Genus_id int,
    Foreign KEY (Genus_id) REFERENCES home_animals (Id) ON DELETE CASCADE ON UPDATE CASCADE
);
INSERT INTO hamsters (Name, Birthday, Commands, Genus_id)
VALUES ('Виталя', '2024-01-02', 'сиди', 3),
('Петрушка', '2023-01-15', "кушай", 3),  
('Вейл', '2022-04-03', 'домой', 3), 
('Бурый', '2022--11-09', 'нельзя', 3);

CREATE TABLE horses 
(       
    Id INT AUTO_INCREMENT PRIMARY KEY, 
    Name VARCHAR(20), 
    Birthday DATE,
    Commands VARCHAR(50),
    Genus_id int,
    Foreign KEY (Genus_id) REFERENCES pack_animals (Id) ON DELETE CASCADE ON UPDATE CASCADE
);
INSERT INTO horses (Name, Birthday, Commands, Genus_id)
VALUES ('Барон', '2015-10-22', 'галоп', 1),
('Сивый', '2016-10-24', 'бегом, шагом', 1),  
('Искра', '2021-11-21', 'Стой,пррр', 1), 
('Молния', '2020-11-10', 'прыжок', 1);

CREATE TABLE donkeys 
(       
    Id INT AUTO_INCREMENT PRIMARY KEY, 
    Name VARCHAR(20), 
    Birthday DATE,
    Commands VARCHAR(50),
    Genus_id int,
    Foreign KEY (Genus_id) REFERENCES pack_animals (Id) ON DELETE CASCADE ON UPDATE CASCADE
);
INSERT INTO donkeys (Name, Birthday, Commands, Genus_id)
VALUES ('Иа', '2019-04-10', 'отдых', 2),
('Блиц', '2019-03-03', 'ускориться', 2),  
('Игорь', '2019-03-30', 'домой', 2), 
('Шустрик', '2020-04-18', 'стоп', 2);

CREATE TABLE camels 
(       
    Id INT AUTO_INCREMENT PRIMARY KEY, 
    Name VARCHAR(20), 
    Birthday DATE,
    Commands VARCHAR(50),
    Genus_id int,
    Foreign KEY (Genus_id) REFERENCES pack_animals (Id) ON DELETE CASCADE ON UPDATE CASCADE
);
INSERT INTO camels (Name, Birthday, Commands, Genus_id)
VALUES ('Горб', '2023-01-10', 'вернись', 3),
('Фараон', '2019-03-22', 'сядь', 3),  
('Старый', '2015-09-12', 'ляж', 3), 
('Ветеран', '2022-11-11', 'домой', 3);

# 10 Удалив из таблицы верблюдов, т.к. верблюдов решили перевезти в другой питомник на зимовку. Объединить таблицы лошади, и ослы в одну таблицу.

SET SQL_SAFE_UPDATES = 0;
DELETE FROM camels;

SELECT Name, Birthday, Commands FROM horses
UNION SELECT  Name, Birthday, Commands FROM donkeys;

# 11 Создать новую таблицу “молодые животные” в которую попадут все животные старше 1 года, но младше 3 лет и 
# в отдельном столбце с точностью до месяца подсчитать возраст животных в новой таблице.\

CREATE TABLE young_animal AS
SELECT Name, Birthday, Commands, genus, TIMESTAMPDIFF(MONTH, Birthday, CURDATE()) AS Age_in_month
FROM animals WHERE Birthday BETWEEN ADDDATE(curdate(), INTERVAL -3 YEAR) AND ADDDATE(CURDATE(), INTERVAL -1 YEAR);
 
# 12 Объединить все таблицы в одну, при этом сохраняя поля, указывающие на прошлую принадлежность к старым таблицам.

SELECT h.Name, h.Birthday, h.Commands, pa.Genus_name, ya.Age_in_month 
FROM horses h
LEFT JOIN young_animal ya ON ya.Name = h.Name
LEFT JOIN pack_animals pa ON pa.Id = h.Genus_id
UNION 
SELECT d.Name, d.Birthday, d.Commands, pa.Genus_name, ya.Age_in_month 
FROM donkeys d 
LEFT JOIN young_animal ya ON ya.Name = d.Name
LEFT JOIN pack_animals pa ON pa.Id = d.Genus_id
UNION
SELECT c.Name, c.Birthday, c.Commands, ha.Genus_name, ya.Age_in_month 
FROM cats c
LEFT JOIN young_animal ya ON ya.Name = c.Name
LEFT JOIN home_animals ha ON ha.Id = c.Genus_id
UNION
SELECT d.Name, d.Birthday, d.Commands, ha.Genus_name, ya.Age_in_month 
FROM dogs d
LEFT JOIN young_animal ya ON ya.Name = d.Name
LEFT JOIN home_animals ha ON ha.Id = d.Genus_id
UNION
SELECT hm.Name, hm.Birthday, hm.Commands, ha.Genus_name, ya.Age_in_month 
FROM hamsters hm
LEFT JOIN young_animal ya ON ya.Name = hm.Name
LEFT JOIN home_animals ha ON ha.Id = hm.Genus_id;


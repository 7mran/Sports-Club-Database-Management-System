/* SECTION 1 - CREATE TABLE STATEMENTS */
create table Members
(
memberID integer primary key,
firstName varchar(30),
surname varchar(30),
phoneNumber varchar(11)
);

create table Instructors
(
instructorID integer primary key,
firstName varchar(30),
surname varchar(30),
email varchar(100)
);

create table Sports
(
sportID varchar(30) primary key,
durationYears integer,
fee£ integer
);

create table Certification
(
memberID integer,
instructorID integer,
sportID varchar(30),
assessmentMonth int(2),
assessmentYear int(4),
foreign key (memberID) references Members (memberID),
foreign key (instructorID) references Instructors (instructorID),
foreign key (sportID) references Sports (sportID),
primary key (memberID, instructorID, sportID)
);

/* SECTION 2 - INSERT STATEMENTS */
insert into Members values (001,'Neymar','Jr.',7833321025),
                        (002,'Roberto','Firmino',7982193550),
                        (003,'Sambi','Lokonga',7901325138),    
                        (004,'Edgar','Davids',null),    
                        (005,'Martin','Skertl',7810810626),    
                        (006,'Daniel','Agger',7677483386),    
                        (007,'Nathaniel','Clyne',null),
                        (008,'Rickie','Lambert',7318658068),
                        (009,'Sami','Hyypia',null),
                        (010,'Eric','Djemba-Djemba',7394280700);

insert into Instructors values (001,'Jurgen','Klopp','jk@lfc.com'),
                        (002,'Pep','Guardiola','pg@mcfc.com'),    
                        (003,'Sean','Dyche','sd@burnley.com'),  
                        (004,'Carlo','Ancelotti','ca@real.com'),    
                        (005,'Vincent','Kompany','vk@burnley.com'),    
                        (006,'Ange','Postecoglou','ap@spurs.com'),    
                        (007,'Mikel','Arteta','ma@gunners.com'),    
                        (008,'Xabi','Alonso','xa@leverkusen.com'),    
                        (009,'Thomas','Frank','tf@brentford.com'),
                        (010,'Steven','Gerrard','sg@saudi.com');
                        
insert into Sports values ('Football',7,600),
                            ('Tennis',5,500),
                            ('Basketball',6,400),
                            ('Rugby',2,200),
                            ('Swimming',4,300),
                            ('Badminton',3,700),
                            ('Squash',1,100),
                            ('Croquet',1,150),
                            ('Hockey',3,250),
                            ('Power Lifting',5,335);

insert into Certification values (001,010,'Football',4,2023),
                                    (009,001,'Squash',2,2023),
                                    (003,002,'Swimming',1,2023),
                                    (004,008,'Rugby',12,2022),
                                    (005,009,'Basketball',3,2023),
                                    (006,005,'Tennis',5,2023),
                                    (007,004,'Badminton',7,2022),
                                    (008,006,'Hockey',8,2023),
                                    (009,007,'Badminton',12,2022),
                                    (010,010,'Power Lifting',11,2023),
                                    (010,003,'Swimming',11,2023),
                                    (002,004,'Badminton',7,2022);
                     
/* SECTION 3 - UPDATE STATEMENTS */
UPDATE Members SET phoneNumber = 7418855122
WHERE memberID = 004;

UPDATE Instructors SET email = 'sd@everton.com'
WHERE instructorID = 003;

/* SECTION 4 - SINGLE TABLE SELECT STATEMENTS - The queries must be explained in natural (English) language first, and then followed up by respective SELECTs*/

/* 1) List the names of all instructors whose surnames begin with 'A'. List each customer only once. */
SELECT firstName, surname
FROM Instructors
WHERE surname LIKE 'A%';

/* 2) List all the details of each sport that has a duration greater than 3 years, ordered from shortest duration to longest duration. */
SELECT *
FROM Sports
HAVING durationYears > 3
ORDER BY durationYears ASC;

/* 3) List the names of the sports and the total fees that will accumulate across the years of each sport, ordered from most expensive to least expensive. Change the name of the output column for the total fees to 'totalFee£'. */
SELECT sportID, durationYears * fee£ AS 'totalFee£'
FROM Sports
ORDER BY totalFee£ DESC;

/* 4) List the names of all the members at the sports club who do not have a phone number. */
SELECT firstName, surname
FROM Members
WHERE phoneNumber is NULL;

/* 5) How many assessments were taken in 2022? Change the name of the output column to 'assessmentsIn2022'. */
SELECT COUNT(*) AS 'assessmentsIn2022'
FROM Certification
WHERE assessmentYear = 2022;

/* 6) How many assessments were taken in either November or December? Change the name of the output column to 'NovDecAssessments'. */
SELECT COUNT(*) AS 'NovDecAssessments'
FROM Certification
WHERE assessmentMonth = 11 OR assessmentMonth = 12;

/* SECTION 5 - MULTIPLE TABLE SELECT STATEMENTS - The queries must be explained in natural (English) language first, and then followed up by respective SELECTs */

/* 1) List the names of all the instructors who have assessed 'Eric Djemba-Djemba'. */
SELECT i.firstName, i.surname
FROM Instructors i, Certification c, Members m
WHERE i.instructorID = c.instructorID
AND m.memberID = c.memberID
AND m.firstName = 'Eric'
AND m.surname = 'Djemba-Djemba';

/* 2) List the names of all the members who have a certification in 'Football'. */
SELECT m.firstName, m.surname
FROM Members m, Certification c, Sports s
WHERE m.memberID = c.memberID
AND s.sportID = c.sportID
AND s.sportID = 'Football';

/* 3) On what dates did 'Sami Hyypia' have an assessment? Order the assessment dates from earliest to latest. */
SELECT c.assessmentMonth, c.assessmentYear
FROM Members m, Certification c
WHERE m.memberID = c.memberID
AND m.firstName = 'Sami'
AND m.surname = 'Hyypia'
ORDER BY c.assessmentYear ASC;
 
/* 4) What is the total number of certifications held by each member*/
SELECT m.memberID, m.firstName, m.surname, COUNT(c.memberID) AS totalCertifications
FROM Members m
LEFT JOIN Certification c ON c.memberID = m.memberID
GROUP BY m.memberId, m.firstName, m.surname;

/* 5) List the names of members who had a badminton assessment in 2022 and were assessed by 'Carlo Ancelotti' */
SELECT m.firstName, m.surname
FROM Members m
WHERE
EXISTS(
    SELECT 1
    FROM Certification c
    JOIN Instructors i ON c.instructorID = i.instructorID
    WHERE
        c.memberID = m.memberID
        AND c.sportID = 'Badminton'
        AND c.sportID = 'Badminton'
        AND c.assessmentYear = 2022
        AND i.firstName = 'Carlo'
        AND i.surname = 'Ancelotti');

/* 6) What is the total number of certifications assessed by each instructor */
SELECT i.instructorID, i.firstName, i.surname, COUNT(c.instructorID) AS totalCertifications
FROM Instructors i
LEFT JOIN Certification c ON i.instructorID = c.instructorID
GROUP BY i.instructorID, i.firstName, i.surname;

/* SECTION 6 - DELETE ROWS (make sure the SQL is commented out in this section)

DELETE FROM Certification 
WHERE memberID = 010 AND sportID = 'Power Lifting';

DELETE FROM Certification 
WHERE instructorID = 001 AND sportID = 'Squash';

*/

/* SECTION 7 - DROP TABLES (make sure the SQL is commented out in this section)

DROP TABLE Certification;
DROP TABLE Members;
DROP TABLE Instructors;
DROP TABLE Sports;

*/

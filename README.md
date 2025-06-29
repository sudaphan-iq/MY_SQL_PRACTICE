# MY_SQL_PRACTICE
I’ve been learning SQL for two weeks. This folder is dedicated to collecting the projects I’ve worked on and tracking my progress in SQL practice. I’m open to any feedback or suggestions for improvement

CREATE TABLE Doctor (
	doc_id INT PRIMARY KEY,
	first_name VARCHAR(50) NOT NULL, 
    last_name VARCHAR(50) NOT NULL,
    speciallist VARCHAR(100) NOT NULL
);

CREATE TABLE Patient (
	hn INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    gender VARCHAR(10),
    age INT,
    date_of_birth DATE,
    telephone VARCHAR(50),
    address TEXT,
    drug_allergy TEXT 
);

CREATE TABLE ER_Room (
	er_id INT PRIMARY KEY AUTO_INCREMENT,
    hn INT,
    doc_id INT,
    date DATE,
    type VARCHAR(50),
    FOREIGN KEY (hn) REFERENCES Patient(hn),
    FOREIGN KEY (doc_id) REFERENCES Doctor(doc_id)
    );
    
INSERT INTO Doctor ( doc_id, first_name, last_name, speciallist)
VALUES (101, 'Arisa', 'Tanakul', 'GP'),(102,'Kittinan','Wongchai','Orthopedic Surgery');

INSERT INTO Patient ( hn, first_name, last_name, gender, age, date_of_birth, telephone, address, drug_allergy)
VALUES ( 2001, 'Napat',	'Sritong', 'Male', 30, '1995-12-10', '0812345670',	'Bangkok',	'Penicillin'),
		(2002, 'Chalida','Raksri', 'Famale', 45, '1980-05-23',	'0865564433', 'Nonthaburi','no'),
		(2003,	'Kittisak',	'Malai', 'Male', 60, '1965-03-14',	'0898877665', 'Chiang Mai',	'Aspirin'),
		(2004,	'Sunee', 'Yimchai',	'Famale', 28, '1997-07-02',	'0901122334',	'Ayutthaya', 'no'),
		(2005,	'Jirapat',	'Klinjun',	'Male',	52,	'1973-08-18', '0887654321',	'Chonburi',	'Ibuprofen'),
		(2006, 'Pranee', 'Wongyai',	'Famale', 35, '1990-10-11',	'0844455667', 'Pathum Thani', 'no'),
		(2007,	'Anan',	'Srisuwan',	'Male',	41,	'1984-06-21', '0912233445',	'Khon Kaen', 'Sulfa drugs'),
		(2008,	'Kanokwan',	'Thephasadin', 'Famale', 25, '2000-01-01', '0876543212', 'Samut Prakan','no'),
		(2009, 'Supoj',	'Boonmee', 'Male', 50, '1975-09-30', '0833334444', 'Nakhon Ratchasima',	'Penicillin'),
		(2010,	'Suthida',	'Ruangchai', 'Famale', 32,	'1993-11-25', '0822221111',	'Bangkok', 'no');

INSERT INTO ER_Room ( er_id, hn, doc_id, date, type)
VALUES (1,	2001, 101,	'2025-06-10',	'Illness'),
		(2,	2002, 101,	'2025-06-15',	'Illness'),
		(3,	2003, 102,	'2025-06-20',	'Accident'),
		(4,	2004, 102,	'2025-06-22',	'Accident'),
		(5,	2005, 101,	'2025-06-23',	'Illness'),
		(6,	2006, 102,	'2025-06-25',	'Accident');


-- Q&A 

-- What is the number of patients per doctor this month
SELECT doc.doc_id, doc.first_name, doc.speciallist, count(erp.hn)
FROM doctor AS doc
RIGHT JOIN (
			SELECT er1.hn, er1.doc_id, er1.type,p.first_name, p.age, p.gender
            FROM er_room AS er1
            JOIN patient AS p
            ON er1.hn = p.hn 
			) AS erp
ON doc.doc_id = erp.doc_id
GROUP BY doc.doc_id
;

-- Summary of the dataset for patients who visited the hospital in June
SELECT doc.doc_id,
		doc.first_name AS Doctor, 
        doc.speciallist, 
        erp.type , 
        erp.first_name AS patient, 
        erp.age, 
        erp.gender
FROM doctor AS doc
RIGHT JOIN (
			SELECT er1.hn, er1.doc_id, er1.type,p.first_name, p.age, p.gender
            FROM er_room AS er1
            JOIN patient AS p
            ON er1.hn = p.hn 
			) AS erp
ON doc.doc_id = erp.doc_id
;

# Learn-SQL-at-Coding-Studio
My notes on learning Fundamental SQL from Coding Studio


CREATE DATABASE Music;

USE music;


CREATE TABLE MsBranch
(
	BranchID VARCHAR(6) PRIMARY KEY,
    BranchName VARCHAR(50) NOT NULL,
    Address VARCHAR(100) NOT NULL,
    Phone VARCHAR(50),
    CONSTRAINT CheckBran1 CHECK (CHAR_LENGTH(BranchID)=5),
    CONSTRAINT CheckBran2 CHECK (BranchID REGEXP '^BR[0-9][0-9][0-9]$') 
);

DROP TABLE MsBranch


CREATE TABLE MsMusicInsType
(
	MusicInsTypeID VARCHAR(6) PRIMARY KEY,
	MusicInsType VARCHAR(50) NOT NULL,	 
	CONSTRAINT CheckMsct1 CHECK (CHAR_LENGTH(MusicInsTypeID)=5),
	CONSTRAINT CheckMsct2 CHECK (MusicInsTypeID REGEXP 'MT[0-9][0-9][0-9]')
);

DROP TABLE MsMusicInType;
DROP DATABASE Music;

CREATE TABLE MsEmployee
(
	EmployeeID VARCHAR(6) PRIMARY KEY,
    EmployeeName VARCHAR(50) NOT NULL,
    Address  VARCHAR(100) NOT NULL,
    Phone VARCHAR(50),
    Gender CHAR(1) NOT NULL,
    DataOfBirth DATETIME,
    Salary DECIMAL(10,2),
    BranchID VARCHAR(6),
    CONSTRAINT CheckEmpl1 CHECK (CHAR_LENGTH(EmployeeID)=5),
    CONSTRAINT CheckEmpl2 CHECK (EmployeeID REGEXP 'EM[0-9][0-9][0-9]'),
    CONSTRAINT CheckEmpl3 CHECK (Gender IN ('M','F')),
    CONSTRAINT FK_MsEmployee_MsBranch FOREIGN KEY (BranchID) REFERENCES MsBranch(BranchID) ON UPDATE CASCADE ON DELETE SET NULL
); 


CREATE TABLE MsMusicIns
(
	MusicInsID VARCHAR(6) PRIMARY KEY,
	MusicIns VARCHAR(50) NOT NULL,
	Price DECIMAL(10,2) NOT NULL, -- 10.00
	Stock INT NOT NULL, -- 10
	MusicInsTypeID VARCHAR(6),
	CONSTRAINT CheckMsci1 CHECK (CHAR_LENGTH(MusicInsID)=5),
	CONSTRAINT CheckMsci2 CHECK (MusicInsID REGEXP 'MI[0-9][0-9][0-9]'),
	CONSTRAINT CheckMsci3 CHECK (Stock >= 0),
	CONSTRAINT FK_MsMusicIns_MsMusicInsType FOREIGN KEY (MusicInsTypeID) REFERENCES MsMusicInsType(MusicInsTypeID) ON UPDATE CASCADE ON DELETE SET NULL
);

DROP TABLE MsMusicIns;

CREATE TABLE HeaderTransaction -- mensimpan data transaksi
(
  TransactionID VARCHAR(6) PRIMARY KEY,
  TransactionDate DATETIME NOT NULL,
  EmployeeID VARCHAR(6) REFERENCES MsEmployee(EmployeeID) ON UPDATE CASCADE ON DELETE SET NULL,
  CustomerName VARCHAR(50),
  CHECK (CHAR_LENGTH(TransactionID)=5),
  CHECK (TransactionID REGEXP 'TR[0-9][0-9][0-9]')
);


CREATE TABLE DetailTransaction -- detai transaksi/struk pembelian
(
  TransactionID VARCHAR(6) REFERENCES HeaderTransaction(TransactionID) ON UPDATE CASCADE ON DELETE CASCADE,
  MusicInsID VARCHAR(6) REFERENCES MsMusicIns(MusicInsID) ON UPDATE CASCADE ON DELETE CASCADE,
  Qty INT NOT NULL,
  PRIMARY KEY(TransactionID, MusicInsID)
);

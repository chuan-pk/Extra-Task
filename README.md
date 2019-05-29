## ER-Diagram to Relation 7-step
1. Strong Entity
2. Weak Entity
3. 1:1 Relationship
4. 1:N Relationship
5. M:N Relationship
6. Multi-value
7. n-ary Relationship

## Extra Task Table

Student

|id (PK)|firstname|lastname|email|addr|phone|birthdate|sex|program_id (FK)|
|---|---|---|---|---|---|---|---|---|

Course

|id (PK)|name|credit|
|---|---|---|

Program

|id (PK)|name|abbr|
|---|---|---|

Attempt

|student_id (PK, FK)|course_id (PK, FK)|year (PK)|semester (PK)|grade|
|---|---|---|---|---|

Prerequisite

|current_course (PK, FK)| before_course (PK, FK)|
|---|---|


## Create Table
```
CREATE TABLE IF NOT EXISTS Program (
        id INTEGER PRIMARY KEY,
        name TEXT,
        abbr TEXT
);

CREATE TABLE IF NOT EXISTS Course (
        id TEXT PRIMARY KEY,
        name TEXT,
        credit INTEGER
);

CREATE TABLE IF NOT EXISTS Student (
        id INTEGER PRIMARY KEY,
        firstname TEXT,
        lastname TEXT,
        email TEXT,
        addr TEXT,
        phone TEXT,
        birthdate TEXT,
        sex TEXT,
        program_id INTEGER,
        FOREIGN KEY (program_id) REFERENCES program(id)
);

CREATE TABLE IF NOT EXISTS Attempt (
    student_id INTEGER,
    course_id TEXT,
    year INTEGER,
    semester INTEGER,
    grade TEXT,
    FOREIGN KEY (student_id) REFERENCES Student(id),
    FOREIGN KEY (course_id) REFERENCES Course(id),
    PRIMARY KEY (student_id, course_id, year, semester)
);

CREATE TABLE IF NOT EXISTS Prerequisite (
    current_course TEXT,
    before_course TEXT,
    FOREIGN KEY (current_course) REFERENCES Course(id),
    FOREIGN KEY (before_course) REFERENCES Course(id),
    PRIMARY KEY (current_course, before_course)
);
```


## Import record from CSV file
```
.mode csv
.import csvfile.csv TableName

// Example
.mode csv
.import courses.csv Course
```

## Q1: Prerequisite for course 01077
```
SELECT before.name AS prerequisite_course
FROM Prerequisite
INNER JOIN Course AS before ON Prerequisite.before_course = before.id
INNER JOIN Course AS current ON Prerequisite.current_course = current.id
WHERE current_course = "01077";
```

## Q2: Count CprE student attempted 01077 and get grade B in year 2550
```
SELECT COUNT(*) FROM Attempt
INNER JOIN Student ON Attempt.student_id = Student.id
INNER JOIN Program ON Student.program_id = Program.id
WHERE Program.abbr = "CprE" AND Attempt.grade = "B" AND 
Attempt.year = 2550 AND Attempt.course_id = "01077";
```
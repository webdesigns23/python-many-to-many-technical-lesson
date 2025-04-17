# Technical Lesson: Many-to-Many Relationships

## Introduction

In this lesson, we will explore how to apply inheritance as well as class attributes and methods.

## Scenario

Imagine you are working for a school administration. You want to keep track of all the students and all the courses. In this case, thinking about the relationship between the two, we can say that:

- A student has many courses
- A course has many students

This makes it a **many-to-many relationship**.

### How can we connect the two?

We can use a **schedule table** to connect them.

- Create intermediary class  
- Build relationships to allow access between classes  
- Build properties to ensure class connection  

## Tools and Resources

- GitHub repo: [python-many-to-many-technical-lesson](https://github.com/learn-co-curriculum/python-many-to-many-technical-lesson)

---

## Instructions

### Set Up

Before we begin coding, let's complete the initial setup for this lesson:

#### Fork and Clone

- Go to the provided GitHub repository link.  
- Fork the repository to your GitHub account.  
- Clone the forked repository to your local machine.

#### Open and Run File

- Open the project in VSCode  
- Run `npm install` to install all necessary dependencies

---

## Instructions

### Task 1: Define the Problem

You need to build out a relationship between students and courses.

You will be tasked with:

- Creating an intermediary class  
- Building relationships to allow access between classes  
- Building properties to ensure class connection  

---

### Task 2: Determine the Design

#### `Schedule`  
**Attributes:**
- Student
- Course
- Grade

**Properties:**
- Student
- Course

#### `Course`  
**Attributes:**
- Title
- Teacher  

**Methods:**
- `students`

#### `Student`  
**Attributes:**
- Name
- Email  

**Methods:**
- `courses`
- `calculate_gpa`

---

### Task 3: Develop, Test, and Refine the Code

#### Step 1: Create a Feature Branch

```bash
git checkout -b many-to-many
```

---

#### Step 2: Set up Intermediary Class

Ensure you understand `Student` and `Course` models. Then build the connecting `Schedule` class:

**File:** `lib/many_to_many.py`

```python
class Student:
    all =[]
    def __init__(self, name, email):
        self.name = name
        self.email = email
        Student.all.append(self)

class Course:
    all =[]
    def __init__(self, title, teacher):
        self.title = title
        self.teacher = teacher
        Course.all.append(self)

class Schedule:
    all =[]
    def __init__(self, student, course, grade):
        self.student = student
        self.course = course
        self.grade = grade
        Schedule.all.append(self)
```

---

#### Step 3: Set up Properties

Add properties to require that `Schedule` uses instances of `Student` and `Course`:

```python
class Schedule:
    all =[]

    def __init__(self, student, course, grade):
        self.student = student
        self.course = course
        self.grade = grade
        Schedule.all.append(self)

    @property
    def student(self):
        return self._student

    @student.setter
    def student(self, value):
        if not isinstance(value, Student):
            raise Exception
        self._student = value

    @property
    def course(self):
        return self._course

    @course.setter
    def course(self, value):
        if not isinstance(value, Course):
            raise Exception
        self._course = value
```

---

#### Step 4: Connecting Methods

Add methods to link `Student` ↔ `Course` through `Schedule`.

```python
class Student:
    all =[]

    def __init__(self, name, email):
        self.name = name
        self.email = email
        Student.all.append(self)

    def courses(self):
        return [schedule.course for schedule in Schedule.all if schedule.student == self]

class Course:
    def __init__(self, title, teacher):
        self.title = title
        self.teacher = teacher
        Course.all.append(self)

    def students(self):
        return [schedule.student for schedule in Schedule.all if schedule.course == self]
```

---

#### Step 5: Additional Methods

Build a `calculate_gpa()` method in `Student`:

```python
class Student:
    ...
    def calculate_gpa(self):
        my_courses = self.courses()
        summative = 0
        for course in my_courses:
            summative += course.grade
        return summative / len(my_courses)
```

Use this to calculate a student's GPA based on course grades via `Schedule`.

---

#### Step 6: Push Changes to GitHub and Merge Branches

```bash
git commit -am "Completed many to many models"
git push origin many-to-many
```

- Create a Pull Request (PR) on GitHub
- Merge the PR into `main` after review

Then, pull the updated `main` branch and clean up:

```bash
git checkout main
git pull origin main
git branch -d many-to-many
# If needed:
git branch -D many-to-many
```

---

## Task 4: Document and Maintain

### Best Practices

- Add comments to explain code
- Clarify intent/functionality for others
- Add screenshots of completed work to README
- Update README text following [makeareadme.com](https://makeareadme.com)
- Delete stale branches
- Remove commented/unnecessary code
- Update `.gitignore` for sensitive data

---

## Considerations

### 1. Ternary

Ternaries are used to clean up for-loops. For example:

```python
# Standard loop
all_students = []
for schedule in Schedule.all:
    if schedule.course == self:
        all_students.append(schedule.student)
return all_students

# Same as:
return [schedule.student for schedule in Schedule.all if schedule.course == self]
```

### 2. Ensuring Class Instances

You can treat your own classes the same way you check for `int` or `str` using `isinstance()`:

```python
if not isinstance(value, Student):
    raise Exception
```

---

# Data Normalization and Entity-Relationship Diagramming
## Table of Original Data Set
| assignment_id | student_id | due_date | professor | assignment_topic                | classroom | grade | relevant_reading    | professor_email   |
| :------------ | :--------- | :------- | :-------- | :------------------------------ | :-------- | :---- | :------------------ | :---------------- |
| 1             | 1          | 23.02.21 | Melvin    | Data normalization              | WWH 101   | 80    | Deumlich Chapter 3  | l.melvin@foo.edu  |
| 2             | 7          | 18.11.21 | Logston   | Single table queries            | 60FA 314  | 25    | Dümmlers Chapter 11 | e.logston@foo.edu |
| 1             | 4          | 23.02.21 | Melvin    | Data normalization              | WWH 101   | 75    | Deumlich Chapter 3  | l.melvin@foo.edu  |
| 5             | 2          | 05.05.21 | Logston   | Python and pandas               | 60FA 314  | 92    | Dümmlers Chapter 14 | e.logston@foo.edu |
| 4             | 2          | 04.07.21 | Nevarez   | Spreadsheet aggregate functions | WWH 201   | 65    | Zehnder Page 87     | i.nevarez@foo.edu |
| ...           | ...        | ...      | ...       | ...                             | ...       | ...   | ...                 | ...               |

## What makes this data set not compliant with 4NF
- **The data set does not satisfy the requirements of second normal form.** There are non-key fields that are facts about only a part of the entity. The composite primary key is composed of both `assignment_id` and `student_id`. However, non-key fields such as `assignment_topic` and `relevant_reading` are only dependent on one subset of the composite primary key, `assignment_id`, instead of being fully dependent on the entire primary key.

- **The data set does not satisfy the requirements of third normal form.** There are non-key fields that are about another non-key field. For example, the non-key field, `professor_email`, is a fact about another non-key field, `professor`.

## 4NF-compliant version of the data set

1. professor

    | professor_id  | professor_email    | professor_name |
    | :------------ | :----------------- | :------------- |
    | 1             | l.melvin@foo.edu   | Melvin         |
    | 2             | e.logston@foo.edu  | Logston        |
    | 3             | i.nevarez@foo.edu  | Nevarez        |
    | ...           | ...                | ...            |

    - Primary key: `professor_id`
    - All non-key fields are not facts about any other non-key field.
    - `professor_email` and `professor_name` are single-valued facts about the primary key. Each professor id can only associate with one email address and one name.

2. course

    | course_id  | course_name                       |
    | :--------- | :-------------------------------- |
    | 1          | Database Design & Implementation  |
    | 2          | Introduction to Databases         |
    | ...        | ...                               |

    - Primary key: `course_id`

3. section
    
    | section_id | course_id  | professor_id  | classroom  |
    | :--------- | :--------- | :------------ | :--------- |
    | 101        | 1          | 1             | WWH 101    |
    | 102        | 1          | 1             | WWH 101    |
    | 103        | 1          | 2             | 60FA 314   |
    | 201        | 2          | 3             | WWH 201    |
    | ...        | ...        | ...           | ...        |

    - Primary key: `section_id`
    - All non-key fields are not facts about any other non-key field.
    - `course_id`, `professor_id` and `classroom` are single-valued facts about the primary key. Each section belongs to only one course, and is taught by one professor, and held in one classroom.

4. student info

    | student_id  | student_last_name | student_first_name  |
    | :---------- | :---------------- | :------------------ |
    | 1           | Adams             | Alex                |
    | 2           | Kim               | Charlie             |
    | 4           | Miller            | Max                 |
    | 7           | Wright            | Riley               |
    | ...         | ...               | ...                 |

    - Primary key: `student_id`
    - All non-key fields are not facts about any other non-key field.
    - `student_last_name` and `student_first_name` are single-valued facts about the primary key. Each student id can only associate with one last name and one first name.

5. student section

    | student_id  | section_id |
    | :---------- | :--------- |
    | 1           | 101        |
    | 2           | 103        |
    | 2           | 201        |
    | 4           | 102        |
    | 7           | 103        |
    | ...         | ...        |

    - Primary key: `student_id`

6. assignment

    | assignment_id  | assignment_topic                 |
    | :------------- | :------------------------------- |
    | 1              | Data normalization               |
    | 2              | Single table queries             |
    | 4              | Spreadsheet aggregate functions  |
    | 5              | Python and pandas                |
    | ...            | ...                              |

    - Primary key: `assignment_id`

7. assignment info

    | assignment_id  | section_id | due_date  | relevant_reading     |
    | :------------- | :--------- | :-------- | :------------------- |
    | 1              | 101        | 23.02.21  | Deumlich Chapter 3   |
    | 1              | 102        | 23.02.21  | Deumlich Chapter 3   |
    | 2              | 103        | 18.11.21  | Dümmlers Chapter 11  |
    | 4              | 201        | 04.07.21  | Zehnder Page 87      |
    | 5              | 103        | 05.05.21  | Dümmlers Chapter 14  |
    | ...            | ...        | ...       | ...                  |

    - Composite primary key: `assignment_id` and `section_id`
    - All non-key fields are not facts about any other non-key field.
    - `due_date` and `relevant_reading` are single-valued facts about the primary key. In one specific section, each assignment has only one due date and only one relevant reading.

8. grade

    | student_id | assignment_id | grade    |
    | :--------- | :------------ | :------- |
    | 1          | 1             | 80       |
    | 2          | 4             | 65       |
    | 2          | 5             | 92       |
    | 4          | 1             | 75       |
    | 7          | 2             | 25       |
    | ...        | ...           | ...      |

    - Composite rimary key: `student_id` and `assignment_id`

## ER diagram(s)
![Entity-Relationship Diagram](images/ERDiagram.svg)

## What changes have been made to make the data 4NF-compliant
1. **In Compliance with with second and third normal form, ensure every non-key field is a fact about the entire primary key.** For instance, if `assignment_topic` is included in the 7th table of assignment info, it will then be partially dependent on one subset of the composite primary key, `assignment_id`, which does not comply with second normal form. Meanwhile, if both `professor_name` and `professor_email` are included in the 3rd table of sections, it would violate third normal form, as they are facts about each other instead of the key field, `section_id`.

2. **In Compliance with fourth normal form, keep the number of independent multi-valued facts about an entity under two.** If `section_id` is not part of the primary key in the 7th table of assignment info, then both `due_date` and `relevant_reading` become independent multi-valued facts that will violate the fourth normal form, as each assignment can have multiple due dates and relevant readings. This occurs specifically in two cases: 1) different sections with different due dates but the same relevant reading, 2) different sections with different relevant readings but with the same due date.
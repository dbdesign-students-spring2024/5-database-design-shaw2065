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
- **The data set does not satisfy the requirements of second normal form.** There are non-key fields that are facts about only a part of the entity. The composite primary key composed of both `assignment_id` and `student_id`. However, non-key fields such as `assignment_topic` and `relevant_reading` are only dependent on one subset of the composite primary key, `assignment_id`, instead of being fully dependent on the entire primary key.

- **The data set does not satisfy the requirements of third normal form.** There are non-key fields that are about another non-key field. For example, the non-key field, `professor_email`, is a fact about another non-key field, `professor`.

## 4NF-compliant version of the data set
1. professors
| professor_id  | professor_email  | professor_name |
| ------------- | ------------- | ------------- |
| 1  | l.melvin@foo.edu  | Melvin  |
| 2  | e.logston@foo.edu  | Logston  |
| 3  | i.nevarez@foo.edu  | Nevarez  |
| ...  | ...  | ...  |

2. courses
| course_id  | course_name |
| ------------- | ------------- |
| 1  | Database Design & Implementation  |
| 2  | Introduction to Databases  |
| ...  | ...  |

3. sections
| course_id  | section_id | professor_id  | classroom_id  |
| ------------- | ------------- | ------------- |
| 1  | 1  | 1  | 1  |
| 1  | 2  | 1  | 1  |
| 1  | 3  | 2  | 2  |
| 2  | 1  | 3  | 3  |
| ...  | ...  | ...  |

4. classrooms
| classroom_id  | classroom |
| ------------- | ------------- |
| 1  | WWH 101  |
| 2  | 60FA 314  |
| 3  | WWH 201  |
| ...  | ...  |


5. students
| student_id  | student_last_name | student_first_name  |
| ------------- | ------------- | ------------- |
| 1  | Adams  | Alex  |
| 2  | Kim  | Charlie  |
| 4  | Miller  | Max  |
| 7  | Wright  | Riley  |
| ...  | ...  | ...   |

6. student courses
| student_id  | course_id | section_id  |
| ------------- | ------------- | ------------- |
| 1  | 1  | 1  |
| 2  | 1  | 3  |
| 2  | 2  | 1  |
| 4  | 1  | 2  |
| 7  | 1  | 3  |
| ...  | ...  | ...   |

7. assignments
| assignment_id  | assignment_topic | relevant_reading  |
| ------------- | ------------- | ------------- |
| 1  | Data normalization  | Deumlich Chapter 3  |
| 2  | Single table queries  | Dümmlers Chapter 11  |
| 4  | Spreadsheet aggregate functions  | Zehnder Page 87  |
| 5  | Python and pandas  | Dümmlers Chapter 14  |
| ...  | ...  | ...  |

8. assignment due dates
| assignment_id  | section_id | due_date  |
| ------------- | ------------- | ------------- |
| 1  | 1  | 23.02.21  |
| 1  | 2  | 23.02.21  |
| 2  | 3  | 18.11.21  |
| 4  | 1  | 04.07.21  |
| 5  | 3  | 05.05.21  |
| ...  | ...  | ...   |

9. grades
| student_id  | assignment_id | grade  |
| ------------- | ------------- | ------------- |
| 1  | 1  | 80  |
| 2  | 4  | 65  |
| 2  | 5  | 92  |
| 4  | 1  | 75  |
| 7  | 2  | 25  |
| ...  | ...  | ...   |


## ER diagram(s)

## What changes have been made to make the data 4NF-compliant
1. **In Compliance with with second and third normal form, ensure every non-key field is a fact about the entire primary key.** For instance, if `section_id` and `due_date` are included in the 7th table of assignments, the other non-key fields, namely `assignment_topic` and `relevant_reading`, will be partially dependent on the subset, `assignment_id`, of the composite primary key, which does not comply with second normal form. Meanwhile, if both `professor_name` and `professor_email` are included in the 3rd table of sections, it would violate third normal form, as they are fact about each other instead of the key field, `section_id`.

2. **In Compliance with fourth normal form, keep the number of independent multi-valued facts about an entity under two.** While the 6th table of student courses may appear to violate fourth normal form, `course_id` and `section_id` are not independent of each other. Their interdependence allows them to be presented in a single record. The same applies to `section_id` and `due_date` in the 8th table of assignment due dates. Conversely, since one specific assignment can have different due dates even if given by the same professor, including both `due_date` and `professor` will result in having two independent multi-valued facts in the same record, as `due_date` and `professor` are not guranteed to be dependent on each other.
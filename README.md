 📊 Course Progress Tracking System (SQL Project)
This repository contains a fully designed SQL database and query suite to manage and analyze user learning progress in an educational platform. It includes schema definitions, data inserts, and practical SQL queries for monitoring course and assignment performance.

 📁 Repository Contents

| File Name              | Description |
|------------------------|-------------|
| `COM USERS.docx`       | SQL table and data for `users` (learners & instructors) |
| `COM courses.docx`     | SQL table and data for `courses` |
| `COM modules.docx`     | SQL table and data for `modules` (linked to courses) |
| `perssion.docx`        | SQL table and data for `permissions` (user access to courses) |
| `progress_inserts.sql` | INSERT statements for the `progress` table (student assignment tracking) |
| `sql question.docx`    | SQL queries to analyze progress, status, and performance |


🧱 Database Schema Overview

 Tables:

- **users**  
  Stores user details such as name, role (learner/instructor), and status.

- **courses**  
  Contains information about courses like title, start & end dates, and creator.

- **modules**  
  Represents logical segments of each course.

- **assignments** *(not uploaded)*  
  Would include assignment details per module.

- **progress**  
  Tracks assignment completion, scores, and timestamps per student.

- **permissions**  
  Manages user access rights to different courses.


 🧪 SQL Queries Highlights

Here are some example SQL queries found in `sql question.docx`:

 ✅ Retrieve Student’s Overall Progress
```sql
SELECT 
    u.user_id,
    u.name,
    c.title AS course_title,
    COUNT(a.assignment_id) AS total_assignments,
    COUNT(p.assignment_id) AS completed_assignments,
    ROUND(COUNT(p.assignment_id) / COUNT(a.assignment_id) * 100, 2) AS completion_percentage
FROM users u
JOIN progress p ON u.user_id = p.user_id
JOIN assignments a ON a.assignment_id = p.assignment_id
JOIN modules m ON m.module_id = a.module_id
JOIN courses c ON c.course_id = m.course_id
WHERE u.user_id = 1
GROUP BY u.user_id, c.course_id;
SELECT 
    u.user_id,
    u.name,
    COUNT(p.assignment_id) AS completed_assignments,
    AVG(p.score) AS average_score
FROM users u
JOIN progress p ON u.user_id = p.user_id
GROUP BY u.user_id
HAVING COUNT(p.assignment_id) < 3 OR AVG(p.score) < 50;
SELECT 
    WEEK(p.completed_at) AS week_number,
    COUNT(*) AS completed_assignments
FROM progress p
WHERE p.user_id = 1342 AND p.completed_at IS NOT NULL
GROUP BY WEEK(p.completed_at)
ORDER BY week_number;


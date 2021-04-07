# Employee-Data-Insights

 <!-- TABLE OF CONTENTS -->
## Table of Contents

* [Introduction](#introduction)
* [Softwares Used](#softwares-used)
* [Tasks](#tasks)
  * Task 1
  * Task 2
  * Task 3
  * Task 4
* [Tableau Dashboard](#tableau-dasboard)


<!-- INTRODUCTION -->
## Introduction
Data stored in the Database [file](https://www.dropbox.com/s/3czfpe0njsq868q/employees_mod.sql?dl=0) has been used to solve various data analysis tasks to give insights to the concerned stakeholders in a company about data of the empoloyees working at the company. A tableau dashboard has been created depicting the combined result of all the tasks.

<!-- SOFTWARE USED -->
## Software Used
* **MySQL Workbench**: To write queries inorder to obtain the corresponding data to solve various tasks.
*  **Tableau Public Desktop**: To visiualize the obtained results for the task

<!-- TASKS -->
## Tasks
* **Task 1 : A visualization that provides a breakdown between the male and female employees working in the company each year, starting from 1990.** 
  SQL Code:

 ` SELECT 
    YEAR(d.from_date) AS calendar_year, 
    e.gender, 
    COUNT(e.emp_no) AS num_of_employees
FROM
    t_employees e
        JOIN
    t_dept_emp d ON d.emp_no = e.emp_no
GROUP BY calendar_year , e.gender
HAVING calendar_year >= 1990
ORDER BY calendar_year ASC; `
 

* **Task 2: Compare the number of male managers to the number of female managers from different departments for each year, starting from 1990.**

SQL Code:

` SELECT d.dept_name, ee.gender , dm.emp_no, dm.from_date, dm.to_date, e.calendar_year,
CASE 
	WHEN YEAR(dm.to_date) >=e.calendar_year AND YEAR(dm.from_date) <= calendar_year THEN 1 ELSE 0
END AS if_active
FROM
	(SELECT YEAR(hire_date) as calendar_year FROM t_employees GROUP BY calendar_year) e 
CROSS JOIN t_dept_manager dm JOIN t_departments d ON dm.dept_no = d.dept_no 
							JOIN 	t_employees ee ON dm.emp_no = ee.emp_no 
                            ORDER BY dm.emp_no, calendar_year; `
			    


* **Task 3: Compare the average salary of female versus male employees in the entire company until year 2002, and add a filter allowing you to see that per each department.**

SQL Code:

` SELECT 
    e.gender,
    d.dept_name,
    ROUND(AVG(s.salary), 2) AS salary,
    YEAR(s.from_date) AS calendar_year
FROM
    t_salaries s
        JOIN
    t_employees e ON s.emp_no = e.emp_no
        JOIN
    t_dept_emp de ON de.emp_no = e.emp_no
        JOIN
    t_departments d ON d.dept_no = de.dept_no
GROUP BY d.dept_no , e.gender , calendar_year
HAVING calendar_year <= 2002
ORDER BY d.dept_no;  ` 

* **An SQL stored procedure that will allow you to obtain the average male and female salary per department within a certain salary range. Let this range be defined by two values the user can insert when calling the procedure.**

SQL Code:

` DROP PROCEDURE IF EXISTS filter_salary; `


`DELIMITER $$
CREATE PROCEDURE filter_salary (IN p_min_salary FLOAT, IN p_max_salary FLOAT)
BEGIN
SELECT 
    e.gender, d.dept_name, AVG(s.salary) as avg_salary
FROM
    t_salaries s
        JOIN
    t_employees e ON s.emp_no = e.emp_no
        JOIN
    t_dept_emp de ON de.emp_no = e.emp_no
        JOIN
    t_departments d ON d.dept_no = de.dept_no
    WHERE s.salary BETWEEN p_min_salary AND p_max_salary
GROUP BY d.dept_no, e.gender;
END$$
DELIMITER ; `



**Complete Tableau Dashboard** 
[Tap here to view the dynamic dashboard](https://public.tableau.com/views/Employeedatainsightsbasedongender/Dashboard1?:language=en-GB&:display_count=y&:origin=viz_share_link)

<div class='tableauPlaceholder' id='viz1617795390480' style='position: relative'><noscript><a href='#'><img alt=' ' src='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;Em&#47;Employeedatainsightsbasedongender&#47;Dashboard1&#47;1_rss.png' style='border: none' /></a></noscript><object class='tableauViz'  style='display:none;'><param name='host_url' value='https%3A%2F%2Fpublic.tableau.com%2F' /> <param name='embed_code_version' value='3' /> <param name='site_root' value='' /><param name='name' value='Employeedatainsightsbasedongender&#47;Dashboard1' /><param name='tabs' value='yes' /><param name='toolbar' value='yes' /><param name='static_image' value='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;Em&#47;Employeedatainsightsbasedongender&#47;Dashboard1&#47;1.png' /> <param name='animate_transition' value='yes' /><param name='display_static_image' value='yes' /><param name='display_spinner' value='yes' /><param name='display_overlay' value='yes' /><param name='display_count' value='yes' /><param name='language' value='en-GB' /></object></div>                



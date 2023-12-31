# LinkedIn Data Analysis - Readme

This readme provides a detailed explanation of every component within the database management and data cleaning project.

## Data Cleaning

The data cleaning process involved the following datasets:

- `company_industries`
- `company_specialities`
- `companies`
- `Job`
- `Job Post`
- `Jon Skills`
- `benefits`
- `employee_counts`

### Data Cleaning Steps

1. **Null Values Handling**: Null values in the datasets were addressed by examining each column and deciding whether to impute or drop the data. Columns with more than 80% null values were dropped. Otherwise, missing values were imputed. Median values were used for continuous data types, while mode values were used for categorical data types.

2. **Non-ASCII Characters Removal**: String data types were examined for non-ASCII characters. These non-ASCII characters, which could hinder database compatibility, were removed from the data.

3. **Data Type Adjustment**: The data types of the columns were adjusted to be in the most appropriate format for database compatibility. This included transforming object data types into text data types.

4. **Special Characters Handling**: Special characters such as single quotes ('), double quotes ("), and commas (,) were removed and replaced with blank spaces for data consistency.

5. **Company Size Removal**: The "company size" column, which was not relevant for the use case and contained many null values, was removed from the dataset.

6. **UTF-8 Format**: Data was saved in UTF-8 format for uniform encoding and compatibility.


## Importing the Database 

A staging database was created to store imported files in the Workbench.
All preprocessed data were imported into MySQL workbech staging database using Table Import Wizard and with proper format.

## Database Schema Design and insertion of Data from staging Database

A database schema was design to perform entire analysis for this project. Cardinalities and relations were identified. The database was created with normalized tables and primary key,
foreign key and index fields. 
Following tables were created along with the attributes.
a. Job: This table stored job details for a company. A job can have multiple jobposts.
b. Jobpost : This table stores details regarding a jobpost posted from a company.A particular jobpost will be published from a company.
c. Benefit: This table stores details regarding benefits offered with a job. A job can offer mutiple benefits.
d. Jobskill: This table stores details regarding the skill required for a job.A job will require multiple job skills.
e. Company: This table stores details of a compnay. A company must have multiple job openings.
f. Employee_cnt: This table stores details of employee strength of a company. A company has multiple employees.
g. Comp_Spclty: This table stores details specilaity of a company. A company can have different specilities.
h. Comp_Industry: This table stores details of industry in which the company belongs.A company might belong to different industries.

![EER Diagram_v0 2](https://github.com/aryama-ray/data-225-lab1-group-project/assets/42118282/a9cf9191-1584-44dd-975a-4bd6536fa0ba)

After schema design and establishing the relation, data were inserted from staging database into this working database.

## Functional Analysis and USE case diagram design

#### Functional Analysis requires identification of functional elements. Here we assume the database system will be accessed by two type of users. 
#### a. Job Seeker

A job seeker can have multiple requirements from the system.
                            
#### Primarily, 1. a job seeker can search for:
                          i. a job on the platform based on skills, experience level, job title, job location, salary etc.
                          ii. a company on the platform based on company name, company size,benefits etc.
 ####           2. a job seeker can view and apply for a job:
                          i. if the job post has application link posted on the platform job seeker can apply
                          ii. if not, job seeker can view the job post via platform
####  b. HR Manager

An HR manager can have multiple requirements from the system.
 ####           1. HR manager can add job post on the platform
 ####           2. HR manager can view performance of the job post, keep track of applications on the platform.
 
![WhatsApp Image 2023-10-16 at 5 05 21 PM](https://github.com/aryama-ray/data-225-lab1-group-project/assets/92011107/a069019a-436c-451f-ba6c-e38dcda5835c)

![WhatsApp Image 2023-10-16 at 9 10 02 PM](https://github.com/aryama-ray/data-225-lab1-group-project/assets/92011107/c778ff46-f1b7-4c77-a72f-3522bd1c6317)


## Log Table and Triggers

A log table was created to maintain a record of database changes, and triggers were implemented for each table to capture `UPDATE`, `DELETE`, and `INSERT` operations and populate the log table with relevant information.

### Log Table Structure

The log table includes the following columns:

- `log_id`: A unique identifier for each log entry.
- `table_name`: The name of the table on which the action was performed.
- `action_type`: The type of action (INSERT, UPDATE, DELETE).
- `action_time`: The timestamp when the action occurred.

### Triggers

1. **Archive Job Trigger**

A trigger named archive_job_trigger moves jobs from the JOBPOST table to an archive_table when the number of applications (applies) for a job exceeds 200.

2. **Triggers with Notification and Logging**

Two tables, trigger_logs and notifications, are involved. The archive_job_trigger not only moves the record but also logs the event in trigger_logs and sends a notification through the notifications table.

3. **Job Expiry and Low-Interest Triggers**

Two other triggers named expire_job_trigger and low_interest_jobs move jobs to the archive_table based on conditions such as post date and low application numbers.

4. **Salary Change Triggers**

Triggers for logging minimum and maximum salary changes (log_salary_changes and log_salary_changes_max) have been implemented. These triggers populate the salary_change_logs table whenever there are changes in salaries.

5. **Notifications for Salary Changes**

Triggers salary_change_notify and salary_change_notify_max send notifications to the HR department when there are changes in the minimum and maximum salaries.


## Views and Procedures

Several views and procedures were created to facilitate data analysis and management.

1. **Labour Cost View and Procedure**: The "Labour Cost View" provides insights into the labour cost associated with each company. The associated procedure fetches and calculates the labour cost based on employee counts and salary data, aiding in financial planning.

2. **Application View**: This view provides insights into the number of job applications received by each company. This information is essential for HR departments to assess the success of their recruitment efforts.

3. **Job Details Procedure**: The procedure retrieves all details associated with a specific job ID. This aids in retrieving job-specific information quickly.

4. **Delete Job Procedure**: This procedure is designed to delete all details associated with a specific job. It provides a way to remove outdated job postings from the database.

5. **Delete Company Procedure**: This procedure allows the deletion of all details associated with a specific company. It facilitates data management for company profiles.


## Repository Structure

This repository contains SQL scripts and files related to the data cleaning process, triggers, views, and procedures. Together, these components create a comprehensive solution for managing company and job posting data, making it more organized and accessible for analysis and database management.

## Performance Analysis 

Utilized JMeter to measure and optimize SQL query performance for enhanced database efficiency.

![WhatsApp Image 2023-10-16 at 9 17 33 PM](https://github.com/aryama-ray/data-225-lab1-group-project/assets/92011107/b221111c-df4b-4daa-8a5b-247967b6e02b)


## Access Privilage 

We have two kinds of users for our system HR and Job seeker. The HR, who has access to procedures, views and all the data in the DB to help post or edit any kinbd of information in DB. 
The Job Seeker has access to views and procedures which helps him to find the optimal job based on requirements.

<img width="679" alt="Screenshot 2023-10-16 at 11 44 42 PM" src="https://github.com/aryama-ray/data-225-lab1-group-project/assets/92011107/450fa33f-615b-408c-96e8-b0408aed9d62">



## AWS Connection 

We are able connect to the aws with help of python pymysql library and also able to execute the query.

![WhatsApp Image 2023-10-16 at 11 30 42 PM](https://github.com/aryama-ray/data-225-lab1-group-project/assets/92011107/92badea1-791f-45c7-9d71-fd466ebcb8e7)


## Logging DB

There is a predefined log in mysql called the General Log which is been viewed by creating a table and sending paramenters into it. This table can be exported in csv form.


CREATE TABLE general log (

event time timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP

ON UPDATE CURRENT_TIMESTAMP,

user_host mediumtext NOT NULL,

thread_id bigint (21) unsigned NOT NULL,

server_id` int(10) unsigned NOT NULL,

command_type` varchar(64) NOT NULL,

argument mediumtext NOT NULL

) ENGINE=CSV DEFAULT CHARSET=utf8 COMMENT="General log';



SET global general_log= 1;

SET global log_output = 'table';


select from mysql.general_log;

SET global general_log= 0;




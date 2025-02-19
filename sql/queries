--create table institutions
CREATE TABLE institutions (
    institution_id INT PRIMARY KEY,
    LeadInstitution VARCHAR(100),
    City VARCHAR(100)
);


--create table researchers
CREATE TABLE researchers (
    researcher_id INT PRIMARY KEY,
    Salutation VARCHAR(50),
    First_Name VARCHAR(150),
    Middle_Name VARCHAR(150),
    Last_Name VARCHAR(150)
);

--table projects
CREATE TABLE projects (
    project_id INT PRIMARY KEY,
    Programs VARCHAR(100),
    PNumber VARCHAR(100),
    PTitle VARCHAR(100),
    PDescription VARCHAR(1000),
    FeildPrimary VARCHAR(100),
    FeildSecondary VARCHAR(100),
    DiscPrimary VARCHAR(100),
    DiscSecondary VARCHAR(100),
    Round VARCHAR(100),
    ApprovalDate DATE,
    lead_research_institution_id INT,
    OntarioFunds DECIMAL(15, 2),
    TotalCosts DECIMAL(15, 2),
    Keyword VARCHAR,
    OIA_Area VARCHAR(100),
    FOREIGN KEY (lead_research_institution_id) REFERENCES institutions(institution_id)
);

--table expenditures
CREATE TABLE expenditures (
    expenditure_id INT PRIMARY KEY,
    Expenditure_Type VARCHAR(200),
    project_id INT,
    FOREIGN KEY (project_id) REFERENCES projects(project_id)
);

--table research areas
CREATE TABLE research_areas (
    research_area_id INT PRIMARY KEY,
    FeildPrimary VARCHAR(100),
    FeildSecondary VARCHAR(100),
    DiscPrimary VARCHAR(100),
    DiscSecondary VARCHAR(100)
);

--creating view view_project_titles

CREATE VIEW view_project_titles AS
SELECT
    p.project_id,
    p.PTitle AS Project_Title,
    i.LeadInstitution AS Lead_Institution
FROM
    projects p
JOIN
    institutions i ON p.lead_research_institution_id = i.institution_id;

--creating view view_institution_projects 
CREATE VIEW view_institution_projects AS
SELECT
    i.institution_id,
    i.LeadInstitution AS Institution,
    COUNT(p.project_id) AS Number_of_Projects
FROM
    institutions i
LEFT JOIN
    projects p ON i.institution_id = p.lead_research_institution_id
GROUP BY
    i.institution_id, i.LeadInstitution;

-- query 1
-- display all projects with cost more than 100000
SELECT * FROM projects
WHERE TotalCosts > 100000;

-- query 2
-- display the number of projects in each inst id

SELECT lead_research_institution_id, COUNT(*) AS NumberOfProjects
FROM projects
GROUP BY lead_research_institution_id
	order by lead_research_institution_id;



-- query 3
-- give lead institution corresponding to each Project title.
SELECT p.PTitle, i.LeadInstitution
FROM projects p
JOIN institutions i ON p.lead_research_institution_id = i.institution_id;

--query 4
-- Display the Institution Ids whose total expenditure 
--(sum of cost of all project) is more than 50000.
SELECT lead_research_institution_id, SUM(TotalCosts) AS TotalExpenditure
FROM projects
GROUP BY lead_research_institution_id
HAVING SUM(TotalCosts) > 500000;

--query 5
-- Display the total Expenditure of all the lead Institutions
WITH ProjectCosts AS (
    SELECT lead_research_institution_id, SUM(TotalCosts) AS TotalExpenditure
    FROM projects
    GROUP BY lead_research_institution_id
)
SELECT i.LeadInstitution, pc.TotalExpenditure
FROM ProjectCosts pc
JOIN institutions i ON pc.lead_research_institution_id = i.institution_id;

--query 6
-- Divide all the projects in Budget Category with 
-- < 100000 as Low Budget, between 100000-500000 as Medium
-- and above 500000 as High Budget
SELECT PTitle, 
       CASE 
           WHEN TotalCosts < 100000 THEN 'Low Budget'
           WHEN TotalCosts BETWEEN 100000 AND 500000 THEN 'Medium Budget'
           ELSE 'High Budget'
       END AS BudgetCategory
FROM projects;

--query 7
-- Display all the project details of projects from Toronto City
SELECT * FROM projects
WHERE lead_research_institution_id IN 
	(SELECT institution_id FROM institutions WHERE City = 'Toronto');

-- query 8
-- Display all the projects with total cost above average cost.
SELECT PTitle, TotalCosts
FROM projects
WHERE TotalCosts > (SELECT AVG(TotalCosts) FROM projects);



-- query 9 using View
-- All project from lead institution ' University of Toronto'
SELECT * FROM view_project_titles
WHERE Lead_Institution = 'University of Toronto';


-- Query 10  Using view_institution_projects
-- All Institutions with Projects more than 5
SELECT Institution, Number_of_Projects
FROM view_institution_projects
WHERE Number_of_Projects > 5;

--Query 11
-- Give lead inst, primary feild and expenditure type of each project
SELECT 
    p.PTitle AS Project_Title, 
    i.LeadInstitution AS Institution,
    r.FeildPrimary AS Primary_Field,
    e.Expenditure_Type AS Expenditure_Type
FROM 
    projects p
JOIN 
    institutions i ON p.lead_research_institution_id = i.institution_id
JOIN 
    research_areas r ON p.research_area_id = r.research_area_id
JOIN 
    expenditures e ON p.project_id = e.project_id;

1)SELECT FirstName, LastName
FROM Employees
FOR XML RAW;
Output:

xml
Copy
Edit
<row FirstName="John" LastName="Doe" />
<row FirstName="Jane" LastName="Smith" />
✅ 2. FOR XML AUTO
Automatically nests elements based on the table structure and JOINs.

sql
Copy
Edit
SELECT E.FirstName, E.LastName, D.DepartmentName
FROM Employees E
JOIN Departments D ON E.DepartmentID = D.ID
FOR XML AUTO;
Output:

xml
Copy
Edit
<Employees FirstName="John" LastName="Doe">
  <Departments DepartmentName="IT" />
</Employees>
✅ 3. FOR XML PATH
Gives you full control over the XML structure—very customizable.

sql
Copy
Edit
SELECT
    FirstName AS 'Person/FirstName',
    LastName AS 'Person/LastName'
FROM Employees
FOR XML PATH('');
Output:

xml
Copy
Edit
<Person>
  <FirstName>John</FirstName>
  <LastName>Doe</LastName>
</Person>
Or to group multiple people under one root:

sql
Copy
Edit
SELECT
    FirstName,
    LastName
FROM Employees
FOR XML PATH('Employee'), ROOT('Employees');
✅ 4. FOR XML EXPLICIT (Advanced and mostly deprecated)
Provides very fine-grained control, but is complex and rarely used now in favor of FOR XML PATH.

✅ 5. Returning XML from a Stored Procedure
You can directly return XML from a stored procedure:

sql
Copy
Edit
CREATE PROCEDURE GetEmployees
AS
BEGIN
    SELECT FirstName, LastName
    FROM Employees
    FOR XML PATH('Employee'), ROOT('Employees');
END
✅ 6. Using XML Data Type
You can declare a variable of type XML, insert data into it, and return it:

sql
Copy
Edit
DECLARE @xml XML;

SET @xml = (
    SELECT FirstName, LastName
    FROM Employees
    FOR XML PATH('Employee'), ROOT('Employees')
);

SELECT @xml;
✅ 7. With Subqueries for Nesting
To get nested XML (e.g., orders inside customers):

sql
Copy
Edit
SELECT
    C.CustomerName,
    (
        SELECT OrderID, OrderDate
        FROM Orders O
        WHERE O.CustomerID = C.CustomerID
        FOR XML PATH('Order'), TYPE
    ) AS Orders
FROM Customers C
FOR XML PATH('Customer'), ROOT('Customers');

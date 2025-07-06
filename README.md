# DSA-Project-Assignment
Adekunle Micheal Assignment

WITH Customer_Sales AS (
    SELECT 
        TRIM(UPPER([Customer_Name])) AS Customer_Name,
        SUM([Sales]) AS Total_Sales
    FROM KMS_Order
    GROUP BY TRIM(UPPER([Customer_Name]))
),

Top_Customers AS (
    SELECT TOP 10 Customer_Name, Total_Sales
    FROM Customer_Sales
    ORDER BY Total_Sales DESC
),

Customer_Category_Sales AS (
    SELECT 
        TRIM(UPPER(o.[Customer_Name])) AS Customer_Name, 
        o.[Product_Category],
        SUM(o.[Sales]) AS Category_Sales
    FROM KMS_Order AS o
    JOIN Top_Customers AS tc
        ON TRIM(UPPER(o.[Customer_Name])) = tc.Customer_Name
    GROUP BY TRIM(UPPER(o.[Customer_Name])), o.[Product_Category]
)

SELECT 
    ccs.Customer_Name,
    ccs.Product_Category,
    ccs.Category_Sales
FROM Customer_Category_Sales AS ccs
ORDER BY ccs.Category_Sales DESC;

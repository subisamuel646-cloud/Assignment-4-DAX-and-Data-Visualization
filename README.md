# Assignment-4-DAX-and-Data-Visualization
PowerBI Assignment2 : DAX And Data Visualization

Calculated Columns:
● Create a Calculated Column for 'Category Type': Add a calculated column in the Order Details table that combines the 'Category' and 'Sub-Category' columns into a single 'Category Type' column.

Sol: In Table View, click the 3 dots next to OrderDetails tablename in the Data pane ---> New column --> Provide the DAX formula as Category Type = 'Order Details'[Category] & " - " & 'Order Details'[Sub-Category] --> Click enter in keyboard

● Calculate Revenue per Order in Order Details Table: Create a calculated column in the Order Details table to compute the revenue (Amount * Quantity) per order.

Sol : In Table View, click the 3 dots next to OrderDetails tablename in the Data pane ---> New column --> Provide the DAX formula as Revenue = 'Order Details'[Amount] * 'Order Details'[Quantity] --> Click enter in keyboard

● Create a Calculated Column to Categorize Sales: Add a calculated column named ‘Sales Category’ in the Order Details table that categorizes each order as 'Above Average' or 'Below Average' based on the Amount value.

Sol : In Table View, click the 3 dots next to OrderDetails tablename in the Data pane ---> New column --> Provide the DAX formula as Sales Category = IF('Order Details'[Amount] >= AVERAGE('Order Details'[Amount]), "Above Average", "Below Average") --> Click enter in keyboard



Calculated Measures:
● Calculate Order Count: Define a measure to count the total number of orders in the Order Details table.

Sol: In Table View, click the 3 dots next to OrderDetails tablename in the Data pane ---> New measure --> Provide the DAX formula as Total Unique Orders = DISTINCTCOUNT('List of Orders'[Order ID]) or Total Orders = COUNTROWS('Order Details') --> Click enter in keyboard

● Calculate Average Profit in Delhi: Create a measure to calculate the average profit for orders placed in Delhi.

In data modelling view , created a one to many relationship between Listoforders and OrderDetails using OrderID as the Key column

Sol: In Table View, click the 3 dots next to OrderDetails tablename in the Data pane ---> New measure --> Provide the DAX formula as Avg Profit Delhi = CALCULATE(AVERAGE('Order Details'[Profit]),'List of Orders'[City] = "Delhi") --> Click enter in keyboard

● Calculate Year-to-Date (YTD) Sales: Define a measure to calculate the total sales amount accumulated from the earliest order date up to each order date.

Sol: Create a calendar Table named Date_Table, 
---> Using New measure option for ListofOrders table, create MinDate and MaxDate measures and use cards to display on the report View
Create a Calendar Table ---> New Table option --> Specify Name of table --> And Formula
		Date_Table = ADDCOLUMNS(CALENDAR(DATE(2018,04,01),DATE(2019,03,31)),"Year",YEAR([Date]),"MonthNumber",MONTH([Date]),"MonthName",FORMAT([Date],"MMM"),"Quarter","Q" & QUARTER([Date]),"Day",DAY([Date]),"DayName",FORMAT([Date],"DDDD"))

Create a base measure that is Total_Sales_Amount in OrderDetails Table,
In Table View, click the 3 dots next to Date_Table tablename in the Data pane ---> New measure --> Provide the DAX formula as Total_Sales_Amount = SUM('Order Details'[Amount]) --> Click enter in keyboard ---> Use the Card visualization to view it on the Report View

In data modelling view , created a one to many relationship between Date_Table and ListofOrders using Date and OrderDate as the Key columns

Create a measure that is Sales_YTD in OrderDetails Table,
In Table View, click the 3 dots next to Date_Table tablename in the Data pane ---> New measure --> Provide the DAX formula as Sales_YTD = TOTALYTD([Total_Sales_Amount],Date_Table[Date]) --> Click enter in keyboard ---> Use the Card visualization to view it on the Report View



Data Visualization:
Sales Target Achievement by Category: 
	Compare actual sales with sales targets by category using a clustered column chart.

Sol: Created a many to many relationship between Sales target and Order Details On Category as key column

In the OrderDetails ---> New measure --> Provide DAX formula Actual_Sales = Sum('Order Details'[Revenue]) as Revenue is Amt*Quantity
In the SalesTarget --> New measure --> Provide DAX formula Sales_Target = Sum('Sales target'[Target])

Go to Report View --> Click on Clustered Column Chart in the Visualization pane ---> X-axis = Category from Order Details and Y axis = Actual_Sales and Sales_Target


2. Max Profit Margin by Sub-Category:
	Analyze the maximum profit margin for each sub-category of products using a donut chart.

Sol: In the OrderDetails ---> New measure --> Provide DAX formula MaxProfitMargin = MAX('Order Details'[Profit])
Go to Report View --> Click on Donut Chart in the Visualization pane ---> Legend = Sub-Category from Order Details and Values = MaxProfitMargin from Order Details ---> Then Format as needed


3. Monthly Sales Trend: 
	Show the trend of monthly sales over time using a line chart.

Sol: As we have the Date Table created using Order Date of ListOfOrders table and relationships are also done.
Go to Report View --> Click on Line Chart in the Visualization pane ---> X-axis = MonthName from Date Table and Y axis = Actual_Sales(calculated measure) from OrderDetails table  ---> Then Format as needed

4. Comparison of Profit and Quantity by Sub-Category: 
	Compare the relationship between profit and quantity sold for different sub-categories using a scatter chart.

Sol: Created 2 measures in Order Details table as Total Profit = SUM('Order Details'[Profit]) and Total Quantity Sold = SUM('Order Details'[Quantity])
Go to Report View --> Click on Scatter Chart in the Visualization pane --->Values = Sub-Category , X-axis = Total Quantity , Y axis = Total Profit from OrderDetails table  ---> Then Format as needed


5. Comparison of Total Sales Amount and Target: 
	Create cards to succinctly display the total sales amount alongside the sales target for quick comparison and analysis. Also, create a multi-row card to display the minimum target for each segment.

Sol: We have Actual_Sales calculated as Sum of Revenue in Order details Table and Sales_Target calculated as Sum of Target in Sales target table
     Calculate Minimum Target for each segment/category in Sales Target table
--> Sales target table Right click---> New Measure --> Min Target By Category as name and provide the DAX formula ---> Click enter

Go to Report View --> Click on Cards in the Visualization pane ---> card 1 : Values = Actual_Sales , Categories = category. card 2 : Values = Sales_target , categories = category ---> Then Format as needed
Go to Report View --> Click on Multi row Card in the Visualization pane ---> Fields = Category and Min Target by Category ---> Then Format as needed


6. Sales Performance Matrix: 
	Build a matrix view to analyze how actual sales compare to sales targets across different categories and months.

Sol: Go to Report View --> Click on Matrix in the Visualization pane ---> Rows: Category(Order Details) , Columns = Month of Order Date(Sales Target), Values : Actual_Sales and Sales_target ---> Then Format as needed

7. Geographic Sales Analysis: Visualize total sales on a map by city to identify regional sales patterns.

Sol: Go to Report View --> Click on Map in the Visualization pane ---> Location: City(ListOfOrders), Bubble Size : total_Sales_Amount/Actual_Sales (Orderdetails) ---> Then Format as needed

8. Sales Distribution by Sub-Category: Represent the sales distribution across different sub-categories using a treemap.

Sol: Go to Report View --> Click on TreeMap in the Visualization pane ---> Category : Category(OrderDetails), Details : Sub-category(OrderDetails), Values : Actual_Sales ---> Then Format as needed

9. Order Count Analysis by State: Create a funnel chart to visualize the distribution of order counts across different states.

Sol: Go to Report View --> Click on Funnel in the Visualization pane ---> Category : State(ListofOrders), Values : total Unique Orders(OrderDetails) ---> Then Format as needed

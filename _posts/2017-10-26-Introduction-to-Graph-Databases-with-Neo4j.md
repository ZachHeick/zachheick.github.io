---  
layout: post  
title: Introduction to Graph Databases with Neo4j  
---  

Relational database management systems (RDBMS) have been the go-to data storage system for many years. The strong theoretical foundation of relational databases and frequent use has led to the availability of many stable and standardized products. While relational databases fit well into many projects, there are trade-offs. I'll be going over some of these trade-offs and discuss why a graph database management system, such as Neo4j, might be better suited for the job. I'll also give a quick tutorial on how to set up and use Neo4j on your local machine!

![neo4j graph](https://zachheick.github.io/images/neo4j_graph.png){: .center-image }    

## Relational vs. Graph  

Relational databases do a great job of storing data into easy-to-undersetand structured tables. But what if I'm interested in the connections between data tables? I could call a complex query containing some "joins", but as data scales this could get very computationally expensive. Also, what if I know the data I'm dealing with won't always be consistent? NoSQL databases are an option, but these database systems store sets of data that are disconnected entirely. 

![relational model](https://zachheick.github.io/images/relational_model.png){: .center-image}  

**Neo4j** is a graph database management system that can handle both structured and unstructured data while storing the connections between data as well. Instead of storing data within <span class="green">tables and rows</span>, Neo4j represents a row as a <span class="green">node object</span>, where the node label would represent a table. These nodes are then connected by objects called <span class="blue">relationships</span>. Nodes and relationships can also have attributes, which are similar to column values in a table. 

![graph model](https://zachheick.github.io/images/graph_model.png){: .center-image}  

Creating a graph of nodes and relationships makes it easy to pick a starting point within the graph and explore neighbors by traversing over these relationships. This results in much faster query times compared to calling a bunch of "joins" in a relational database and searching through every row. The query times of the graph network is still very fast as it scales and becomes more complicated.   

## Setting Up Neo4j Locally  

Neo4j has created an easy-to-install desktop application for managing your local and remote databases.  

  1. [Download](https://neo4j.com/download/) Neo4j and start the desktop application.    
  2. Once you're in, create a new project and select "new database" and then "local". Give it a name and description and hit "create".  
  3. Start the database and open the URL [http://localhost:7474](http://localhost:7474) to begin working with Neo4j!   

Now that the database is up and running, we can now insert and extract data using Neo4j's query langauge, [Cypher](https://neo4j.com/developer/cypher-query-language/). Before moving onto the example, type `:play start` into the desktop editor. Here you can find built-in tutorials that cover the editor, the Cypher language, and playing around with a built-in movie dataset.    

## Insert and Extract Data with Python  

The "hello world" of database systems is the [Northwind database](https://northwinddatabase.codeplex.com/). We'll use Python and the CSV files from this database and transform it from tables and rows into a graph within our local Neo4j database. In order to connect to our local Neo4j database, install the Python driver with the command `pip install neo4j-driver`.  

Create a new Python file and import the driver. Then, create a driver and a new session.  

```python
from neo4j.v1 import GraphDatabase, basic_auth
driver = GraphDatabase.driver('bolt://localhost:7687', auth=basic_auth('neo4j', 'neo4j'))
session = driver.session()  
```  

Using the CSV files, create the customer, product, supplier, employee, category, and order nodes first.  

```python  
query = """
USING PERIODIC COMMIT
LOAD CSV WITH HEADERS FROM "{0}" AS row
CREATE (:Customer {{companyName: row.companyName, customerID: row.customerID, fax: row.fax, phone: row.phone}})
""".format(customers_csv)

session.run(query)
```  

```python  
query = """
USING PERIODIC COMMIT
LOAD CSV WITH HEADERS FROM "{0}" AS row
CREATE (:Product {{productName: row.productName, productID: row.productID, unitPrice: toFloat(row.unitPrice)}});
""".format(products_csv)

session.run(query)
```  

```python  
query = """
USING PERIODIC COMMIT
LOAD CSV WITH HEADERS FROM "{0}" AS row
CREATE (:Supplier {{companyName: row.companyName, supplierID: row.supplierID}});
""".format(suppliers_csv)

session.run(query)
```  

```python  
uery = """
USING PERIODIC COMMIT
LOAD CSV WITH HEADERS FROM "{0}" AS row
CREATE (:Employee {{employeeID:row.employeeID,  firstName: row.firstName, lastName: row.lastName, title: row.itle}});
""".format(employees_csv)

session.run(query)
```  

```python  
query = """
USING PERIODIC COMMIT
LOAD CSV WITH HEADERS FROM "{0}" AS row
CREATE (:Category {{categoryID: row.categoryID, categoryName: row.categoryName, description: row.description}});
""".format(categories_csv)

session.run(query)
```  

```python  
query = """
USING PERIODIC COMMIT
LOAD CSV WITH HEADERS FROM "{0}" AS row
MERGE (order:Order {{orderID: row.orderID}}) ON CREATE SET order.shipName =  row.shipName;
""".format(orders_csv)

session.run(query)
```  

Next, create relationships of orders to products and employees.  

```python  
query = """
USING PERIODIC COMMIT
LOAD CSV WITH HEADERS FROM "{0}" AS row
MATCH (order:Order {{orderID: row.orderID}})
MATCH (product:Product {{productID: row.productID}})
MERGE (order)-[pu:PRODUCT]->(product)
ON CREATE SET pu.unitPrice = toFloat(row.unitPrice), pu.quantity = toFloat(row.quantity);
""".format(order_details_csv)

session.run(query)
``` 

```python  
query = """
USING PERIODIC COMMIT
LOAD CSV WITH HEADERS FROM "{0}" AS row
MATCH (order:Order {{orderID: row.orderID}})
MATCH (employee:Employee {{employeeID: row.employeeID}})
MERGE (employee)-[:SOLD]->(order);
""".format(orders_csv)

session.run(query)
```  

```python  
query = """
LOAD CSV WITH HEADERS FROM "{0}" AS row
MATCH (order:Order {{orderID: row.orderID}})
MATCH (customer:Customer {{customerID: row.customerID}})
MERGE (customer)-[:PURCHASED]->(order);
""".format(orders_csv)

session.run(query)
```  

Then, create relationships between products, suppliers, and categories.  

```python  
query = """
USING PERIODIC COMMIT
LOAD CSV WITH HEADERS FROM "{0}" AS row
MATCH (product:Product {{productID: row.productID}})
MATCH (supplier:Supplier {{supplierID: row.supplierID}})
MERGE (supplier)-[:SUPPLIES]->(product);
""".format(products_csv)

session.run(query)
```  

```python  
query = """
USING PERIODIC COMMIT
LOAD CSV WITH HEADERS FROM "{0}" AS row
MATCH (product:Product {{productID: row.productID}})
MATCH (category:Category {{categoryID: row.categoryID}})
MERGE (product)-[:PART_OF]->(category);
""".format(products_csv)

session.run(query)
```  

Finally, create a relationship between employees and set our own indexes on each node.  

```python  
query = """
USING PERIODIC COMMIT
LOAD CSV WITH HEADERS FROM "{0}" AS row
MATCH (employee:Employee {{employeeID: row.employeeID}})
MATCH (manager:Employee {{employeeID: row.reportsTo}})
MERGE (employee)-[:REPORTS_TO]->(manager);
""".format(employees_csv)

session.run(query)
```  

```python  
query = """
CREATE INDEX ON :Product(productID);
CREATE INDEX ON :Product(productName);
CREATE INDEX ON :Category(categoryID);
CREATE INDEX ON :Employee(employeeID);
CREATE INDEX ON :Supplier(supplierID);
CREATE INDEX ON :Customer(customerID);
CREATE INDEX ON :Customer(customerName);
CREATE CONSTRAINT ON (o:Order) ASSERT o.orderID IS UNIQUE;
"""

session.run(query)
```  

## Other Resources  



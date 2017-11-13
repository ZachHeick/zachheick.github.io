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

Now that the database is up and running, we can now insert and extract data using Neo4j's query langauge, [Cypher](https://neo4j.com/developer/cypher-query-language/).  

## Insert and Extract Data with Python  

## Other Resources  



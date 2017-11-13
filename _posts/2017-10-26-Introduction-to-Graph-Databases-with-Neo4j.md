---  
layout: post  
title: Introduction to Graph Databases with Neo4j  
---  

Relational database management systems (RDBMS) have been the go-to data storage system for many years. The strong theoretical foundation of relational databases and frequent use has led to the availability of many stable and standardized products. While relational databases fit well into many projects, there are trade-offs. I'll be going over some of these trade-offs and discuss why a graph database management system, such as Neo4j, might be better suited for the job. I'll also give a quick tutorial on how to set up and use Neo4j on your local machine!

![neo4j graph](https://zachheick.github.io/images/neo4j_graph.png){: /center-image }    

## Relational vs. Graph  

Relational databases do a great job of storing data into easy-to-undersetand structured tables. But what if I'm interested in the connections between data tables? I could call a complex query containing some "joins", but as data scales this could get very computationally expensive. Also, what if I know the data I'm dealing with won't always be consistent? NoSQL databases are an option, but these database systems store sets of data that are disconnected entirely. 

**Neo4j** is a graph database management system that can handle both structured and unstructured data while storing the connections between data as well. Instead of storing data within tables and rows, each row is represented as a node object, where the type of node would represent a table. These nodes are then connected by objects called relationships. Structuring the data as nodes and relationships makes it easy to pick a starting point within the graph and explore neighbors by traversing over these relationships, resulting in much faster query times than calling a bunch of "joins" in a relational database. The query times of the graph network is still very fast as it scales and becomes more complicated.   

## Setting Up Neo4j Locally  

## Load and Extract Data with Python  

## Other Resources  



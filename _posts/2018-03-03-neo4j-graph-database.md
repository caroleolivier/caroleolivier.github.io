---
layout: post
title: Neo4j, a graph database
date: 2018-03-03
---

I went to a very cool meetup this week about [Neo4j](https://neo4j.com/), a graph database. It is not something I have had the chance to get my hands on yet, so it was great to get a taste of it. This post is a short introduction to graph database and a small example of what one can do with Neo4j.

### Graph databases

##### Definition

> In computing, a graph database is a database that uses graph structures for semantic queries with nodes, edges and properties to represent and store data. A key concept of the system is the graph (or edge or relationship), which directly relates data items in the store.
> [Wikipedia](https://en.wikipedia.org/wiki/Graph_database)

So a graph database uses a graph at its core, this is very important, and to insist even more on the idea of graph:

> [...] a graph database is a database designed to treat the relationships between data as a first-class citizen in the data model.
> [Neo4j](https://neo4j.com/developer/graph-database/)

Based on these definitions and the experience I have working with relational databases, I feel like relational databases are good for storing data and dealing with very simple relationships (e.g. customer, account, which account belongs to which customer).
On the other hand, graph databases are good for storing data together with their relationships and resolve complex relationships queries (e.g. people, connection, who are the friends of my friends of my friends).
<br/>This being said, I am very new to this world so don't take my word for granted!

##### Structure

At its minimum a graph database is made of:
* __Node__
<br/>Represents a basic entity that can exist on its own (e.g. a person)
* __Relationship__
<br/>Represents a connection between two nodes (e.g. is friends with)

And to complete the representation of the graph and make it more interesting, graph databases also define:
* __Properties__
<br/>Information about nodes (e.g. name of a person) or relationships (met in xx).


### Neo4j

Let's take a look at an actual graph database: [Neo4j](https://neo4j.com/). Bear in mind that I didn't pick Neo4j because I think it is the best graph database platform out there, I picked it because this is what I learnt this week.
<br/>One of the great thing though, is that it has a very nice [sandbox](https://neo4j.com/sandbox-v2/#) available for free. So it is very easy to get started, no need to install anything.

Neo4j uses a query language called Cypher. This [cheatsheet](https://neo4j.com/docs/cypher-refcard/current/) gives a good idea of what the language looks like.

##### Creating a graph and relationships

Here a little graph of people that I have created:

![People graph]({{ "/assets/blog/neo4j_basic_graph.png" | absolute_url }}){:height="80%" width="80%"}

You can see the command I run to create the nodes: `CREATE (:Person { name: "Alice"})`. `Person` is called a label. `name: "Carole"` is a property of the node with key `name` and value `Alice`.

I then created relationships between people, here is my final graph:

![Friends graph]({{ "/assets/blog/neo4j_graph_with_friends.png" | absolute_url }}){:height="80%" width="80%"}

I find the graph so cool! It is super simple to create relationships. You need to match the nodes you want to create relationships on and then create the relationships:
```
MATCH(claire:Person{name: "Claire"})
MATCH(alice:Person{name: "Alice"})
CREATE (claire)-[r:FRIENDS_WITH]->(alice)
```


##### Analysing the graph

Now that I have a graph (yes, it's a tiny one...), let's take a look at people's relationships.


###### Find friends

Who are my friends?

![Carole's friend]({{ "/assets/blog/neo4j_carole_friends.png" | absolute_url }}){:height="80%" width="80%"}

This is sad, I should work on my social life... 


Who are Alice's friends?

![Carole's friend]({{ "/assets/blog/neo4j_alice_friends.png" | absolute_url }}){:height="80%" width="80%"}

Well, she is more popular than me...

Let's have a look at the query:
```
MATCH (alice:Person{ name: "Alice" })-[:FRIENDS_WITH]-(p:Person)
RETURN p
```
I find the query super readable: we are looking at the `Person` node Alice, i.e. with property `name` equal to `"Alice"` and we are looking for the `Person` nodes who have the relationships `FRIENDS WITH` with her.


###### Find friends of my friends

Now, a slightly more complex query, who are friends with my friends? (well, friend in this case...)

![Carole's friend's friends]({{ "/assets/blog/neo4j_friends_of_friends.png" | absolute_url }}){:height="80%" width="80%"}

Thanks to my single friend Alice, I am indeed connected to John and Claire.
<br/>
And this is the query:
```
MATCH (carole:Person{ name: "Carole" })-[:FRIENDS_WITH]-(p1:Person)-[:FRIENDS_WITH]-(p2:Person)
RETURN p2
```

Again super readable! We are simply traversing the graph and describing what we are looking for: looking at the `Person` node `Carole` we are interested in her friends, i.e. `Person` nodes with relationships `FRIENDS_WITH` and from there we are looking for their friends as well.

<br/>

And that is my full experience with graph database so far!

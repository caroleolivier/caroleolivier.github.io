---
layout: post
title: HowTo GraphQL and Learnings
date: 2022-03-05
---

This post contains a few useful resources for learning the fundamentals of GraphQL and my main learnings so far.

## HowTo GraphQL

### Fundamentals

- [GraphQL](https://graphql.org/): great for the theory, so a good place to go back to learn definitions and the spec.

- [How to GraphQL](https://www.howtographql.com/): website that's packed with great tutorials, so very hands-on.

### Mental Model

- [Thinking in Graphs](https://graphql.org/learn/thinking-in-graphs/): useful to understand what the thinking behind GraphQL is: it's all about thinking in graphs.

- [GraphQL: The Mental Model](https://www.youtube.com/watch?v=zWhVAN4Tg6M): great video about the mental model behind GraphQL.  
Again, it's all about **graphs**.


### In practice

This is what I did in practice and it taught me a great deal about GraphQL:

- Server side: [Apollo GraphQL Server](https://www.howtographql.com/typescript-apollo/1-getting-started/)

- Client side: [Apollo GraphQL Client](https://www.apollographql.com/docs/react/get-started/)

I did both in 1/2 day, the learning curve is small.  
Note for self: this is my [playground](https://github.com/caroleolivier/graphql-playground).

## Main learnings (so far)

This sections contains my main learnings and takeaways.  
Some are obvious.  
Others aren't (at least to me).  

### About the concepts

- GraphQL is a specification

- GraphQL stands for ? - no idea, I can't find the "official" answer.  
But one could think it has to do with `graphs` and `query language`.

- GraphQL has the word graph, it's a good idea to think about your business model as a graph when you use it, it'll help you best use it.  
(Even if in the official [spec](https://spec.graphql.org/October2021/), it has the word `graph` only twice).

- It's a client and server side spec.


### In practice

- [Apollo](https://www.apollographql.com/) is one of the library that implements the GraphQL spec.  
They talk a lot about graphs there. They even refer to **The** graph.  
It's super easy to use with TypeScript, NodeJS, React.

- Security: if you're not careful you could be exposing your schema to the world  
So it's a good idea to use things like [Persisted Queries](https://www.apollographql.com/docs/apollo-server/performance/apq/)

- Performance: if you're not careful and don't understand your graph well and how caching works, you may end up hammering your server with similar-ish queries over and over from different React components.  
It's so easy to fetch data from the client.  
This is where I need to focus next.

- Visualisation of the graph: this is a cool [tool](https://studio.apollographql.com/sandbox/explorer).
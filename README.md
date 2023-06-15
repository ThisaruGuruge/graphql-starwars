# Star Wars GraphQL API

[![Star on Github](https://img.shields.io/badge/-Star%20on%20Github-blue?style=social&logo=github)](https://github.com/ballerina-platform/module-ballerina-graphql)

_Authors_: @DimuthuMadushan @ThisaruGuruge  
_Reviewers_: @shafreenAnfar @ThisaruGuruge  
_Created_: 2021/02/07  
_Updated_: 2023/06/15  

## Overview

This is an implementation of `Star Wars` example in Ballerina. The `Star Wars` schema and resolvers are based on popular Star Wars characters. This sample implementation can be used to learn the Ballerina GraphQL package functionalities.

It includes the Query, Mutation, and Subscription operations. It also includes the GraphQL Union types and Interfaces.

## Implementation

Implementation is purely done using the Ballerina GraphQL package. In order to maintain the simplicity a simple in memory datasource is created using Ballerina tables.

It extensively uses the Ballerina Query feature. A SQL like query language which radically simplifies the implementation of the service.

Following is the complete GraphQL schema of the API:

```graphql
type Query {
  "Fetch the hero of the Star Wars"
  hero(episode: Episode): Character!
  "Returns reviews of the Star Wars"
  reviews(episode: Episode!): [Review]!
  "Returns characters by id, or null if character is not found"
  characters(idList: [String!]!): [Character]!
  "Returns a droid by id, or null if droid is not found"
  droid(id: String! = ""): Droid
  "Returns a human by id, or null if human is not found"
  human(id: String!): Human
  "Returns a starship by id, or null if starship is not found"
  starship(id: String!): Starship
  "Returns search results by text, or null if search item is not found"
  search(text: String!): [SearchResult!]
}

type Mutation {
    "Add new reviews and return the review values"
    createReview(
        "Episode name"
        episode: Episode!
        "Review of the episode"
        reviewInput: ReviewInput!
    ): Review!
}

type Subscription {
    "Subscribe to review updates"
    reviewAdded(
        "Episode name"
        episode: Episode!
    ): Review!
}

"A mechanical character from the Star Wars universe"
interface Character {
  "The unique identifier of the character"
  id: String!
  "The name of the character"
  name: String!
  "This character's friends, or an empty list if they have none"
  friends: [Character!]!
  "The episodes this character appears in"
  appearsIn: [Episode!]!
}

"An autonomous mechanical character in the Star Wars universe"
type Droid implements Character {
  "The unique identifier of the droid"
  id: String!
  "The name of the droid"
  name: String!
  "This droid's friends, or an empty list if they have none"
  friends: [Character!]!
  "The episodes this droid appears in"
  appearsIn: [Episode!]!
  "This droid's primary function"
  primaryFunction: String
}

"A humanoid creature from the Star Wars universe"
type Human implements Character {
  "The unique identifier of the human"
  id: String!
  "The name of the human"
  name: String!
  "The home planet of the human, or null if unknown"
  homePlanet: String
  "Height in meters, or null if unknown"
  height: Float
  "Mass in kilograms, or null if unknown"
  mass: Int
  "This human's friends, or an empty list if they have none"
  friends: [Character!]!
  "The episodes this human appears in"
  appearsIn: [Episode!]!
  "A list of starships this person has piloted, or an empty list if none"
  starships: [Starship!]!
}

"A ship from the Star Wars universe"
type Starship {
  "The unique identifier of the starship"
  id: String!
  "The name of the starship"
  name: String!
  "The length of the starship, or null if unknown"
  length: Float
  "Coordinates of the starship, or null if unknown"
  coordinates: [[Float!]!]
}

type Review {
  episode: Episode!
  stars: Int!
  commentary: String
}

enum Episode {
    JEDI
    EMPIRE
    NEWHOPE
}

"auto-generated union type from Ballerina"
union SearchResult = Human|Droid|Starship

input ReviewInput {
  stars: Int!
  commentary: String
}
```

## Starting the Service

To start the service, move into the starwars folder and execute the below command.

```shell
bal run
```

It will build the Star Wars Ballerina project and then run it.

**Example Query**

```graphql
query {
    hero(episode: EMPIRE) {
        ...on Human {
            name
            homePlanet
            friends {
                ...on Droid {
                    name
                    primaryFunction
                }
            }
        }
    }
}
```

## TRIVIA-APP Application Programming Interface(API)

### Introduction
This API allows you to create questions and categorize  for  Trivia  games.  Teachers or Managers can utilize this API to make their learning sessions/meetings fun and interactive.

### Getting Started
This API can presently be run only on a local server using the **Base URL** provided below(it has not been deployed to any remote  server). 
- Base URL: ```http://localhost:5000```

### Error Handling
#####  Response Object
The errors are returned as JSON Objects in the following format:
```
{
    "error": 400,
    "message": " Bad request"
}
```
##### Response Keys
`error` : Status of the error that occurred. Visit this [website](https://httpstatusdogs.com/) to learn more about the http error status codes
`message` : Meaning of the  error that accured. 
##### Status Codes
The API will return any of the following codes should your request fail:
`400 (Bad Request) : Your client request was not properly formatted.`
`404 (Not Found) : The resource you are requesting for is not found.`
`422 (Unproccessable) : The server understood your request but it could not process it.`
`500 (Server Error) : Something went wrong on the server.`

### Endpoint Library
### Categories

### `GET /categories`

This fetches all the question categories as a json object with each category's id as the key and type as value.

#### Query Parameters

This endpoint takes in no query parameter.

#### Request Body

This endpoint doesn't require a request body.

#### Sample Request

`curl http://localhost:5000/categories`

#### Sample Response

```
{
    "1": "Science",
    "2", "Art",
    "3": "History"
}
```

### `POST /categories`

This adds a category to the collection of categories in the database. It takes in category type .

#### Query Parameters

This endpoint takes in no query parameters

#### Request Body

`type`: string <small> (required) </small> - Category type <br>

```
{"type": "Entertainment"}
```

#### Sample Request

`curl http://localhost:5000/categories -X POST -H "{Content-Type: 'application/json'}" -d '{"type": "Entertainment"}'`

#### Sample Response

`added`: int - Id of the added category. <br>
`success`: boolean - Request success status. <br>

```
{
    "added": 1,
    "success": True
}
```

### `GET /categories/{category_id}/questions`

This returns all the questions along with the total number of questions within a particular category.

#### Query Parameters

This endpoint does not take in query parameters.

#### Request Body

This endpoint does not require a request body.

#### Sample Request

`curl http://localhost:5000/categories/`

#### Sample Response

`questions`: array - All questions within the specified category. <br>
`totalQuestions`: int - Total number of questions within specified category. <br>

```
{
  "questions": [
    {
      "answer": "Brazil",
      "category": 6,
      "difficulty": 3,
      "id": 10,
      "question": "Which is the only team to play in every soccer World Cup tournament?"
    },
    {
      "answer": "Uruguay",
      "category": 6,
      "difficulty": 4,
      "id": 11,
      "question": "Which country won the first ever soccer World Cup in 1930?"
    }
  ],
  "totalQuestions": 2
}
```

### Questions

### `GET /questions`

This returns a paginated list of all questions within the database along, all categories and the total number of questions. Each page contains a maximum of 10 questions.

#### Query Parameters

`page`: int <small> (optional) </small> - Page number starting from 1.

#### Request Body

This endpoint does not require a request body

#### Sample Request

`curl http://localhost:5000/questions?page=2`

#### Sample Response

`questions`: array - Fetched questions. <br>
`totalQuestions`: int - Total number of questions in the database. <br>

```
{
  "questions": [
    {
      "answer": "Brazil",
      "category": 6,
      "difficulty": 3,
      "id": 10,
      "question": "Which is the only team to play in every soccer World Cup tournament?"
    }, {
      "answer": "Uruguay",
      "category": 6,
      "difficulty": 4,
      "id": 11,
      "question": "Which country won the first ever soccer World Cup in 1930?"
    }
  ],
  "categories" : {
    "1": "Science",
    "2", "Art",
    "3": "History"
   },
  "totalQuestions": 2
}
```

### `POST /questions`

This adds a question to the collection of questions in the database. It takes in the question, its category id, its difficulty rating and answer.

#### Query Parameters

This endpoint takes in no query parameters

#### Request Body

`question`: string <small> (required) </small> - Question text content. <br>
`answer`: string <small> (required) </small> - Answer to the question. <br>
`category`: int <small> (required) </small> - Category id of the question's category. <br>
`difficulty`:string <small> (required) </small> - Question's difficulty from 1 to 5. <br>

```
{
    "question": "Which country won the first ever soccer World Cup in 1930?",
    "answer": "Uruguay",
    "category": 6,
    "difficulty": 4
}
```

#### Sample Request

`curl http://localhost:5000/questions -X POST -H "{Content-Type: 'application/json'}" -d '{ "question": "Which country won the first ever soccer World Cup in 1930?", "answer": "Uruguay", "category": 6, "difficulty": 4 }'`

#### Sample Response

`added`: int - Id of the added question. <br>
`success`: boolean - Request success status. <br>

```
{
    "added": 1,
    "success": True
}
```

### `POST /questions` (SEARCH)

This performs a case insensitive search of questions from the database based on a search term. It returns an array of the questions and the total amount of questions that match the search term.

#### Query Parameters

This endpoint takes in no query parameters

#### Request Body

`searchTerm`: string <small> (required) </small> - Term to search for. <br>

`{ "searchTerm": "soccer"}`

#### Sample Request

`curl http://localhost:5000/questions -X POST -H "{Content-Type: 'application/json'}" -d '{ "searchTerm": "soccer"}'`

#### Sample Response

`questions`: array - All questions that match the search term. <br>
`totalQuestions`: int - Total number of questions that match the search term. <br>

```
{
  "questions": [
    {
      "answer": "Brazil",
      "category": 6,
      "difficulty": 3,
      "id": 10,
      "question": "Which is the only team to play in every soccer World Cup tournament?"
    },
    {
      "answer": "Uruguay",
      "category": 6,
      "difficulty": 4,
      "id": 11,
      "question": "Which country won the first ever soccer World Cup in 1930?"
    }
  ],
  "totalQuestions": 2
}
```

### `DELETE /questions/{question_id}`

This deletes the question with the specified id. It returns the id of the deleted question and a success status.

#### Query Parameters

This endpoint takes in no query parameters.

#### Request Body

This endpoint requires no request body.

#### Sample Request

`curl http://localhost:5000/questions/1 -X DELETE`

#### Sample Response

`deleted`: int - Id of the deleted question. <br>
`success`: boolean - Request success status. <br>

```
{
    "deleted": 1,
    "success": True
}
```

### Quizzes

### `POST /quizzes`

This returns a random question from the database within a specified category or from a random category if none is specified. It accepts an array of previous questions to ensure that a question that has been chosen before is not chosen again. If there are no other questions to left, it returns null.

#### Query Parameters

This endpoint takes in no query parameters

#### Request Body

`previous_questions`: array <small> (required) </small> - Contains ids of previously chosen questions. <br>
`quiz_category`: int <small> (optional) </small> - Current category. <br>

```
{
    "previous_questions": [10],
    "quiz_category": 6
}
```

#### Sample Request

`curl http://localhost:5000/questions -X POST -H "{Content-Type: 'application/json'}" -d '{"previous_questions": [10], "quiz_category": 6 }'`

#### Sample Response

`question`: object|null - randomly chosen question.

```
{
    "question": {
      "answer": "Uruguay",
      "category": 6,
      "difficulty": 4,
      "id": 11,
      "question": "Which country won the first ever soccer World Cup in 1930?"
    },
}
```

### Leaderboard

### `GET /leaderboard`

This returns a paginated list of all players and their scores in the database along. Each page contains a maximum of 10 results.

#### Query Parameters

`page`: int <small> (optional) </small> - Page number starting from 1.

#### Request Body

This endpoint does not require a request body

#### Sample Request

`curl http://localhost:5000/leaderboard?page=2`

#### Sample Response

`results`: array - Fetched results. <br>
`totalresults`: int - Total number of results in the database. <br>

```
{
  "results": [
    {
      "id": 1,
      "player": "Karen",
      "score": 5,
    }, {
      "id": 10,
      "player": "Sandy",
      "score": 4,
    }
  ],
  "totalResults": 2
}
```

### `POST /leaderboard`

This adds a player's name and score to the database.

#### Query Parameters

This endpoint takes in no query parameters

#### Request Body

`player`: string <small> (required) </small> - Player's name. <br>
`score`: int <small> (required) </small> - Player's score. <br>

```
{
    "player": "Karen O",
    "score": 4
}
```

#### Sample Request

`curl http://localhost:5000/leaderboard -X POST -H "{Content-Type: 'application/json'}" -d '{ "player": "Karen O" "score": 4 }'`

#### Sample Response

`added`: int - Id of the added result. <br>
`success`: boolean - Request success status. <br>

```
{
    "added": 1,
    "success": True
}
```







This recipe for **cereal and milk** has been passed down my family for months.

### Ingredients

- Cereal (you can find cool cereals [here](www.example.com/coolcereals))
- Milk


### Directions

If I were writing these out as _code_, it might look something like this:

```if bowl is empty:
    add cereal
if bowl only has cereal in it:
    add milk
```
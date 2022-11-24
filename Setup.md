## Prerequisite Setup
The given document helps to set up the APIs which are being automated by the framework.</br>

The APIs automated in the given framework are created using json server. Steps for creating the given API are mentioned below -

1. To create a Fake Rest API we need to install node.js.
2. Once Node.js is installed, run command **npm install -g json server** within command prompt. The given command would install json-sevrer npm module on your machine.
3. Now let’s create a new JSON file with name response.json. This file contains the data which should be exposed by the REST API. For objects contained in the JSON structure CRUD entpoints are created automatically. Take a look at the following sample response.json file:

```js

{
  "employees": [
    {
      "id": 1,
      "first_name": "Vishesh",
      "last_name": "Gupta",
      "email": "vishesh@abc.com",
      "jobs": [
        "tester",
        "trainer"
      ],
      "favfoods": {
        "breakfast": "Poha",
        "lunch": "Roti",
        "dinner": [
          "Pulav",
          "Milk"
        ]
      }
    },
    {
      "id": 545,
      "first_name": "Grant",
      "last_name": "Kiehn",
      "email": "Abe.Reilly@hotmail.com",
      "jobs": [
        "Tester",
        "Trainer"
      ],
      "favFoods": {
        "breakfast": "idly",
        "lunch": "rice",
        "dinner": [
          "Chapati",
          "Milk"
        ]
      }
    }
  ]
}

```

4. Run the Json server at location where your response.json file is present using command **json-server --watch response.json** . Watch parameter ensures that the server is started in watch mode which means that it watches for file changes and updates the exposed API accordingly.

5.The following htttp end points are created by Json Server

```js

GET    /employees
GET    /employees/{id}
POST   /employees
PUT    /employees/{id}
PATCH  /employees/{id}
DELETE /employees/{id}

```

### Note
1.By default Json-Server runs on port 3000, If you wish to run on different port use command **json-server -p 4000 response.json** </br>


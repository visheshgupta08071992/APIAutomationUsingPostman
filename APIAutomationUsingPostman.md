## APIAutomationUsingPostman
The given repo explains how could we create an Automation Framework using Postman

**Prerequisite** </br>
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

4. Run the Json server at location where your db.json file is present using command **json-server --watch response.json** . Watch parameter ensures that the server is started in watch mode which means that it watches for file changes and updates the exposed API accordingly.

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


### Automated API Collection 

Attaching the Zipped Postman collection Json which consist of Automated Test Scenarios on the above created APIs

[APIAutomationUsingPostman.postman_collection.zip](https://github.com/visheshgupta08071992/APIAutomationUsingPostman/files/9692430/APIAutomationUsingPostman.postman_collection.zip)


### Some of the generic validation used in above framework against below response

**Response**
```js


{
    "id": 408,
    "first_name": "Brennon",
    "last_name": "Stehr",
    "email": "abc@abc.com",
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

```

**Validations**
```js

//Storing the response body
var jsonData = pm.response.json();

//Verify Status Code
pm.test("Verify Create Employee request is successfull with Status code 201", function () {
    pm.response.to.have.status(201);


    if(pm.response.code===201){ 
       //If request is successful the store id as collection variable and hit next request
       pm.collectionVariables.set("id", pm.response.json().id);
       postman.setNextRequest("GetEmployeeDetailFromEmployeeId");
    }
    else{
        //If the Create Employee request is unsucceful then stop the execution
        postman.setNextRequest(null);
    }
});



```




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

**1.To Store response body as Json in a variable**

```js
var jsonData = pm.response.json();
```

**2.To Verify Status Code and If request is successful then store id as collection variable which can be used in other request within the same collection and hit next request**

```js

//Verify Status Code
pm.test("Verify Create Employee request is successfull with Status code 201", function () {
    pm.response.to.have.status(201);


    if(pm.response.code===201){ 
       //If request is successful the store id as collection variable which can be used in other request within the same collection and hit next request
       pm.collectionVariables.set("id", pm.response.json().id);
       postman.setNextRequest("GetEmployeeDetailFromEmployeeId");
    }
    else{
        //If the Create Employee request is unsuccessfull then stop the execution
        postman.setNextRequest(null);
    }
});

```

**3. To Verify response body value check**

```js

//Verify response body value check

//Verify email value
pm.test("Verify correct email is displayed", function () {
    pm.expect(jsonData.email).to.eql('abc@abc.com');
});

//Verify values of Jobs Array

pm.test("Verify Value of Job1 is correct", function () {
    pm.expect(jsonData.jobs[0]).to.eql('Tester');
});

//Verifying value of Job2 as a separate Test not including it Job1 test as if the value of Job1 fails then the value of Job2 would not be verified
pm.test("Verify Value of Job2 is correct", function () {
    pm.expect(jsonData.jobs[1]).to.eql('Trainer');
});

//Verify value of favFoods Object
pm.test("Verify Value of breakfast is correct", function () {
    pm.expect(jsonData.favFoods.breakfast).to.eql('idly');
});

//Verify value of favFoods Object
pm.test("Verify Value of lunch is correct", function () {
    pm.expect(jsonData.favFoods.lunch).to.eql('rice');
});

//verify value of dinner array present within favFoods Object
pm.test("Verify Value of dinner1 is correct", function () {
    pm.expect(jsonData.favFoods.dinner[0]).to.eql('Chapati');
});

//verify value of dinner array 2 present within favFoods Object
pm.test("Verify Value of dinner2 is correct", function () {
    pm.expect(jsonData.favFoods.dinner[1]).to.eql('Milk');
});

```

**4. To Validate Json Schema**

```js
/*
Validate complete Json schema, The given test would validate whether all the required attributes are present within the response. 
It would also validate whether  values of all the attributes are of correct type be it String,Integer,Boolean,Object or an Array
Validating JSON Schema solves most of the communication problems between provider and consumer of the RESTful API service.
*/

/*
We can get the schema of our json by pasting the json here https://www.liquid-technologies.com/online-json-to-schema-converter.
Ensure to remove attribute "$schema" attribute which is added in schema after converting json into schema
*/

var schema ={
  "type": "object",
  "properties": {
    "id": {
      "type": "integer"
    },
    "first_name": {
      "type": "string"
    },
    "last_name": {
      "type": "string"
    },
    "email": {
      "type": "string"
    },
    "jobs": {
      "type": "array",
      "items": [
        {
          "type": "string"
        },
        {
          "type": "string"
        }
      ]
    },
    "favFoods": {
      "type": "object",
      "properties": {
        "breakfast": {
          "type": "string"
        },
        "lunch": {
          "type": "string"
        },
        "dinner": {
          "type": "array",
          "items": [
            {
              "type": "string"
            },
            {
              "type": "string"
            }
          ]
        }
      },
      "required": [
        "breakfast",
        "lunch",
        "dinner"
      ]
    }
  },
  "required": [
    "id",
    "first_name",
    "last_name",
    "email",
    "jobs",
    "favFoods"
  ]
};

pm.test('Verify CreateEmployee response Schema', function () {
   pm.response.to.have.jsonSchema(schema);
});

```

**4. To Validate Json Schema does not have addtional properties**

```js

/*
Validate Create Employee response schema does not have additional attributes, This validation is important to ensure our API does not send additional
attributes apart from the ones mentioned in the contract, If Additional attributes are received then the TC would fail. This scenario could have been
covered within schema validation test but adding it within a separate test to get a clear picture with regards to Test Failure

To validate test for additional properties, we must have additionalProperties Tag set to false within the json schema

To get schema of our json with additionalProperties set as false with below steps -

1.Paste json here https://www.liquid-technologies.com/online-json-to-schema-converter.
2.Click on Options button and uncheck defaultAdditionalProperties checkbox
3.Click on generate Schema
4.Ensure to remove attribute "$schema" attribute which is added in schema after converting json into schema
*/

var additionalPropertySchema ={
  "type": "object",
  "properties": {
    "id": {
      "type": "integer"
    },
    "first_name": {
      "type": "string"
    },
    "last_name": {
      "type": "string"
    },
    "email": {
      "type": "string"
    },
    "jobs": {
      "type": "array",
      "items": [
        {
          "type": "string"
        },
        {
          "type": "string"
        }
      ]
    },
    "favFoods": {
      "type": "object",
      "properties": {
        "breakfast": {
          "type": "string"
        },
        "lunch": {
          "type": "string"
        },
        "dinner": {
          "type": "array",
          "items": [
            {
              "type": "string"
            },
            {
              "type": "string"
            }
          ]
        }
      },
      "additionalProperties": false,
      "required": [
        "breakfast",
        "lunch",
        "dinner"
      ]
    }
  },
  "additionalProperties": false,
  "required": [
    "id",
    "first_name",
    "last_name",
    "email",
    "jobs",
    "favFoods"
  ]
};

pm.test('Verify CreateEmployee response Schema does not have any additional properties', function () {
   pm.response.to.have.jsonSchema(additionalPropertySchema);
});

```

If there are additional properties within the response then the validation would fail with error **data should NOT have additional properties** . The assertion does not specify what additional properties are present in the response, If there are two additional properties then the error message would be displayed twice, similary if there are n additional properties then the error message would be displayed n times. Please refer below image where two additional properties found in response and we can see the Assertion error twice.

![image](https://user-images.githubusercontent.com/52998083/193455159-12e5ce02-b0e0-4a9e-b3b8-f8e05d99659a.png)


**5. To Validate response is of type array**

```js
//verify response is of type array

pm.test("Verify Get Employee Details request returns array", function () {
    pm.expect(jsonData).to.be.an('array');
});

```

**6. To Validate response is of type object**

```js
//verify response is of type object

pm.test("Verify Delete Employee Details request returns object", function () {
    pm.expect(jsonData).to.be.an('object');
});

```

**7. To Validate response is an empty object**

```js
//verify response is an empty object

pm.test("Verify Delete Employee Details request returns an empty object", function () {
    pm.expect(jsonData).to.be.empty;
});

```



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

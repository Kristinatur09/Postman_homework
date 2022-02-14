# Homework_3, Postman
____
## **1. Необходимо залогиниться**
___
+ Request:
```js
POST

http://162.55.220.72:5005/login

login : test_login

password : test_1
```

+ Responce:
```json
{
    "token": "/s34lfgbj/test_login/jjd909 38073kjkWpqc2069fuler456148681evny"
}
```
1. Приходящий токен необходимо передать во все остальные запросы:

```js
var jsonData = pm.response.json();
pm.environment.set("auth_token", jsonData.token);
```

## **2. http://162.55.220.72:5005/user_info**
___

+ Request (raw json)

```js
POST

{
    "age": 29,
    "salary": 1000,
    "name": "Kristina",
    "auth_token": "{{auth_token}}"

}
```
    

+ Response:

```Js 
{
    'start_qa_salary':salary,
    'qa_salary_after_6_months': salary * 2,
    'qa_salary_after_12_months': salary * 2.9,
    'person': {'u_name':[user_name, salary, age],
                                    'u_age':age,
                                    'u_salary_1.5_year':
                                        salary * 4}
                                    }
```

## Tests:
1) Статус код 200
```js
pm.test("Check the status code is 200", function () {
    pm.response.to.have.status(200);
 });
```
2. Проверка структуры json в ответе:
```js
    var jsonData = pm.response.json();

    var schema = {
       "type":"object",
        "properties":{
            "person":{"type":"object"},
            "properties":{
            "u_age": {"type":"int"},
            "u_name": {"type":"array",
                    "prefixItems": [
                    { "type": "string"},
                    { "type":"int"},
                    { "type":"int"},
                    ]     
        },
        "u_salary_1_5_year":{"type":"int"},
        },
        "qa_salary_after_12_months": {"type":"int"},
        "qa_salary_after_6_months": {"type":"int"},
        "start_qa_salary": {"type":"int"},
        },
    }
    pm.test('Shema is valid',function(){
        pm.expect(tv4.validate(jsonData,schema)).to.be.true;
    })
```

 3. В ответе указаны коэффициенты умножения salary, напишите тесты по проверке правильности результата перемножения на коэффициент.
```js
    var req = JSON.parse(request.data)
    pm.test("Check salary", function () {
        pm.expect(jsonData.qa_salary_after_6_months).to.eql(parseInT(req.salary*2));
        pm.expect(jsonData.qa_salary_after_12_months).to.eq(parseInt(req.salary*2.9));
        pm.expect(jsonData.person['u_salary_1_5_year']).to.eql(parseInt(req.salary*4))
    });
```
 4. Достать значение из поля 'u_salary_1.5_year' и передать в поле salary запроса http://162.55.220.72:5005/get_test_user:
```js
    var salary_1 = jsonData.person.u_salary_1_5_year
    pm.environment.set('salary',salary_1);
```

## **3. `http://162.55.220.72:5005/new_data`**
___

+ Request:
```js
POST
age: 29

salary: 1000

name: "Kristina"

auth_token:{{auth_token}}
```
+ Responce:
```js
{
    "name":"name",
    "age": "int(age)",
    "salary": [
                "salary", 
                "str(salary*2)", 
                "str(salary*3)"
            ]
}
```
## Tests:   

1. Статус код 200
```js
pm.test("check Status code is 200", function () {
    pm.response.to.have.status(200);
});
```
2.  Проверка структуры json в ответе.
```js
var jsonData = pm.response.json();
var schema = {
    "properties": {
    "age": {
      "type": "integer"
    },
    "name": {"type": "string"},
    "salary": {"type": "array"},
      "items": [
        {"type": "integer"},
        {"type": "string"},
        {"type": "string"}
      ]
    }
  }

pm.test('Shema is valid',function(){
    pm.expect(tv4.validate(jsonData,schema)).to.be.true;
});
```
3. В ответе указаны коэффициенты умножения salary, напишите тесты по проверке правильности результата перемножения на коэффициент:
```js
    var req = (request.data)
    pm.test("check salary", function () {
        pm.expect(jsonData.salary[0]).to.eq(parseInt(req.salary));
    });
    pm.test("check salary*2", function () {
        pm.expect(1*(jsonData.salary[1])).to.eq (req.salary*2);
    });
    pm.test("check salary*3", function () {
        pm.expect(1*(jsonData.salary[2])).to.eq (req.salary*3);
    });
```
4. Проверить, что 2-й элемент массива salary больше 1-го и 0-го
```js
pm.test("check salary [2]>salary[0]", function () {
    pm.expect(1*(jsonData.salary[2])).to.above(parseInt(jsonData.salary[0]));
});
pm.test("check salary [2]>salary[1]", function () {
pm.expect(1*(jsonData.salary[2])).to.above(parseInt(jsonData.salary[1]));
});
```

## **4. `http://162.55.220.72:5005/test_pet_info`**
___
+ Request:
```js
    POST
    age: 29
    weight: 55
    name: "Kris"
    auth_token:{{auth_token}}

```
+ Responce:
```json
{
    'name': "name",
    'age': age,
    'daily_food':weight * 0.012,
    'daily_sleep': weight * 2.5
}
```
## Tests:

1) Статус код 200
```js
pm.test(" check Status code is 200", function () {
    pm.response.to.have.status(200);
});
```
2) Проверка структуры json в ответе.
```js
var jsonData = pm.response.json();
var schema = {
           "type": "object",
            "additionalProperties": false,
            "properties": {
                "age": {
                    "type": "integer"
                },
                "daily_food": {
                    "type": "number"
                },
                "daily_sleep": {
                    "type": "number"
                },
                "name": {
                    "type": "string"
                }
            },
            "required": [
                "age",
                "daily_food",
                "daily_sleep",
                "name"
            ],
        }
    }
}
        pm.test('Shema is valid',function(){
            pm.expect(tv4.validate(jsonData,schema)).to.be.true;
        });
```
3) В ответе указаны коэффициенты умножения weight, напишите тесты по проверке правильности результата перемножения на коэффициент.
```js
var req = (request.data);
pm.test("check weight", function () {
    pm.expect(jsonData.daily_food).to.eql(req.weight * 0.012);
});
pm.test("check weight", function () {
    pm.expect(jsonData.daily_sleep).to.eql(req.weight *2.5);
});
```

## **5. `http://162.55.220.72:5005/get_test_user`**
____
+ Request:
```js
POST

    age: 25
    salary: {{salary}}
    name: {{name}}
    auth_token: {{auth_token}}

```
+ Responce
```js

    {
    "age": "25",
    "family": {
        "children": [
            [
                "Alex",
                24
            ],
            [
                "Kate",
                12
            ]
        ],
        "u_salary_1_5_year": 16000
    },
    "name": "Kris",
    "salary": 4000
}
```
Tests:

1) Статус код 200
```js
pm.test("check Status code is 200", function () {
    pm.response.to.have.status(200);
```

2) Проверка структуры json в ответе.
```js
var jsonData = pm.response.json();
var schema = {

  "type": "object",
  "properties": {
    "age": { "type": "string" },
    "family": {
      "type": "object",
      "properties": {
        "children": { "type": "array", "items": { "type": "array" } },
        "u_salary_1_5_year": { "type": "integer" }
      }
    },
    "name": { "type": "string" },
    "salary": { "type": "integer" }
  }
}
pm.test('Shema is valid',function(){
    pm.expect(tv4.validate(jsonData,schema)).to.be.true;
});    

3) Проверить что занчение поля name = значению переменной name из окружения
```js
pm.environment.get('name')

pm.test("check name equal variable name", function () {
    pm.expect(jsonData.name).to.eql(pm.environment.get('name'));
 });
```
4) Проверить что занчение поля age в ответе соответсвует отправленному в запросе значению поля age
```js
var req = (request.data)

pm.test("Check age", function () {
    pm.expect(jsonData.age).to.eql(req.age);
});
```

## **6. `http://162.55.220.72:5005/currency`**
____
+ Request:
```js
    POST
auth_token:{{auth_token}}
```
+ Response: Передаётся список массив объектов.
```js
[
    {"Cur_Abbreviation": "str",
    "Cur_ID": "int",
    "Cur_Name": "str"
    }
   
    {"Cur_Abbreviation": "str",
    "Cur_ID": "int",
    "Cur_Name": "str"
    }
]
```
## Tests:
1) Можете взять любой объект из присланного списка, используйте js random:
```js
let resp = pm.response.json();    

let curr_code = resp[Math.floor(Math.random()*resp.length)];
```
В объекте возьмите Cur_ID и передать через окружение в следующий запрос:
```js
pm.environment.set("cur_code", curr_code.Cur_ID);
```

## **7. `http://162.55.220.72:5005/curr_byn`**
___

+ Request:
```js
POST
auth_token: {{auth_token}}
curr_code: 492
```

+ Responce:
```json
{
    "Cur_Abbreviation": str
    "Cur_ID": int,
    "Cur_Name": str,
    "Cur_OfficialRate": float,
    "Cur_Scale": int,
    "Date": str
}
```

## Test:
1) Статус код 200
```js
pm.test("check Status code is 200", function () {
    pm.response.to.have.status(200);
```
2) Проверка структуры json в ответе:
```js
var jsonData = pm.response.json();
{
  "type": "object",
  "properties": {
    "Cur_Abbreviation": { "type": "string" },
    "Cur_ID": { "type": "integer" },
    "Cur_Name": { "type": "string" },
    "Cur_OfficialRate": { "type": "number" },
    "Cur_Scale": { "type": "integer" },
    "Date": { "type": "string" }
  }
}
pm.test('Shema is valid',function(){
    pm.expect(tv4.validate(jsonData,schema)).to.be.true;
});

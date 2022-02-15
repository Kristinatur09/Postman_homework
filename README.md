# Postman_homework
___
This repository stores homeworks on the course of Vadim Ksendzov.
## 1. **The fisrt Homework**:
### Send requests to the server using Get and Post methods.
___
## 2. **The second Homework**: 
### Writing tests.
 *Tests in Postman make sure the API works as expected. The folowing Snippets was used in my homeworks*:

+ Testing status codes.

    *If the response code is 200, the test will pass, otherwise it will fail*.
```js
    pm.test("Status code is 200",           function () {
        pm.response.to.have.status(200);
    });
```
+ Test if the response body contains a string:
```pm.test("Body matches string", function () {
    pm.expect(pm.response.text()).to.include("string_you_want_to_search");
});
```
+  Parsing response body data.

    *To parse JSON data, use the following syntax*:
```js 
responseJson = pm.response.json();
```
+ Parsing request 
```js
var req = request.data
```
+ Parsing request from method Get
```js
var req = pm.request. url.query.toObject();
```
+ Testing response body
```js
pm.test("Your test name", function () {
    pm.expect(jsonData.value).to.eql(100);
});
```
+ Set the an environment variable
```js
pm.environment.set("variable_key", "variable_value");
```
___
## 3. **The third Homework**:
### We continue to write tests in Postman. Learning Json Schema.
*JSON Schema is a vocabulary that allows you to annotate and validate JSON documents*.
*I used the following tool to convert Json to Json Schema*:
https://www.convertsimple.com/convert-json-to-json-schema/

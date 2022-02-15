# Postman
___
This repository contains homeworks that were made during the author's course by Vadim Ksendzov.

## 1. **The fisrt Homework**:
### Send requests to the server using Get and Post methods.
___
## 2. **The second Homework**: 
### Writing tests.
 *Tests in Postman make sure the API works as expected. The folowing Snippets was used in my homeworks*:

:small_orange_diamond: Testing status codes.

    If the response code is 200, the test will pass, otherwise it will fail.
```js
    pm.test("Status code is 200",           function () {
        pm.response.to.have.status(200);
    });
```
:small_orange_diamond: Test if the response body contains a string:
```pm.test("Body matches string", function () {
    pm.expect(pm.response.text()).to.include("string_you_want_to_search");
});
```
:small_orange_diamond:  Parsing response body data.

    To parse JSON data, use the following syntax:
```js 
responseJson = pm.response.json();
```
:small_orange_diamond: Parsing request 
```js
var req = request.data
```
:small_orange_diamond: Parsing request from method Get
```js
var req = pm.request. url.query.toObject();
```
:small_orange_diamond: Testing response body
```js
pm.test("Your test name", function () {
    pm.expect(jsonData.value).to.eql(100);
});
```
:small_orange_diamond: Set the an environment variable
```js
pm.environment.set("variable_key", "variable_value");
```
___
## 3. **The third Homework**:
### We continue to write tests in Postman. Learning Json Schema.
*JSON Schema is a vocabulary that allows you to annotate and validate JSON documents*.
*I used the following tool to convert Json to Json Schema*:

:link: https://www.convertsimple.com/convert-json-to-json-schema/

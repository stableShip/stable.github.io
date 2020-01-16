---

 title: SpringBoot Unit Test

date: 2020-01-15 10:19:32

tags: [SpringBoot, UnitTest]

---

## For What

When we developing a project and serving for the user with a lot of function. And some of this function is called in multiple place. when a new requirement is come which need to modify the code of the old function. How can you ensure the ensure old function won't be broken by the change you make. human test? too native. Human always make mistake. So we need the machine to check for us.

  

## Layer Test : Mockito

we use mockito to mock all the  dependencies injected (the database, the third party api) to the layer, in the **Controller** layer , we mock the  **Service**  function, in the  **Service**  layer, we mock the  **Dao**  function;

****
Controller:

```Java
`@Test`

`public` `void` `testAddEmployee()`

`{`

`MockHttpServletRequest request =` `new` `MockHttpServletRequest();`

`RequestContextHolder.setRequestAttributes(``new` `ServletRequestAttributes(request));`

`when(employeeDAO.addEmployee(any(Employee.``class``))).thenReturn(``true``);`

`Employee employee =` `new` `Employee(``1``,` `"Lokesh"``,` `"Gupta"``,` `"howtodoinjava@gmail.com"``);`

`ResponseEntity<Object> responseEntity = employeeController.addEmployee(employee);`

`assertThat(responseEntity.getStatusCodeValue()).isEqualTo(``201``);`

`assertThat(responseEntity.getHeaders().getLocation().getPath()).isEqualTo(``"/1"``);`

`}`
```

****
Service:

``` Java
`@Test`

`public` `void` `getEmployeeByIdTest()`

`{`

`when(dao.getEmployeeById(``1``)).thenReturn(``new` `EmployeeVO(``1``,``"Lokesh"``,``"Gupta"``,``"user@email.com"``));`

`EmployeeVO emp = manager.getEmployeeById(``1``);`

`assertEquals(``"Lokesh"``, emp.getFirstName());`

`assertEquals(``"Gupta"``, emp.getLastName());`

`assertEquals(``"user@email.com"``, emp.getEmail());`

`}`
```
****

### Conclusion

**Good: only test the necessary part.**

**Bad: it take a lot of effort for all the layer for same functionality.**

  

### Example:

[https://howtodoinjava.com/spring-boot2/testing/rest-controller-unit-test-example/](https://howtodoinjava.com/spring-boot2/testing/rest-controller-unit-test-example/)

[https://howtodoinjava.com/spring-boot2/testing/spring-boot-mockito-junit-example/](https://howtodoinjava.com/spring-boot2/testing/spring-boot-mockito-junit-example/)

  

## integration test : embedded db(in-memory db) + sql initialization

  

Because the Layer Test is cause a lot of effort， so it is easy to give up. and then the Integration test came out

  

**Integration test can test different modules are bounded correctly and if they work as expected in only one test case, no need to write for different layer , save a lot of effort.**

********Integration test can more similar as the prod by using the embedded db(H2).********

****Integration test can test for certain behaviors by using the `@sql` to initial to test data to db.****

****
```java
`@Sql``({` `"schema.sql"``,` `"data.sql"` `})`

`@Test`

`public` `void` `testAllEmployees()`

`{`

`assertTrue(`

`this``.restTemplate`

`.getForObject(``"[http://localhost:](http://localhost/)"` `+ port +` `"/employees"``, Employees.``class``)`

`.getEmployeeList().size() ==` `3``);`

`}`
```
****
  

### Conclusion

integration test is a better choice

  

### Example:

[https://howtodoinjava.com/spring-boot2/testing/spring-integration-testing/](https://howtodoinjava.com/spring-boot2/testing/spring-integration-testing/)

demo- [https://alm-github.systems.uk.hsbc/45022338/spring_tag](https://alm-github.systems.uk.hsbc/45022338/spring_tag)
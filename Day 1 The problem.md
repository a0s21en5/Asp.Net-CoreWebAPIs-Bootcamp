# Need for Web API

## Problems if not using Web API

- **Duplicate Logic for Each Application**

  - In all the application, we have some business logic. if you will call your database directly from the website and android application and the iOS application, then you need to write your business logic again and again. And this will duplicate your code.

- **Error-Prone Code (Business Logic Is Written in Each Application)**

  - The Error Prone Code because the business logic is written in all the application. so there are chances that you might miss some logic in some application and this will add more errors in yours code.

- **Some Front-End Frameworks Cannot Communicate Directly with Databases**

  - suppose we want to create your website by using angular framework then remember the ANGULAR is a front-end framework and this framework can not interact with database directly. so in this situation where the website will interact with the database directly , you cannot use these type of frameworks.

- **Difficult to Maintain**

  - This type of structure is hard to maintain because we have written the code again and again means if we need to improve something in one application, then we need to do the same thing in all application and this is very hard to maintain, etc..

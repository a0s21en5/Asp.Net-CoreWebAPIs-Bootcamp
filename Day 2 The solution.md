# Need of Web Api

- **front end applications can not talk with the database directly. so we need a mediator i.e. API**
    - that is web API

- **Avoid business logic duplication**
    - And if you are Web API web project, then you do not need to write your business logic again and again for each application Web API project is going to be a centralized project and all applications will interact only with this

- **Extend application functionality**
    - suppose in the future, along with the website and android application you want to create one more type of application. then again, you do not need to write any kind of extra logic for that only webpage that you have already created you can use them directly into your new project.

- **Abstraction**
    - abstraction because we have written all the business logic in a new project that is the web API project and a separate code. so we have added an extra layer of abstraction because this logic is not visible to the front end user or the front end developers. it means we have added a new layer of abstraction over here.
    
- **Security**
    - none of the application cannot interact with the data directly. this will provide you more security.
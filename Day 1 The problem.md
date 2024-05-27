# Need for Web API

## Problems if not using Web API

- **Duplicate Logic for Each Application**

  - In all applications, there exists some business logic. If you call your database directly from the website, Android application, and iOS application, you'll need to rewrite your business logic repeatedly. This leads to code duplication.

- **Error-Prone Code (Business Logic Is Written in Each Application)**

  - The code becomes error-prone because the business logic is duplicated across all applications. Consequently, there's a risk of missing some logic in certain applications, resulting in additional errors in your code.

- **Some Front-End Frameworks Cannot Communicate Directly with Databases**

  - For instance, if you intend to build your website using the Angular framework, remember that Angular is a front-end framework that cannot directly interact with databases. Therefore, frameworks of this nature cannot be used in situations where the website needs to interact directly with the database.

- **Difficult to Maintain**
  - This structural approach is challenging to maintain because code is duplicated across multiple applications. Any improvements or changes made in one application need to be replicated across all applications, making maintenance laborious.
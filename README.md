# Master task

**This task is to be completed if you are already familiar with topics covered in stage 2 and want to skip it.**

## Description
An application is a vague simulation of an online shop. It has three entities:
1. User (CR)
   - id
   - name
   - email
   - password
   - role (GUEST, USER, ADMIN)

2. Order (CRUD)
   - id
   - user_id
   - price
   - update_date

3. order_item (CRUD)
   - id
   - product_name
   - price
   - amount

Your application should use **one** servlet and implement **Command** pattern. Your servlet should support the following functionality:

| Action                | Endpoint                                                  |
|:----------------------|:----------------------------------------------------------|
| Register (POST)       | http://localhost:8080/?command=REGISTER                   |
| Login (POST)          | http://localhost:8080/?command=LOGIN                      |
| Logout (POST)         | http://localhost:8080/?command=LOGOUT                     |
| Place order (POST)    | http://localhost:8080/?command=PLACE_ORDER                |
| Edit order (POST)     | http://localhost:8080/?command=EDIT_ORDER                 |
| Get order by id (GET) | http://localhost:8080/?command=GET_ONE_ORDER&orderId={id} |
| Get all users (GET)   | http://localhost:8080/?command=GET_ALL_USERS              |
| Get all orders (GET)  | http://localhost:8080/?command=GET_ALL_ORDERS             |
| Get my orders (GET)   | http://localhost:8080/?command=GET_MY_ORDERS              |
| Remove order (POST)   | http://localhost:8080/?command=REMOVE_ORDER               |


**Content Type for POST requests: multipart/form-data**

| Param value    | Action                                                                                      | Required parameters                                                                                                                          | Avaliable for |
|:---------------|:--------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------|:--------------|
| LOGIN          | Login with existing credentials                                                             | *email*,<br/> *password*                                                                                                                     | Guest         |
| REGISTER       | Register new user                                                                           | *email*,<br/> *password*,<br/> *name*,<br/> *second-password*                                                                                | Guest         |
| LOGOUT         | Logout                                                                                      | -                                                                                                                                            | User, Admin   |
| PLACE_ORDER    | Create a new order by passing a list of orderItems                                          | *orderItems* - _must contain all the necessary valid fields for "OrderItem" splited by ";"_ (productName;price;amount)                       | User          |
| EDIT_ORDER     | Edit existing order by orderId with value regarding transfering orderItems and userId value | <br/>*orderItems* - _must contains all necessary valid fields for "OrderItem" splited with ";"_ (could be a list)<br/>*userId*<br/>*orderId* | User          |
| REMOVE_ORDER   | Remove existing order by orderId                                                            | *orderId*                                                                                                                                    | User          |
| GET_ONE_ORDER  | Get order by orderId                                                                        | *orderId*                                                                                                                                    | User, Admin   |
| GET_MY_ORDERS  | Get all existing orders of the current user                                                 | *orderId*                                                                                                                                    | User          |
| GET_ALL_ORDERS | Get all existing orders                                                                     | *orderId*                                                                                                                                    | Admin         |
| GET_ALL_USERS  | Get all existing users                                                                      | *orderId*                                                                                                                                    | Admin         |

Your application should have three roles:

| Guest    | User                   | Admin           |
|:---------|:-----------------------|:----------------|
| Login    | Get this user's orders | View all users  |
| Register | Place order            | View all orders |
| Logout   | Edit order             |                 |
|          | Remove order           |                 |

Your application should have the following validation rules:

| Case                                        | Error message                                        |
|:--------------------------------------------|:-----------------------------------------------------|
| Register with used email (email@email.com)  | User with email email@email.com already exists       |
| Email does not match pattern (email.com)    | Email is not valid: email.com                        |
| Register with empty or too long name (name) | Name cannot be empty or longer than 40 symbols: name |
| Password and second-password are not equals | Passwords are not equals                             |
| Login with wrong email and/or password      | No user with email email@email.com and such password |
| User is not authorized to perform action    | Unauthorized for role USER                           |
| Get order by wrong id (-2)                  | No order with id -2                                  |
| Place order with a negative price (-20)     | A negative price value is not allowed: -20.0         |
| Place order with a negative amount (-3)     | A negative amount value is not allowed: -3           |
| Place order with empty product name         | Product name can not be empty                        |

Success response example:
```
{
    "id": 11,
    "user": {
        "id": 1,
        "name": "name",
        "email": "email@email.com",
        "role": "USER"
    },
    "price": 120.0,
    "updateDate": "2022-12-14",
    "orderItems": [
        {
            "id": 11,
            "productName": "Fender",
            "price": 20.0,
            "amount": 3
        },
        {
            "id": 11,
            "productName": "Gibson",
            "price": 30.0,
            "amount": 1
        },
        {
            "id": 11,
            "productName": "Hohner",
            "price": 15.0,
            "amount": 2
        }
    ]
}
```

Error response example:
```
{
   "error": "A negative price value is not allowed: -30"
}
```

## Requirements:

- JDK version is not important
- Application root com.mjc
- JavaEE/Jakarta
- MySQL/PostgreSQL as database server
- JDBC for data access
- Gradle for building project
- Web server is not important

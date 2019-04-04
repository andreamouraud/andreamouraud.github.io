![Logo](https://hardwareassociation.ie/wp-content/uploads/2017/12/Dcu-logo.png)
# EE417: Web Application Development

## Project Report : Andrea Mouraud - 18103154

### Introduction
***
The goal of this project was to develop one of the randomly assigned subject from a list (available [here](http://ee417.eeng.dcu.ie/home/assignment)).

My subject was to create an **Online Take-Away System**, following these rules :
- Restaurant owners can register their restaurants and create an account
- They can add a menu of items for sale from their takeaway with corresponding description and prices
- Anonymous users can search for takeaways and browse their menus and place an order  (payment process not required)
- The application had to be using the DCU Oracle database and be deployed as a WAR on a the DCU Tomcat server 

The rest was up to me.

The project was developed using :
* Java 8
    * JSTL (JavaServer Pages Standard Tag Library)
* IntelliJ IDEA 2018.3.4 (Ultimate Edition)
* No Javascript library 
* HTML5 and CSS3
* Github and MarkDown (report) 

### Structure
***
The system is structured in such a way :
	
| Route                                                              | Description                  | Login Required | Account Creation |
|--------------------------------------------------------------------|------------------------------|:--------------:|:----------------:|
| [/](http://ssd.eeng.dcu.ie:8091/mouraua2/)                         | Anonymous ordering page      | No             | -                |
| [/professional](http://ssd.eeng.dcu.ie:8091/mouraua2/professional) | Restaurant managing page     | Yes            | Yes              |
| [/admin](http://ssd.eeng.dcu.ie:8091/mouraua2/admin)               | Database administration page | Yes            | No               |
| [/report](http://ssd.eeng.dcu.ie:8091/mouraua2/report)             | Report page                  | No             | -                |

### Admin 
***
The [administration system](http://ssd.eeng.dcu.ie:8091/mouraua2/admin) is used to manage the application:
- Reset the database
- Populate the database

In the accessed with a single account, that cannot be removed nor changed:

| Email        | Password  |
|--------------|-----------|
| admin@dcu.ie | adminpass |

### Application
***
The application is separated in two parts

#### Anonymous ordering

The [anonymous ordering page](http://ssd.eeng.dcu.ie:8091/mouraua2/) is where the user will be able to order his food.
The ordering path is quite simple :
* A list of available restaurants that the user can select is presented, if none exists, a message notifies him.
* A list of available menus that the user can select (from the chosen restaurant) is presented, if none exists, a message notifies him.
* An order finalization page is presented to the user where he can set his name and order.
* An order confirmation page is presented.

#### Restaurant managing 

Sample login details (generated from the [administration page](http://ssd.eeng.dcu.ie:8091/mouraua2/admin) populate):

| Email        | Password  |
|--------------|-----------|
| user@dcu.ie  | password  |

The [restaurant managing page](http://ssd.eeng.dcu.ie:8091/mouraua2/professional) is where any restaurateur can register and manage his restaurants for customers to order.
This is what he can do:
* Create and modify restaurants.
* Create and modify menus.
* See the last orders in each of his restaurants.

### Database
***
If you wish to manually build the database tables instead of using the administration system, here are queries :

*The queries must be perform in this exact order because of constraint rules*

#### Users

```sql
CREATE TABLE mouraua2_users (
                id INTEGER GENERATED ALWAYS as IDENTITY(START with 1 INCREMENT by 1),
                email VARCHAR(255) NOT NULL UNIQUE,
                password VARCHAR(255) NOT NULL,
                firstname VARCHAR(255) NOT NULL,
                lastname VARCHAR(255) NOT NULL,
                gender INTEGER,
                salt VARCHAR(255),
                PRIMARY KEY(id)
            )
```

#### Restaurants

```sql
CREATE TABLE mouraua2_restaurants (
                id INTEGER GENERATED ALWAYS as IDENTITY(START with 1 INCREMENT by 1),
                name VARCHAR(255),
                location VARCHAR(255),
                owner INTEGER,
                description VARCHAR(255),
                phoneNumber VARCHAR(255),
                PRIMARY KEY(id),
                CONSTRAINT FK_RESTAURANTS_USERS_owner FOREIGN KEY (owner) REFERENCES MOURAUA2_USERS(ID) ON DELETE SET NULL
            )
```

#### Menus

```sql
CREATE TABLE mouraua2_menus (
                id INTEGER GENERATED ALWAYS as IDENTITY(START with 1 INCREMENT by 1),
                name VARCHAR(255),
                description VARCHAR(255),
                restaurant INTEGER,
                price BINARY_DOUBLE,
                PRIMARY KEY(id),
                CONSTRAINT FK_MENUS_RESTAURANTS_id FOREIGN KEY (restaurant) REFERENCES MOURAUA2_RESTAURANTS(ID) ON DELETE SET NULL
            )
```

#### Orders

```sql
CREATE TABLE mouraua2_orders (
                id INTEGER GENERATED ALWAYS as IDENTITY(START with 1 INCREMENT by 1),
                restaurant INTEGER,
                menu INTEGER,
                customer VARCHAR(255),
                time TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
                PRIMARY KEY(id),
                CONSTRAINT FK_ORDERS_RESTAURANT_id FOREIGN KEY (restaurant) REFERENCES MOURAUA2_RESTAURANTS(ID) ON DELETE SET NULL,
                CONSTRAINT FK_ORDERS_MENU_id FOREIGN KEY (menu) REFERENCES MOURAUA2_MENUS(ID) ON DELETE SET NULL
            )
```

### Special effort and functionality 
***
* Design
* Administration page : Automated database reset and populate
* SHA256 Password hashing with SecureRandom generated salt
* Field validation (Regex)
* MVC Architecture
* Session tracking and logout
* Complete Javadoc documentation
* MarkDown report (Turned to HTML scraping Github, *allowed by teacher*)
* Field modification 

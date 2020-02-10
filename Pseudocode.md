
#### Technologies Used - 

1. Server: Node.js (RESTFUL API on Express JS)
2. ORM layer: Sequelize (promise-based Node.js ORM for Postgres, MySQL, MariaDB, SQLite and Microsoft SQL Server)
3. Client Side: HTML5, CSS3 (Foundation), ES6 Javascript
4. Automation: npm scripts

The design pattern is Model View Controller. 

#### Analysis - 

1. package.json 

Dependencies - 
    "bcryptjs": "^2.4.3",
    "express": "^4.17.1",
    "express-session": "^1.17.0",
    "mysql2": "^1.7.0",
    "passport": "^0.4.1",
    "passport-local": "^1.0.0",
    "sequelize"

2. The database name is "passport_demo". config.json needs credentials relevant to your mysql workbench. 

3. Also there is middleware folder that includes isAuthenticated.js, for restricting routes a user is not allowed to visit if not logged in

Two things are done here - 

a) If the user is logged in, continue with the request to the restricted route

b) If the user isn't logged in, redirect them to the login page

4. passport.js - 

a) Telling passport we want to use a Local Strategy. In other words, we want login with a username/email and password 

b) Our user will sign in using an email, rather than a "username"

c) When a user tries to sign in, it uses where clause

d) Then it sends a promise, which executes two conditions - 

i) Validates email - false condition
ii) Validates password - false condition
iii) If both of above false conditions fails, it returns user database

e) In order to help keep authentication state across HTTP requests,
    Sequelize needs to serialize and deserialize the user
    Just consider this part boilerplate needed to make it all work

    ````
    passport.serializeUser(function(user, cb) {
    cb(null, user);
    });

    passport.deserializeUser(function(obj, cb) {
    cb(null, obj);
    });
    ````

6. In models folder, 

a) The index.js remains untouched, it explains the linking of sequelize with database. 
b) In user.js, 

i) It requires bcrypt for password hashing. 
ii) Creates User models, which includes email and password. 

7. In public folder, 

a) All files will execute static HTML along with styling and js. 

login.js - 

i) Getting references to our form and inputs
ii) When the form is submitted, validate there's an email and password entered
iii) If we have an email and password we run the loginUser function and clear the form
iv) If there's an error, log the error

members.js - 

i) Uses GET request to figure out which user is logged in and updates the HTML on the page

signup.js - 

i) When the signup button is clicked, one validate the email and password are not blank
ii) If we have an email and password, run the signUpUser function
iii) Does a post to the signup route. If successful, we are redirected to the members page otherwise we log any errors
iv) If there's an error, handle it by throwing up a bootstrap alert

stylesheet - Includes style for html

htmlpages - Includes skeleton for login, members and signup

8. In routes folder, 

a) api-routes.js

Using the passport.authenticate middleware with our local strategy.
  1) If the user has valid login credentials, send them to the members page. Otherwise the user will be sent an error
  2)  Route for signing up a user. The user's password is automatically hashed and stored securely thanks to how we configured our Sequelize User Model. If the user is created successfully, proceed to log the user in, otherwise send back an error
  3) Route for logging user out
  4) Route for getting some data about our user to be used client side

b) html-routes.js

    1) Requires path to so we can use relative routes to our HTML files
    2) Requires our custom middleware for checking if a user is logged in
    3) If the user already has an account send them to the members page
    4) If a user who is not logged in tries to access this route they will be redirected to the signup page





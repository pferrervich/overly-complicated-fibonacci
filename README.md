# overly-complicated-fibonacci  
[![Build Status](https://travis-ci.org/pferrervich/overly-complicated-fibonacci.svg?branch=master)](https://travis-ci.org/pferrervich/overly-complicated-fibonacci)
  
A fibonatti calculator at an absolutely over the top complicated fashion, just to get a more experience with a multi container Deployment.
  
![](https://raw.githubusercontent.com/pferrervich/overly-complicated-fibonacci/master/diagram.png)

The user visits the NGINX server. This server can route the user to:
* **The React Server**: to get some front-end assets like the JavaScript and HTML.
* **Express Server**: this is where the back-end API is located and will send and read the indicies.

This Express Server can connect to:
* **The Redis Database**: the indicies will be stored in memory, so it will be temporary data as the user inputs the values.
* **Postgres**: stores the data permanently.

When the user **submits** an indicie on the form, will be **sent to the React Server**. Then, this server will make a **request** to the **Express back-end server**.

The **Express server will store** the value entered by the user to the **PostgresDB**.
**At the same time** the Express server will take the values from Postgres and will **store them to the Redis**.

When the Redis **recieves a new number**, it will **set up** a new server called **Worker**, where his only job is to **look for new indicies** and **calculate the values**. 

So each time a new value comes to the Redis server, this Worker server will take this value and calculate it, and then **put it back to the Redis server**, so it can be requested by the React App and be displayed to the user.
#Mongoose Initial Start

# Step-1

To start a project you have to create folder
~~~
mkdir my-project
~~~

Then go into your folder

~~~
cd my-project
~~~

# Step-2
~~~
npm init -y
~~~

# step-3
install packages

~~~
npm install dotenv express jsonwebtoken cors mongodb cookie-parser mongoose
~~~

# Step-4

Create index.js file and setup this
~~~
const express = require('express');
const app = express();

const PORT = 3000;

<!-- body parser -->
app.use(express.json());


app.get('/', (req, res) => {
  console.log("I am inside home page route handler");
  res.send("Hello Jee, Welcome to CodeHelp");
})

app.listen(PORT, () => {
  console.log("Server is Up")
})
~~~

then check server is running or not

# step-5

create .env file and write your mongoDB uri including username and password too

~~~
those are username and password which are commented. we are going to use this in the uri directly
# DB_USER=assingment-10
# DB_PASS=DD79zLTjAb-3g9A

MONGODB_URI=mongodb+srv://assingment-10:DD79zLTjAb-3g9A@cluster0.vkwnn.mongodb.net/TakeCare?retryWrites=true&w=majority&appName=Cluster0
~~~

# step-6 

create db.js file and call your mongoDB uri from .env then use it in mongoose.connect

~~~
const mongoose = require('mongoose');
const dotenv = require('dotenv');

// load env configuration
dotenv.config();

const connectDB = async () => {
    try {
      const conn = await mongoose.connect(process.env.MONGODB_URI, {
        useNewUrlParser: true,
      });
      console.log(`MongoDB Connected: {conn.connection.host}`);
    } catch (error) {
      console.error(error.message);
      process.exit(1);
    }
  }
  
  module.exports = connectDB;
~~~

# step-7 

import db.js in index.js and store it in a vairable called connectDB

~~~
const express = require('express');
const app = express();
const connectDB = require('./db')


const PORT = 3000;

// body parser
app.use(express.json());

// connect to database
connectDB();


app.get('/', (req, res) => {
  console.log("I am inside home page route handler");
  res.send("Hello Jee, Welcome to CodeHelp");
})

app.listen(PORT, () => {
  console.log("Server is Up")
})
~~~
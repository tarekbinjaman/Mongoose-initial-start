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
by doing this you will connect mongoDB through mongoose

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

# step-8
create model for your creating data
For this time we are going to create a folder called userModel.js

it will look like this
~~~
const {Schema,model} = require("mongoose");

const userSchema = new Schema({
    name: {
      type: String,
      required: true,
      maxlength: 50
    },
    age: {
        type: Number,
        required: true,
    },
    weight: {
        type: Number,
    },
    createdAt: {
      type: Date,
      default: Date.now,
    },
  });

  const userModel = model("user", userSchema)
  module.exports = userModel
~~~


# step-9
Now create a folder called route. In this folder we will create specific file for routing.

📁 route
inside route folder i am creating my first route called users

Remembder we are going to store the userModel in a variable called User

~~~
const express = require('express');
const router = express.Router();

const User = require('../userModel');

// routes


// CRUD Operation


// view/Read

router.get('/users', async (req, res) => {
    try {
        const users = await User.find();
        res.status(200).json(users);
    }
    catch (err) {
        res.status(500).json({
            success: false,
            message: err.message
        })
    }
})

//Create

router.post('/users', async (req, res) => {
    try {
        const { name, age, weight } = req.body;
        const newUser = new User({ name, age, weight });
        await newUser.save();
        res.status(200).json({
            success: true,
            user: newUser
        })
    }
    catch (err) {
        res.status(500).json({
            success: false,
            message: err.message
        })
    }
})

// update

router.put('/users/:id', async (req, res) => {
    const { id } = req.params;
    const { name, age, weight } = req.body;
    try {
        const updatedUser = await User.findByIdAndUpdate(id, {name, age, weight});
        if(!updatedUser) {
            res.json({
                message: "User Not found"
            })
        }
        // but if you have updated the user successfully
        res.status(200).json({
            success: true,
            user: updatedUser
        })
    }
    catch (err) {
        res.status(500).json({
            success: false,
            message: err.message
        })
    }
})

// delete
router.delete('/users/:id', async(req, res) => {
    const {id} = req.params;
    try{
        const deletedUser = await User.findByIdAndDelete(id);
        if(!deletedUser) {
            res.json({
                message: "User not found"
            })
        }
        // if user found and deleted successfully
        res.status(200).json({
            success: true,
            user: deletedUser
        })
    }
    catch (err) {
        res.status(500).json({
            success: false,
            message: err.message
        })
    }
})





module.exports = router;
~~~


# step-10 

Now we going to connect api to our database 

At first, we store our users route in a variable called users

here it is 
~~~
const express = require('express');
const app = express();
const connectDB = require('./db')
const users = require('./routes/users')


const PORT = 3000;

// body parser
app.use(express.json());

// connect to database
connectDB();

// users routes
app.use('/api', users);

app.get('/', (req, res) => {
  console.log("I am inside home page route handler");
  res.send("Hello Jee, Welcome to CodeHelp");
})

app.listen(PORT, () => {
  console.log("Server is Up")
})
~~~

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
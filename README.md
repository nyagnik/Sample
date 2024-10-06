Here's a simple **Node.js** application that you can use as the source for your Docker-related questions.

### **Node.js App Example** (for Dockerfile)

#### **1. Application Structure**
```
my-node-app/
│
├── Dockerfile           # You will write this based on the test
├── package.json         # Node.js dependencies
├── app.js               # Main application file
└── .dockerignore        # Ignore unnecessary files for Docker build
```

#### **2. `app.js` (Main Node.js application)**
This is a basic Express app that responds with a message when accessed at the root URL.

```javascript
// app.js
const express = require('express');
const app = express();

const port = process.env.PORT || 3000;
const environment = process.env.NODE_ENV || 'production';

app.get('/', (req, res) => {
    res.send(`Hello, World! Running in ${environment} mode.`);
});

app.listen(port, () => {
    console.log(`App running on port ${port}`);
});
```

#### **3. `package.json` (Dependencies and metadata)**
This file defines the application’s dependencies, specifically `express`, which is the web framework used.

```json
{
  "name": "my-node-app",
  "version": "1.0.0",
  "description": "A simple Node.js app",
  "main": "app.js",
  "scripts": {
    "start": "node app.js"
  },
  "dependencies": {
    "express": "^4.17.1"
  }
}
```

#### **4. `.dockerignore` (Optional)**
This file ensures that unnecessary files like `node_modules` or local configs are not included in the Docker build context, making the image smaller.

```
node_modules
npm-debug.log
.DS_Store
```

### **Sample Dockerfile**
Here’s an example of a basic Dockerfile for this application:

```dockerfile
# Step 1: Use official Node.js image as the base
FROM node:14

# Step 2: Set the working directory
WORKDIR /usr/src/app

# Step 3: Copy package.json and install dependencies
COPY package.json ./
RUN npm install

# Step 4: Copy the source code
COPY . .

# Step 5: Expose the port the app runs on
EXPOSE 3000

# Step 6: Set environment variables
ENV NODE_ENV=production

# Step 7: Define the command to run the application
CMD ["npm", "start"]
```

### Running the App with Docker

1. **Build the Docker image**:
   ```bash
   docker build -t my-node-app .
   ```

2. **Run the container**:
   ```bash
   docker run -p 3000:3000 my-node-app
   ```

3. Visit `http://localhost:3000`, and you should see the message `Hello, World! Running in production mode.`

---

You can use this application for the Dockerfile exercises in the test you create. It covers basic Docker concepts like installing dependencies, copying files, setting environment variables, and running the application.

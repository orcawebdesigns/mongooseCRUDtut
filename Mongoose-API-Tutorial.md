## 1. We will begin with making sure that we require the mongoose module 

```
const mongoose = require('mongoose');
```

## 2. Creation and usage of schemas



- All CRUD application begin with a schema, so we will create a directory and name it 'models', and this is where we will store our schemas.
- 
![Screen Shot 2021-03-08 at 8.18.27 PM.png](https://www.dropbox.com/s/3caqnm8zhjbobk1/Screen%20Shot%202021-03-08%20at%208.18.27%20PM.png?dl=0&raw=1)

```
const postSchema = new mongoose.Schema({
   
    title: String,
    tag1: String,
    tag2: String,
    tag3: String,
    subject: String,
    content: String,
    username: String,
    userID: {
      type: mongoose.Schema.Types.ObjectId,
      ref: 'User',
    },
    image: String,
    createdAt: {
        type: Date,
        default: new Date()
    }
});

```
 - Above you created your schema and its needs to be declared as a varaiable which is 'postSchema' . Now we call the mongoose.Schema method
 - Now to complete this step we will require the mongoose.Schema at the top of the page
 
```
const mongoose = require('mongoose');
const Schema = mongoose.Schema;
```
 

- When you have made your schema, declare a constant variable to give your collection a name, and then you are ready to 

```
const Post = mongoose.model('Post', postSchema);
 
module.exports = Post;
```



!!3. Now we connect to the database and build routes

- Create index.js file inside the root directoy of the API
- You will need to require the modules needed for this API

```
const bodyParser = require('body-parser');
const express = require('express');
const mongoose = require('mongoose');

const app = new express();
```
- Connect to the database 

```
mongoose.connect('mongodb://localhost:27017/admin', { useNewUrlParser: true })
    .then(() => 'You are now connected to Mongo!')
    .catch(err => console.error('Something went wrong', err))
```
* Now we will require the schemas and routes we will need

```
const storePostController = require('./controllers/storePost')
const Post = require('./database/models/Post');
```

- Utilizing express syntax we create the routes needed.

```
app.get("/", homePageController);
app.post("/posts/store", auth, storePostController);
```
- The last part we will use express to listen to the port.

```
app.listen(4000, () => {
    console.log('App listening on port 4000')
}); 
```

- Now we our index.js file will look like the image below.



![Screen Shot 2021-03-10 at 12.43.33 PM.png](https://www.dropbox.com/s/mhxhfa3y8hey5t6/Screen%20Shot%202021-03-10%20at%2012.43.33%20PM.png?dl=0&raw=1)

- Create a file called storePost.js, and we begin by making 2 requirements

```
const path = require('path')
const Post = require('../database/models/Post')
```
- Next we create our module to export the post and image

```
module.exports = (req, res) => {

req.body.userID = req.session.userId
  
if(req.files){
    const { image } = req.files
 
    image.mv(path.resolve(__dirname, '..','public/posts', image.name), (error) => {
      //HEre
        Post.create({
            ...req.body,
            image: `/posts/${image.name}`
        }, (error, post) => {
            res.redirect('/myposts');
        });
    })
  } else if (!req.files) {
    

    
    Post.create(req.body, (error, post) => {
        res.redirect('/myposts')
    })
    
  }
};

```
- The completion of the file will look like this.

![Screen Shot 2021-03-08 at 8.18.13 PM.png](https://www.dropbox.com/s/fvey777cifmcnpp/Screen%20Shot%202021-03-08%20at%208.18.13%20PM.png?dl=0&raw=1)

## POST REQUEST


- When you make a POST request, data is then sent to the database

![Screen Shot 2021-03-10 at 10.38.42 PM.png](https://www.dropbox.com/s/hm16s9sjylwz0xk/Screen%20Shot%202021-03-10%20at%2010.38.42%20PM.png?dl=0&raw=1)


##  GET REQUEST 

- When we make a GET request we call from the database.

![Screen Shot 2021-03-10 at 10.39.13 PM.png](https://www.dropbox.com/s/fcb0w8nj9zni0l3/Screen%20Shot%202021-03-10%20at%2010.39.13%20PM.png?dl=0&raw=1)

## DELETE REQUEST

![Screen Shot 2021-03-10 at 7.30.41 PM.png](https://www.dropbox.com/s/zlw0iue3agadyqo/Screen%20Shot%202021-03-10%20at%207.30.41%20PM.png?dl=0&raw=1)

















 













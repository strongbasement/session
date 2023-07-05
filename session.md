### step 1: 
```npm i express-session```


### step 2: declare variable

``` js
const session=require('express-session')
```

### step 3:intialize session
``` js
app.use(session({
secret:"key that will sign cookie",
resave:false,
saveUnintialized:false,
store:store,
 cookie:{expires:365 * 24 * 60 * 60 * 1000} 
}));

```

### step 4: test the session inside get route

``` js
app.get('/',(req,res)=>{
    
    console.log(req.session);
    console.log(req.session.id)
})


```
### step 4 install mongoose and install connect-mongodb-session

```
npm i mongoose connect-mongodb-session

```
``` js
const mongoose=require('mongoose');
const MongoDbSession=require('connect-mongodb-session')(session);
const urip="mongodb://127.0.0.1:27017/loneuser"

const store=new MongoDbSession({
    uri:urip,
    collection:"mysessions"
})
```

### step 4 Registration

``` js

app.get('/register',(req,res)=>
{
    if(req.session.isAuth==true)
    {
        res.redirect("/dashboard");
    }
    else
    {
        res.render('register');
    }

})

```

### step 5 login

``` js

app.get("/login",(req,res)=>{
    if(req.session.isAuth==true)
    {
res.redirect("/dashboard");
    }else
    {
        res.render('login');
    }
    
}
)
```

### dashbord

``` js

app.get("/dashboard",isAuth,async(req,res)=>{
    await res.render("dashboard",{username:req.session.username})
})

```


### isAuth


``` js

const isAuth=(req,res,next)=>
{
    if(req.session.isAuth==true)
    {
     next();
    }
    else
    {
     res.redirect("/login");
    }
}

```

### bcrypt


``` js
const bcrypt=require('bcrypt');


```


### bcrypt hash

``` js

await bcrypt.hash(password,12)
    

```

### bcrypt compare


``` js


            bcrypt.compare(password,userj.password).then(function(result) {
            if(result)
            {
                req.session.isAuth=true;
                req.session.username=email;
                res.redirect("/dashboard");
            }else
            {
                res.send("wrong password");
            }
            });

```

### logout

``` js

app.post("/logout",async(req,res)=>{
    let _id=req.session.id;
    await req.session.destroy((err)=>{
        if(err) throw err;
         res.redirect('/login');
    })

})

```

### post methods

#### reegistration

``` js

app.post("/register",async (req,res)=>{
const {email,password}=req.body;
    let userName= await User.findOne({email});
    console.log(userName)
    if(userName)
    {
        res.send("user already exists");
    }
    else
    {
        const newUser=new User({
        email:email,
        password:await bcrypt.hash(password,12)
    })
    await newUser.save();
    
    res.redirect("/login");
}
        
    
})

```

####  login


``` js
   
   app.post('/login',async(req,res)=>
{
    
    let email=req.body.email;
    console.log(req.body.email);
    let password=req.body.password; 
    let userj= await User.findOne({email});
    // console.log(userj.password);
    
        if(userj)
        {
            bcrypt.compare(password,userj.password).then(function(result) {
            if(result)
            {
                req.session.isAuth=true;
                req.session.username=email;
                res.redirect("/dashboard");
            }else
            {
                res.send("wrong password");
          app.post('/login',async(req,res)=>
{
    
    let email=req.body.email;
    console.log(req.body.email);
    let password=req.body.password; 
    let userj= await User.findOne({email});
    // console.log(userj.password);
    
        if(userj)
        {
            bcrypt.compare(password,userj.password).then(function(result) {
            if(result)
            {
                req.session.isAuth=true;
                req.session.username=email;
                res.redirect("/dashboard");
            }else
            {
                res.send("wrong password");
            }
            });
        }
        else{
           await res.send("user not found");
        }
    
    
    
})
  }
            });
        }
        else{
           await res.send("user not found");
        }
    
    
    
})




   ```




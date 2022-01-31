# Login-Form-with-NodeJS
Create Login Form With NodeJS

# Required

 1. Make sure you have NodeJS

 2. You need a code editor to make it easier to code

# Creating file and install

- You can start from creating server.js:

```
  1. First create file named server.js

  2. Run npm init

  3. Run npm install dotenv

  4. Run npm install express

  5. Run npm install flash

  6. Run npm install passport

  7. Run npm install bcrypt

  8. Run npm install method-override
```

# Source Code of server.js

- Go to your code editor and start to code

- Source code of server.js

```javascript
if (process.env.NODE_ENV !== 'production') {
    require('dotenv').config()
}

const express = require('express')
const app = express()
const bcrypt = require('bcrypt')
const passport = require('passport')
const flash = require('express-flash')
const session = require('express-session')
const methodOverride = require('method-override')

const initializePassport = require('./passport-config')
initializePassport(
    passport,
    email => users.find(user => user.email === email),
    id => users.find(user => user.id === id)
)

const users = []

app.set('view-engine', 'ejs')
app.use(express.urlencoded({ extended: false }))
app.use(flash())
app.use(session({
    secret: process.env.SESSIONS_SECRET,
    resave: false,
    saveUninitialized: false
}))
app.use(passport.initialize())
app.use(passport.session())
app.use(methodOverride('_method'))

app.get('/', checkAuthenticated, (req, res) => {
    res.render('index.ejs', { name: req.user.name })
})

app.get('/login', checkNotAuthenticated, (req, res) => {
    res.render('login.ejs')
})

app.post('/login', checkNotAuthenticated, passport.authenticate('local', {
    successRedirect: '/',
    failureRedirect: '/login',
    failureFlash: true
}))

app.get('/register', checkNotAuthenticated, (req, res) => {
    res.render('register.ejs')
})

app.post('/register', checkNotAuthenticated, async (req, res) => {
    try {
        const hashedPassword = await bcrypt.hash(req.body.password, 10)
        users.push({
            id: Date.now().toString(),
            name: req.body.name,
            email: req.body.email,
            password: hashedPassword
        })
        res.redirect('/login')
    } catch {
        res.redirect('/register')
    }
})

app.delete('/logout', (req, res) => {
    req.logOut()
    res.redirect('/register')
    console.warn(`Someone has been logged out ...`)
})

function checkAuthenticated(req, res, next) {
    if (req.isAuthenticated()) {
        return next()
    }

    res.redirect('/login')
}

function checkNotAuthenticated(req, res, next) {
    if (req.isAuthenticated()) {
        return res.redirect('/')
    }
    next()
}

console.clear()
console.log('Server started http://localhost:3000/')

app.listen(3000)
```

# Usage
 ```html
 > git clone https://github.com/progamergiuseppe/Login-Form-with-NodeJS.git

 > npm start devStart

 > Run http://localhost:3000/ in your web broswer

 ```

- This is the basic NodeJS Login

- Enjoy and Have Fun!

# Code Editor

- My code editor <a href="https://code.visualstudio.com/">VScode</a>

 - More code on<a href="https://github.com/progamergiuseppe"> Progamming Giuseppe</a>

# Thank You

 - Thank you for using this program!

 - See you on the next program!

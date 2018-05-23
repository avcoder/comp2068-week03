[Week 3 Slides](http://avillaruz.computerstudi.es/comp2068/week03/index.html)

# npm init

[slide 1-9](http://avillaruz.computerstudi.es/comp2068/week03/index.html#slide=9)

1.  run `npm init`

    ## Explain a bit each prompt

    * name: defaults to folder name, if you want to give some other name you can, otherwise just hit enter
    * version: talk about [SEMVER](https://semver.org/)
    * description: First node app with npm
    * entry point: What is the first javascript file that is run
    * test: if you want to setup unit testing, you can put it here
    * git repository: if you have a repo setup, put it here
    * keywords: if you publish, then keywords are the words people might use when searching npmjs.com
    * author: your name
    * license: by default this is designed to be open source packages and there are different open source licenses

    ## What did above do?

    * created package.json
    * What format is package.json?
    * Where else might you find JSON used as config files?
    * easier to automate this than to manually type json on our own

# Installing accounting package

1.  Create currency.js

    ```js
    /* create a variable and assign a numeric value */
    const amount = 98.3456;

    /* display the amount */
    console.log(amount);
    ```

1.  npm i accounting

    * What changed in package.json?
    * What does ^ mean? [see meaning of ^ and ~](https://michaelsoolee.com/npm-package-tilde-caret/)
    * What changed in Local directory tree?

1.  make use of the accounting to display proper dollar amount

    ```js
    const accounting = require('accounting');
    console.log(accounting.formatMoney(amount));
    ```

    note: can use /_ eslint no-console: 0 _/ but [check with docs first](https://eslint.org/docs/rules/no-console)

[slide 11-13](http://avillaruz.computerstudi.es/comp2068/week03/index.html#slide=11)

# git integration with VS Code

    * Ctrl+Shift P and search for 'git initialize repository'
    * Click Git icon and beside "Changes" click "+" to Stage all changes (equivalent to 'git add .')
    * type a message in textbox then click Checkmark to Commit
    * use terminal to type: `git remote add origin https://github.com/whatever`
    * Click ... icon and select 'Push'  (click OK to publish to branch)

[slide 14](http://avillaruz.computerstudi.es/comp2068/week03/index.html#slide=14)

# use of exports.

1.  Create hello.js

    ```js
    const message = 'hello there';

    /* make public function that can be called from other files */
    exports.sayHello = () => console.log(message);
    ```

    * Talk about valid js identifiers are any variable name that begins with a letter, or $, or \_, after which may be a number
    * js is case sensitive, use semicolons, and ignores white space between statements

1)  Create callingHello.js

    * but intentionally leave out ./ for hello.js
    * you don't need .js extension for hello -- take it out
    * talk about core modules that node has (http fs url)
    * It's like we created our own module since we required it

    ```js
    const hello = require('./hello.js');

    /* call the public (i.e. exports) function in hello.js */
    hello.sayHello();
    ```

1)  Create callingHello2.js

    * npm install @avcoder/hello2npm (show npm.im/@avcoder/hello2npm)
    * then require my module and try running sayHello()

    ```js
    const hello = require('@avcoder/hello2npm');

    console.log(hello.sayHello());
    ```

[slide 15](http://avillaruz.computerstudi.es/comp2068/week03/index.html#slide=15)

# Installing connect

1.  Install `npm i connect`
1.  Create server.js

    ```js
    const connect = require('connect');

    // instantiate a new connect object
    const app = connect();

    app.listen(3000);

    console.log('Server running at port 3000');
    ```

    * Run it above and you'll get error in browser "Cannot GET /" - why?

1.  set up a hello function

    ```js
    const connect = require('connect');

    /* instantiate a new connect object */
    const app = connect();

    /* set up a hello function */
    const hello = (req, res, next) => res.end('hello world');

    /* execute hello fn when requests come in */
    app.use('/', hello);
    app.listen(3000);

    console.log('Server running at port 3000');
    ```
    * style of code = anonymous fn within app.use('/', (req, res) => res.end('hi'))
    * Run above and you'll see hello world
    * Change res.end to res.write -- what happens?  How to end it?
    * Try sending <h1>Hi</h1>
    * Problem: even if you goto localhost:3000/about it'll be the same

1.  Make /hello and /goodbye say the appropriate message

    ```js
    const goodbye = (req, res, next) => res.end('Goodbye world');
    ```

    * Run above and both /hello and /goodbye works
    * Yet, if you go to / or /somethingelse it doesn't work still - how to solve that?

1.  Make a generic fallback

    ```js
    const generic = (req, res, next) =>
      res.end('Oops. I havent coded this route yet');

    app.use(generic);
    ```

    * But where exactly will you put the app.use(generic) statement?

[slide 16](http://avillaruz.computerstudi.es/comp2068/week03/index.html#slide=16)    
* 200 status code = ok
* both http and connect use the method 'listen' to begin accepting connections
* what parameters does createServer use? req and res

[slide 17](http://avillaruz.computerstudi.es/comp2068/week03/index.html#slide=17)    
# Install nodemon

1.  Type `npm i -g nodemon`
1.  Test it by typing `nodemon server`
1.  Try changing your file and see what happens (add an emoji )
1.  Try putting a syntax error


[slide 18](http://avillaruz.computerstudi.es/comp2068/week03/index.html#slide=18)    
# Create npm start to nodemon server.js

    * try running npm test
    * try putting "start": "ls" for now

1.  In package.json under scripts type
    ```js
    "scripts": {
        "start": "nodemon server.js"
    }
    ```

# Calculate Tax

Make it so that if user goes to localhost:3000/calculatetax?subtotal=49.99
then it will calculate the tax total and display it.

1.  Create new fn calculateTax. Need URL package

    ```js
    const { URL } = require('url');
    const accounting = require('accounting');

    const calculateTax (req, res, next) {
        /* get the full query string ?amount=1000 */
        const qs = url.parse(req.url, true).query;

        /* get the amount value from querystring */
        const { amount } = qs;
        
        // calculate the tax amount on this subtotal

        // calculate the total

        // display all 3 amounts
    }

    app.use('/calculatetax', calculateTax);
    ```

# Lab 3 - Steps to initialize project with Connect

1.  Go into new folder Lab3
1.  Run `npm init`
    * For the author field type your name
1.  Verify package.json was created
1.  Run `npm i connect`
1.  In package.json, under "scripts", add "start": "nodemon index" (but don't forget prior comma)
1.  Create server.js
1.  But now you have to edit package.json so its "main" file is `server.js` and its start script is `nodemon server`
1.  set up your app with:

    ```js
    const connect = require('connect');
    const url = require('url');

    const app = connect();

    app.listen(3000);
    console.log('Connect server running on port 3000');
    ```

1.  Create function according to lab instructions such as:

    ```js
    const whatever = (req, res) => {
      const queryString = url.parse(req.url, true).query;
      const { amount } = queryString;

      res.writeHead(200, { 'Content-Type': 'text/html' });
      res.end('<h1>hello</h1>');
    };
    ```

1.  use it `app.use('/', whatever);`

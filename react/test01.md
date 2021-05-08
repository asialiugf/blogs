``` js
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />

    <title>Hello React!</title>

    <script src="https://unpkg.com/react@^16/umd/react.production.min.js"></script>
    <script src="https://unpkg.com/react-dom@16.13.0/umd/react-dom.production.min.js"></script>
    <script src="https://unpkg.com/babel-standalone@6.26.0/babel.js"></script>
  </head>

  <body>
    <div id="root"></div>
    <div id="root1"></div>
    <div id="root2"></div>

    <script type="text/babel">
      // React code will go here
      let eee =[ <h1>Hello world!</h1>,<h1>Hello world!</h1>] ;

      class App extends React.Component {
        render() {
          return ( /* <!-- 这里只能有一个 <div></div> --> */
              <div> 
                <h1> haha </h1> 
                <h2> test </h2>
                <div>
                    <h1> haahaaa </h1>
                </div>
              </div>
          )
        }
      }

      class App1 extends React.Component {
        render() {
          return eee;
        }
      }

      class App2 extends React.Component {
        render() {
          return eee;
        }
      }

      ReactDOM.render(<App />, document.getElementById('root'))
      ReactDOM.render(<App1 />, document.getElementById('root1'))
      ReactDOM.render(<App2 />, document.getElementById('root2'))

    </script>
  </body>
</html>
```
在 render() 里，retrun() 里面，不能并列包括多个 <div></div> ， 只能包括一个。比如： <div>  <div> aaa </div> <a>bbbb</a>  </div>

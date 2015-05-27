![General Assembly Logo](http://i.imgur.com/ke8USTq.png)

## Objectives
* Create a frontend app that will use an API to list, read, create, update and delete movies. 
* This frontend app will use Ajax for to create, read, update, delete and list movies.
* This frontend app will process the JSON responses from the backend API.
* Draw Diagrams that show the flow of a HTTP Requests and Responses and the how the Model, View and Controller interact.


## Create a frontend app

**Create a directory for a front-end application.**

```
mkdir movies_crud_frontend
cd movies_crud_frontend
subl .
```


### List all the Movies

**In the index.html add**

```html
<html>
<head>
  <title>Movies</title>
</head>
<body>
  <h1>Movies</h1>
  <ul id='movies'>
  </ul>
  <button id='movies_button'>Get Movies</button>
  
  <script type="text/javascript" src="http://code.jquery.com/jquery-2.1.4.min.js"></script>
  <script type="text/javascript" src='movies.js'></script>
</body>
</html>
```

**In the movies.js add**

```javascript
$(document).ready(function(){
  var $moviesList = $('#movies');

  // Get the movies from the movies.json file served from
  // the web server listening on port 3000.
  var movies_url = 'http://localhost:3000/movies';

  $('#movies_button').on('click', function(event){
    $.ajax({
      url: movies_url,
      dataType: 'json'
    })
      .done(function(movies){
        // clear the list of movies
        $moviesList.html('');

        movies.forEach(function(movie){
          $moviesList.append('<li id="' + movie.id + '" >'+ movie.name + '</li>');
        });
      })
      .fail(function(data){
        var errorMsg = 'Error: Accessing the URL' + movies_url;
        alert(errorMsg);
        console.log(errorMsg);
      });
  })
});
```

**Run a SIMPLE web server**

This will run a very simple web server on port 5000 of your local machine. 

**First lets create an alias for the simple web server**

Put this at the beginning of your .bashrc file, 

```
alias rubys='ruby -run -e httpd . -p5000' 
```

**Run the front-end server on port 5000**

```
rubys 
```
This just creates an alias from rubys to that really hard to remember command to start up the ruby server.


##Let's Use the Rails API we just created

That provides us with a Movies API.

**Open a terminal in the movies backend directory**

**And run the backend on port 5000**

```
rails server
```

### Show a specific Movie

**Add to the movies.js**

```
 // div to show info about a specific movie
  var $current_movie = $('#current_movie');

  // Add a click handler to show one movie
  $moviesList.on('click', function(event){
    // get the list element that was selected
    var movie_id = event.target.id;
    // form the URL to get info for a specific movie
    var movie_url = 'http://localhost:3000/movies/' + movie_id;

    // Send and Ajax GET request
    $.ajax({
      url: movie_url,
      dataType: 'json'
    })
    .done(function(movie){
      var movie_html = '<dl><dt>name</dt><dd>' + movie.name + '</dd>';
      movie_html += '<dt>rating</dt><dd>' + movie.rating + '</dd>';
      movie_html += '<dt>description</dt><dd>' + movie.desc + '</dd>';
      movie_html += '<dt>length</dt><dd>' + movie.length + '</dd></dl>';

      $current_movie.html(movie_html);
    })
    .fail(function(data){
      var errorMsg = 'Error: Accessing the URL' + movie_url;
      alert(errorMsg);
      console.log(errorMsg);
    }); // end of $.ajax

  }); // end of click handler
```


### Create a Movie

### Update a Movie


### Delete a Movie



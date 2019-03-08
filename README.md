# login-register-using-MEAN
Build Login/Regsiter page using MEAN app with proper Authentication.

//Create the following registration template in views/index.ejs
<script type="text/ng-template" id="/register.html">
  <div class="page-header">
    <h1>Flapper News</h1>
  </div>

  <div ng-show="error" class="alert alert-danger row">
    <span>{{ error.message }}</span>
  </div>

  <form ng-submit="register()"
    style="margin-top:30px;">
    <h3>Register</h3>

    <div class="form-group">
      <input type="text"
      class="form-control"
      placeholder="Username"
      ng-model="user.username"></input>
    </div>
    <div class="form-group">
      <input type="password"
      class="form-control"
      placeholder="Password"
      ng-model="user.password"></input>
    </div>
    <button type="submit" class="btn btn-primary">Register</button>
  </form>
</script>

//Create the following register template in views/index.ejs:

<script type="text/ng-template" id="/register.html">
  <div class="page-header">
    <h1>Flapper News</h1>
  </div>

  <div ng-show="error" class="alert alert-danger row">
    <span>{{ error.message }}</span>
  </div>

  <form ng-submit="register()"
    style="margin-top:30px;">
    <h3>Register</h3>

    <div class="form-group">
      <input type="text"
      class="form-control"
      placeholder="Username"
      ng-model="user.username"></input>
    </div>
    <div class="form-group">
      <input type="password"
      class="form-control"
      placeholder="Password"
      ng-model="user.password"></input>
    </div>
    <button type="submit" class="btn btn-primary">Register</button>
  </form>
</script>

//Create the following login template in views/index.ejs:
<script type="text/ng-template" id="/login.html">
  <div class="page-header">
    <h1>Flapper News</h1>
  </div>

  <div ng-show="error" class="alert alert-danger row">
    <span>{{ error.message }}</span>
  </div>

  <form ng-submit="logIn()"
    style="margin-top:30px;">
    <h3>Log In</h3>

    <div class="form-group">
      <input type="text"
      class="form-control"
      placeholder="Username"
      ng-model="user.username"></input>
    </div>
    <div class="form-group">
      <input type="password"
      class="form-control"
      placeholder="Password"
      ng-model="user.password"></input>
    </div>
    <button type="submit" class="btn btn-primary">Log In</button>
  </form>
</script>

//Finally, let's add two new states that make our login and register pages accessible:

// Create two new states for the login and register page:

.state('login', {
  url: '/login',
  templateUrl: '/login.html',
  controller: 'AuthCtrl',
  onEnter: ['$state', 'auth', function($state, auth){
    if(auth.isLoggedIn()){
      $state.go('home');
    }
  }]
})
.state('register', {
  url: '/register',
  templateUrl: '/register.html',
  controller: 'AuthCtrl',
  onEnter: ['$state', 'auth', function($state, auth){
    if(auth.isLoggedIn()){
      $state.go('home');
    }
  }]
});

//Adding Navigation
// Let's add a navbar to our application so we can easily tell if the user is logged in or not.
// Start by making a simple controller for our navbar that exposes the isLoggedIn, currentUser, and logOut methods from our auth factory.

.controller('NavCtrl', [
'$scope',
'auth',
function($scope, auth){
  $scope.isLoggedIn = auth.isLoggedIn;
  $scope.currentUser = auth.currentUser;
  $scope.logOut = auth.logOut;
}]);
//Now we can add the markup for our navbar to our application:
<body ng-app="flapperNews">
  <nav class="navbar navbar-default pull-right" ng-controller="NavCtrl">
    <ul class="nav navbar-nav">
      <li ng-show="isLoggedIn()"><a>{{ currentUser() }}</a></li>
      <li ng-show="isLoggedIn()"><a href="" ng-click="logOut()">Log Out</a></li>
      <li ng-hide="isLoggedIn()"><a href="/#/login">Log In</a></li>
      <li ng-hide="isLoggedIn()"><a href="/#/register">Register</a></li>
    </ul>
  </nav>
  
  //First, we'll need to inject the auth service to our posts service
.factory('posts', ['$http', 'auth', function($http, auth){

//We'll need to pass the following object as the last argument for our $http calls for the create, upvote, addComment, and upvoteComment methods in our posts service:
o.create = function(post) {
  return $http.post('/posts', post, {
    headers: {Authorization: 'Bearer '+auth.getToken()}
  }).success(function(data){
    o.posts.push(data);
  });
};

o.upvote = function(post) {
  return $http.put('/posts/' + post._id + '/upvote', null, {
    headers: {Authorization: 'Bearer '+auth.getToken()}
  }).success(function(data){
    post.upvotes += 1;
  });
};

o.addComment = function(id, comment) {
  return $http.post('/posts/' + id + '/comments', comment, {
    headers: {Authorization: 'Bearer '+auth.getToken()}
  });
};

o.upvoteComment = function(post, comment) {
  return $http.put('/posts/' + post._id + '/comments/'+ comment._id + '/upvote', null, {
    headers: {Authorization: 'Bearer '+auth.getToken()}
  }).success(function(data){
    comment.upvotes += 1;
  });
};
// We can now display the post authors in the /home.html template:
<span ng-show="post.author">
  posted by <a>{{post.author}}</a> |
</span>

We now have an application with authentication!


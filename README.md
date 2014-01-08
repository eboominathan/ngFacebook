Angular Facebook
================
Angular service to handle facebook

Installation
------------
1. Download the package, you can download the [zip file](https://github.com/GoDisco/ngFacebook/archive/master.zip), or use bower: `bower install ng-facebook`
1. Modify your application to include `ngFacebook` in your application dependencies
1. Configure the ngFacebook module using the configuration steps outlined in the section titled "Configuration" below.
1. Load the [*Facebook SDK for javascript*](https://developers.facebook.com/docs/reference/javascript/), **BUT DO NOT** call `FB.init` or set `window.fbAsyncInit`. These steps are automatically done by the ngFacebook module.

Example:

```
angular.module('your-app', ['ngFacebook'])

.config( function( $facebookProvider ) {
  $facebookProvider.setAppId('<your-app-id>');
})

.run( function( $rootScope ) {
  // Load the SDK asynchronously
  (function(){
     // If we've already installed the SDK, we're done
     if (document.getElementById('facebook-jssdk')) {return;}

     // Get the first script element, which we'll use to find the parent node
     var firstScriptElement = document.getElementsByTagName('script')[0];

     // Create a new script element and set its id
     var facebookJS = document.createElement('script'); 
     facebookJS.id = 'facebook-jssdk';

     // Set the new script's source to the source of the Facebook JS SDK
     facebookJS.src = '//connect.facebook.net/en_US/all.js';

     // Insert the Facebook JS SDK into the DOM
     firstScriptElement.parentNode.insertBefore(facebookJS, firstScriptElement);
   }());
})

;

var DemoCtrl = function ($scope, $facebook) {
  ...
  function refresh() {
    $facebook.api("/me").then( 
      function(response) {
        $scope.welcomeMsg = "Welcome " + response.name;
      },
      function(err) {
        $scope.welcomeMsg = "Please log in";
      });
  }
};

```

For more details check out this [plunker which uses ngFacebook](http://plnkr.co/edit/HcYBFKbqFcgQGhyCGQMw?p=preview).

Configuration
-----
You *must* configure your `facebook application ID` in your app, for example:

    app.config(function(FacebookProvider) {
      $facebookProvider.setAppId(11111111111);
    });

### Additional configurations
You can also configure the next properties.

Use `set` and `get`. For example `$facebookProvider.setAppId(11111)`


1. `permissions(<string>)` for permissions which required by your app.

    Example:

        $facebookProvider.setPermissions("email,user_likes");

1. `customInit(<object>)` custom initial p.

    Example to set:

        $facebookProvider.setCustomInit({
          channelUrl : '//WWW.YOUR_DOMAIN.COM/channel.html'
        });


Using
-----
### Methods
1. `$facebook.config(property)`   - Return the config property.
1. `$facebook.getAuthResponse()`  - Return the `AuthResponse`(assuming you already connected)
1. `$facebook.getLoginStatus()`   - Return *promise* of the result.
1. `$facebook.login()`   - Logged in to your app by facebook. Return *promise* of the result.
1. `$facebook.logout()`   - Logged out from facebook. Return *promise* of the result.
1. `$facebook.ui(params)`   - Do UI action(see facebook sdk docs). Return *promise* of the result.
1. `$facebook.api(args...)`   - Do API action(see facebook sdk docs). Return *promise* of the result.
1. `$facebook.cachedApi(args...)`   - Do API action(see above), but the result will cached. Return *promise* of the result.

    Example:

        app.controller('indexCtrl', function($scope, $facebook) {
          $scope.user=$facebook.cachedApi('/me');
        });

### Events
The service will broadcast the facebook sdk events with the prefix `fb.`.

In return you will get the next arguments to your `$on` handler: `event,response,FB` (`FB` is the facebook native js sdk).

1. `fb.auth.login`
1. `fb.auth.logout`
1. `fb.auth.prompt`
1. `fb.auth.sessionChange`
1. `fb.auth.statusChange`
1. `fb.auth.authResponseChange`
1. `fb.xfbml.render`
1. `fb.edge.create`
1. `fb.edge.remove`
1. `fb.comment.create`
1. `fb.comment.remove`
1. `fb.message.send`

*For additional information about the events see the sdk docs.*

License
--------
This project is released over [MIT license](http://opensource.org/licenses/MIT "MIT License")


Authors
-------

1. [Almog Baku](http://www.AlmogBaku.com "AlmogBaku") - by [GoDisco](http://www.godisco.net)
1. [Amir Valiani](https://github.com/avaliani "Avaliani")

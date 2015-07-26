Authentication
==============

Single User Mode
---------------------
The SDK can take care of all UI interaction for authenticating a user. If necessary, a WebView will be presented from your app's activity to collect credentials from the user. 
```java
BoxSession session = new BoxSession(context);
session.authenticate();
```

When you no longer need the session, it is a good practice to logout.
```java
session.logout();
```

Once you are authenticated and wanna reuse the session either on server or on your android app.
```java
@Override
  public void onAuthCreated(BoxAuthentication.BoxAuthenticationInfo boxAuthenticationInfo) {
      // Box API manages the acces token and redresh token and takes care of refreshing them
      // In case you want to access box from server after authenticating on your android app
      // you can store access and refreshToken on a database on your server for future access.
      String accessToken = boxAuthenticationInfo.accessToken();
      String refreshToken = boxAuthenticationInfo.refreshToken();
      // Here you can write code to make a call to your server to store it.
      
      // Once the user is authenticated on your android app you could store the userId so that
      // you wouldn't have to authenticate again to use the box session for future use.
      // You could use android SharedPreferences to store the user id which was authenticated.
      String userId = boxAuthenticationInfo.getUser().getId();
      // You can use next time use it to create Box session using:
      // new BoxSession(context, userId) where ever you wanna have access to the authenticated
      // users box account
  }
```

Multi-Account Mode
------------------------
To support account switching, create a session with the userId explicitly set to null. This will launch an account chooser to select an authenticated account or log in to a different account. 
```java
BoxSession session = new BoxSession(context, null);
session.authenticate();
```

Log all users out.

```java
BoxAuthentication.getInstance().logoutAllUsers(context);
```

Security
------------------------
The SDK stores authentication information in shared preferences by default. It is recommended that you create your own implementation of `BoxAuthentication.AuthStorage` to provide additional security as needed. You can set the authentication storage as follows:
```java
BoxAuthentication.getInstance().setAuthStorage(new CustomAuthStorage());
```

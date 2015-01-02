# OAuth.io Android SDK

This is the official android sdk for [OAuth.io](https://oauth.io). 

[OAuth](http://oauth.net/core/1.0/) is a protocol to interchange user's credentials, third party apps and providers, to provide a secure way for users to login and use Facebook, Fitbit or Twitter (and many other) providers in your app. This SDK handles both 1.0 and 2.0. 

[OAuth.io](https://oauth.io) is a site that makes integrating very easy by hosting all of the providers and managing the various login, error, and response request connections on one server. 

This SDK writes connectivity points for Auth.IO, using Java for Android. Since most of OAuth.io is written for the web- to access from mobile, namely Android, this SDK streamlines a lot of those requests so it's even easier to connect.

## Get OAuth.io App

This SDK requires a registered OAuth.io app. Go to the [site](https://oauth.io), register one, and grab the public key.

### Install Library

1. To install this sdk, download oauth.jar and place in the (Eclipse) project's "libs" directory. (For other IDE's, make sure the *.jar is linked as a library.)
2. Then refresh the libs directory in eclipse, right click on the oauth.jar and add it in your build path (This step is not necessary in some IDEs, such as IntelliJ).

3. In the AndroidManifest.xml, make sure you have this permission:

    &lt;uses-permission android:name="android.permission.INTERNET" /&gt;


### Login User

In your Activity, instantiate the OAuth class:

 ```java
final OAuth oauth = new OAuth(context);
oauth.initialize('Public key');
 ```


To launch a login to a provider (e.g. facebook):

 ```java
oauth.popup('facebook', callback);

// or (with "options," an org.json.JSONObject):

oauth.popup('facebook', options, callback);
 ```

OAuth.io sends the request to the provider (Facebook) and the result can be accessed in the "onFinished" callback. This callback implements OAuth's OAuthCallback class.

 ```java
void onFinished(OAuthData data);
 ```
  
You can access the OAuthData object in the callback. It is of type: OAuthData class and has the following attributes :

 ```java
    public String provider;     // name of the provider
    public String state;        // state send
    public String token;        // token received
    public String secret;       // secret received (only in oauth1)
    public String status;       // status of the request (succes, error, ....)
    public String expires_in;   // if the token expires
    public String error;        // error encountered
 ```

#### Making An API Call

The `OAuthData` class has the headers and tokens to make a valid API call.

To make an api call: pass the url, and an instance of an OAuthRequestObject to your favorite http library. You can add headers to inject stored oauth tokens. The url can be absolute or relative.

The SDK is written so you can make your own networking library choice:  `URLConnection` / `HttpURLConnection`, `HttpRequest`, or any other. Here is an exmaple using `UrlConnection`:

 ```java
data.http("/me", new OAuthRequest() {
    @Override
    public void onSetURL(String _url) {
        // This method is called once the final url is returned.
    }
    
    @Override
    public void onSetHeader(String header, String value) {
        // This method is called for each header to add to the request.
    }
    
    @Override
    public void onReady() {
        // This method is called once url and headers are set.
    }
    
    @Override
    public void onError(String message) {
        // This method is called if an error occured
    }
});
 ```

For OAuth 1 API requests, the request is proxified via https://oauth.io to sign your request without exposing your secret key. For OAuth 2 API requests, the SDK passes the API request's authorizing description beside the tokens.

### Run the included sample

1. Create a new project as described in the [Android documentation](http://developer.android.com/training/basics/firstapp/index.html). For example, in eclipse:

        File -> New -> Other -> Android Project

2. Install OAuth.io Android sdk into the project

3. Replace the generated example *res/layout/activity_main.xml* , _MainActivity.java_ with the files included in the example folder. Don't forget to add uses-permission android:name="android.permission.INTERNET"  to your AndroidManifest.xml. A valid key is provided, but you can do your own app on [OAuth.io](https://oauth.io/).

4. Plug your phone & run it ! (or use a virtual device)

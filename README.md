# OAuth.io Android SDK

This is the official android sdk for [OAuth.io](https://oauth.io) !

The OAuth.io android sdk allows you to use OAuth in android applications, as well as connect any OAuth provider [available on OAuth.io](https://oauth.io/providers): Twitter, Fitbit, etc.



## OAuth.io Requirements and Set-Up

This SDK requires a registered OAuth.io app. Get the public key for this app from the Oauth web site (https://oauth.io/docs).


    
### Installation

1. Create an android project. These instructions are for Eclipse.
2. To install the android SDK, download oauth.jar and place in the project's "libs" directory. (For other IDE's, make sure the *.jar is linked as a library.)
3. Then refresh the libs directory in eclipse, right click on the oauth.jar and add it in your build path (This step is not necessary in some IDEs, such as IntelliJ).

4. In the AndroidManifest.xml, make sure you have this permission:
    
    <uses-permission android:name="android.permission.INTERNET" />


### Usage

In your Activity, you can instantiate a OAuth class:

 ```java
final OAuth oauth = new OAuth(context);
oauth.initialize('Public key');
 ```


To connect your user to a provider (e.g. facebook):

 ```java
oauth.popup('facebook', callback);

// or (with options a org.json.JSONObject):

oauth.popup('facebook', options, callback);
 ```

The callback is a class that implement the `OAuthCallback` method.

The `OAuthCallback` interface implements a single method :

 ```java
void onFinished(OAuthData data);
 ```
  
The OAuthData class has the following attributes :

 ```java
    public String provider;     // name of the provider
    public String state;        // state send
    public String token;        // token received
    public String secret;       // secret received (only in oauth1)
    public String status;       // status of the request (succes, error, ....)
    public String expires_in;   // if the token expires
    public String error;        // error encountered
 ```

#### API Calls

The `OAuthData` class contains the method `http` to fill an http request with the needed infos to authorize an API call.

You can pass the url of the API call (the url can be absolute or relative to the provider's API base url), and an implementation of `OAuthRequest` to set your http request's url and add headers to inject your oauth tokens.

The implementation is left to you because this design choice allow you to make the request with your preferred way, which can be `URLConnection` / `HttpURLConnection`, `HttpRequest`, or any third party library.

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

See the example for a full implementation using `URLConnection`.

For OAuth 1 API requests, the request is proxified via https://oauth.io to sign your request without exposing your secret key. The OAuth 2 API requests are direct since we pass the API request's authorizing description beside the tokens.

### Run the included sample

1. Create a new project as described in the [Android documentation](http://developer.android.com/training/basics/firstapp/index.html). By example in eclipse:

        File -> New -> Other -> Android Project

2. Install OAuth.io Android sdk into the project

3. Replace the generated example *res/layout/activity_main.xml* , _MainActivity.java_ with the files included in the example folder. Don't forget to add uses-permission android:name="android.permission.INTERNET"  to your AndroidManifest.xml. A valid key is provided, but you can do your own app on [OAuth.io](https://oauth.io/).

4. Plug your phone & run it ! (or use a virtual device)

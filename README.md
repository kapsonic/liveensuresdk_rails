# LiveEnsure Python SDK

This is the LiveEnsure® Ruby on rails DEMO SDK for LiveEnsure Authentication (www.liveensure.com)
>From this SDK you will be able to launch a full API stack in Django and demonstrate the 
full capabilities of LiveEnsure® Authentication for web, cloud, apps and mobile.

The SDK is provided to illustrate how to integrate LiveEnsure® with your identity, web or
app platforms. For futher information, visit the SDK docs at http://developer.liveensure.com 

You will need to obtain LiveEnsure® developer API keys to test this API and sign up for a 
paid LiveEnsure service subscription to use the API/SDK in production. 

This SDK functions in desktop-browser and mobile-browser (app to app) modes, depending on how
you access the pages. From a desktop, you will scan with the free LiveEnsure app to authenticate
based on the settings you configure to drive the API. If you acccess the pages from your mobile
device, the SDK will behave in an app-to-app fashion, rolling over to authenticate as in an app
or mobile HTML implementation, without providing or requiring a scan step. 

For questions about this SDK or LiveEnsure® authentication, visit support.liveensure.com 

## Getting Started

* **First,** sign up at http://www.liveensure.com/signup.html for your developer API keys.
* **Second,** download the free LiveEnsure app for iOS or Android http://www.liveensure.com/app.html
* **Third,** download, configure and run this SDK on your local or networked machine as instructed.

### Prerequisites

Below packages need to be installed and configured:
- network accessible server (or virtual instance)
- ruby 1.9 or above rails 4.
- Obtain LiveEnsure® developer API keys by signup (http://www.liveensure.com/signup.html) and then click the link "Send me my credentials by email" after login
- Google MAP api key (optional for location factors)

### Installing

To install `liveensuredemo` app, run the following command
    
    git clone https://github.com/LiveEnsure/Ruby-SDK

### Configuration

* Include following setting in your `config/initializers/constants.rb` file:

```    
LIVE_ENSURE = {
  "API_KEY": "<API key for liveensure>",
  "API_PASSWORD": "<API password>",
  "AGENT_ID": "<Agent ID for liveensure>",
  "API_HOST": "<API host to access liveensure API>",
  "GOOGLE_MAP_KEY": "<Optional Google Map key to access map to run location auth demo>"
}

```

* Start server to access app using below command:

```
rails server
```

### Desktop/Browser Authentication via Mobile Scan

Walk through each factor tab and how to test/engage desktop with mobile scan.

[1] To test to basic device authentication factor, simply click [DEVICE] tab,
enter your email and click login. Scan screen to authenticate. 

On your first login, you will get an emailed OOB token to enter/register in app.

[2] To test knowledge challenge, click the [Knowledge] tab, enter your email and
choose a challenge question and anwner/response. This is teeing up the API, as the
end-user would not see this. Then click and scan, enter response to challenge
in the device, to authenticate. 

[3] To test location, click the [Location] challenge, enter email and click
the map to register your current lat/long. If you want location to pass,
simply login, scan and pass location proximity test. If you want it to fail,
drag the map to an alternate location, login and fail due to proximity difference.

[4] To test behavior, click the [Behavior] tab and choose an orientation, and a 
grid pattern (up to 2 places) for single or multi-touch on the device. 

Click login and touch and hold the desired locations AS YOU SCAN, until it beeps. 
To fail, tap wrong locations on the screen or release before you scan. 

In the demo you can only choose one factor per test, but in the full API SDK
you can add multiple challenges in combination as needed.

### App to App Mobile Authentication via App Only

Walk thorugh each factor and how to test/engage from mobile browser to
the mobile app, from app-to-app (no QR scan).

First, access the demos from a mobile browser on iOS on Android.

For the mobile demos, repeat the steps as above, except you will rollover
from mobile browser to app to authenticate after each "login" press/tap.

For behavior, you must hold the touch locations while and until the clock sweeps,
since there is no scan.

The rest of the demos function as they do in the desktop version.

## Using the SDK with your own stack/app/code

To use the SDK in your own code, You can copy `app/api/liveensure_api.rb` in your own stack. It is a class based implementaton of all the api, which internally calls the liveensure API using `HTTParty`.

This can be used as follows:
- Create LiveEnsure object

```  
  # api_key is the API key for liveensure
  # api_password is the API password for liveensure 
  # agent_id is the Agent ID for liveensure
  # host_to_access_API is the Host location where API's are hosted
  
  # Make sure you have all these keys before you start using the APIs
  
  liveAuthObj = LiveensureAPI.new("<api_key>", "<api_password>", "<agent_id>", "<host_to_access_api>")
```

- Start session

```      
  # email is the userid for which authentication is to be done

  liveAuthObj.init_session("<email>")
```

  It will return JSON object which have the `sessionToken`, that will be used in all subsequent calls.
  

- Add factors
    * Add knowledge challenge

          ```          
            # question is the question to be asked after scanning the code
            # answer is the answer to the question 
            # session Token is the session key that is returned by initSession call.
    
            liveAuthObj.add_prompt_challenge('<question>', '<answer>', '<sessionToken>')
          ```

      It will return json object with status of the API call.

    * Add location challenge

          ```
            # lat is lattitude of location
            # lang is the langitude of the location
            # radius is the radius limit of location authentication
            
            liveAuthObj.add_location_challenge('<lat>', '<lang>', <radius>, '<sessionToken>')
          ```

    * Add behaviour challenge
          
          ```
            # orientation is the orientation of mobile phone used to scan the 
            # code. It can have 4 values range from 0 to 3
            # 0 -> Portrait
            # 1 -> Upside down
            # 2 -> Landscape Left
            # 3 -> Landscape Right
            
            # touches number of touch points up to 6
            liveAuthObj.add_touch_challenge('<orientation>', '<touches>', '<sessionToken>')
          ```
- Get the code

```
liveAuthObj.get_auth_object("<TYPE>", "<sessionToken>")
```

- poll for status

```      
liveAuthObj.poll_status('<sessionToken>')
```

- delete user

```    
  # email: email of the user that is to be deleted
  liveAuthObj.deleteUser('<email>')
```

## Built With

* rails 4
* Bootstrap
* Google Map APIs
* LiveEnsure Authentication APIs


## Versioning

Current Version: **0.01**

## Authors

* **Christian Hessler** - *Author* - [LiveEnsure](https://github.com/LiveEnsure)
* **Narender Poonia** - *Author* - [LiveEnsure](https://github.com/LiveEnsure)


## License

This project is proprietary software (c) 2016 LiveEnsure Inc. 
Visit http://www.liveensure.com for more information.

## Contact

* Web: http://www.liveensure.com 
* Dev: http://developer.liveensure.com
* Support: http://support.liveensure.com 
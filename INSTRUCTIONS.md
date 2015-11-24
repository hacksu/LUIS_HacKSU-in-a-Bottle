# Setup

* Go to [bit.ly/luisKSU](http://bit.ly/luisKSU)
* Your done

# Investigate the API
* Go to
 [https://api.projectoxford.ai/luis/v1/application?id=4519cffc-c721-4add-8e8f-55f13c821438&subscription-key=779bedbe00204ab690fc8f5245527a22&q=](https://api.projectoxford.ai/luis/v1/application?id=4519cffc-c721-4add-8e8f-55f13c821438&subscription-key=779bedbe00204ab690fc8f5245527a22&q=)
 or copy the link from the code
* At the very end of the address type a query like ```give me an api``` and press enter
* You should see something like this:
```json
{
  "query": "give me an api",
  "intents": [
    {
      "intent": "GetAPI",
      "score": 0.9989177
    },
    {
      "intent": "AddAPI",
      "score": 0.0304752346
    },
    {
      "intent": "None",
      "score": 0.0112464009
    }
  ],
  "entities": []
}
```

* This is JSON. We'll be parsing it with JavaScript in a minute, but for now lets take a look at it
* We get an array of "intents" these tell us what the API thinks the user is asking for. The top one is the most likely
and here it is "GetAPI" with a score of 0.998
* right now our model does one other thing. Let's ask it to add an api say ```LUIS```
* Replace everything after the ```=``` sing with something like ```I found this api, LUIS```
* You should see something like 

```json
{
  "query": "I found this api, LUIS",
  "intents": [
    {
      "intent": "AddAPI",
      "score": 0.9104535
    },
    {
      "intent": "None",
      "score": 0.051490292
    },
    {
      "intent": "GetAPI",
      "score": 0.00480208732
    }
  ],
  "entities": [
    {
      "entity": "luis",
      "type": "API"
    }
  ]
}
```

* Notice we now see an item in the list of ```entities``` these are things we've trained LUIS to take note of.
 Other examples might be the time of day or the name of an activity
 
# Looking at the code

 * You'll notice there is already a lot in the index.html file. 
 * We importing jQuery with ```html <script src="https://code.jquery.com/jquery-2.1.4.js"></script>```
 * We also have a form that calls handle_input on when it is submitted
 * The few other things we have are hidden by default

# Fetch the JSON

* Add $.getJson to the handle_input function. The exact location doesn't mater
* We will be getting the JSON from our API so lets use the LUIS_API variable we have
* Something like this:

```JavaScript
$.getJSON(LUIS_API, function(result) {
    console.log(result);
})
```

* When we enter something into the form and press enter we now get some JSON printed in the console, but not the results we want.
* We need to add the query
* We could just use ```LUIS_API + $("#input").val()```, but certain characters can't be put in the URL
* Instead we use the ```encodeURIComponent``` function to encode the string
* Replace ```LUIS_API``` in ```getJSON``` with ```LUIS_API + encodeURIComponent($("#input").val())```
* Now when we run the script and enter something we get results

# React with Intent

* We need to interpret the JSON and vary our reaction depending on what we get
* In particular we need to get the name of the intent LUIS thinks it's detected
* It's exactly as you'd expect. We select the intents array in result by either ```result.intents``` or ```result["intents"]``
* Then we select the first intent in the array by indexing the 0th position. 
* Then we get the name of that intent AKA intent
* So to get the name of the intent we would do:

```JavaScript
var intent = result.intents[0].intent;
```

* Lets add some if statements

```JavaScript
if(intent == "GetAPI") {
    console.log("Getting API");
} else if (intent == "AddAPI") {
    console.log("Add API");
} else {
    console.log("Bad Input");
}
```

# Get That API

* If we'll asking to get an api lets get it.
* First lets generate a random index into the ```apis``` array. 
* We generate a random number between 0 and 1 with ```Math.random()```
* Multiplying it by ```apis.length``` changes the range to 0 to api.length
* We want it to round downn with converting to an int so we run that through ```Math.floor()```
* Let's set a var i to that:

```Javascript
var i = Math.floor(Math.random() * apis.length);
```

* Then lets set the contents of the tag with the html content to the api that refers to

```Javascript
$("#api").html(apis[i]);
```

* Then we need to fade the message in and out like this:

```Javascript
$('#api-message').fadeIn(200).delay(5000).fadeOut(200); // fade the message in and out
```

* Test it out we should get a message telling us a random api fading in and out (don't worry if you get the same one multiple times that is normal)

# Add That API

* Adding an API is much easier
* First we need to get the API that was asked to be added
```Javascript
var api = result.entities[0].entity;
```
* You might notice something though, What happens if there aren't any entices right now we'll get an error.
* Lets add a check to the if statement like this:

```Javascript
if (intent == "AddAPI" && result.entities.length > 0)
```

* We want to add the ```api``` to the array of apis. We do it like this

```javascript
apis.push(api);
```

* This is enough on it's own, but we ahve a message to show for this too so 

```Javascript
$('#added-api').html(api);
$('#added').fadeIn(200).delay(1000).fadeOut(200);
```

* The last thing is removing the text in the form when we submit it. That's easy enough
```Javascript
$("#input").val("");
```
* And that's it



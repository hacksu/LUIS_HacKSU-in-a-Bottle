# Setup

* Go to [bit.ly/luisKSU](http://bit.ly/luisKSU)
* Your done

# Investigate the API
* Go to
 [https://api.projectoxford.ai/luis/v1/application?id=4519cffc-c721-4add-8e8f-55f13c821438&subscription-key=779bedbe00204ab690fc8f5245527a22&q=](https://api.projectoxford.ai/luis/v1/application?id=4519cffc-c721-4add-8e8f-55f13c821438&subscription-key=779bedbe00204ab690fc8f5245527a22&q=)
 or copy the link from the code
* At the very end of the address type a query like ```give me an api``` and press enter
* You should see something like this:

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
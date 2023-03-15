# API's

These perform a task similar to encapsulating things. Mostly these are used by service providers to create a response for the user like weather api, pokemon api etc. API is mostly used between developers (API Deverloper and Client Developer). User of API acts as a client for that API.

While talking about API's we need to understand few basic things ***Endpiont, Paths, Parameters, Authentication***.

1. **Endpoint** is the address or url of the api from where you can request something from it. like `api.kanye.rest` is the url of the kanye west quote api. This url acts as a **endpoint** as it gives a response back.

2. **Paths and parameters** are part of request url which specify more data about what kind of data we need. For example in this url `https://v2.jokeapi.dev/joke/Dark?blacklistFlags=religious` endpoint is `https://v2.jokeapi.dev/joke` but after that the part `/Dark` represents the ***Path*** and anything that will come after **`?`** will act like a parameter. These things are important as we can do different things with api and pass various parameters to get the response back. 

   If we had more than 1 parameter than it will concatenate with the previous parameter with & as delimieter.

   ```json
   {
       "error": false,
       "category": "Dark",
       "type": "single",
       "joke": "I hate double standards. Burn a body at a crematorium, you're \"being a respectful friend.\" Do it at home and you're \"destroying evidence.\"",
       "flags": {
           "nsfw": false,
           "religious": false,
           "political": false,
           "racist": false,
           "sexist": false,
           "explicit": true
       },
       "safe": false,
       "id": 274,
       "lang": "en"
   }
   ```

3. **Authentication** is required by some api's to work. ***ChatGPT and OpenWeather*** are 2 famous examples. We need to specify the api key in url parameters to add our authentication token. In Openweather api it's added using a key `appid` and it's value as token.



## Sending Requests to Another server

In most of the cases while using api's we will be fetching data from some other server from our server, which makes our server client and server at the same time. Think of a situation where we created a website for weather forcast using openweather api. Our website will request data from our server and our server will request the same data from openweather servers.

```javascript
const express = require('express');
const https = require('https');

const app = express();

app.get('/weather', (req, res) => {
    const url = "https://api.something.com/"
    https.get(url, response => {
        console.log(response.statusCode);
        response.on("data", data => {
            // convert response into readable json format
        	const weatherData = JSON.parse(data);
            console.log(data);
            const temp = data.main.temp;
        });
    });
});
```



## Post request with https

There will be instances where we need to add users using some api. For that we need to send a POST request. 

```javascript
const https = require("https");

const url = "www.somewebsite.com";
const options = {
    method: "POST",
    .
    .
    .
}

https.request(url, options, response => {
    response.on("data", data => {
        console.log(data);
    });
});
```



On sending a post request we can also redirect a request to another get request.

```javascript
app.get('/', (req, res) => {
    res.sendFile("Index.html");
})

app.post('/retry', (req, res) => {
    res.redirect('/');
});
```


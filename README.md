# How to connect & work with an API

Throughout this guide we will use our central message API for training:

[https://rob-message-api.herokuapp.com/messages](https://rob-message-api.herokuapp.com/messages)
(on first load this can take up to 15 seconds - because it is running on the Heroku free plan - so just be patient :))

The following sections show how to test an API

1) with a REST Client (Insomnia)
2) from a React frontend (using fetch or axios)


## Testing with Insomnia

Coding a frontend for testing an API is often too time consuming and tedious.

An API test tool (often called "Rest Client") will help us to test if our API is working as expected, before we actually connect to it from a frontend.

Have not Insomnia so far? 
- Download: https://insomnia.rest/download/core/
- Install hint: For Windows & Linux it will be sufficient to just double click the downloaded file and follow the instructions. For detailed install instructions and first steps with Insomnia, you can follow the getting started guide below 
- Getting started Guide: https://support.insomnia.rest/category/9-getting-started

Startup Insomnia

First setup a new folder for the API you wanna test:
- Click the little + Icon on the left sidebar
- Name the folder "Message API"

Done!

Now we are ready to train...

### Creating requests

#### GET request (all messages)

Now setup your first GET request to fetch all existing messages from the API 

- Click the link "click to add first request" below the new folder you created 
- Name the request, e.g. "GET all messages"
- Keep the method "GET" in the dropdown and click button "Create"
- Put the following url into the top input field (right next to the "GET" dropdown): 
  - `http://rob-message-api.herokuapp.com/messages`
- Submit the request by clicking the button "Send"
- In the right sidebar you should receive the result: all messages in a JavaScript array



#### GET request (single message)

Now setup a GET request to fetch a SINGLE message from the API by its ID

- Left sidebar: Click the little down arrow icon next to your request folder
- Select "New request"
- Name the request, e.g. "GET single message"
- Keep the method "GET" in the dropdown and click button "Create"
- Put the following url into the top input field (right next to the "GET" dropdown): 
  - `http://rob-message-api.herokuapp.com/messages/${id}`
  - Please replace {id} with the id of an existing message!
- Submit the request by clicking the button "Send"
- In the right sidebar you should receive the result: the single message as a JavaScript object


#### POST request

Now setup a POST request to create & store a new message at the API.

- Left sidebar: Click the little down arrow icon next to your request folder
- Select "New request"
- Name the request, e.g. "POST message"
- Select the method "POST" from the dropdown
- Select "JSON" from the dropdown which appears (default "No Body")
- Click button "Create"

- Put the following url into the top input field (right next to the "POST" dropdown): 
  - `http://rob-message-api.herokuapp.com/messages`
- Place a new message in the BODY field (below the URL):
  - a new message must be an object that contains the fields "msg" and "user"
  - example: 
  ```
    {
      "msg": "Somehow this will work...",
      "user": "Rob"
    }
  ```
  - watch out! the body must follow the JSON convention
    - meaning: in the javascript object all keys must be surrounded by double quotes ("") - like in the example above
    - something like: `{ user: "Rob" }` will not work!

- Submit the request by clicking the button "Send"
- In the right sidebar you should receive the result: the new message in a JavaScript object

Now test if your new messages was appended to ALL messages:
- click on your "GET All messages" request in the left sidebar
- click "Send" to submit the request
- now in the right sidebar all messages should appear with your new message somewhere at the bottom...


#### PATCH request

In order to patch (=update) an existing item, please use your existing GET request to see ALL messages and grab the ID of a message you wanna patch. 

Now setup a PATCH request to create & store a new message at the API.

- Left sidebar: Click the little down arrow icon next to your request folder
- Select "New request"
- Name the request, e.g. "Update message"
- Select the method "PATCH" from the dropdown
- Select "JSON" from the dropdown which appears (default "No Body")
- Click button "Create"

- Put the following url into the top input field (right next to the "PATCH" dropdown): 
  - `http://rob-message-api.herokuapp.com/messages/{id}`
  - Please replace {id} with the id of an existing message!

- Place an "update object" in the BODY field (below the URL):
  - an update object must be an JS object that contains either the fields "msg", "user" or both (so these fields will get updated with new values!)
  - example: 
  ```
    {
      "msg": "I am updated...",
      "user": "Rob the II."
    }
  ```
  - watch out! the body must follow the JSON convention
    - meaning: in the javascript object all keys must be surrounded by double quotes ("") - like in the example above
    - something like: `{ msg: "I am updated" }` will not work!

- Submit the request by clicking the button "Send"
- In the right sidebar you should receive the result: the update message in a JavaScript object

Now test if you see your message update in the list of all messages:
- click on your "GET All messages" request in the left sidebar
- click "Send" to submit the request
- now in the right sidebar all messages should appear with your updated message...


#### DELETE request

In order to delete an existing item, please use your existing GET request to see ALL messages and grab the ID of a message you wanna patch. 

Now setup a DELETE request to create & store a new message at the API.

- Left sidebar: Click the little down arrow icon next to your request folder
- Select "New request"
- Name the request, e.g. "DELETE message"
- Select the method "DELETE" from the dropdown and click button "Create"

- Put the following url into the top input field (right next to the "DELETE" dropdown): 
  - `http://rob-message-api.herokuapp.com/messages/{id}`
  - Please replace {id} with the id of an existing message!

- Submit the request by clicking the button "Send"
- In the right sidebar you should receive the result: the deleted message in a JavaScript object

Now test if your message was deleted from ALL messages:
- click on your "GET All messages" request in the left sidebar
- click "Send" to submit the request
- now in the right sidebar all messages should appear, but your deleted message should be gone...



## Testing from a frontend


### Using Fetch

Following see samples on how to use your API from a frontend using fetch

#### GET request

```
fetch('http://rob-message-api.herokuapp.com/messages')
.then(res => res.json())
.then(messagesAPI => {
  console.log(messagesAPI) // log the returned data
  // do something with the data here, e.g. store it in state
})
```


#### POST request

Send and store a NEW message in the API

```

const onSubmit = () => {

  let msgNew = {
    user: 'Donald Trump',
    msg: "APIs are fake news!"
  }

  // perform POST request against the API to store message...
  fetch('http://rob-message-api.herokuapp.com/messages', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify( msgNew )
  })
  .then(res => res.json())
  .then(messageCreatedApi => {
    console.log(messageCreatedApi) // log the returned data

    // do something with the data here, 
    // e.g. add it to your list of messages...
  })

}

```

#### PATCH request

Update an existing message in the API

In order to patch (=update) an existing item, please use your GET request
to see all messages and grab the ID of a message you wanna patch. 

```

const onSubmit = () => {

  let msgId = "<someRealId>" // put the ID of the message here...  
  let msgUpdate = {
    user: 'Donald Trump',
    msg: "I changed my mind, API has good content!"
  }

  // perform PATCH request against the API to update a message...

  fetch(`http://rob-message-api.herokuapp.com/messages/${msgId}`, {
    method: 'PATCH',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify( msgNew )
  })
  .then(res => res.json())
  .then(messageUpdatedApi => {
    console.log(messageUpdatedApi) // log the returned data

    // do something with the data here, 
    // e.g. add it to your list of messages...
  })

}

```

#### DELETE request

Delete an existing message in the API

In order to delete an existing message item, please use your GET request
to see all messages and grab the ID of a message you wanna remove. 

```

const onSubmit = () => {

  let msgId = "<someRealId>" // put the ID of the message here...  

  // perform DELETE request against the API to remove a message...

  fetch(`http://rob-message-api.herokuapp.com/messages/${msgId}`, {
    method: 'DELETE
  })
  .then(res => res.json())
  .then(messageDeletedApi => {
    console.log(messageDeletedApi) // log the returned data
  })

}

```


### Using Axios


Following see samples on how to use your API from a React frontend using axios

Axios is a third party package you need to install first:
`npm i axios`

Axios is nice for usage with JSON APIs, because it already does the Content-Type setting & stringify under the hood for us.

Also the error handling with AXIOS is more intuitive compared to fetch. 

Fetch does not interpret 4XX and 5XX error code responses from an API as errors. Fetch only catches errors when we cannot connect to the API at all (so either if we did not spell the API URL correctly or if the API is not reachable)

Axios interprets 4XX and 5XX error responses from an API as errors and handles them in the catch handler. This makes handling error responses from the backend way more robust.

#### GET request

```
axios.default.baseURL = "http://rob-message-api.herokuapp.com"

axios.get('/messages')
.then(response => {
  console.log(response.data) // log the returned data
  // do something with the data here, e.g. store it in state
})
.catch(error => console.log(error))

```


#### POST request

Send and store a NEW message in the API

```

axios.default.baseURL = "http://rob-message-api.herokuapp.com"

const onSubmit = () => {

  let msgNew = {
    user: 'Donald Trump',
    msg: "APIs are fake news!"
  }

  // perform POST request against the API to store message...
  axios.post('/messages', msgNew)
  .then(response => {
    console.log(response.data) // log the returned data (= created message)

    // do something with the data here, 
    // e.g. add it to your list of messages...
  })
  .catch(error => console.log(error))

}

```

#### PATCH request

Update an existing message in the API

In order to patch (=update) an existing item, please use your GET request
to see all messages and grab the ID of a message you wanna patch. 

```

axios.default.baseURL = "http://rob-message-api.herokuapp.com"

const onSubmit = () => {

  let msgId = "<someRealId>" // put the ID of the message here...  
  let msgUpdate = {
    user: 'Donald Trump',
    msg: "I changed my mind, API has good content!"
  }

  // perform PATCH request against the API to update a message...

  axios.patch(`/messages/${msgId}`, msgUpdate)
  .then(response => {
    console.log(response.data) // log the updated message

    // do something with the data here, 
    // e.g. add it to your list of messages...
  })
  .catch(error => console.log(error))


}

```

#### DELETE request

Delete an existing message in the API

In order to delete an existing message item, please use your GET request
to see all messages and grab the ID of a message you wanna remove. 

```

axios.default.baseURL = "http://rob-message-api.herokuapp.com"

const onSubmit = () => {

  let msgId = "<someRealId>" // put the ID of the message here...  

  // perform DELETE request against the API to store message...

  axios.delete(`/messages/${msgId}`)
  .then(response => {
    console.log(response.data) // log the returned data
  })
  .catch(error => console.log(error))

}

```

#### Configure Axios baseURL just once

All axios snippets above set a baseURL to your API (for quick out of the box testing of the snippet). 

However: In a real project you would set this baseURL only ONCE for all your requests. Either in index.js, App.js or you create an axios configuration file.

Following we show, how to setup a configuration file in /helpers/axios.js in your React project.

There we create our global axios instance, configure it and export it:

```
import axios from 'axios'

const axiosInstance = axios.create({
    baseURL: "http://rob-message-api.herokuapp.com"
});

export default axiosInstance

```

Now we can use this pre-configured instance throughout our app.

We just need to import that configured axios wherever we need it, e.g.:
`import axios from '../helpers/axios' `

Now all requests you do will have the API base URL already set. 

And you can simply just use the path part of the url in your requests like here  `axios.get('/messages')` to communicate with the API.


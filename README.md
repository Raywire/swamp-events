Before the web app implementation, we can try our server using cURL to check that our server is working correctly.

My recommendation is using a Terminal with three open tabs:

# Server execution
```
    $ node server.js
```
Swamp Events service listening on port 3000

# Open connection waiting updates
```
$ curl  -H Accept:text/event-stream http://localhost:3000/events
data: []
# POST request to add new nest
$ curl -X POST \
 -H "Content-Type: application/json" \
 -d '{"momma": "swamp_princess", "eggs": 40, "temperature": 31}'\
 -s http://localhost:3000/nest
{"momma": "swamp_princess", "eggs": 40, "temperature": 31}
```

After the POST request we should see an update like this on the second tab:

```
data: {"momma": "swamp_princess", "eggs": 40, "temperature": 31}
```

Now the nests array is populated with one item, if we close the communication on second tab and open it again, we should receive a message with this item and not the original empty array:

```
$ curl  -H Accept:text/event-stream http://localhost:3000/events
data: [{"momma": "swamp_princess", "eggs": 40, "temperature": 31}]
```

Remember that we implemented the GET /status endpoint. Use it before and after the /events connection to check the connected clients.

The back-end is fully functional, and it’s now time to implement the EventSource API on the front-end.

Finally we push the new nest to our list of nests and the table gets re-rendered.

It’s time for a complete test, I suggest you restart the Node.js server. Refresh the web app and we should get an empty table.

Try adding a new nest:

```
$ curl -X POST \
 -H "Content-Type: application/json" \
 -d '{"momma": "lady.sharp.tooth", "eggs": 42, "temperature": 34}'\
 -s http://localhost:3000/nest
{"momma":"lady.sharp.tooth","eggs":42,"temperature":34}
```
The POST request added a new nest and all the connected clients should have received it, if you check the browser you will have a new row with this information.


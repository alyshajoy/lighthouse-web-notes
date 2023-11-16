# Lecture: Networking with TCP

### Networking
* two machines being able to communicate with each other

### Internet Protocol (IP)
* each computer on the network has a unique address
* similar to a street address
  * IPv4 (ie. 127.0.0.1 or 192.168.0.192) -> ran out of IP addresses, but the vast majority of computers still use this
  * IPv6 (ie. 2001:db8:3333:4444:5555:6666:7777:8888)

### Port
* is the unique identifier of a particular application on the machine
* like apartment numbers
* 65,535 ports to choose from on each computer
  * anything less than 1000 is reserved for the system
  * typically choose 3000-8000 for development ports
  * 80 is reserved for HTTP
  * 443 is reserved for HTTPS
  * 22 is reserved for SSH
  * 5432 is reserved for Postgres

* Clients -> applications that want something
* Servers -> application that has something

### Transport Layer Protocols
* all packages are the same size
* breaks all communication down into packets
* packets have headers -> where is it going, where is it from (like the outside of an envelope)

### TCP
* Transmission Control Protocol
* requires a connection before any communication can happen (referred to as the 3-way handshake)
* any corrupted/missing packets are re-sent
* packets are guaranteed to arrive in order
* useful when communication needs to be guaranteed

### UDP
* User Datagram Protocol
* connectionless
* missing packets are lost
* packets can arrive in any order
* low latency (the amount of time between an event occurring, and you seeing the event occur) applications


Create a TCP Server:

``` javascript
const net = require("net"); // (use ./ only if referring to something you wrote yourself - just put the name when requiring a module that's in node or that you imported)
const server = net.createServer();// creates and returns a server object
const port = 1337;

// listen for incoming connections
server.on("connection", (connection) => { // this particular event on this particular object
  console.log("a client has connected");

  connection.write("welcome to the chat room!");

  // configure the connection to accept utf8
  connection.setEncoding("utf8");

  // listen for incoming data
  connection.on("data", (message) => {
    console.log("client says: ", message);
  });
}); 

server.listen(port, () => {
  console.log(`server is listening on ${port}`);
}); // causes the server to turn on and start listening - can take in a number or a string
 ```

 Create Client:

 ```javascript
 const net = require("net");

 const config = {
  host: 'localhost', // use IP address if not on own computer
  port: '1337'
 };

 const connection = net.createConnection(config);

 // set the encoding on the connection object
  connection.setEncoding("utf-8");

 // listen for someone to type something in
 // stdin represents the connection to the terminal
  process.stdin.on("data", (input) => {
    connection.write(input);
  })

  // listen for incoming messages
  connection.on("data", (message) => {
    console.log("message from server: ", message);
});
 ```

 * client can only connect if server is running
 * every time we use connection.write in the server file, that pairs with a connection.on("data") in the client file
 * buffer messages represent the raw binary -> you need to set the encoding (connection.setEncoding("utf8"))

### Event-Driven Programming
* register a function to be called when a specific event occurs


### stdin-practice
* stdin is a socket to the terminal
* the following code allows you to type something into the console, and that would become messageFromTerminal

```javascript
process.stdin.setEncoding("utf8");

process.stdin.on("data", (messageFromTerminal) => {
  console.log(messageFromTerminal);
});
```

### APIs

* application programming interfaces
* in REST APIs, REST stands for Representational State Transfer
* programmableweb.com provides a list of many available APIs

# Lecture: Promises

* promises are an object class

```javascript

const returnPromise = (value, delay = 1000) => {
  return new Promise((resolve, reject) => {
    setTimeout(() => resolve(`yay! resolved!: ${value}`), delay);
  });
};

const returnRejectedPromise = (value, delay = 1000) => {
  const new Promise((resolve, reject) => {
    setTimeout(() => reject(`doh! rejected: ${value}`), delay);
  });
};

```

* in the above code, "delay = 1000" in the parameters sets the delay value to equal 1000 only if another value isn't specified when the function is called (sets a default)

* the two parameters given in the Promises object are both callbacks (resolve, reject)
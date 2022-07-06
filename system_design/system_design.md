# System Design

## Scaling
| Horizontal      | Vertical |
| ----------- | ----------- |
| Load balancing required      | No load balancing       |
| Resilient 🟢  |  Single point of failure        |
| Network Calls(RPC)   |  Interprocess Communication  🟢  |
|  Data Inconsistency  |   Consistent Data 🟢  |
| Scales Fast 🟢  |  Hardware Limit        |
| Resilient   |  Single point of failure        |

## Load Balancing

Balancing load of requests over 'n' servers.
Done using **consistent hashing**. It **Distributes load** and avoids **Duplication**. 

Suppose we have a horizontal scaled system with 'n' servers, we get each request with a request id which is unique and same to a user. 
>hash(request id) % n is how the request is assigned to their corresponding server.

## Tier
Logical and physical separation between components in an application or service.
### Components
* Database
* Backend application server
* User Interface
* Messaging
* Cache

### Single-tier application
In a single-tier application, the user interface, backend business logic, and the database reside in the same machine.
> Example - PC games, Photoshop etc

| Pros        | Cons           |
| ------------- |-------------|
|   No network latency as every component is in a single machine    | Application publisher has no control over it once published  |

### Two-tier Application
A two-tier application involves a client and a server. The client contains the user interface with the business logic in one machine. Meanwhile, the backend server includes the database running on a different machine. The database server is hosted by the business and has control over it.
> Example of two-tier apps is the browser and mobile app-based games. The game files are pretty heavy, and they only get downloaded on the client once when the user uses the application for the first time. And they make the network calls to the backend only to persist the game state.
### Three-Tier Application
In a three-tier application, the user interface, business logic, and the database all reside on different machines and, thus, have different tiers. They are physically separated.
>Example - Simple blog. The user interface will be written using HTML, JavaScript, and CSS, the backend application logic will run on a server like Apache and the database will be MySQL. A three-tier architecture works best for simple use cases.
### N-Tier Application(Distributed System)
An n-tier application is an application that has more than three components (user interface, backend server, database) involved in its architecture.

What are those components?

* Cache
* Message queues for asynchronous behavior
* Load balancers
* Search servers for searching through massive amounts of data
* Components involved in processing massive amounts of data
* Components running heterogeneous tech commonly known as web services, microservices, etc.
> Example - Instagram, Facebook, TikTok, large-scale consumer services like Uber, Airbnb, online massive multiplayer games like Pokémon Go, Roblox, etc., are n-tier applications. n-tier applications are more popularly known as distributed systems.

Why n-tier or distributed system?

1. Single Responsiblity principle
2. Separation Of concern

## Web architecture
### Client Server architecture
The architecture works on a request-response model. The client sends the request to the server for information and the server responds with it.
### Client
The client holds our user interface. The user interface is the presentation part of the application. It’s written in HTML, JavaScript, CSS and is responsible for the look and feel of the application.

The user interface runs on the client. In very simple terms, a client is a gateway to our application. It can be a mobile app, a desktop or a tablet like an iPad. It can also be a web-based console, running commands to interact with the backend server.
#### Thin Client
Client that just holds the user interface, no business logic.(3-Tier)
#### Thick Client
Client that holds some or all business logic(2-tier)
### Web Server
The primary task of a web server is to receive the requests from the client and provide the response after executing the business logic based on the request parameters received from the client.
### AJAX (Async javascript and xml)
Instead of requesting the data manually every time with the click of a button, AJAX enables us to fetch the updated data from the server by automatically sending the requests over and over at stipulated intervals.
AJAX uses an XMLHttpRequest object to send the requests to the server. This object is built in the browser and uses JavaScript to update the HTML DOM (Document Object Model).

AJAX is commonly used with the jQuery framework to implement the asynchronous behavior on the UI.


### REST API
Client-server communication happens over HTTP which is a stateless connection.
Rest Endpoints
An API/REST/Backend endpoint means the URL of the service that the client could hit. For instance, https://myservice.com/users/{username} is a backend endpoint for fetching the user details of a particular user from the service.

### Client side rendering
When a browser receives a web page from the server in response, it has to render the response on the window in the form of an HTML page.

To pull this off, the browser has several components, such as:

* The browser engine
* Rendering engine
* JavaScript interpreter
* Networking and the UI backend
* Data storage etc.

### Server side rendering
To cut down all this rendering time on the client, developers often render the UI on the server, generate HTML there and directly send the HTML page to the UI.

This technique is known as server-side rendering. It ensures faster rendering of the UI, averting the UI loading time in the browser window because the page is already created and the browser doesn’t have to do much assembling and rendering work.

|| Client side rendering  | Server side rendering          |
| -------------| ------------- |-------------|
|Use case|modern websites are dependent on **AJAX**, doing this on server side is resource intensive |Delivering static content, good for SEO as crawlers can easily read the generated page|

## Scalability
Scalability means the application’s ability to handle and withstand increased workload without sacrificing performance.

If your app takes x seconds to respond to a user request. It should take the same x seconds to respond to each of your app’s million concurrent user requests.
There are two ways to scale an application:

* Vertically - adding more power to our server.
* Horizontally - also known as scaling out, means adding more hardware to the existing hardware resource pool. This increases the computational power of the system as a whole.
### Latency
Latency is measured as the time difference between the action that a user takes on the website and the system’s response in reaction to that action. The action can be an event like clicking a button, scrolling down a web page, etc.
* Application Latency
* Network Latency
### Vertical Scaling(scaling up)
Adding more power to existing servers

### Horizontal Scaling(scaling out)
adding more hardware to the existing hardware resource pool.

### Improve Scalability of application
1. Profiling - Time/Space complexity of your code. Errors, robustness and safety.
2. Caching - Cache all static data, hitting the database only when required. use **write-through**
3. Data compression - stored in compressed form will consume less bandwidth.
4. Avoid unnessesary request-response cycles
5. Optimising parameter like- 
* CPU usage
* Network bandwidth consumption
* Throughput
* Number of requests processed within a stipulated time
* Latency
* Memory usage of the program
* End-user experience when the system is under heavy load

### Availabilty
High availability, also known as HA, is the ability of the system to stay online despite having failures at the infrastructural level in real-time.

### DNS 
When a user types in the URL of the website in their browser and hits enter, this event is known as DNS querying.

Four key components, or a group of servers, make up the DNS infrastructure. These are:

1. DNS Recursive nameserver aka DNS Resolver
2. Root nameserver
3. Top-Level Domain nameserver
4. Authoritative nameserver

DNS load balancing enables the authoritative server to return different IP addresses of a particular domain to the clients. Every time it receives a query for an IP, it returns a list of IP addresses of a domain to the client.
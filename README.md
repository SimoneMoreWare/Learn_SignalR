# Learn_SignalR
SignalR Tutorial 

# Notes

# Video Courses
* https://www.youtube.com/watch?v=pl0OobPmWTk

# Topic
* [Overview of ASP.NET Core SignalR](#Overview-of-ASP.NET-Core-SignalR)
    * [What is SignalR?](#What-is-SignalR?)
    * [Use Cases Of SignalR](#Use-Cases-Of-SignalR)
    * [Web Monitoring Application](#WebMonitoringApplication)
    * [Transports](#Transports)
    * [Hubs](#Hubs)
    * [Browsers that don't support ECMAScript 6 (ES6)](#Browsers-that-don't-support-ECMAScript-6-(ES6))
    * [RoadMap](#RoadMap)
* [SignalR Typical Flow](#SignalR-Typical-Flow)
* [Best practices](#Best-practices)
* [Sample](#Sample)
    * [Step 1: Create SignalR Hub](#step-1-create-signalr-hub)
    * [Step 2 and 6: Add methods to hub and SignalR Hub invokes method in client JS to notify clients](#step-2-and-6-add-methods-to-hub-and-signalr-hub-invokes-method-in-client-js-to-notify-clients)
    * [Other steps](#Other-steps)
* [Repository](#Repository)
* [Site](#Site)
* [Exercises](#Exercises)
* [Payment Courses](#Payment-Courses)

# Overview of ASP.NET Core SignalR

## What is SignalR?

ASP.NET Core SignalR is an open-source library that simplifies adding real-time web functionality to apps. Real-time web functionality enables server-side code to push content to clients instantly.

Good candidates for SignalR:

* Apps that require high frequency updates from the server. Examples are gaming, social networks, voting, auction, maps, and GPS apps.
* Dashboards and monitoring apps. Examples include company dashboards, instant sales updates, or travel alerts.
* Collaborative apps. Whiteboard apps and team meeting software are examples of collaborative apps.
* Apps that require notifications. Social networks, email, chat, games, travel alerts, and many other apps use notifications.

SignalR provides an API for creating server-to-client remote procedure calls ([RPC](https://wikipedia.org/wiki/Remote_procedure_call)). The RPCs invoke functions on clients from server-side .NET Core code. There are several [supported platforms](https://learn.microsoft.com/en-us/aspnet/core/signalr/supported-platforms?view=aspnetcore-8.0), each with their respective client SDK. Because of this, the programming language being invoked by the RPC call varies.

The impact of SignalR is not restricted to enhancing user experience. Additionally, it decreases server load time by staying away from unnecessary polling. This reduction in unnecessary network traffic and server load can result in more scalable applications that can support much more simultaneous connections without sacrificing performance. Reduced Network Traffic Compared to Polling or Constant Refreshes: Traditional content update methods including polling or periodic page refreshes consume network traffic and server load. SignalR performs all communications between server and client efficiently, sending only data when updates can be found. This dramatically decreases bandwidth use and server resources, leading to a far more scalable and performant application.

Reduced Network Traffic Compared to Polling or Constant Refreshes: Traditional content update methods including polling or periodic page refreshes consume network traffic and server load. SignalR performs all communications between server and client efficiently, sending only data when updates can be found. This dramatically decreases bandwidth use and server resources, leading to a far more scalable and performant application.

Here are some features of SignalR for ASP.NET Core:

* Handles connection management automatically.
* Sends messages to all connected clients simultaneously. For example, a chat room.
* Sends messages to specific clients or groups of clients.
* Scales to handle increasing traffic.
* [SignalR Hub Protocol](https://github.com/dotnet/aspnetcore/blob/main/src/SignalR/docs/specs/HubProtocol.md)

The source is hosted in a [SignalR repository on GitHub](https://github.com/dotnet/AspNetCore/tree/main/src/SignalR)

## Use Cases Of SignalR

### Chat Applications

SignalR is a leader in chat application development with instant messaging capabilities. [4]
It allows rich messaging applications where users can create rooms or groups and get messages in real time without having to refresh their internet browser.
This is crucial for applications that need group chats, private messaging, and handling a high number of concurrent users.

### Real-time Dashboards:

SignalR’s real time data push capabilities are especially helpful for dashboards that show live data updates, like stock trading platforms, weather apps or sports scores.
It allows the server to push updates to the dashboard when new data becomes available, enabling users to constantly have the up to date info without refreshing manually.
This is crucial for decision making in dynamic environments where data changes rapidly.

### Online Gaming:

In online gaming, SignalR can be used to map game state between players to ensure that all players see the same game world.
It enables the creation of real time, multiplayer games through rapid and secure server-client interaction.
This synchronization is crucial for competitive and cooperative gaming where latency or game state discrepancies can have a large effect on gameplay.

### Collaborative Editing:

SignalR supports the development of applications where several users can edit documents or projects at the same time.
SignalR facilitates collaborative work by making changes made by one user visible to all other users working on the document at the same time.
This is especially crucial in academic, creative, and professional settings where teamwork and collaboration is crucial to productivity.

## Web Monitoring Application

Suppose we want to create a web monitoring application that provides the user with a dashboard capable of displaying a series of information that updates over time.

One approach is to cyclically call an API at certain intervals (polling) to update the data on the dashboard. However, there is a problem: if there is no updated data, we are unnecessarily increasing network traffic with our requests. An alternative is the technique of long-polling: if the server has no data available, instead of sending an empty response, it can keep the request alive until something happens or a predetermined timeout is reached. If there is new data, the complete response reaches the client. A completely different approach is to reverse the roles: the backend contacts the clients when new data is available (push).

Remember that HTML5 has standardized WebSocket, which is a configurable permanent bidirectional connection via a JavaScript interface in compatible browsers. Unfortunately, there must be full WebSocket support, both client-side (browsers) and server-side, to make it available. Therefore, we need to provide alternative mechanisms (fallbacks) to ensure that our application always works.

Microsoft released an open-source library called SignalR for ASP.NET in 2013, which was rewritten for ASP.NET Core in 2018. SignalR abstracts all communication mechanism details by choosing the best one available. The result is being able to write code as if we were always in push mode. With SignalR, the server can call a JavaScript method on all its connected clients or on a specific one.


## Transports
SignalR supports the following techniques for handling real-time communication (in order of graceful fallback):

* [WebSockets](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/websockets?view=aspnetcore-8.0)
    - WebSocket is a bidirectional full-duplex communication protocol that allows interactive communication between clients and servers.
    - With SignalR, WebSocket is the preferred transport method when available, as it offers optimal performance and reduced latency.
    - WebSocket establishes a persistent connection between the client and the server, allowing data to be sent both from the client to the server and from the server to the client without waiting for an explicit request.
- This protocol is supported by many modern browsers and web servers.
* Server-Sent Events
    - SSE is a mechanism that allows the server to send events to the browser asynchronously over a single HTTP connection.
    - Unlike WebSocket, SSE only allows the server to send data to the client; the client cannot send data to the server using SSE.
    - SignalR supports SSE as one of the alternative transport protocols when WebSocket is not available.
    - SSE is supported by many modern browsers, but it may not be available on all servers.
* Long Polling
    - Long Polling is a technique that simulates bidirectional connection by emulating a persistent connection between client and server through a series of HTTP requests.
    - When the client makes a request to the server, the server does not respond immediately. Instead, it keeps the connection open until it has data to send to the client or until a timeout expires.
    - When the server has data to send, it responds to the client's request with the data, and the client immediately sends a new request to keep the connection open.
    - Long Polling is a less efficient technique compared to WebSocket and SSE, as it requires more bandwidth and increases latency.
    - However, Long Polling can be used as a fallback when WebSocket and SSE are not available.

SignalR automatically chooses the best transport method that is within the capabilities of the server and client.

## Hubs

SignalR uses hubs to communicate between clients and servers.

A hub is a high-level pipeline that allows a client and server to call methods on each other. SignalR handles the dispatching across machine boundaries automatically, allowing clients to call methods on the server and vice versa. You can pass strongly-typed parameters to methods, which enables model binding. SignalR provides two built-in hub protocols: a text protocol based on JSON and a binary protocol based on [MessagePack](https://msgpack.org/). MessagePack generally creates smaller messages compared to JSON. Older browsers must support [XHR level 2](https://caniuse.com/#feat=xhr2) to provide MessagePack protocol support.

Hubs call client-side code by sending messages that contain the name and parameters of the client-side method. Objects sent as method parameters are deserialized using the configured protocol. The client tries to match the name to a method in the client-side code. When the client finds a match, it calls the method and passes to it the deserialized parameter data.

In SignalR, a hub is a component that serves as a central point of contact for real-time communication between clients and servers. A hub is a fundamental concept in SignalR and is implemented as a server-side class that exposes methods that can be called by clients and can in turn send messages to clients.

Key Features of a Hub:

1. **Exposed Methods**: A hub defines a set of methods that can be invoked by clients to interact with the server. These methods can accept parameters and may return values or asynchronous tasks.

2. **Bidirectional Communication**: Hubs enable bidirectional communication between clients and servers. This means that clients can call methods exposed by the hub on the server, and at the same time, the server can send messages to clients through the hub.

3. **Automatic Connection Management**: SignalR automatically manages connection handling for hubs. This means that when a client connects or disconnects, SignalR handles the connection lifecycle automatically and manages connections transparently for the application.

4. **Transport Protocol Abstraction**: Hubs provide an abstraction over the various transport protocols supported by SignalR (such as WebSocket, SSE, and Long Polling), allowing developers to write server-side code without having to worry about the details of the underlying transport protocol.

### Example Hub Definition:

```csharp
using Microsoft.AspNetCore.SignalR;
using System.Threading.Tasks;

public class ChatHub : Hub
{
    public async Task SendMessage(string user, string message)
    {
        // Send the message to all clients
        await Clients.All.SendAsync("ReceiveMessage", user, message);
    }
}
```

![imgHub](https://github.com/SimoneMoreWare/Learn_SignalR/blob/main/img/hub.png)

## Browsers that don't support ECMAScript 6 (ES6)

SignalR targets ES6. For browsers that don't support ES6, transpile the library to ES5. For more information, see [Getting Started with ES6 – Transpiling ES6 to ES5 with Traceur and Babel](https://weblogs.asp.net/dwahlin/getting-started-with-es6-%E2%80%93-transpiling-es6-to-es5).

## Additional resources
* [Introduction to ASP.NET Core SignalR](https://learn.microsoft.com/en-us/training/modules/aspnet-core-signalr)
* [Get started with SignalR for ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/tutorials/signalr?view=aspnetcore-8.0)
* [Supported Platforms](https://learn.microsoft.com/en-us/aspnet/core/signalr/supported-platforms?view=aspnetcore-8.0)
* [Hubs](https://learn.microsoft.com/en-us/aspnet/core/signalr/hubs?view=aspnetcore-8.0)
* [JavaScript client](https://learn.microsoft.com/en-us/aspnet/core/signalr/javascript-client?view=aspnetcore-8.0)
* [Browsers that don't support ECMAScript 6 (ES6)](https://learn.microsoft.com/en-us/aspnet/core/signalr/supported-platforms?view=aspnetcore-8.0#es6)

## RoadMap
https://learn.microsoft.com/it-it/aspnet/signalr/overview/getting-started/real-time-web-applications-with-signalr

# SignalR Typical Flow

1. Create SignalR Hub
2. Add methods to hub
3. add client side signalR
4. connect to signalR hub from client js
5. Call signalR hub method
6. SignalR Hub invokes method in client JS to notify clients
7. Client receives update from SignalR hub and performs action

# Best practices

* Minimize Message Size:
    * Keep the data payloads as small as possible. This might involve sending only the necessary data or changes rather than full objects. For example, if you’re updating a user’s status, send only the status change rather than the entire user object: await Clients.All.SendAsync("UpdateStatus", userId, newStatus);
* Reduce Frequency of Messages
    *   Implement mechanisms to throttle the number of messages sent over a period, especially in scenarios with rapid state changes. Consider a scenario where you’re tracking mouse movements in a collaborative drawing app. Instead of sending updates for every pixel change, you could send updates at regular intervals
* Reconnecting Clients: Implement client-side logic to automatically reconnect in case of disconnection.
``` 
connection.onclose(async () => {
    await start();
});

async function start() {
    try {
        await connection.start();
        console.log("SignalR Connected.");
    } catch (err) {
        console.log(err);
        setTimeout(start, 5000);
    }
}

start();
```
*  Authentication:
    *  SignalR can use ASP.NET Core’s authentication system. Ensure that your hub methods are accessible only to authenticated users by applying the [Authorize] attribute.
*  Authorization:
    *  For more granular control, you can implement custom authorization policies based on user roles or claims.
*  Logging:
    *  Implement logging to monitor connection density, message throughput, and error rates. ASP.NET Core’s built-in logging providers can be configured in Startup.cs.
*  Scale-Out:
    *  For applications needing to support thousands of concurrent connections, consider using Azure SignalR Service or similar scale-out mechanisms. To use Azure SignalR Service, add the service connection string to your appsettings.json and modify the Startup.cs to include: services.AddSignalR().AddAzureSignalR(connectionString);
 
# Sample

## Step 1: Create SignalR Hub

Create a new ASP.NET core MVC .net 8 project in Visual Studio 2022. You should configure the project with "individual account" in "authentication type". 

Add this sample to a GitHub repository. 

Now, we will create the hub. Add the new folder in the project called "Hubs". In this folder you should create a new class file called "UserHub.cs". You remember that you include "using Microsoft.AspNetCore.SignalR;"+

At the moment, you add signalR service in the program.cs file `builder.Services.AddSignalR();`. However, you map an endpoint for a SignalR hub `app.MapHub<UserHub>("/hubs/user");`

Here's a breakdown of each part:
* app: Refers to the IApplicationBuilder object, which is used to configure the ASP.NET Core application during the startup phase.
* MapHub<UserHub>("/hubs/user"): This MapHub method is configuring the path ("/hubs/user") for your SignalR hub. The type UserHub specified within the angle brackets is the type of your hub, which should extend Hub provided by SignalR.
* When a client connects to "/hubs/user", SignalR will automatically handle the communication between the client and the server using the UserHub hub. This is useful for achieving bidirectional real-time communication between the client and the server, such as implementing real-time chat or real-time updates of information displayed in the application.


## Step 2 and 6: Add methods to hub and SignalR Hub invokes method in client JS to notify clients

```
using Microsoft.AspNetCore.SignalR;

namespace SignlalRSample.Hubs
{
	public class UserHub : Hub
	{
		public static int TotalViews { get; set; } = 0;

		public async Task NewWindowLoaded()
		{
			TotalViews++;
			//send update to all clients that total views have been updated
			await Clients.All.SendAsync("updateTotalViews", TotalViews);
		}
}

```

The responsibility of UserHub is to count the number of views on a web page.

`using Microsoft.AspNetCore.SignalR;`
This line imports the Microsoft.AspNetCore.SignalR namespace, which contains classes and tools necessary for using SignalR within an ASP.NET Core application.

UserHub Class Definition
`public class UserHub : Hub`
The UserHub class is a class that extends Hub. This serves as the focal point for managing client connections and sending messages to clients.

Static Property TotalViews
`public static int TotalViews { get; set; } = 0;`
This is a static property that tracks the total number of views. It is shared among all instances of the UserHub class.

```
Method NewWindowLoaded()
public async Task NewWindowLoaded()
{
    TotalViews++;
    //send update to all clients that total views have been updated
    await Clients.All.SendAsync("updateTotalViews", TotalViews);
}
```
This method is called when a new window is loaded (presumably on the client side). It increments the count of total views and then sends an update to all connected clients using the SendAsync method. The name of the method to be called on the client is "updateTotalViews", and the new value of TotalViews is passed as an argument.

## Other steps

Now, you should add a client-side library. At the moment, you should choose unpkg provider and @microsoft/signalr@latest library. Now, there is signalr.js in the wwwroot folder. You move the signalr.js file in the JS folder.

```
//create connection
var connectionUserCount = new signalR.HubConnectionBuilder().withUrl("/hubs/userCount").build();

//connect to methods that hub invokes aka receive notfications from hub
connectionUserCount.on("updateTotalViews", (value) => {
    var newCountSpan = document.getElementById("totalViewsCounter");
    newCountSpan.innerText = value.toString();
});

connectionUserCount.on("updateTotalUsers", (value) => {
    var newCountSpan = document.getElementById("totalUsersCounter");
    newCountSpan.innerText = value.toString();
});

//invoke hub methods aka send notification to hub
function newWindowLoadedOnClient() {
    connectionUserCount.send("NewWindowLoaded");
}

//start connection
function fulfilled() {
    //do something on start
    console.log("Connection to User Hub Successful");
    newWindowLoadedOnClient();
}
function rejected() {
    //rejected logs
}

connectionUserCount.start().then(fulfilled, rejected);
```
This JavaScript file, usersCount.js, handles the connection to the SignalR service within an ASP.NET Core MVC project. SignalR is a framework for building real-time web applications that enables bidirectional communication between client and server.

Here's what happens step by step within this file:

1. Creation of SignalR hub connection:
An object connectionUserCount is created using HubConnectionBuilder provided by SignalR. This object is configured with the URL of the SignalR hub ("/hubs/userCount").
2. Handling notifications from the server:
A callback function on is defined to handle the "updateTotalView" event sent by the server. When the server sends this notification, the associated value is updated inside an HTML element with id "totalViewsCounter".
3. Sending notifications to the server:
A function newWindowLoadedOnClient is defined to send a notification to the server called "NewWindowLoaded" via the connectionUserCount connection. This function seems to be called when a new window is loaded in the client.
4. Handling connection state:
A function fulfilled is defined which is called when the connection to the SignalR hub is successfully established. Similarly, a function rejected is defined which should handle any connection errors. However, the rejected function appears to be empty, so it does nothing.
5. Starting the connection:
The connection to the SignalR hub is started using the start() method. .then() is used to handle the result of starting the connection, so that if the connection succeeds, the fulfilled function is called, otherwise the rejected function is called.

## Update number of user

```
using Microsoft.AspNetCore.SignalR;

namespace SignlalRSample.Hubs
{
	public class UserHub : Hub
	{
		public static int TotalViews { get; set; } = 0;
		public static int TotalUsers { get; set; } = 0;

		public async Task NewWindowLoaded()
		{
			TotalViews++;
			//send update to all clients that total views have been updated
			await Clients.All.SendAsync("updateTotalViews", TotalViews);
		}

		public override Task OnConnectedAsync()
		{
			TotalUsers++;
			Clients.All.SendAsync("updateTotalUsers", TotalViews).GetAwaiter().GetResult();
			return base.OnConnectedAsync();
		}

		public override Task OnDisconnectedAsync(Exception? exception)
		{
			TotalUsers--;
			Clients.All.SendAsync("updateTotalUsers", TotalViews).GetAwaiter().GetResult();
			return base.OnDisconnectedAsync(exception);
		}


	}
}
```
Explanation of the Methods:

OnConnectedAsync()
This method is automatically called when a client connects to the server via SignalR.
It increments the TotalUsers counter, presumably tracking the total number of connected users.
Then, it sends a message to all connected clients using Clients.All.SendAsync("updateTotalUsers", TotalUsers). This method asynchronously sends a message to all connected clients, which will call the JavaScript function updateTotalUsers on the client side with TotalUsers as an argument.
Finally, it returns the execution to the base method base.OnConnectedAsync().

OnDisconnectedAsync(Exception? exception)
This method is automatically called when a client disconnects from the SignalR server.
It decrements the TotalUsers counter.
Then, it sends a message to all connected clients to update the user count, using Clients.All.SendAsync("updateTotalUsers", TotalUsers).
Finally, it returns the execution to the base method base.OnDisconnectedAsync(exception).

About .GetAwaiter().GetResult():
The .GetAwaiter().GetResult() method is used to perform an asynchronous operation synchronously. In practice, when using .GetAwaiter().GetResult(), you are blocking the current thread until the asynchronous operation completes, either getting the result or handling any exceptions.
In the context of your OnConnectedAsync() and OnDisconnectedAsync() methods, these two methods are used after invoking SendAsync() to wait for the message to be sent to connected clients before proceeding with the rest of the code.
However, using .GetAwaiter().GetResult() in an asynchronous environment can pose risks of thread blocking and potential performance issues, especially in high-concurrency scenarios. It's generally preferable to use the complete asynchronous pattern and avoid blocking the main thread. Instead of .GetAwaiter().GetResult(), you might consider using await in an asynchronous context.

# Repository
* https://github.com/dotnet/AspNetCore/tree/main/src/SignalR
* https://github.com/bhrugen/SignalRSample

# PDFs
* [SignalR Basic Flow](https://github.com/SimoneMoreWare/Learn_SignalR/blob/main/pdf/SignalR%20Basic%20Flow.pdf)
* [SignalR Slides](https://github.com/SimoneMoreWare/Learn_SignalR/blob/main/pdf/SignalR.pdf)

# Site
* https://dotnet.microsoft.com/en-us/apps/aspnet/signalr
* https://learn.microsoft.com/en-us/azure/azure-signalr/

# Exercises
* https://www.sitepoint.com/build-real-time-signalr-dashboard-angularjs/
* https://www.telerik.com/blogs/real-time-winui-dashboard-with-signalr-backend
* https://medium.com/@ajith.ramawickrama/develop-a-real-time-dashboard-using-azure-cosmosdb-azure-functions-azure-signalr-services-and-ac670de580f4

# Payment Courses
* https://www.udemy.com/course/signalr-mastery/?couponCode=LETSLEARNNOW
* https://www.udemy.com/course/signalr-the-complete-guide/?couponCode=LETSLEARNNOW

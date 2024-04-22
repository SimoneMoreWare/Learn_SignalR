# Learn_SignalR
SignalR Tutorial 

# Notes

# Topic
* [Overview of ASP.NET Core SignalR](#Overview-of-ASP.NET-Core-SignalR)
* [Repository](#Repository)

# Overview of ASP.NET Core SignalR

ASP.NET Core SignalR is an open-source library that simplifies adding real-time web functionality to apps. Real-time web functionality enables server-side code to push content to clients instantly.

Good candidates for SignalR:

* Apps that require high frequency updates from the server. Examples are gaming, social networks, voting, auction, maps, and GPS apps.
* Dashboards and monitoring apps. Examples include company dashboards, instant sales updates, or travel alerts.
* Collaborative apps. Whiteboard apps and team meeting software are examples of collaborative apps.
* Apps that require notifications. Social networks, email, chat, games, travel alerts, and many other apps use notifications.

SignalR provides an API for creating server-to-client remote procedure calls ([RPC](https://wikipedia.org/wiki/Remote_procedure_call)). The RPCs invoke functions on clients from server-side .NET Core code. There are several [supported platforms](https://learn.microsoft.com/en-us/aspnet/core/signalr/supported-platforms?view=aspnetcore-8.0), each with their respective client SDK. Because of this, the programming language being invoked by the RPC call varies.

Here are some features of SignalR for ASP.NET Core:

* Handles connection management automatically.
* Sends messages to all connected clients simultaneously. For example, a chat room.
* Sends messages to specific clients or groups of clients.
* Scales to handle increasing traffic.
* [SignalR Hub Protocol](https://github.com/dotnet/aspnetcore/blob/main/src/SignalR/docs/specs/HubProtocol.md)

The source is hosted in a [SignalR repository on GitHub](https://github.com/dotnet/AspNetCore/tree/main/src/SignalR)

## Transports
SignalR supports the following techniques for handling real-time communication (in order of graceful fallback):

* [WebSockets](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/websockets?view=aspnetcore-8.0)
* Server-Sent Events
* Long Polling

SignalR automatically chooses the best transport method that is within the capabilities of the server and client.

## Hubs

SignalR uses hubs to communicate between clients and servers.

A hub is a high-level pipeline that allows a client and server to call methods on each other. SignalR handles the dispatching across machine boundaries automatically, allowing clients to call methods on the server and vice versa. You can pass strongly-typed parameters to methods, which enables model binding. SignalR provides two built-in hub protocols: a text protocol based on JSON and a binary protocol based on [MessagePack](https://msgpack.org/). MessagePack generally creates smaller messages compared to JSON. Older browsers must support [XHR level 2](https://caniuse.com/#feat=xhr2) to provide MessagePack protocol support.

Hubs call client-side code by sending messages that contain the name and parameters of the client-side method. Objects sent as method parameters are deserialized using the configured protocol. The client tries to match the name to a method in the client-side code. When the client finds a match, it calls the method and passes to it the deserialized parameter data.

## Browsers that don't support ECMAScript 6 (ES6)

SignalR targets ES6. For browsers that don't support ES6, transpile the library to ES5. For more information, see [Getting Started with ES6 – Transpiling ES6 to ES5 with Traceur and Babel](https://weblogs.asp.net/dwahlin/getting-started-with-es6-%E2%80%93-transpiling-es6-to-es5).

## Additional resources
* [Introduction to ASP.NET Core SignalR](https://learn.microsoft.com/en-us/training/modules/aspnet-core-signalr)
* [Get started with SignalR for ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/tutorials/signalr?view=aspnetcore-8.0)
* [Supported Platforms](https://learn.microsoft.com/en-us/aspnet/core/signalr/supported-platforms?view=aspnetcore-8.0)
* [Hubs](https://learn.microsoft.com/en-us/aspnet/core/signalr/hubs?view=aspnetcore-8.0)
* [JavaScript client](https://learn.microsoft.com/en-us/aspnet/core/signalr/javascript-client?view=aspnetcore-8.0)
* [Browsers that don't support ECMAScript 6 (ES6)](https://learn.microsoft.com/en-us/aspnet/core/signalr/supported-platforms?view=aspnetcore-8.0#es6)

# Repository
* https://github.com/dotnet/AspNetCore/tree/main/src/SignalR

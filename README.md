# API Gateway from SDET Perspective
## What a Test Automation Engineer should know about API GW?

May IT employers require knowledge of API Gateway (GW), its components, monolithic or microservices architecture. An SDET should know about the role of the API GW, in terms of the cloud, local architecture setup, etc. Take an e-commerce app for example.

...

For the Cart, just to add something, I need some Authentication. Apply the Authentication concept logic to the cart API.

When I want to add something to my product, or delete a product, I need an Admin Role for my app. A regular customer cannot delete add/delete a product. Only an Admin, with an admin role and access, can add/delete a product, someone like a Product Category Manager. For this purpose, I add __Authorization__.

__Authentication:__
- for a regular customer
- add to cart or get some cart value
- can do with simple authentication
- user has to be logged in

__Authorization:__
- require Admin Access

For /home and /product, I don't need any Access without username/password, I can hit the app and access it.

Authentication and Authorization are not related to the app features or functionality, they are a separate thing all together.
I move the Authentication/Authorization logic inside my API GW. It means that when a Request comes from the client/user side, firstly the user gets authorized before, eg, accessing a cart, and only then the cart API can be accessed.

The Client sends a Request to the API GW, where Authentication is defined. Authentication notifies the Client that they can now access the product API. A client would state their Authentication role and proceed with accessing the product API, adding or deleting a product, etc.

Authentication Role is defined at the API GW level, not at the business/app level. I've disctinstly segregated my Business Logic.

Whatever request I send, I want it to be sent via a Secure HTTP -> HTTPS. For secure communication, I need __SSL Certification__ (secure connection certificate) at the API GW level. Can renew SSL Certification every 30-60 days at the API GW level. No point putting the Certification at the app level.

Requests from a Client come through the SSL Certification at the API GW level. When the certificate is valid, the client can access the app. The advantage of SSL Certification is that the API client can only access a specific entry point with the help of whichever URL they want to access. Business Logic is segregated. For any bad actor (hacker), I actually make the app safe by defining the SSL Certification at the API GW level.


__Feature # 1__
- Authentication
- Authorization
- SSL Certification


Let's say I deal with a Monolithic architecture. All the APIs, business logic, etc. are available under one machine/system/component. I convert my Monolithic architecture into Microservices, tiny services available for each feature. 3 segregated Microservices are available.

...

A client makes a request (via HTTPS) directly to the API GW only. When the client wants to retrive the product info, I can define that in __Routing__. A client asks to give them (requests) the Product API, so that they can access the Product. So the Routing defines that if the client requests Product, the request is only sent to the Product APIs. Then the response is sent back to the client.

The same thing happens, when a client requests the Cart APIs. When the client requests for the Cart, Routing decides that the request gets sent to the Cart API. Routing gets the response, forwards it back with the cart info. And it spins up in a form of a web [browser] app.

__Feature # 2__
- Routing

Suppose the app is getting popular and profitable. A big crowd uses it. I offer a couple of  extra features during a holiday season: trends, deals, discounts, etc. When Client sends many requests, it can cause a delay. The more requests, the more delay.

...

I can improce this case by adding another component called __Adaptor__ at the API GW level. My developers always say thay they add the Adaptor at the API GW level. Adaptor decides how many requests to handle.

Improved Diagram:
...

Adaptor takes the Request from the Client/Device. Client sends Request to the Adaptor via one single network call. It's faster than before, when Client sent 4 different requests. Now Client sends 1 single request over the network to the API GW. The Gateway makes a number of calls, consolidates the response, then gives it back to the Client side in the form of a Response.

__Feature # 3__
- Adaptor

Let the Adaptor decide how many calls to make and to which Microservices. Not from the Client side. Client send an n-number of requests (10, 20 ... 100), the API GW decides how to send them. Client just clicks on Homepage, Trends, Deals, etc.

I can improve my design/diagram. I have the Static Html Homepage [Microservice], which is not a part of the Business Logic. It's a simple homepage, static content. I can decommission the Static Content Html from the Microservices Web App site and define/place it at the aPI GW level. No need to call the actial app server.

...

__Feature # 4__
- Static Content stored at the API GW level

...


Client sends Request via API GW and gets the response back form the 'Trending Products' Miscroservice - the 50 products that are available between 10-11 am. But what if the Client send another Request at 10:10, then another at 10:15 am and 10:20 am, and so forth (multiple times until 11 am). Every time the Client sends the Request, they hit this particular Microservice and a Response back. It's unnecessary, because every time the product count remains the same. The next list of available trending product becomes available after one hour. Once this hour expires at 11 am, I get the next list with the next 50 products available.

What to do? Set __Caching__ at the API GW level. The 1st time the Request is sent to this particular Microservice, the Response is sent back, and the 50 products are stored in the Cache, eg, at 10 am. After 10 minutes, at 10:10 am, I send another request to the API GW. The Gateway has the intelligence to understand that the Client requests a list of the current Trending Products. It does not call the Microservice any longer. There's no point in calling the same Microservice again and again (with the same request), while the product output remains the same. It's better to retrieve the Response from the Cache only. 

This is the biggest advantage of Caching. The same Response is given back the Client up until 11 am. I can define/configure a threshold timeout of 1 hour at the API GW level. After 1 hour, the  Cache with 50 products gets deleted/detroyed. At 11 am the Request is sent again to the API GW. Then again this particular Microservice gets called, retrieving the next 50 Trending Products. And again the new 50 products get stored in the Cache for the next 1 hour until 12 pm.

The Cache is always handled at the API GW level, not at the Business Logic/WebApp/Microservice level.

__Feature #5__
- Caching

Example: In my web app I provide different videos. There are some video APIs, meaning _fast_ streaming APIs and _slow_ streaming APIs.

...

Client # 1 sends a Request from a Mobile device. What to do? Define Routing at the device level. When Request comes via Mobile, I need a low-quality video. The Low Quality video Microservice responds back and provides the Client with a low-quality video  on their phone (mobile device).

Client # 2 wants an API and sends a Request from a Desktop. The Router determines that the request comes from the desktop device, and the Desktop route is taken. The Router defines that desktop request should go to (call) the HD Quality content [microservice], where HD-quality videos are available. IT responds back and routes over to the Desktop Client. Then the Desktop [browser] Client is able to view high-quality videos.

Router defines which  route to take - Mobile or Desktop? I configure this at the API GW level.

__Feature # 7__
- Load Balancer

...

__Feature # 8__
- Protocol Adaptor

...


__Feature # 9__
- Monitoring

...

__Feature # 10__
- Billing

...

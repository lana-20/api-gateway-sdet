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


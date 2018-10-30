Components
==========

Auth Service
************

**Laravel Passport** Auth Service provides RESTful API endpoints with secure Authentication and Authorization. It holds user security credentials, handles password resets, and also manages API keys. It is built on top of the `League OAuth2 server <https://github.com/thephpleague/oauth2-server>`_ that is maintained by Andy Millington and Simon Hamp. The Auth Service is used together with Zizaco's `entrust <https://github.com/Zizaco/entrust>`_ to provide role-based access controls.

**JSON Web Tokens (JWT)**, which is a JSON object that is defined in RFC 7519 as a safe way to represent a set of information between two parties, is used to handle third party applications' authentication.

Authentication Process
**********************

**JWT**

#. A user first signs into the authentication server using the authentication server’s login system (e.g. username and password).

#. The authentication server then creates the JWT and sends it to the user.

#. When the user makes API calls to the application, the user passes the JWT along with the API call.

 In this setup, the application server would be configured to verify that the incoming JWT are created by the authentication server. So, when the user makes API calls with the attached JWT, the application can use the JWT to verify that the API call is coming from an authenticated user.
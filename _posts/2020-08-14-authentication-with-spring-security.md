---
layout: post
title:  "Authentication with spring security"
date:   2020-08-14 11:54:00 +0100
categories: Spring security, Authentication, Spring, Spring boot, Spring webflux, java, kotlin
---
# Spring security
For java/kotlin, spring boot application, spring security covers lots of security aspects, like authentication, authorization, single sign on ans etc.
Here is the official spring security documentation https://docs.spring.io/spring-security/site/docs/current/reference/html5/
All the examples in this article are based on kotlin, spring webflux, so we should provide:
* Bean SecurityWebFilterChain for security configuration
* Bean MapReactiveUserDetailsService for username/password verification 
Another point, here I will not discuss about the csrf configuration, since it is not the target topic of this article.
So in all examples, csrf is disabled.
## Spring security configurable Username/Password Authentication
### Basic Authentication
#### Workflow
Basic authentication is the simplest way and most stateless way to do authentication.
![Basic auth](/images/2020-08-14-authentication-with-spring-security/basic_auth.png "Basic auth")

For each resource request, client should send username/password in the http header: Authorization with format
    
    username:password base64 encode

So here is an example, if we have username: user, password: user, with base64 encode
    
    echo 'user:user' | base64 (command in linux)

we can get the token:

    dXNlcjp1c2VyCg==    

Then we can put the token in http header
    
    Authorization: Basic dXNlcjp1c2VyCg==
  
So in fact, for each resource request, we should attach this http header, and there is no need to add login or logout 
to handle authentication. And another way to do the basic auth, is to add the username and password in url:

    https://username:password@example.com/resource
    
Just for any special symbol in username or password should be put with url encoding
#### Example configuration spring security
    @Configuration
    @EnableWebFluxSecurity
    class SecurityConfig {
        @Bean
        fun securityWebFilterChain(http: ServerHttpSecurity): SecurityWebFilterChain {
            return http.csrf().disable()
                    .authorizeExchange()
                    .anyExchange().authenticated()
                    .and().httpBasic()
                    .and().formLogin().disable()
                    .build();
        }
    
        @Bean
        fun userDetailsService(): MapReactiveUserDetailsService? {
            val user: UserDetails = User.withDefaultPasswordEncoder()
                    .username("user")
                    .password("user")
                    .roles("USER")
                    .build()
            return MapReactiveUserDetailsService(user)
        }
    }
    
#### Security issue
Through the username and password are sent with base64 encoded, which is easily reversible,
https connection is required, otherwise, your username and password are visible.   

### Form login
Form login is widely used in most application for authentication part.
#### Workflow
![Form login](/images/2020-08-14-authentication-with-spring-security/form_login.png "Form login")

1. Client request protected resource
2. Server check if client is authenticated
3. Client send username & password via POST /login
4. Server check username & password validity
5. If step 4 is ok, add session id to cookie(by default, spring security add to cookie,
but we can choose to put into cookie or http header), and return to client
![Form login](/images/2020-08-14-authentication-with-spring-security/login_postman.png "Form login")
* The advantage of putting session id into cookie is to avoid XSS attack with httpOnly(so javascript could not read cookie, secure so https only) 
6. After some operations, client wants to log out, then POST /logout, server invalidate session
#### Example configuration spring security
    @Configuration
    @EnableWebFluxSecurity
    class SecurityConfig {
        @Bean
        fun securityWebFilterChain(http: ServerHttpSecurity): SecurityWebFilterChain {
            return http.csrf().disable()
                    .authorizeExchange()
                    .anyExchange().authenticated()
                    .and().formLogin()
                    .and().logout()
                    .and().build();
        }
    
        @Bean
        fun userDetailsService(): MapReactiveUserDetailsService? {
            val user: UserDetails = User.withDefaultPasswordEncoder()
                    .username("user")
                    .password("user")
                    .roles("USER")
                    .build()
            return MapReactiveUserDetailsService(user)
        }
    }
    
### Digest Authentication
~~The digest is based on basic auth and with hash MD5, and it return a hashed token instead of base64 encoded token, 
and the token is generated with below way:~~
    
    HA1 = MD5(username:realm:password)
    HA2 = MD5(method:digestURI)
    response = MD5(HA1:nonce:HA2) 

~~where nonce is generated by server side randomly 
Since MD5 nowadays is not a secure hash algorithm, so spring security also recommend not to use~~
## Spring security username/password verification 
As mentioned in spring security official documentation, different user credential storage systems are configurable & pluggable:
* Simple Storage with In-Memory Authentication
* Relational Databases with JDBC Authentication
* Custom data stores with UserDetailsService
* LDAP storage with LDAP Authentication
## FormLogin with jwt
The default form login use session to implement authentication and which is stateful, 
so not easy for resilience and scalability. That's why jwt or/adn jws plays an important role more and more.
Since it is not the objective of this article, I will not discuss here. 

---
layout: post
title:  "HTTP redirection in java spring webflux"
date:   2020-07-15 16:27:00 +0100
categories: HTTP redirection, spring webflux redirection, java
---
# Context
Recently when I work for some authentication & authorization projects, most time is spent into the debate of protocol OAuth2 
with its different implementation: implicit flow, authorization code flow, authorization code flow with PKCE. And what's more,
we had mentioned a lot of times about redirection to do authentication & to retrieve authorization code for instance. So sometimes,
I found out the redirection mechanism is not so evident as it sounds. So here, I wanna go deep dive into the process of redirection.

# Server side implementation
After trying different solutions from Internet, I found out the below implementation of redirection in java spring boot is the simplest &
most independent, since here I just want a RESTful API and I do not want to handle front staff.
    
    @GetMapping("/redirect")
    ResponseEntity<Void> redirect() {
        return ResponseEntity.status(HttpStatus.FOUND)
                .location(URI.create("http://www.yahoo.com"))
                .build();
    }  

What this code does here, it returns a http status code 302 with label Found and a http header:
    
    location: yahoo website link

# Client side implementation
The server side implementation has been running in localhost at port 8090
## Using curl
If you do the simple curl request

    $ curl  localhost:8090/redirect
    $ 

then nothing appears in the response, which performs correctly, since the http response is status code 302 and a http header with empty body.
By default, the curl will not automatically follow the redirection, so in order to follow the redirection automatically, we should add the -L option

    $ curl -L localhost:8090/redirect
    
then you will get the yahoo web page html
## Java spring webflux http client: web client
Same as curl, by default web client will not follow all redirection
        
        WebClient webClient = WebClient.builder().baseUrl("http://localhost:8090")
                .build();

        final String result = webClient.get().uri("/redirect")
                .retrieve()
                .bodyToMono(String.class)
                .block();
        System.out.println(result);
       
In order to follow all redirection, you can do it manually, which means you parse the response and do a httpp operation by 
retrieving the url from http header location. And here is a simple way:
        
        WebClient webClient = WebClient.builder().baseUrl("http://localhost:8090")
                .clientConnector(new ReactorClientHttpConnector(
                        HttpClient.create().followRedirect(true)
                ))
                .build();

        final String result = webClient.get().uri("/redirect")
                .retrieve()
                .bodyToMono(String.class)
                .block();
        System.out.println(result);
        
# Front end redirection
Concerning the front end redirection is more complicated, apart from the aspect to follow all redirection, we should
consider api data call like ajax and traditional page rendering call.
## Simple page rendering call
For simple page rendering call, for the front end part is same as back end, follow all the redirection will resolve the problem.

## Partial content rendering 
In this case, things are much more complicated, since you should consider the integration part in case of using Content Management System
or/and iframe or/and other technology.
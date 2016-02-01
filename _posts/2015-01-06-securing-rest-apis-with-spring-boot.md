---
id: 946
title: Securing REST APIs With Spring Boot
date: 2015-01-06T14:16:21+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=946
permalink: /2015/01/06/securing-rest-apis-with-spring-boot/
dsq_thread_id:
  - 4535001953
dsq_needs_sync:
  - 1
categories:
  - Java
---
In my previous [post](http://ryanjbaxter.com/2014/12/17/building-rest-apis-with-spring-boot/ "Building REST APIs With Spring Boot") I described how to build REST APIs with Spring Boot.  One glaring omission to that post was security.  Security can be a daunting part of building any app because if you get it wrong there are huge implications.  Luckily with Spring Boot, like with most other things, adding security to your applications is pretty simple.

All you need to do is add a dependency to your Maven POM.  If we take the project from the previous blog post all you need to add is

<pre class="font-size:15 lang:default decode:true">&lt;dependency&gt;
  &lt;groupId&gt;org.springframework.boot&lt;/groupId&gt;
  &lt;artifactId&gt;spring-boot-starter-security&lt;/artifactId&gt;
&lt;/dependency&gt;</pre>

That&#8217;s it, your entire app is protected via basic auth. The username is user and the password is a string randomly generated when you start the app.  If you start your Spring Boot app you will see the below message printed to the console (with a different password).

<pre class="font-size:15 lang:default highlight:0 decode:true">Using default security password: 740954d0-4b93-49f8-a9e1-eb296ab74ab7</pre>

Obviously this isn&#8217;t production grade since the password is random and will change every time you start the app and the username is not customizable, but it is a good start.

If you try to hit one of our REST APIs like http://localhost:8080/contacts, you will get back a 401 saying you are unauthorized.  You need to supply the username and password with the request in order to authorize.  Most REST clients like Paw or the FireFox REST Client plugin support basic authorization and have a way for you to supply a username and password for your request.  If you are using CuRL from the command line you need to supply the username and password using the -u option, for example

<pre class="font-size:15 lang:default highlight:0 decode:true">$ curl -u user:740954d0-4b93-49f8-a9e1-eb296ab74ab7 http://localhost:8080/contacts</pre>

After supplying the basic authorization credentials, your APIs should behave just as before.

You can customize the default username, password, and role used by Spring security by settings a couple properties in application.properties.  See the <a href="http://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#production-ready-sensitive-endpoints" target="_blank">Spring Boot documentation</a> for more details.

In a real world use case you will want to customize the users who are authorized to access your APIs.  This is relatively easy to do, but requires you write a little code.  Given our application is already using Mongo DB for backend persistence it would be good if we could store and lookup user information in a Mongo DB collection.  As explained in my previous [post](http://ryanjbaxter.com/2014/12/17/building-rest-apis-with-spring-boot/ "Building REST APIs With Spring Boot"), it is pretty easy to do that.  First we need to create a Java object for each user, lets call this object an Account.

<pre class="font-size:15 lang:default decode:true">import org.springframework.data.annotation.Id;

public class Account {
  
  @Id
  private String id;
  private String username;
  private String password;
  
  public Account(){}
  
  public Account(String username, String password) {
    this.username = username;
    this.password = password;
  }
  public String getId() {
    return id;
  }
  public void setId(String id) {
    this.id = id;
  }
  public String getUsername() {
    return username;
  }
  public void setUsername(String username) {
    this.username = username;
  }
  public String getPassword() {
    return password;
  }
  public void setPassword(String password) {
    this.password = password;
  }

}</pre>

Our account object is pretty basic, it has properties for username and password and then a unique ID for each account.  Next we will need a way to interface with the Mongo DB collections, so lets create an interface which extends MongoRepository.

<pre class="font-size:15 lang:default decode:true">import org.springframework.data.mongodb.repository.MongoRepository;

public interface AccountRepository extends MongoRepository&lt;Account, String&gt; {
  
  public Account findByUsername(String username);

}</pre>

As you can see our repository is very simple.  In order to look up a user account we have added a method called findByUsername to our repository interface.  This method will perform a query on our collection to find an Account by their username.  We are taking advantage of a little Spring Data magic here because we don&#8217;t actually have to provide an implementation for this method.  Spring Data uses the method name to create the query and implementation of the method for us.

We now have a simple representation for our accounts but we need to tell Spring Security to use the accounts from the Mongo DB Collection.  We can do this by creating a class that extends GlobalAuthenticationConfigurerAdapter.  We can add a basic implementation to Application.java

<pre class="font-size:15 lang:default decode:true ">import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.authentication.configurers.GlobalAuthenticationConfigurerAdapter;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.core.authority.AuthorityUtils;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;

@Configuration
@ComponentScan
@EnableAutoConfiguration
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
    
    @Bean
    CommandLineRunner init(final AccountRepository accountRepository) {
      
      return new CommandLineRunner() {

        @Override
        public void run(String... arg0) throws Exception {
          accountRepository.save(new Account("rbaxter", "password"));
          
        }
        
      };

    }
    
}

@Configuration
class WebSecurityConfiguration extends GlobalAuthenticationConfigurerAdapter {

  @Autowired
  AccountRepository accountRepository;

  @Override
  public void init(AuthenticationManagerBuilder auth) throws Exception {
    auth.userDetailsService(userDetailsService());
  }

  @Bean
  UserDetailsService userDetailsService() {
    return new UserDetailsService() {

      @Override
      public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        Account account = accountRepository.findByUsername(username);
        if(account != null) {
        return new User(account.getUsername(), account.getPassword(), true, true, true, true,
                AuthorityUtils.createAuthorityList("USER"));
        } else {
          throw new UsernameNotFoundException("could not find the user '"
                  + username + "'");
        }
      }
      
    };
  }
}

@EnableWebSecurity
@Configuration
class WebSecurityConfig extends WebSecurityConfigurerAdapter {
 
  @Override
  protected void configure(HttpSecurity http) throws Exception {
    http.authorizeRequests().anyRequest().fullyAuthenticated().and().
    httpBasic().and().
    csrf().disable();
  }
  
}</pre>

The key here is our UserDetailsService implementation which uses the AccountRepository to look up the user and see if there is an account in our repository for them.  If there is we return a User object with the details from the Account.  The User object is much more robust than our Account implementation.  It supports things like password expiration and accounts being locked.  It also supports the concept of roles, allowing you to have roles like USER or ADMIN to separate out permissions to perform certain operations.  In our example everyone has the USER role, but for more complex use cases you might want to have additional roles.

Also noticed we have modified our Application class so that it initializes the Mongo DB collection with a user called rbaxter when the application starts up.  Right now our application does not have a way to add additional users dynamically without modifying the code.  You could create some REST endpoints to register new users if you want, you can follow the same process as I showed in my previous blog post on how to do that.

If we start our application and make a request to http://localhost:8080/contacts supplying the username rbaxter and the password password as our basic auth creds we should get back a 200.  That is all there is to implementing basic authentication with Spring Boot.

Spring supports other types of security as well.  A common use case would be to use an LDAP server for authentication, <a href="http://docs.spring.io/spring-security/site/docs/3.2.5.RELEASE/reference/htmlsingle/#ldap" target="_blank">Spring supports this as well</a>.  OAuth is another type of security that is very popular especially for APIs on the open web.  Spring has support for OAuth 1 and 2 via the <a href="http://projects.spring.io/spring-security-oauth/" target="_blank">Spring Security OAuth project</a>.  In addition, the <a href="http://docs.spring.io/spring-social/docs/1.1.0.RELEASE/reference/htmlsingle/#section_signin" target="_blank">Spring Social project</a> allows you to authenticate users using social networks like Facebook or Twitter.  As you can see the basic auth support in Spring Security is just a starting point, there are many other options out there for you to use to secure your Spring Boot apps.

&nbsp;
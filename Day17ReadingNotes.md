# Day 17

## Spring Boot and OAuth2

- The samples are all single-page apps using Spring Boot and Spring Security on the back end. They also all use plain jQuery on the front end. But, the changes needed to convert to a different JavaScript framework or to use server-side rendering would be minimal.

All samples are implemented using the native OAuth 2.0 support in Spring Boot.

There are several samples building on each other, adding new features at each step:

simple: a very basic static app with just a home page and unconditional login via Spring Boot’s OAuth 2.0 configuration properties (if you visit the home page, you will be automatically redirected to GitHub).

click: adds an explicit link that the user has to click to login.

logout: adds a logout link as well for authenticated users.

two-providers: adds a second login provider so the user can choose on the home page which one to use.

custom-error: adds an error message for unauthenticated users, and a custom authentication based on GitHub’s API.

Each app can be imported into an IDE. You can run the main method in SocialApplication to start an app. They all come up with a home page on http://localhost:8080 (and all require that you have at least a GitHub and Google account if you want to log in and see the content).

You can also run all the apps on the command line using mvn spring-boot:run or by building the jar file and running it with mvn package and java -jar target/*.jar (per the Spring Boot docs and other available documentation). There is no need to install Maven if you use the wrapper at the top level, e.g.
o make the application secure, you can simply add Spring Security as a dependency. Since you’re wanting to do a "social" login (delegate to GitHub), you should include the Spring Security OAuth 2.0 Client starter:

pom.xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-oauth2-client</artifactId>
</dependency>

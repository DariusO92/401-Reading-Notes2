# Day 16
- Authentication and Access Control
Application security boils down to two more or less independent problems: authentication (who are you?) and authorization (what are you allowed to do?). Sometimes people say “access control” instead of "authorization", which can get confusing, but it can be helpful to think of it that way because “authorization” is overloaded in other places. Spring Security has an architecture that is designed to separate authentication from authorization and has strategies and extension points for both.

Authentication
The main strategy interface for authentication is AuthenticationManager, which has only one method:

public interface AuthenticationManager {

  Authentication authenticate(Authentication authentication)
    throws AuthenticationException;
}
An AuthenticationManager can do one of 3 things in its authenticate() method:

Return an Authentication (normally with authenticated=true) if it can verify that the input represents a valid principal.

Throw an AuthenticationException if it believes that the input represents an invalid principal.

Return null if it cannot decide.

AuthenticationException is a runtime exception. It is usually handled by an application in a generic way, depending on the style or purpose of the application. In other words, user code is not normally expected to catch and handle it. For example, a web UI might render a page that says that the authentication failed, and a backend HTTP service might send a 401 response, with or without a WWW-Authenticate header depending on the context.

The most commonly used implementation of AuthenticationManager is ProviderManager, which delegates to a chain of AuthenticationProvider instances. An AuthenticationProvider is a bit like an AuthenticationManager, but it has an extra method to allow the caller to query whether it supports a given Authentication type:

public interface AuthenticationProvider {

	Authentication authenticate(Authentication authentication)
			throws AuthenticationException;

	boolean supports(Class<?> authentication);
}
The Class<?> argument in the supports() method is really Class<? extends Authentication> (it is only ever asked if it supports something that is passed into the authenticate() method). A ProviderManager can support multiple different authentication mechanisms in the same application by delegating to a chain of AuthenticationProviders. If a ProviderManager does not recognize a particular Authentication instance type, it is skipped.

A ProviderManager has an optional parent, which it can consult if all providers return null. If the parent is not available, a null Authentication results in an AuthenticationException.

Sometimes, an application has logical groups of protected resources (for example, all web resources that match a path pattern, such as /api/**), and each group can have its own dedicated AuthenticationManager. Often, each of those is a ProviderManager, and they share a parent. The parent is then a kind of “global” resource, acting as a fallback for all providers.

- Customizing Authentication Managers
Spring Security provides some configuration helpers to quickly get common authentication manager features set up in your application. The most commonly used helper is the AuthenticationManagerBuilder, which is great for setting up in-memory, JDBC, or LDAP user details or for adding a custom UserDetailsService. The following example shows an application that configures the global (parent) AuthenticationManager:

@Configuration
public class ApplicationSecurity extends WebSecurityConfigurerAdapter {

   ... // web stuff here

  @Autowired
  public void initialize(AuthenticationManagerBuilder builder, DataSource dataSource) {
    builder.jdbcAuthentication().dataSource(dataSource).withUser("dave")
      .password("secret").roles("USER");
  }

}
This example relates to a web application, but the usage of AuthenticationManagerBuilder is more widely applicable (see Web Security for more detail on how web application security is implemented). Note that the AuthenticationManagerBuilder is @Autowired into a method in a @Bean — that is what makes it build the global (parent) AuthenticationManager. In contrast, consider the following example:

@Configuration
public class ApplicationSecurity extends WebSecurityConfigurerAdapter {

  @Autowired
  DataSource dataSource;

   ... // web stuff here

  @Override
  public void configure(AuthenticationManagerBuilder builder) {
    builder.jdbcAuthentication().dataSource(dataSource).withUser("dave")
      .password("secret").roles("USER");
  }

}
If we had used an @Override of a method in the configurer, the AuthenticationManagerBuilder would be used only to build a “local” AuthenticationManager, which would be a child of the global one. In a Spring Boot application, you can @Autowired the global one into another bean, but you cannot do that with the local one unless you explicitly expose it yourself.

Spring Boot provides a default global AuthenticationManager (with only one user) unless you pre-empt it by providing your own bean of type AuthenticationManager. The default is secure enough on its own for you not to have to worry about it much, unless you actively need a custom global AuthenticationManager. If you do any configuration that builds an AuthenticationManager, you can often do it locally to the resources that you are protecting and not worry about the global default.

Authorization or Access Control
Once authentication is successful, we can move on to authorization, and the core strategy here is AccessDecisionManager. There are three implementations provided by the framework and all three delegate to a chain of AccessDecisionVoter instances, a bit like the ProviderManager delegates to AuthenticationProviders.

An AccessDecisionVoter considers an Authentication (representing a principal) and a secure Object, which has been decorated with ConfigAttributes:

boolean supports(ConfigAttribute attribute);

boolean supports(Class<?> clazz);

int vote(Authentication authentication, S object,
        Collection<ConfigAttribute> attributes);
The Object is completely generic in the signatures of the AccessDecisionManager and AccessDecisionVoter. It represents anything that a user might want to access (a web resource or a method in a Java class are the two most common cases). The ConfigAttributes are also fairly generic, representing a decoration of the secure Object with some metadata that determines the level of permission required to access it. ConfigAttribute is an interface. It has only one method (which is quite generic and returns a String), so these strings encode in some way the intention of the owner of the resource, expressing rules about who is allowed to access it. A typical ConfigAttribute is the name of a user role (like ROLE_ADMIN or ROLE_AUDIT), and they often have special formats (like the ROLE_ prefix) or represent expressions that need to be evaluated.

Most people use the default AccessDecisionManager, which is AffirmativeBased (if any voters return affirmatively, access is granted). Any customization tends to happen in the voters, either by adding new ones or modifying the way that the existing ones work.

It is very common to use ConfigAttributes that are Spring Expression Language (SpEL) expressions — for example, isFullyAuthenticated() && hasRole('user'). This is supported by an AccessDecisionVoter that can handle the expressions and create a context for them. To extend the range of expressions that can be handled requires a custom implementation of SecurityExpressionRoot and sometimes also SecurityExpressionHandler.

Web Security

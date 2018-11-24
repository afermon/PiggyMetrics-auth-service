
# Piggy Metrics (Kubernetes) - Auth service

[![CircleCI](https://circleci.com/gh/afermon/PiggyMetrics-auth-service.svg?style=svg)](https://circleci.com/gh/afermon/PiggyMetrics-auth-service) [![GitHub license](https://img.shields.io/github/license/mashape/apistatus.svg)](https://github.com/afermon/PiggyMetrics-auth-service/blob/master/LICENCE)

Authorization responsibilities are completely extracted to separate server, which grants [OAuth2 tokens](https://tools.ietf.org/html/rfc6749) for the backend resource services. Auth Server is used for user authorization as well as for secure machine-to-machine communication inside a perimeter.

In this project, I use [`Password credentials`](https://tools.ietf.org/html/rfc6749#section-4.3) grant type for users authorization (since it's used only by native PiggyMetrics UI) and [`Client Credentials`](https://tools.ietf.org/html/rfc6749#section-4.4) grant for microservices authorization.

Spring Cloud Security provides convenient annotations and autoconfiguration to make this really easy to implement from both server and client side. You can learn more about it in [documentation](http://cloud.spring.io/spring-cloud-security/spring-cloud-security.html) and check configuration details in [Auth Server code](https://github.com/afermon/PiggyMetrics-auth-service/tree/master/src/main/java/com/piggymetrics/auth).

From the client side, everything works exactly the same as with traditional session-based authorization. You can retrieve `Principal` object from request, check user's roles and other stuff with expression-based access control and `@PreAuthorize` annotation.

Each client in PiggyMetrics (account-service, statistics-service, notification-service and browser) has a scope: `server` for backend services, and `ui` - for the browser. So we can also protect controllers from external access, for example:

``` java
@PreAuthorize("#oauth2.hasScope('server')")
@RequestMapping(value = "accounts/{name}", method = RequestMethod.GET)
public List<DataPoint> getStatisticsByAccountName(@PathVariable String name) {
	return statisticsService.findByAccountName(name);
}
```

For more information please refer to the main repository [afermon/PiggyMetrics-Kubernetes](https://github.com/afermon/PiggyMetrics-Kubernetes)

## Credits

* Forked from [sqshq/PiggyMetrics](https://github.com/sqshq/PiggyMetrics)
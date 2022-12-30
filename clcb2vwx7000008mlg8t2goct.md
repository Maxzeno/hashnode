# Mastering Middleware

## **What is Middleware?**

In Django, middleware is a lightweight plugin that processes requests and responses. It sits between the request and response, and can modify the request or response before it reaches the view or after the view processes the request. Middleware is useful for tasks such as authentication, rate limiting, and more.

## **How to Implement Custom Middleware in Django**

To create custom middleware in Django, you need to do the following steps:

1. Create a middleware class that defines the process\_request and process\_response methods.
    
2. Add the middleware class to MIDDLEWARE in your Django settings.
    

Let's go through each step in detail.

### **Step 1: Create a Middleware Class**

To create a middleware class, you need to define a class that has the following methods:

* `__init__`: This method is called when the middleware is initialized. You can use it to do any setup tasks, such as setting instance variables.
    
* `process_request(self, request)`: This method is called before the view processes the request. You can use it to modify the request or to perform tasks such as authentication.
    
* `process_response(self, request, response)`: This method is called after the view processes the request. You can use it to modify the response or to perform tasks such as setting cookies.
    

Here is an example of a simple middleware class that logs the IP address of the client making the request:

```python
class LogIPMiddleware:
    def __init__(self, get_response):
        self.get_response = get_response

    def __call__(self, request):
        # Code to be executed for each request before
        # the view (and later middleware) are called.

        response = self.get_response(request)

        # Code to be executed for each request/response after
        # the view is called.

        return response

    def process_request(self, request):
        ip_address = request.META.get('REMOTE_ADDR')
        print(f'Received request from IP address: {ip_address}')

```

Note that the middleware class has a `__call__` method, which is called for each request. This method calls the `get_response` function, which is responsible for calling the view and returning the response.

### **Step 2: Add the Middleware Class to MIDDLEWARE**

Once you have created your middleware class, you need to add it to MIDDLEWARE in your Django settings. MIDDLEWARE is a list of middleware classes that Django will use to process requests and responses.

To add your middleware class to MIDDLEWARE, open your Django settings file and add the full path to your middleware class to MIDDLEWARE. For example:

```python
MIDDLEWARE = [    ...    'myapp.middleware.LogIPMiddleware',]
```

## **Example: Implementing Authentication Middleware**

Now that you know how to create custom middleware in Django, let's look at an example of how to implement authentication middleware.

Suppose you want to require that all requests to your Django application are authenticated. You can do this by creating a middleware class that checks the request for a valid authentication token.

If the request does not have a valid authentication token, the middleware will return a 403 Forbidden response.

Here is what the authentication middleware class might look like:

```python
class AuthenticationMiddleware:
    def __init__(self, get_response):
        self.get_response = get_response

    def __call__(self, request):
        # Check for valid authentication token
        if not request.META.get('HTTP_AUTH_TOKEN'):
            return HttpResponseForbidden()

        response = self.get_response(request)
        return response

```

To use this middleware, you would add it to MIDDLEWARE in your Django settings:

```python
MIDDLEWARE = [    ...    'myapp.middleware.AuthenticationMiddleware',]
```

With this middleware in place, all requests to your Django application will be checked for a valid authentication token. If the request does not have a valid authentication token, the middleware will return a 403 Forbidden response.

## **Conclusion**

In this tutorial, you learned how to create custom middleware in Django. Middleware is a powerful tool that allows you to modify requests and responses and perform tasks such as authentication and rate limiting. With the steps outlined in this tutorial, you should be able to create custom middleware for your Django application.
# Servlet Request Parameters

## Learning Goals

- Define HTTP request parameters
- Define URL structure and query string
- Write a servlet to get a request parameter from the query string
- Write a servlet to generate a dynamic response based on request parameter values

## Code Along

## Introduction

We will continue working with the `GreetServlet` from the previous lesson.

So far the servlet behaves like a static web page because it always generates
the same HTML content as a response.  We are going to update the servlet to receive
data from the client request and generate a dynamic response using the data.


## Query String

A **query string** is a part of a URL that assigns values to specified parameters.
A query string is the part of a URL that follows a question mark `?`. For example:

- Given the URL: `http://localhost:8080/greet?name=Bola`.
- The query string is `name=Bola`, which consists of:
    - a parameter `name`.
    - a value `Bola`.

A query string may contain multiple parameters that will be separated
with an ampersand `&`.  For example: `http://localhost:8080/area?width=6&height=2`.
The parameters would be `width` and `height`, with values `6` and `2`.

## Request Parameters

The `HttpServletRequest` interface provides methods for accessing request information.
`HttpServletRequest` extends the `ServletRequst` interface, which provides a method
`getParameter(String parameter)` that returns the value
of a parameter from the query string.

We can update the `doGet()` method to retrieve the name from the query string using the `getParameter()` method:

```java
// Get the name parameter from the query string
String name = req.getParameter("name");
```

We then update the response to include the name:

```java
out.println(String.format("<h1>Greetings %s!</h1>", name));
```

Edit `GreetServlet` to get the `name` parameter from the query string, and generate
the response to include the name in the greeting:

```java
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import java.io.IOException;
import java.io.PrintWriter;

@WebServlet(urlPatterns = "/greet")
public class GreetServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp)
            throws ServletException, IOException {

        // Get the name parameter from the query string
        String name = req.getParameter("name");

        // Prepare the server response
        resp.setContentType("text/html; charset=UTF-8");
        PrintWriter out = resp.getWriter();
        out.println("<html>");
        out.println("<head><title>Servlet Example</title></head>");
        out.println("<body>");
        out.println(String.format("<h1>Greetings %s!</h1>", name));
        out.println("</body>");
        out.println("</html>");
    }

}
```

Save the class, then stop and restart the Tomcat9 plugin.

1. Stop executing the web application by pressing the red stop icon on the Maven toolbar.
2. Double-click on **tomcat9:run** to restart the web application.

Test the servlet by passing different names through the query string:

| URL                                   | Server Response |
|---------------------------------------|-----------------|
| http://localhost:8080/greet?name=Dani | Greetings Dani! |
| http://localhost:8080/greet?name=Nur  | Greetings Nur!  |


Congratulations, you just wrote a dynamic web application that
generates a unique response based on the client data passed in the request!

## Passing multiple parameters in the client request

We will create another servlet class named `AreaServlet` that will
take two parameters containing integer values for `width` and `height`.
The servlet will calculate the area based on the parameter values and
generate a response.

1. Create a new class in the `java` directory named `AreaServlet`.
2. Edit the class to extend `HttpServlet`, define the `@WebServlet` url pattern,
   and override the `doGet()` method as shown:

```java
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import java.io.IOException;
import java.io.PrintWriter;

@WebServlet(urlPatterns = "/area")
public class AreaServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp)
            throws ServletException, IOException {

        // Get the width parameter from the query string, convert to integer
        Integer width = Integer.parseInt(req.getParameter("width"));
        Integer height = Integer.parseInt(req.getParameter("height"));

        // Calculate the area using the request data
        int area = width * height;

        // Prepare the server response
        resp.setContentType("text/html; charset=UTF-8");
        PrintWriter out = resp.getWriter();
        out.println("<html>");
        out.println("<head><title>Servlet Example</title></head>");
        out.println("<body>");
        out.println(String.format("<h1>Area = %d</h1>", area));
        out.println("</body>");
        out.println("</html>");
    }

}
```

Note that the `getParameter` method returns a string.
Therefore, we need to use `Integer.parseInt()` to convert the string to an integer.

```java
// Get the width parameter from the query string, convert to integer
Integer width = Integer.parseInt(req.getParameter("width"));
Integer height = Integer.parseInt(req.getParameter("height"));
```

We will need to pass two parameters to the query string.  Multiple parameters
are separated with an ampersand `&`, for example `http://localhost:8080/area?width=5&height=2`.

Make sure you save the `AreaServlet` class, then stop and restart the Tomcat9 plugin.

1. Stop executing the web application by pressing the red stop icon on the Maven toolbar.
2. Double-click on **tomcat9:run** to restart the web application.

Test the servlet by passing different width and height values through the query string.
Note that you can pass the parameters in any order:

| URL                                         | Server Response |
|---------------------------------------------|-----------------|
| http://localhost:8080/area?width=5&height=2 | Area = 10       |
| http://localhost:8080/area?height=7&width=4 | Area = 28       |

## Conclusion

The `HttpServletRequest` provides methods for accessing parameters of a request.
The type of the request determines where the parameters come from. Typically, a `GET`
request takes the parameters from the query string.

- Given the URL: `http://localhost:8080/greet?name=Bola`.  
- The query string is the part after the question mark: `name=Bola`.  
- We can get the `name` parameter with the method call `req.getParameter("name")`.  

Our servlet can then generate a dynamic response to include the parameter value.

## Resources

- [ServletRequest](https://docs.oracle.com/javaee/7/api/javax/servlet/ServletRequest.html)     
- [HttpServletRequest](https://docs.oracle.com/javaee/7/api/javax/servlet/http/HttpServletRequest.html)    
- [What is a URL](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_is_a_URL)  
- [URL query string](https://en.wikipedia.org/wiki/Query_string)   
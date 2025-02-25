---
date   2015-04-26 09:00:00
---

When we're building a RESTful web service, the principles of REST tells us that a URL should represent a resource.

So ``/api/products/123`` will represent the product with an ID of 123.
If we follow that pattern, then ``/api/products/`` should represent *all* products.

That's all well and good, but what if there are thousands of products? We can't send all of them in the response, as that would really upset consumers on a slow connection. We're going to need to implement some sort of paging.

We have a number of options available to us, but my personal opinion is that we should use the query string.

# Why query string paging?

Using the query string for paging is easy for the consumer to test in a browser and easy to reason about.

My preference is to offer a URL like this:

``/api/products?offset=20&limit=40``.

This means 'starting from item 20, give me the next 40 items'. This allows the consumer to fine control over the paging of the resource to suit their needs.

I've seen some responses on Stack Overflow that discourage the query string being used *at all* when adhering to the principles of REST.

<a href="http://www.ietf.org/rfc/rfc3986.txt">RFC 3968</a> says:

>The query component contains non-hierarchical data that, along with data in the path component (Section 3.3), serves to identify a resource 

Which seems to indicate that we can use the query string to help represent the resource. I would argue that *offset* and *limit* information is 'non-hierarchical' data, and thus is permitted.

# Linking between pages

RESTful web services should provide 'dicoverability'. In other words, one resource should link to another so that the consumer can navigate the API using only these links.

When paging, the body of the response should provide links to the ``next``, ``previous``, ``first`` and ``last`` page of this resource (or as many of these as are relevant).

Each page should, **as with all resources in the API**, provide a link to the itself using ``self``.

Here is an example (this uses the <a href="http://stateless.co/hal_specification.html">HAL</a> schema for JSON, others are available):

```csharp
{
    "_links": {
        "self":  { "href": "/api/products?offset=40&limit=40"  },
        "first": { "href": "/api/products?offset=0&limit=40"   },
        "last":  { "href": "/api/products?offset=120&limit=40" },
        "next":  { "href": "/api/products?offset=80&limit=40"  },
        "prev":  { "href": "/api/products?offset=0&limit=40"   }
    }
}
```

Because we're using the query string, it's really easy to provide links to the next page. It's also very easy for the consumer of our API to understand and modify for their needs.

# Linking to the resource from another resource

We need to be able to link to this resource from other pages, for example:
```csharp
{
    "_links": {
    	"self":      { "href": "/api/" },
        "products":  { "href": "/api/products" }
    }
}
```

So how do we tell the consumer about paging?

One option is that we just don't mention it here at all and just put it in the documentation.

When the consumer navigates to the ``/api/products`` URL, the ``self`` link in the response will indicate that they are on the first page, and allow them to navigate from there.

Unfortunately, this isn't helpful until you actually *get* to the ``products`` resource.

Another option is to link the the first page:
```csharp
{
    "_links": {
    	"self":      { "href": "/api/" },
        "products":  { "href": "/api/products?offset=0&limit=40" }
    }
}
```
The problem with this is that if we add more query string parameters (for example, searching or ordering) then the URL grows and becomes even harder to read.

Because we are constructing a real URL, we would also have to provide default values for each query string parameter. What should the default value of a ``searchTerm`` parameter be, for example?

The URI Templating Spec (<a href="http://tools.ietf.org/html/rfc6570">RFC6570</a>) helps us with this, allowing us to provide a template that describes the query string parameters.

For our ``products`` resource, it would look like this:

```csharp
{
    "_links": {
    	"self":      { "href": "/api/" },
        "products":  { "href": "/api/products{?offset,limit}" }
    }
}
```
I find this to be much clearer.

# Total count

Finally, it's also nice to include the *total* amount of ``products`` availble.

The most common way to do this is with a custom HTTP response header (for example ``X-Total-Count``).

There's nothing to say you *have* to do this, but it's good practice to do so and the consumers of your API will thank you for it.


# Further reading

If you want to read more about the things mentioned in this article, check out these links:

<ul>
	<li>Chapter 5 from Roy Fielding's Dissertation: <a href="Representational State Transfer (REST)">Representational State Transfer (REST)</a>
	<li>Uniform Resource Identifier (URI): Generic Syntax : <a href="http://www.ietf.org/rfc/rfc3986.txt">(RFC 3968)</a></li>
	<li>Hypertext Application Language <a href="http://stateless.co/hal_specification.html">(HAL)</a></li>
	<li>The URI Templating Spec <a href="http://tools.ietf.org/html/rfc6570">(RFC6570)</a></li>
</ul>



# REST API

A REST API is a particular kind of [[API]] served over [[HTTP]]. REST stands for REpresentational State Transfer. A REST API is not necessarily _just_ an API served over HTTP, however. Typically, it is a kind of API that conforms to specific standards.


## Resources

At the heart of a REST API is resources. Resources can be thought of as objects or types. They are the primary means through which one interacts with a REST API.

A resource is a thing. A blog resource might be represented with the URL path:

```nil
/blog
```

(the domain is omitted)


## Verbs

HTTP verbs are a crucial part of a REST API. There are six HTTP verbs:

-   `GET` - retrieves a resource. Idempotent
-   `POST` - creates a resource. Side effects expected
-   `PUT` - updates a resource. Idempotent
-   `PATCH` - updates a resource. Idempotent
-   `DELETE` - deletes a resource. Side effects expected
-   `OPTIONS` - lists actions allowed on a resource. Idempotent

Examples:

```text
GET    /blog/1 - retrieves a blog
GET    /blog   - retrieves a list of blogs
PUT    /blog/1 - with some body, updates blog 1
DELETE /blog/1 - deletes blog 1
POST   /blog   - creates a new blog
```


## Relationships

```text
GET  /blog/1/posts   - retrieves a list of posts
POST /blog/1/posts   - creates a new post for blog 1
PUT  /blog/1/posts/2 - deletes a post.
```


## Links

The next piece of a REST API are links. Links describe relationships between objects.

When a resource is retrieved via a GET, it should return a body of links describing what options are possible with that resource.

```json
{
  "type": "blog",
  "id": 1,
  "name": "first blog",
  "links": {
    "self": "/blog/1",
    "posts": "/blog/1/posts"
  }
}
```


## Metadata

Finally, metadata describes the type information of a resource.

```json
{
  "type": "blog",
  "id": 1,
  "attributes": {
    "name": "foo blog"
  },
  "meta": {
    "type": "blog",
    "attributes": {
      "name": {
        "type": "string",
        "maxLength": 20
      }
    }
  }
}
```

For any attribute specified on the resource, the meta must describe it.

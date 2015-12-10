Auth Predicates
===============

This library offers a small "DSL" to articulate security conditions 
in [express.js](http://expressjs.com) routes.

It allows to express security conditions in routes as functions
returning boolean values (is "allowed" vs. is "not allowed"). This very
easy interface allows trivial composition of security conditions
by chaining (for conjunction)

    app.get('/helloworld', assure(cond1), assure(cond2), f)

the route will proceed to function `f` when both conditions `cond1` and 
`cond2` succeed.

For disjunction (multiple alternatives that may allow access) the combinator `either` can be used:

    app.get('/helloworld', assure.either(cond1, cond2), f)

Here the route will proceed to function `f` if either `cond1` or
`cond2` suceed. It suffices that one of the functions returns
`true`.

By combining piping and `either`, arbitrary conditions can be 
expressed in conjunctive normal form.


How does a predicate look like?
-------------------------------
Like route callback functions in express.js predicates take a
`req` and a `res` argument, but should return a boolean value.

For example:

    function cond1(req, res) {
      return req.headers.user === "admin";
    }


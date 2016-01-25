### Request Hooks
> A common pattern to share data between request hook functions and view functions is to use the g context global. For example, a before_request handler can load the logged- in user from the database and store it in g.user. Later, when the view function is invoked, it can access the user from there./

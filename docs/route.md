# Route Object & Route Matching

Vue-router supports matching paths that contain dynamic segments, star segments and query strings. All these information of a parsed route will be accessible on the exposed **Route Context Objects** (we will just call them "route" objects from now on). The route object will be injected into every component in a vue-router-enabled app as `this.$route`, and will be updated whenever a route transition is performed.

A route object exposes the following properties:

- **$route.path**

  A string that equals the path of the current route, always resolved as an absolute path. e.g. `"/foo/bar"`.

- **$route.params**

  An object that contains key/value pairs of dynamic segments and star segments. More details below.

- **$route.query**

  An object that contains key/value pairs of the query string. For example, for a path `/foo?user=1`, we get `$route.query.user == 1`.

### Using in Templates

You can directly bind to the `$route` object inside your component templates. For example:

``` html
<div>
  <p>Current route path: {{$route.path}}</p>
  <p>Current route params: {{$route.params | json}}</p>
</div>
```

### Dynamic Segments

Dynamic segments can be defined in the form of path segments with a leading colon, e.g. in `user/:username`, `:username` is the dynamic segment. It will match paths like `/user/foo` or `/user/bar`. When a path containing a dynamic segment is matched, the dynamic segments will be available inside `$route.params`.

Example Usage:

``` js
router.map({
  '/user/:username': {
    component: {
      template: '<p>username is {{$route.params.username}}</p>'
    }
  }
})
```

A path can contain multiple dynamic segments, and each of them will be stored as a key/value pair in `$route.params`.

Examples:

| pattern | matched path | $route.params |
|---------|------|--------|
| /user/:username | /user/evan | `{ username: 'evan' }` |
| /user/:username/post/:post_id | /user/evan/post/123 | `{ username: 'evan', post_id: 123 }` |

### Star Segments

While dynamic segments can correspond to only a single segment in a path, star segments is basically the "greedy" version of it. For example `/foo/*bar` will match anything that starts with `/foo/`. The part matched by the star segment will also be available in `$route.params`.

Examples:

| pattern | matched path | $route.params |
|---------|------|--------|
| /user/*any | /user/a/b/c | `{ any: 'a/b/c' }` |
| /foo/*any/bar | /foo/a/b/bar | `{ any: 'a/b' }` |
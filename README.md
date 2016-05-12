# Shallow Nested Model
Use shallow nested models to allow Backbone to work with Rails shallow nested routes.

A shallow nested model created through a collection will only be posted to its own `url` if the collection's `url` does not exist.  A preexisting model will use its own `url`, which is sufficient to uniquely identify it.

## Example

```ruby
resources :users, shallow: true do
  resources :posts
end
```

```bash
$ rake routes
  user_posts POST   /users/:user_id/posts(.:format) posts#create
        post GET    /posts/:id(.:format)            posts#show
             DELETE /posts/:id(.:format)            posts#destroy
```

```javascript
Post = Backbone.Model.extend({
  urlRoot: '/posts',
});

User = Backbone.Model.extend({
  urlRoot: '/users',
  initialize: function () {
    this.posts = new UserPosts(this);
  },
});

UserPosts = Backbone.Collection.extend({
  model: Post,
  url: '/posts',
});

var user = new User();
var post = user.posts.create({...});
post.fetch();
```

The new post will be posted to `/users/:user_id/posts`, but will be fetched from `/posts/:id`.

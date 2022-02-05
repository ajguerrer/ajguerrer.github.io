+++
title = "Fake Your Way To Better Tests"
date = 2013-06-28
weight = 26
[taxonomies]
categories = ["Testing on the Toilet"]
tags = ["testing", "google", "tott"]
[extra]
+++

Suppose you would like to test your blog platform API, but you don't want your tests talking to a
remote server.

``` cpp
void DeletePostsWithTag(const Tag& tag) {
  for (const Post post : blog_service_->GetAllPosts()) {
    if (post.HasTag(tag)) {
      blog_service_->DeletePost(post.GetId());
    }
  }
}
```

A fake is a lightweight implementation of an API that behaves like the real implementation, but
isn't suitable for production.

```cpp
class FakeBlogService : public BlogService {
 public:
  void AddPost(const Post& post) { posts.insert(post); }
  void DeletePost(const int id) {
    for (auto& post : posts) {
      if (post.GetId() == id) { posts.erase(post); return; }
    }
  }
  std::set<Post> GetAllPosts() const { return posts; }

 private:
  std::set<Post> posts;
};
```

Fakes should be created and maintained by the person or team that owns the real implementation.

Fakes should have their own tests to make sure they behave like the real implementation.
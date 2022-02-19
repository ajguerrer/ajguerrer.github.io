+++
title = "Avoid Hardcoding Values for Better Libraries"
date = 2020-08-19
[taxonomies]
categories = ["Testing on the Toilet"]
tags = ["testing", "google", "tott"]
[extra]
+++

Naming constants is good practice, but they are still hardcoded values that can make library code
less reusable and less predictable.

{% bad(header="Bad") %}
```cpp
constexpr int kThumbnailSizes[] = {480, 576, 720};

// Returns thumbnails of various sizes for the given image.
std::vector<Image> GetThumbnails(const Image& image) {
  std::vector<Image> thumbnails;
  for (const int size : kThumbnailSizes) {
    thumbnails.push_back(ResizeImage(image, size));
  }
  return thumbnails;
}
```
{% end %}

Instead, let the caller decide.

{% good(header="Good") %}
```cpp
// Returns thumbnails of various sizes for the given image.
std::vector<Image> GetThumbnails(const Image& image,
                                 absl::Span<const int> sizes) {
  std::vector<Image> thumbnails;
  for (const int size : sizes) {
    thumbnails.push_back(ResizeImage(image, size));
  }
  return thumbnails;
}
```

```cpp
// Declared in the public header.
inline constexpr int kDefaultThumbnailSizes[] = {480, 576, 720};

// Default argument allows the function to be used without specifying a size.
std::vector<Image> GetThumbnails(
    const Image& image, absl::Span<const int> sizes = kDefaultThumbnailSizes);
```
{% end %}
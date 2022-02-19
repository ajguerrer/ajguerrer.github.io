+++
title = "Separation of Concerns? That's a Wrap!"
date = 2020-12-09
[taxonomies]
categories = ["Testing on the Toilet"]
tags = ["testing", "google", "tott"]
[extra]
+++

Combining domain logic with third party API calls makes code that is harder to understand especially
if the API details creep outside of the call site.

{% bad(header="Bad") %}
```cpp
absl::StatusOr<speedy_img::Image> GetThumbnails(
    absl::Span<const speedy_img::Decoder> decoders,
    absl::Span<const std::byte> data) {
  const speedy_img::Options options = GetDefaultConvertOptions();

  for (const speedy_img::Decoder decoder : decoders) {
    const speedy_img::Result decode_result =
        decoder.Decode(decoder.FormatBytes(data));
    speedy_img::Image image = decode_result.GetImage(options);
    
    absl::Status result = ValidateGoodImage(image);
    if (result.ok()) {
      return absl::StatusOr<speedy_img::Image>(std::move(image));
    }
  }

  return absl::InvalidArgumentError("unable to decode image with any decoder");
}
```
{% end %}

Encapsulate the external API in a wrapper with a readable interface that insulates the API from the
rest of the codebase. 

{% good(header="Good") %}
```cpp
absl::StatusOr<Image> GetThumbnails(absl::Span<const ImageDecoder> decoders,
                                    absl::Span<const std::byte> data) {
  for (const ImageDecoder decoder : decoders) {
    absl::StatusOr<Image> result = decoder.Decode(data);
    if (result.ok()) {
      return result;
    }
  }

  return absl::InvalidArgumentError("unable to decode image with any decoder");
}
```
{% end %}

{% info(header="Note") %}
Not all external APIs need to be wrapped. For example, the `absl` library provides fundamental
library types; wrapping the API would not make a clear improvement to the code.
{% end %}
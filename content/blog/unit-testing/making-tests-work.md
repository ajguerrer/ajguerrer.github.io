+++
title = "Making your Tests Work for You"
weight = 2
date = 2020-03-01
[taxonomies]
categories = ["Unit Testing Principles, Practices, and Patterns"]
tags = ["testing", "unit-testing"]
[extra]
+++

## Four Pillars of a Good Test

1. **Protection against regressions** - Ability to indicate the presence of regressions.
1. **Resistance to refactoring** - Degree to which a test can sustain refactoring without producing
  a false positive.
1. **Fast feedback** - Measure of how quickly the test executes.
1. **Maintainability** - Ability to read and run the test.

| Error Types     | Functionality is Correct                                  | Functionality is Broken                                        |
| --------------- | --------------------------------------------------------- | -------------------------------------------------------------- |
| **Test Passes** | True negative                                             | False negative (indicates poor protection against regressions) |
| **Test Fails**  | False positive (indicates poor resistance to refactoring) | True positive                                                  |

$$Test\ accuracy = {Signal \over Noise} = {True\ positives \over False\ positives}$$

## The Ideal Test

- *Protection Against Regressions*
- *Resistance to Refactoring*
- *Fast Feedback*
 
Choose two. 

<div class="svg-content">
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
<svg xmlns="http://www.w3.org/2000/svg"
   xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1" width="811px" height="791px" viewBox="-0.5 -0.5 811 791">
   <g>
      <ellipse cx="410.5" cy="540.5" rx="250" ry="250" fill-opacity="0.5" class="yellow-circle" stroke="none" pointer-events="all"/>
      <ellipse cx="250.5" cy="250.5" rx="250" ry="250" fill-opacity="0.5" class="blue-circle" stroke="none" pointer-events="all"/>
      <ellipse cx="560.5" cy="250.5" rx="250" ry="250" fill-opacity="0.5" class="red-circle" stroke="none" pointer-events="all"/>
      <foreignObject pointer-events="none" width="100%" height="100%" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility" style="overflow: visible; text-align: left;">
         <div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 258px; height: 1px; padding-top: 221px; margin-left: 42px;">
            <div style="box-sizing: border-box; font-size: 0px; text-align: center;">
               <div style="display: inline-block; font-size: 24px; line-height: 1.2; pointer-events: all; white-space: normal; overflow-wrap: normal;">
                  <font style="font-size: 42px">Protection Against Regressions</font>
               </div>
            </div>
         </div>
      </foreignObject>
      <rect x="541" y="132" width="210" height="178.5" fill="none" stroke="none" pointer-events="all"/>
      <foreignObject pointer-events="none" width="100%" height="100%" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility" style="overflow: visible; text-align: left;">
         <div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 208px; height: 1px; padding-top: 221px; margin-left: 542px;">
            <div style="box-sizing: border-box; font-size: 0px; text-align: center;">
               <div style="display: inline-block; font-size: 42px; line-height: 1.2; pointer-events: all; white-space: normal; overflow-wrap: normal;">Fast Feedback</div>
            </div>
         </div>
      </foreignObject>
      <rect x="275.25" y="532" width="270.5" height="161.5" fill="none" stroke="none" pointer-events="all"/>
      <foreignObject pointer-events="none" width="100%" height="100%" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility" style="overflow: visible; text-align: left;">
         <div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 269px; height: 1px; padding-top: 613px; margin-left: 276px;">
            <div style="box-sizing: border-box; font-size: 0px; text-align: center;">
               <div style="display: inline-block; font-size: 42px; line-height: 1.2; pointer-events: all; white-space: normal; overflow-wrap: normal;">Resistance to Refactoring</div>
            </div>
         </div>
      </foreignObject>
      <rect x="355.25" y="180.5" width="110.5" height="81.5" fill="none" stroke="none" pointer-events="all"/>
      <foreignObject pointer-events="none" width="100%" height="100%" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility" style="overflow: visible; text-align: left;">
         <div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe flex-start; justify-content: unsafe center; width: 109px; height: 1px; padding-top: 188px; margin-left: 356px;">
            <div style="box-sizing: border-box; font-size: 0px; text-align: center;">
               <div style="display: inline-block; font-size: 28px; line-height: 1.2; pointer-events: all; white-space: normal; overflow-wrap: normal;">Brittle Tests</div>
            </div>
         </div>
      </foreignObject>
      <rect x="191" y="412" width="170" height="50" fill="none" stroke="none" pointer-events="all"/>
      <foreignObject pointer-events="none" width="100%" height="100%" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility" style="overflow: visible; text-align: left;">
         <div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe flex-start; justify-content: unsafe center; width: 168px; height: 1px; padding-top: 419px; margin-left: 192px;">
            <div style="box-sizing: border-box; font-size: 0px; text-align: center;">
               <div style="display: inline-block; font-size: 28px; line-height: 1.2; pointer-events: all; white-space: normal; overflow-wrap: normal;">Large Tests</div>
            </div>
         </div>
      </foreignObject>
      <rect x="441" y="412" width="180" height="50" fill="none" stroke="none" pointer-events="all"/>
      <foreignObject pointer-events="none" width="100%" height="100%" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility" style="overflow: visible; text-align: left;">
         <div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe flex-start; justify-content: unsafe center; width: 178px; height: 1px; padding-top: 419px; margin-left: 442px;">
            <div style="box-sizing: border-box; font-size: 0px; text-align: center;">
               <div style="display: inline-block; font-size: 28px; line-height: 1.2; pointer-events: all; white-space: normal; overflow-wrap: normal;">Trivial Tests</div>
            </div>
         </div>
      </foreignObject>
      <rect x="360.25" y="310.5" width="100.5" height="70" fill="none" stroke="none" pointer-events="all"/>
      <foreignObject pointer-events="none" width="100%" height="100%" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility" style="overflow: visible; text-align: left;">
         <div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe flex-start; justify-content: unsafe center; width: 1px; height: 1px; padding-top: 318px; margin-left: 411px;">
            <div style="box-sizing: border-box; font-size: 0px; text-align: center;">
               <div style="display: inline-block; font-size: 42px; line-height: 1.2; pointer-events: all; white-space: nowrap;">
                  <span style="font-size: 28px">N/A</span>
               </div>
            </div>
         </div>
      </foreignObject>
   </g>
   <switch>
      <g requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility"/>
      <a transform="translate(0,-5)" xlink:href="https://www.diagrams.net/doc/faq/svg-export-text-problems" target="_blank">
         <text text-anchor="middle" font-size="10px" x="50%" y="100%">Text is not SVG - cannot display</text>
      </a>
   </switch>
</svg>
</div>

Always chose to maximize *resistance to refactoring*. Then, test size becomes a slider between 
*protection against regressions* and *fast feedback*.

<div class="svg-content">
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
<svg xmlns="http://www.w3.org/2000/svg"
  xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1" width="801px" height="491px" viewBox="-0.5 -0.5 801 491">
  <g>
    <ellipse cx="400" cy="150" rx="50" ry="50" class="outline-text" stroke-width="3" pointer-events="all"/>
    <ellipse cx="530" cy="310" rx="50" ry="50" class="outline-text" stroke-width="3" pointer-events="all"/>
    <ellipse cx="270" cy="310" rx="50" ry="50" class="outline-text" stroke-width="3" pointer-events="all"/>
    <path d="M 494.64 274.64 L 435.36 185.36" class="outline-text" stroke-width="3" stroke-miterlimit="10" pointer-events="stroke"/>
    <path d="M 480 310 L 320 310" class="outline-text" stroke-width="3" stroke-miterlimit="10" pointer-events="stroke"/>
    <path d="M 305.36 274.64 L 364.64 185.36" class="outline-text" stroke-width="3" stroke-miterlimit="10" pointer-events="stroke"/>
    <rect x="265" y="370" width="10" height="60" class="solid-text" stroke-width="3" pointer-events="all"/>
    <rect x="395" y="370" width="10" height="60" class="solid-text" stroke-width="3" pointer-events="all"/>
    <rect x="525" y="370" width="10" height="60" class="solid-text" stroke-width="3" pointer-events="all"/>
    <path d="M 222.5 406 L 222.5 417.5 L 201.5 400 L 222.5 382.5 L 222.5 394 L 577.5 394 L 577.5 382.5 L 598.5 400 L 577.5 417.5 L 577.5 406 Z" class="outline-text" stroke-width="3" stroke-linejoin="round" stroke-miterlimit="10" pointer-events="all"/>
    <foreignObject pointer-events="none" width="100%" height="100%" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility" style="overflow: visible; text-align: left;">
      <div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 158px; height: 1px; padding-top: 50px; margin-left: 321px;">
        <div style="box-sizing: border-box; font-size: 0px; text-align: center;">
          <div style="display: inline-block; font-size: 28px; line-height: 1.2; pointer-events: all; white-space: normal; overflow-wrap: normal;">Resistance to Refactoring</div>
        </div>
      </div>
    </foreignObject>
    <rect x="0" y="260" width="220" height="80" fill="none" stroke="none" pointer-events="all"/>
    <foreignObject pointer-events="none" width="100%" height="100%" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility" style="overflow: visible; text-align: left;">
      <div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 218px; height: 1px; padding-top: 300px; margin-left: 1px;">
        <div style="box-sizing: border-box; font-size: 0px; text-align: center;">
          <div style="display: inline-block; font-size: 28px; line-height: 1.2; pointer-events: all; white-space: normal; overflow-wrap: normal;">
            <font style="font-size: 28px">Protection Against Regressions</font>
          </div>
        </div>
      </div>
    </foreignObject>
    <foreignObject pointer-events="none" width="100%" height="100%" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility" style="overflow: visible; text-align: left;">
      <div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 118px; height: 1px; padding-top: 310px; margin-left: 601px;">
        <div style="box-sizing: border-box; font-size: 0px; text-align: center;">
          <div style="display: inline-block; font-size: 28px; line-height: 1.2; pointer-events: all; white-space: normal; overflow-wrap: normal;">Fast Feedback</div>
        </div>
      </div>
    </foreignObject>
    <foreignObject pointer-events="none" width="100%" height="100%" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility" style="overflow: visible; text-align: left;">
      <div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 143px; height: 1px; padding-top: 455px; margin-left: 329px;">
        <div style="box-sizing: border-box; font-size: 0px; text-align: center;">
          <div style="display: inline-block; font-size: 28px; line-height: 1.2; pointer-events: all; white-space: normal; overflow-wrap: normal;">
            <font style="font-size: 28px">Medium</font>
          </div>
        </div>
      </div>
    </foreignObject>
    <foreignObject pointer-events="none" width="100%" height="100%" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility" style="overflow: visible; text-align: left;">
      <div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 83px; height: 1px; padding-top: 455px; margin-left: 229px;">
        <div style="box-sizing: border-box; font-size: 0px; text-align: center;">
          <div style="display: inline-block; font-size: 28px; line-height: 1.2; pointer-events: all; white-space: normal; overflow-wrap: normal;">
            <font style="font-size: 28px">Large</font>
          </div>
        </div>
      </div>
    </foreignObject>
    <rect x="487.5" y="440" width="85" height="30" fill="none" stroke="none" pointer-events="all"/>
    <foreignObject pointer-events="none" width="100%" height="100%" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility" style="overflow: visible; text-align: left;">
      <div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 83px; height: 1px; padding-top: 455px; margin-left: 489px;">
        <div style="box-sizing: border-box; font-size: 0px; text-align: center;">
          <div style="display: inline-block; font-size: 28px; line-height: 1.2; pointer-events: all; white-space: normal; overflow-wrap: normal;">Small</div>
        </div>
      </div>
    </foreignObject>
    <foreignObject pointer-events="none" width="100%" height="100%" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility" style="overflow: visible; text-align: left;">
      <div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 58px; height: 1px; padding-top: 150px; margin-left: 371px;">
        <div style="box-sizing: border-box; font-size: 0px; text-align: center;">
          <div style="display: inline-block; font-size: 28px; line-height: 1.2; pointer-events: all; white-space: normal; overflow-wrap: normal;">Max</div>
        </div>
      </div>
    </foreignObject>
    <switch>
      <g requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility"/>
      <a transform="translate(0,-5)" xlink:href="https://www.diagrams.net/doc/faq/svg-export-text-problems" target="_blank">
        <text text-anchor="middle" font-size="10px" x="50%" y="100%">Text is not SVG - cannot display</text>
      </a>
    </switch>
  </g>
</svg>
</div>

A diverse ratio of test sizes provides an ideal amount of both *protection against regressions* and 
*fast feedback*. For any non-trivial production system, test counts should form a pyramid where the
test count shrinks as test size grows.

<div class="svg-content">
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
<svg xmlns="http://www.w3.org/2000/svg"
  xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1" width="443px" height="363px" viewBox="-0.5 -0.5 443 363">
  <g>
    <path d="M 41 -39 L 401 181 L 41 401 Z" class="outline-text" stroke-width="3" stroke-miterlimit="10" transform="rotate(-90,221,181)" pointer-events="all"/>
    <path d="M 367.96 244 L 74.92 242.92" class="outline-text" stroke-width="3" stroke-miterlimit="10" pointer-events="stroke"/>
    <path d="M 146.2 124.12 L 297.12 123.4" class="outline-text" stroke-width="3" stroke-miterlimit="10" pointer-events="stroke"/>
    <foreignObject pointer-events="none" width="100%" height="100%" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility" style="overflow: visible; text-align: left;">
      <div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 118px; height: 1px; padding-top: 306px; margin-left: 162px;">
        <div style="box-sizing: border-box; font-size: 0px; text-align: center;">
          <div style="display: inline-block; font-size: 28px; line-height: 1.2; pointer-events: all; white-space: normal; overflow-wrap: normal;">Small Tests</div>
        </div>
      </div>
    </foreignObject>
    <foreignObject pointer-events="none" width="100%" height="100%" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility" style="overflow: visible; text-align: left;">
      <div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 128px; height: 1px; padding-top: 181px; margin-left: 157px;">
        <div style="box-sizing: border-box; font-size: 0px; text-align: center;">
          <div style="display: inline-block; font-size: 28px; line-height: 1.2; pointer-events: all; white-space: normal; overflow-wrap: normal;">Medium Tests</div>
        </div>
      </div>
    </foreignObject>
    <foreignObject pointer-events="none" width="100%" height="100%" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility" style="overflow: visible; text-align: left;">
      <div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 73px; height: 1px; padding-top: 81px; margin-left: 185px;">
        <div style="box-sizing: border-box; font-size: 0px; text-align: center;">
          <div style="display: inline-block; font-size: 28px; line-height: 1.2; pointer-events: all; white-space: normal; overflow-wrap: normal;">Large Tests</div>
        </div>
      </div>
    </foreignObject>
  </g>
  <switch>
    <g requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility"/>
    <a transform="translate(0,-5)" xlink:href="https://www.diagrams.net/doc/faq/svg-export-text-problems" target="_blank">
      <text text-anchor="middle" font-size="10px" x="50%" y="100%">Text is not SVG - cannot display</text>
    </a>
  </switch>
</svg>
</div>

## Preventing False Positives

When tests fail for the wrong reasons, developers quickly loose trust in their tests. However, 
without trustworthy tests, refactoring is risky. This trap leads to real bugs slipping through.

Fast positives are formed from coupling between the test and implementation details:

{% bad(header="Bad") %}
```cpp
TEST(Render, IsCorrect) {
  std::ifstream in("message_renderer.cc");
  std::stringstream sstr;
  sstr << in.rdbuf();

  EXPECT_EQ(sstr.str(), R"(#include "message_renderer.h"
#include "body_renderer.h"
#include "title_renderer.h"
#include "footer_renderer.h"

#include <memory>
#include <numeric>
#include <string>

MessageRenderer::MessageRenderer()
    : sub_renderers_({std::make_unique<TitleRenderer>(),
                      std::make_unique<BodyRenderer>(),
                      std::make_unique<FooterRenderer>()}) {}

std::string MessageRenderer::Render(const Message& message) {
  return std::accumulate(sub_renderers_.begin(), sub_renderers_.end(),
                         std::string{},
                         [&](std::string m, std::shared_ptr<Renderer> r) {
                           return std::move(m) + r->Render(message);
                         });
})");
}
```
{% end %}

Instead of testing the code, test the behavior.

{% good(header="Good") %}
```cpp
TEST(Render, BasicMessage) {
  MessageRenderer renderer;
  const Message message{title : "a", body : "b", footer : "c"};

  std::string html = renderer.Render(message);

  EXPECT_EQ(html,
            "<head><title>a</title></head><body>b</body><footer>c</footer>");
}
```
{% end %}

## Types of Test Doubles

- **Mock** *(mock, spy)* - Emulate and verify *outgoing* interactions from the SUT.
- **Stub** *(stub, dummy, fake)* - Emulate *incoming* interactions to the SUT.

{% info(header="Info") %}
The term *mock* can also refer to the mocking framework itself. Mocking frameworks are also 
generally responsible for creating stubs which can be confusing.
{% end %}

Mocks can emulate interactions like stubs, but stubs should never assert interactions like mocks. 
Asserting interactions with stubs is *over-specification*, a common anti-pattern.

## Encapsulation

A well-designed API hides all implementation details behind a private API, leaving only observable 
behavior in the public API. Implementation details should never *leak* into the public API.

#### Invariant violation

API's that require several steps to achieve an individual goal are prone to *invariant violation*.

{% bad(header="Bad") %}
```cpp
class User {
 public:
  void SetName(const std::string& name) { name_ = name; }

  std::string NormalizeName(const std::string& name) {
    return std::string(absl::StripAsciiWhitespace(name)).substr(50);
  }

 private:
  std::string name_;
};

// Client must remember to `NormalizeName` before `SetName`.
std::string normalized_name = user.NormalizeName(new_name);
user.SetName(normalized_name);
```
{% end %}

A client should be able to achieve any individual goal atomically.

{% good(header="Good") %}
```cpp
class User {
 public:
  void SetName(const std::string& name) { name_ = NormalizeName(name); }

 private:
  std::string NormalizeName(const std::string& name) {
    return std::string(absl::StripAsciiWhitespace(name)).substr(50);
  }

  std::string name_;
};

// Client no longer needs to be concerned about `NormalizeName`.
user.SetName(new_name);
```
{% end %}

#### Leaking state

Public access to state that isn't directly related to a client's goal is considered *leaking state*. 

{% bad(header="Bad") %}
```cpp
class MessageRenderer : public Renderer {
 public:
  std::vector<std::shared_ptr<Renderer>> GetSubRenderers() {
    return sub_renderers_;
  }
  virtual std::string Render(const Message& message);

 private:
  std::vector<std::shared_ptr<Renderer>> sub_renderers_;
};
```
{% end %}

Keep the public API surface as small as possible while still meeting the clients needs.

{% good(header="Good") %}
```cpp
class MessageRenderer : public Renderer {
 public:
  virtual std::string Render(const Message& message);

 private:
  std::vector<std::unique_ptr<Renderer>> sub_renderers_;
};
```
{% end %}

## Styles of Unit Testing

- **Output-based** - Verify output with a given input. Requires 
  [pure functions](#functional-architecture).
- **State-based** - Perform operation, then verify state of SUT and collaborators.
- **Communication-based** - Perform operation, then verify communication with collaborators.

| Style                         | Output-based | State-based | Communication-based |
| ----------------------------- | ------------ | ----------- | ------------------- |
| **Resistance to refactoring** | High         | Medium      | Medium              |
| **Maintainability**           | High         | Medium      | Low                 |

## Hexagonal Architecture

*Hexagonal architecture*, proposed by Alistair Cockburn, breaks applications into two layers:

- **Domain layer** - Logic and models essential to the business domain.
- **Controller layer** - All other responsibilities of the application.

Interaction between the Domain layer and the Controller layer follows three guidelines: 

1. The Domain layer is isolated from the Controller layer.
1. The Domain layer may not depend on the Controller layer, but the Controller layer may 
   depend on the Domain layer.
1. Communication with external applications is handled by the Controller layer, not the 
   Domain layer.

<div class="svg-content">
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
<svg xmlns="http://www.w3.org/2000/svg"
  xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1" width="676px" height="453px" viewBox="-0.5 -0.5 676 453">
  <g>
    <path d="M 101 91 L 301 91 L 401 271 L 301 451 L 101 451 L 1 271 Z" class="outline-text" stroke-width="3" stroke-miterlimit="10" pointer-events="all"/>
    <path d="M 369.81 194.05 L 455.43 145.95" class="outline-text" stroke-width="3" stroke-miterlimit="10" pointer-events="stroke"/>
    <path d="M 363.92 197.36 L 369.57 189.03 L 369.81 194.05 L 373.97 196.87 Z" class="solid-text" stroke-width="3" stroke-miterlimit="10" pointer-events="all"/>
    <path d="M 461.32 142.64 L 455.68 150.97 L 455.43 145.95 L 451.27 143.13 Z" class="solid-text" stroke-width="3" stroke-miterlimit="10" pointer-events="all"/>
    <path d="M 499.11 1 L 615.32 1 L 673.42 101 L 615.32 201 L 499.11 201 L 441 101 Z" class="outline-text" stroke-width="3" stroke-miterlimit="10" pointer-events="all"/>
    <ellipse cx="201.01" cy="271" rx="83.75" ry="83.75" class="outline-text" stroke-width="3" pointer-events="all"/>
    <foreignObject pointer-events="none" width="100%" height="100%" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility" style="overflow: visible; text-align: left;">
      <div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 122px; height: 1px; padding-top: 271px; margin-left: 140px;">
        <div style="box-sizing: border-box; font-size: 0px; text-align: center;">
          <div style="display: inline-block; font-size: 28px; line-height: 1.2; pointer-events: all; white-space: normal; overflow-wrap: normal;">Domain Layer</div>
        </div>
      </div>
    </foreignObject>
    <foreignObject pointer-events="none" width="100%" height="100%" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility" style="overflow: visible; text-align: left;">
      <div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 152px; height: 1px; padding-top: 136px; margin-left: 125px;">
        <div style="box-sizing: border-box; font-size: 0px; text-align: center;">
          <div style="display: inline-block; font-size: 28px; line-height: 1.2; pointer-events: all; white-space: normal; overflow-wrap: normal;">Controller Layer</div>
        </div>
      </div>
    </foreignObject>
    <foreignObject pointer-events="none" width="100%" height="100%" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility" style="overflow: visible; text-align: left;">
      <div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 181px; height: 1px; padding-top: 101px; margin-left: 467px;">
        <div style="box-sizing: border-box; font-size: 0px; text-align: center;">
          <div style="display: inline-block; font-size: 28px; line-height: 1.2; pointer-events: all; white-space: normal; overflow-wrap: normal;">External<br />
Dependency</div>
        </div>
      </div>
    </foreignObject>
  </g>
  <switch>
    <g requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility"/>
    <a transform="translate(0,-5)" xlink:href="https://www.diagrams.net/doc/faq/svg-export-text-problems" target="_blank">
      <text text-anchor="middle" font-size="10px" x="50%" y="100%">Text is not SVG - cannot display</text>
    </a>
  </switch>
</svg>
</div>

The Domain layer has high cyclomatic complexity and domain significance, while the Controller layer 
has a large number of collaborators. Code that is both complex and involves many collaborators is 
overcomplicated and what Hexagonal architecture aims to dissolve. 

## Communication

Communication can be classified into two different types:

- **Intra-process** - Communication within the application; implementation details.
- **Inter-process** - Communication with other applications; observable behavior.

Mocks are necessary for emulating external applications and verifying inter-system communication 
patterns. However, mocks couple tests to implementation details, reducing their resistance to 
refactoring. For this reason, the use of mocks should be avoided when dealing with intra-system 
communication. External dependencies only accessible by the application, are implementation 
details too, and should not be mocked either.

## Functional Architecture

*Functional architecture* builds off of Hexagonal architecture with an added guideline that business 
logic is written in a functional paradigm. The architecture generally flows in a three step process:

1. The Controller layer gathers and prepare input.
1. The Domain layer makes decisions based on prepared input.
1. The Controller layer converts decisions into side effects.

<div class="svg-content">
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
<svg xmlns="http://www.w3.org/2000/svg"
  xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1" width="458px" height="427px" viewBox="-0.5 -0.5 458 427">
  <g>
    <rect x="301" y="193" width="200" height="40" rx="6" ry="6" class="background-fill-outline-text" stroke-width="3" transform="rotate(90,401,213)" pointer-events="all"/>
    <path d="M 241 203 Q 311 203 311 138 Q 311 73 356 73 Q 401 73 401 102.9" class="outline-text" stroke-width="3" stroke-miterlimit="10" pointer-events="stroke"/>
    <path d="M 401 109.65 L 396.5 100.65 L 401 102.9 L 405.5 100.65 Z" class="solid-text" stroke-width="3" stroke-miterlimit="10" pointer-events="all"/>
    <rect x="1" y="193" width="240" height="40" rx="6" ry="6" class="background-fill-outline-text" stroke-width="3" pointer-events="all"/>
    <path d="M 251 213 Q 321 213 321 148 Q 321 83 366 83 Q 411 83 411 112.9" class="outline-text" stroke-width="3" stroke-miterlimit="10" pointer-events="stroke"/>
    <path d="M 411 119.65 L 406.5 110.65 L 411 112.9 L 415.5 110.65 Z" class="solid-text" stroke-width="3" stroke-miterlimit="10" pointer-events="all"/>
    <path d="M 41 53 L 41.07 182.9" class="outline-text" stroke-width="3" stroke-miterlimit="10" pointer-events="stroke"/>
    <path d="M 41.08 189.65 L 36.57 180.65 L 41.07 182.9 L 45.57 180.64 Z" class="solid-text" stroke-width="3" stroke-miterlimit="10" pointer-events="all"/>
    <path d="M 81 63 L 81.07 192.22" class="outline-text" stroke-width="3" stroke-miterlimit="10" pointer-events="stroke"/>
    <path d="M 81.08 198.97 L 76.57 189.97 L 81.07 192.22 L 85.57 189.96 Z" class="solid-text" stroke-width="3" stroke-miterlimit="10" pointer-events="all"/>
    <path d="M 161 63 L 161 189.74" class="outline-text" stroke-width="3" stroke-miterlimit="10" pointer-events="stroke"/>
    <path d="M 161 196.49 L 156.5 187.49 L 161 189.74 L 165.5 187.49 Z" class="solid-text" stroke-width="3" stroke-miterlimit="10" pointer-events="all"/>
    <path d="M 201 53 L 201.07 182.9" class="outline-text" stroke-width="3" stroke-miterlimit="10" pointer-events="stroke"/>
    <path d="M 201.08 189.65 L 196.57 180.65 L 201.07 182.9 L 205.57 180.64 Z" class="solid-text" stroke-width="3" stroke-miterlimit="10" pointer-events="all"/>
    <path d="M 41 233 L 41 342.9" class="outline-text" stroke-width="3" stroke-miterlimit="10" pointer-events="stroke"/>
    <path d="M 41 349.65 L 36.5 340.65 L 41 342.9 L 45.5 340.65 Z" class="solid-text" stroke-width="3" stroke-miterlimit="10" pointer-events="all"/>
    <path d="M 81 233 L 81 352.9" class="outline-text" stroke-width="3" stroke-miterlimit="10" pointer-events="stroke"/>
    <path d="M 81 359.65 L 76.5 350.65 L 81 352.9 L 85.5 350.65 Z" class="solid-text" stroke-width="3" stroke-miterlimit="10" pointer-events="all"/>
    <path d="M 120.5 233 L 120.57 362.9" class="outline-text" stroke-width="3" stroke-miterlimit="10" pointer-events="stroke"/>
    <path d="M 120.58 369.65 L 116.07 360.65 L 120.57 362.9 L 125.07 360.64 Z" class="solid-text" stroke-width="3" stroke-miterlimit="10" pointer-events="all"/>
    <path d="M 161 233 L 161 352.9" class="outline-text" stroke-width="3" stroke-miterlimit="10" pointer-events="stroke"/>
    <path d="M 161 359.65 L 156.5 350.65 L 161 352.9 L 165.5 350.65 Z" class="solid-text" stroke-width="3" stroke-miterlimit="10" pointer-events="all"/>
    <path d="M 201 233 L 201 342.9" class="outline-text" stroke-width="3" stroke-miterlimit="10" pointer-events="stroke"/>
    <path d="M 201 349.65 L 196.5 340.65 L 201 342.9 L 205.5 340.65 Z" class="solid-text" stroke-width="3" stroke-miterlimit="10" pointer-events="all"/>
    <foreignObject pointer-events="none" width="100%" height="100%" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility" style="overflow: visible; text-align: left;">
      <div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 152px; height: 1px; padding-top: 33px; margin-left: 45px;">
        <div style="box-sizing: border-box; font-size: 0px; text-align: center;">
          <div style="display: inline-block; font-size: 28px; line-height: 1.2; pointer-events: all; white-space: normal; overflow-wrap: normal;">Input</div>
        </div>
      </div>
    </foreignObject>
    <foreignObject pointer-events="none" width="100%" height="100%" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility" style="overflow: visible; text-align: left;">
      <div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 152px; height: 1px; padding-top: 393px; margin-left: 45px;">
        <div style="box-sizing: border-box; font-size: 0px; text-align: center;">
          <div style="display: inline-block; font-size: 28px; line-height: 1.2; pointer-events: all; white-space: normal; overflow-wrap: normal;">Side effects</div>
        </div>
      </div>
    </foreignObject>
    <foreignObject pointer-events="none" width="100%" height="100%" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility" style="overflow: visible; text-align: left;">
      <div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 183px; height: 1px; padding-top: 33px; margin-left: 272px;">
        <div style="box-sizing: border-box; font-size: 0px; text-align: center;">
          <div style="display: inline-block; font-size: 28px; line-height: 1.2; pointer-events: all; white-space: normal; overflow-wrap: normal;">Prepared input</div>
        </div>
      </div>
    </foreignObject>
    <foreignObject pointer-events="none" width="100%" height="100%" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility" style="overflow: visible; text-align: left;">
      <div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 183px; height: 1px; padding-top: 393px; margin-left: 265px;">
        <div style="box-sizing: border-box; font-size: 0px; text-align: center;">
          <div style="display: inline-block; font-size: 28px; line-height: 1.2; pointer-events: all; white-space: normal; overflow-wrap: normal;">Decisions</div>
        </div>
      </div>
    </foreignObject>
    <path d="M 421 333 Q 421 353 366 353 Q 311 353 311 298 Q 311 243 274.23 243" class="outline-text" stroke-width="3" stroke-miterlimit="10" pointer-events="stroke"/>
    <path d="M 267.48 243 L 276.48 238.5 L 274.23 243 L 276.48 247.5 Z" class="solid-text" stroke-width="3" stroke-miterlimit="10" pointer-events="all"/>
    <path d="M 411 323 Q 411 343 366 343 Q 321 343 321 288 Q 321 233 261.1 233" class="outline-text" stroke-width="3" stroke-miterlimit="10" pointer-events="stroke"/>
    <path d="M 254.35 233 L 263.35 228.5 L 261.1 233 L 263.35 237.5 Z" class="solid-text" stroke-width="3" stroke-miterlimit="10" pointer-events="all"/>
    <path d="M 401 313 Q 401 333 366 333 Q 331 333 331 278 Q 331 223 251.1 223" class="outline-text" stroke-width="3" stroke-miterlimit="10" pointer-events="stroke"/>
    <path d="M 244.35 223 L 253.35 218.5 L 251.1 223 L 253.35 227.5 Z" class="solid-text" stroke-width="3" stroke-miterlimit="10" pointer-events="all"/>
    <rect x="11" y="203" width="240" height="40" rx="6" ry="6" class="background-fill-outline-text" stroke-width="3" pointer-events="all"/>
    <rect x="24.13" y="213" width="240" height="40" rx="6" ry="6" class="background-fill-outline-text" stroke-width="3" pointer-events="all"/>
    <foreignObject pointer-events="none" width="100%" height="100%" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility" style="overflow: visible; text-align: left;">
      <div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 238px; height: 1px; padding-top: 233px; margin-left: 25px;">
        <div style="box-sizing: border-box; font-size: 0px; text-align: center;">
          <div style="display: inline-block; font-size: 28px; line-height: 1.2; pointer-events: all; white-space: normal; overflow-wrap: normal;">Controller</div>
        </div>
      </div>
    </foreignObject>
    <path d="M 121 73 L 121.08 202.22" class="outline-text" stroke-width="3" stroke-miterlimit="10" pointer-events="stroke"/>
    <path d="M 121.09 208.97 L 116.58 199.97 L 121.08 202.22 L 125.58 199.96 Z" class="solid-text" stroke-width="3" stroke-miterlimit="10" pointer-events="all"/>
    <rect x="311" y="203" width="200" height="40" rx="6" ry="6" class="background-fill-outline-text" stroke-width="3" transform="rotate(90,411,223)" pointer-events="all"/>
    <rect x="321" y="213" width="200" height="40" rx="6" ry="6" class="background-fill-outline-text" stroke-width="3" transform="rotate(90,421,233)" pointer-events="all"/>
    <g transform="rotate(90 421 233)">
      <foreignObject pointer-events="none" width="100%" height="100%" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility" style="overflow: visible; text-align: left;">
        <div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 198px; height: 1px; padding-top: 233px; margin-left: 322px;">
          <div style="box-sizing: border-box; font-size: 0px; text-align: center;">
            <div style="display: inline-block; font-size: 28px; line-height: 1.2; pointer-events: all; white-space: normal; overflow-wrap: normal;">Domain Logic</div>
          </div>
        </div>
      </foreignObject>
    </g>
    <path d="M 264.13 223 Q 331 223 331 158 Q 331 93 376 93 Q 421 93 421 122.9" class="outline-text" stroke-width="3" stroke-miterlimit="10" pointer-events="stroke"/>
    <path d="M 421 129.65 L 416.5 120.65 L 421 122.9 L 425.5 120.65 Z" class="solid-text" stroke-width="3" stroke-miterlimit="10" pointer-events="all"/>
  </g>
  <switch>
    <g requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility"/>
    <a transform="translate(0,-5)" xlink:href="https://www.diagrams.net/doc/faq/svg-export-text-problems" target="_blank">
      <text text-anchor="middle" font-size="10px" x="50%" y="100%">Text is not SVG - cannot display</text>
    </a>
  </switch>
</svg>
</div>

> Object-oriented programming makes code understandable by encapsulating moving parts. Functional
> programming makes code understandable by minimizing moving parts. *--Michael Feathers*

Functional architecture trades maintainability for performance; pure functions tend to eagerly load 
data when they could have been lazy.

## Controller Orchestration

Functional architecture assumes a clearcut pipeline of inputs, decisions, and side effects. However,
production applications are rarely that simple. Decisions may involve gathering more input followed
by making additional decisions. There are three strategies to consider:

1. **Eagerly gather all the input** - Preserve controller simplicity and isolated domain logic, 
   but concede performance.
1. **Inject dependencies into the Domain layer** - Preserve controller simplicity and performance, but
   concede isolated domain logic.
1. **Allow controller orchestration** - Preserve isolated domain logic and performance, but concede 
   controller simplicity.

Isolated domain logic is an attribute that should always be maximized because it has a huge impact 
on *maintainability* and *resistance to refactoring*. Injecting dependencies into the Domain layer
is rule out. 

In cases where performance is not critical, feel free to stick to a Functional architecture and 
eagerly gather input. Otherwise, allow controllers to orchestrate gathering input to meet the 
needs of the Domain layer.

{% info(header="Info") %}
Controller orchestration will make controllers more complex, but complexity can be mitigated with 
familiar patterns like switching on a result or listening for Domain layer events.
{% end %}

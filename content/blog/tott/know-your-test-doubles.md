+++
title = "Know Your Test Doubles"
date = 2013-07-18
weight = 25
[taxonomies]
categories = ["Testing on the Toilet"]
tags = ["testing", "google", "tott"]
[extra]
+++

A test double is an object that can stand in for a real object in test. The most common types of
test doubles are:

- **Stub** - Returns a specific values to promote a specific state.

  ```cpp
  AccessManager access_manager(kStubAuthenticationService);

  ON_CALL(kStubAuthenticationService, IsAuthenticated(kUserId))
      .WillByDefault(Return(false));
  EXPECT_FALSE(access_manager.UserHasAccess(kUserId));

  ON_CALL(kStubAuthenticationService, IsAuthenticated(kUserId))
      .WillByDefault(Return(true));
  EXPECT_TRUE(access_manager.UserHasAccess(kUserId));
  ```

- **Mock** - Sets expectations about how other objects should interact with it.

  ```cpp
  AccessManager access_manager(mockAuthenticationService);

  EXPECT_CALL(mockAuthenticationService, IsAuthenticated(kUserId));
  access_manager.UserHasAccess(kUserId);
  ```

- **Fake** - A lightweight implementation when the real implementation is unsuitable for test.

  ```cpp
  FakeAuthenticationService fake_authentication_service;
  AccessManager access_manager(fake_authentication_service);

  EXPECT_FALSE(access_manager.UserHasAccess(kUserId));

  fake_authentication_service.AddAuthenticatedUser(kUser);
  EXPECT_TRUE(access_manager.UserHasAccess(kUserId));
  ```
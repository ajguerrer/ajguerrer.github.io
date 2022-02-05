+++
title = "Only Expect State-Changing Method Calls"
date = 2017-12-11
weight = 8
[taxonomies]
categories = ["Testing on the Toilet"]
tags = ["testing", "google", "tott"]
[extra]
+++

Expecting calls to methods that don't change state can make a test brittle, less readable, and
provide a false sense of security.

{% bad(header="Bad") %}
```cpp
TEST(UserAuthorizer, AddPermissionToDatabase) {
  UserAuthorizer user_authorizer(mock_user_service_, mock_permission_db_);

  EXPECT_CALL(mock_user_service_, IsUserActive(kUser));
  EXPECT_CALL(mock_permission_db_, GetPermission(kUser));
  EXPECT_CALL(mock_permission_db_, IsValidPermission(kReadAccess));
  EXPECT_CALL(mock_permission_db_, AddPermission(kUser, kReadAccess));

  user_authorizer.GrantPermission(kUser, kReadAccess);
}
```
{% end %}

It is fine, however, to use non-state-changing methods for simulating test conditions.

{% good(header="Good") %}
```cpp
ON_CALL(mock_user_service_, IsUserActive(kUser)).WillByDefault(Return(false));
```
{% end %}

With unnecessary `EXPECT_CALL`s removed, the test becomes:

{% good(header="Good") %}
```cpp
Test(UserAuthorizer, AddPermissionToDatabase) {
  UserAuthorizer user_authorizer(mock_user_service_, mock_permission_db_);

  EXPECT_CALL(mock_permission_db_, AddPermission(kUser, kReadAccess));

  user_authorizer.GrantPermission(kUser, kReadAccess);
}
```
{% end %}

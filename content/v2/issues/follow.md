---
title: API v2 - Issues - Follow
---

# Issue Following

To check wether the current user is following an issue see the attribute `current_user_relationship` on the issue show or issue list actions. 

## Follow an Issue

Issues can be followed by any authenticated user.

    PUT /issues/<issue_id>/follow

### Request

<%=
  json({value: 1})
%>

### Response

<%=
  json({
    metadata: nil,
    result: 'success',
    errors: nil
  })
%>

## Revoke following an Issue

Issues can be unfollowed by any authenticated user.

    PUT /issues/<issue_id>/follow

### Request

<%=
  json({value: -1})
%>

### Response

<%=
  json({
    metadata: nil,
    result: 'success',
    errors: nil
  })
%>
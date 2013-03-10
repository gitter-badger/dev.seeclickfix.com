---
title: Issues | SeeClickFix API
---

# Issues

Issues use [these custom media types](#custom-media-types). You can
read more about the use of media types in the API [here](/v3/media/).

* TOC
{:toc}

## List issues

List all issues across all the authenticated user's visible repositories
including owned repositories, member repositories, and organization
repositories:

    GET /issues


### Parameters

bounds
: * `:sw_lat,:sw_lng|:ne_lat,:ne_lng`: Issues within the defined rectangle

Example

    <%= root_v2_url %>/issues?bounds=0.0,0.0|4.0,4.0


page
: * `page` : number of the page to return, default: 1

per_page
: * `per_page` : number of issues returned per page, default: 20

Example

    <%= root_v2_url %>/issues?page=2&per_page=20


filter_by
: * `assigned`: Issues assigned to you (default)
  * `created`: Issues created by you
  * `mentioned`: Issues mentioning you
  * `subscribed`: Issues you're subscribed to updates for
  * `all`: All issues the authenticated user can see, regardless of particpation or creation

state
: `open`, `closed`, default: open

labels
: _String_ list of comma separated Label names.  Example:
`bug,ui,@high`

sort
: `created`, `updated`, `comments`, default: `created`.

direction
: `asc` or `desc`, default: `desc`.

since
: _Optional_ **string** of a timestamp in ISO 8601 format: YYYY-MM-DDTHH:MM:SSZ

### Response

<%= headers 200, :pagination => true %>
<%= json(:issue) { |h| [h] } %>

## Get a single issue

    GET /issues/:number

### Response

<%= headers 200 %>
<%= json :issue %>

## Create an issue

Any user with pull access to a repository can create an issue.

    POST /issues

### Input

title
: _Required_ **string**

body
: _Optional_ **string**

assignee
: _Optional_ **string** - Login for the user that this issue should be
assigned to. _NOTE: Only users with push access can set the assignee for new
issues. The assignee is silently dropped otherwise._


milestone
: _Optional_ **number** - Milestone to associate this issue with. _NOTE: Only
users with push access can set the milestone for new issues. The milestone is
silently dropped otherwise._


labels
: _Optional_ **array** of **strings** - Labels to associate with this
issue. _NOTE: Only users with push access can set labels for new issues. Labels are
silently dropped otherwise._

<%= json \
  :title     => "Found a bug",
  :body      => "I'm having a problem with this.",
  :assignee  => "octocat",
  :milestone => 1,
  :labels    => %w(Label1 Label2)
%>

### Response

<%= headers 201,
      :Location =>
'https://api.github.com/repos/user/repo/issues/1' %>
<%= json :issue %>

## Edit an issue

Issue owners and users with push access can edit an issue.

    PATCH /repos/:owner/:repo/issues/:number

### Input

title
: _Optional_ **string**

body
: _Optional_ **string**

assignee
: _Optional_ **string** - Login for the user that this issue should be
assigned to.

state
: _Optional_ **string** - State of the issue: `open` or `closed`.

milestone
: _Optional_ **number** - Milestone to associate this issue with.

labels
: _Optional_ **array** of **strings** - Labels to associate with this
issue. Pass one or more Labels to _replace_ the set of Labels on this
Issue. Send an empty array (`[]`) to clear all Labels from the Issue.

<%= json \
  :title     => "Found a bug",
  :body      => "I'm having a problem with this.",
  :assignee  => "octocat",
  :milestone => 1,
  :state     => "open",
  :labels    => %w(Label1 Label2)
%>

### Response

<%= headers 200 %>
<%= json :issue %>

---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - Dart

includes:
  - errors

search: true
---

# Introduction

Welcome to the LOOP API! You can use our API to access Realtime Chat endpoints, to access chat history, create groups, create loops and send messages, plus much more!

We have language bindings in Dart! You can view code examples in the dark area to the right.

# Loop

## Get Groups List

> the lastSeen variable above is a timestamp converted to milliseconds since epoch.

```dart
await getRequest(
      url: '$loopId/groups',
      data: {
        'groups': {
          'loop_news': {
            'lastSeen': 12344543322,
          },
        },
      },
    );
```

> The above command returns JSON structured like this:

```json
{
    "error": false,
    "groups": [
        {
            "memberCount": 1,
            "type": "ADMIN",
            "updated": {
                "_seconds": 1588124049,
                "_nanoseconds": 764000000
            },
            "joinRequestCount": 0,
            "description": "This is the latest news and events associated with Loop.",
            "created": {
                "_seconds": 1585314000,
                "_nanoseconds": 0
            },
            "owner": "iEcUgwssrrMmSTyYrcVjdFyARxY2",
            "name": "Loop News",
            "privacy": "PUBLIC",
            "recentActivity": [
                {
                    "_seconds": 1587475856,
                    "_nanoseconds": 448000000
                },
                {
                    "_seconds": 1587475917,
                    "_nanoseconds": 749000000
                }
            ],
            "invitePermissions": "ADMIN",
            "loop": "global_loop",
            "backup": false,
            "id": "loop_news",
            "unreadCount": 15
        }
    ],
    "message": ""
}
```

### HTTP Request

`GET /:loopId/groups`


### Route Parameters

Parameter | Description
--------- | -----------
loopId | This is the Id of the loop from where you want to retrieve the groups.

### Query Parameters

Parameter | Description
--------- | -----------
groups | This is a map of the different groups associated with a user, available in their loop userRegistry document.
       | The first level of the map, will be the group Id.
       | *** lastSeen will need to be converted to millisecondsSinceEpoch.
### Notes

This call retrieves the group documents of which a user is apart of in the any given Loop.

Inside the groups list will be the information of the group, the role of the user in the group and a count for the
number of unread threads inside the group.


# Thread

## Thread View Count
> the created variable above is a timestamp converted to milliseconds since epoch.

```dart
await getRequest(
    url: '${threadId}/count',
    data: {
      'loopId': loopId,
      'groupId': groupId,
      'created': created.millisecondsSinceEpoch,
    },
  );
```

> The above command returns JSON structured like this:

```json
{
    "error": false,
    "uidList": [
        "DoL15BBZR6V2EHuz8ctNTW6NqwZ2",
        "wBeNivjrhWZ0g7CUo0mq554yycg2",
        "nlFkj8sO7ugIPKL2o3hL13k6Wzb2",
        "iEcUgwssrrMmSTyYrcVjdFyARxY2",
        "8VoWiOWhzIQRqWWBFs01gn9aTcz1",
        "eWGX42FG8sTqml0vxeb3DEBOJGx1",
        "HaifLL9MpxYVrqvyD6OuSrLJaJ72",
        "0KoRwgxXHFQwWW9WAzk65LOuad63",
        "iMHlOyz6UgaXbs6bG2nSrcYDGof2",
        "NyvJ2MCgLsPqN2l5Y9JnT7sbV2z1",
        "f7fu49rVHgMca8CwNr868GAoz4O2",
        "MkBHk5UL2MbtYikpF777vyziZWx2",
        "vE7cGBdKHNbHNruq8Z76VrbW6fJ3"
    ],
    "count": 13,
    "message": ""
}
```

### HTTP Request

`GET /:threadId/count`

### Route Parameters

Parameter | Description
--------- | -----------
threadId | This is the Id of the thread from which you want to retrieve the count.

### Query Parameters

Parameter | Description
--------- | -----------
loopId | This is the Id of the loop which contains the thread.
groupId | The Id of the group which contains the thread.
created | DateTime timestamp converted to millisecondsSinceEpoch.

### Notes

This call retrieves the count of users who have seen a thread. Along with the list of user id's who have seen the thread.


## Post a new thread

```dart
await postRequest(
        url: 'postThread/$groupId',
        data: {
          'id': threadData.id,
          'body': threadData.body,
          'media': mediaData,
          'uid': threadData.uid,
          'name': threadData.name,
          'groupName': groupName,
          'repliesCount': 0,
        },
      );
```

> The above command returns JSON structured like this:

```json
{
    "error": false
}
```

### HTTP Request

`POST /postThread/:groupId`

### Route Parameters

Parameter | Description
--------- | -----------
groupId | This is the Id of the group where you would like to post the thread.

### Body Parameters

Parameter | Description
--------- | -----------
id | This is the Id of the the thread, created on the client side.
body | The text body of the new thread to be created.
media | The media object to be associated with the thread to be created.
      | {"url": "https://example_of_the_firebase_storage_url.com", contentType: "image/jpeg", contentLength: 12345}
uid | The Id of the user who is creating the thread.
name | The name of the user who is creating the thread.
groupName | The name of the group.
repliesCount | The number of replies, initialised at 0 to be incremented by each reply.


### Notes

This call creates a new thread in a group. A firebase notification will also be triggered when the thread is created.

# Reply

## Post a new reply to a thread

```dart
await postRequest(
        url: 'postReply/$groupId/$threadId',
        data: {
          'id': replyMessage.id,
          'groupName': groupName,
          'userName': userName,
          'reply': {
            'body': replyMessage.body,
            'media': mediaData,
            'uid': replyMessage.uid,
          },
        },
      );
```

> The above command returns JSON structured like this:

```json
{
    "error": false
}
```

### HTTP Request

`POST /postReply/:groupId/:threadId`

### Route Parameters

Parameter | Description
--------- | -----------
groupId | This is the Id of the group which contains the thread.
threadId | This is the Id of the thread where you would like to post the reply.

### Body Parameters

Parameter | Description
--------- | -----------
id | This is the Id of the the reply, created on the client side.
groupName | The name of the group.
userName | The name of the user who is creating the reply.
reply | This is the object of the reply to be created, consisting of below:
    body | The text body of the new reply to be created.
    media | The media object to be associated with the reply to be created.
          | {"url": "https://example_of_the_firebase_storage_url.com", contentType: "image/jpeg", contentLength: 12345}
    uid | The Id of the user who is creating the reply.


### Notes

This call creates a new reply in a thread. A firebase notification will also be triggered when the reply is created.

# Private Message

## Get Private Message History


```dart
await getRequest(url: 'privateMessages/$privateId');
```

> The above command returns JSON structured like this:

```json
{
    "error": false,
    "privateMessages": [
        {
            "body": "in Android settings",
            "created": {
                "_seconds": 1588124899,
                "_nanoseconds": 632000000
            },
            "senderId": "HaifLL9MpxYVrqvyD6OuSrLJaJ72"
        },
        {
            "senderId": "HaifLL9MpxYVrqvyD6OuSrLJaJ72",
            "body": "I had to go in and manually approve camera and microphone permissions",
            "created": {
                "_seconds": 1588124893,
                "_nanoseconds": 981000000
            }
        },
        {
            "senderId": "f7fu49rVHgMca8CwNr868GAoz4O2",
            "body": "Well shit, that bombed",
            "created": {
                "_seconds": 1588124893,
                "_nanoseconds": 907000000
            }
        },
        {
            "body": "I can see you but can't hear anything",
            "created": {
                "_seconds": 1588124831,
                "_nanoseconds": 494000000
            },
            "senderId": "HaifLL9MpxYVrqvyD6OuSrLJaJ72"
        },
        {
            "senderId": "HaifLL9MpxYVrqvyD6OuSrLJaJ72",
            "body": "I did not",
            "created": {
                "_seconds": 1588124781,
                "_nanoseconds": 174000000
            }
        },
        {
            "senderId": "f7fu49rVHgMca8CwNr868GAoz4O2",
            "body": "Did you not see my video chat coming in? There is no ringtone yet",
            "created": {
                "_seconds": 1588124762,
                "_nanoseconds": 70000000
            }
        },
        {
            "senderId": "HaifLL9MpxYVrqvyD6OuSrLJaJ72",
            "body": "Gotcha",
            "created": {
                "_seconds": 1588124496,
                "_nanoseconds": 341000000
            }
        },
        {
            "body": "Not in PMs yet",
            "created": {
                "_seconds": 1588124466,
                "_nanoseconds": 822000000
            },
            "senderId": "f7fu49rVHgMca8CwNr868GAoz4O2"
        },
        {
            "body": "looks like gifs aren't sending! But hello anyway!",
            "created": {
                "_seconds": 1588124020,
                "_nanoseconds": 813000000
            },
            "senderId": "HaifLL9MpxYVrqvyD6OuSrLJaJ72"
        },
        {
            "senderId": "HaifLL9MpxYVrqvyD6OuSrLJaJ72",
            "body": "",
            "created": {
                "_seconds": 1588123991,
                "_nanoseconds": 388000000
            }
        },
        {
            "senderId": "HaifLL9MpxYVrqvyD6OuSrLJaJ72",
            "body": "",
            "created": {
                "_seconds": 1588123986,
                "_nanoseconds": 415000000
            }
        }
    ],
    "count": 11,
    "next": 1588123986415,
    "end": true
}
```

### HTTP Request

`GET /privateMessages/:privateId`


### Route Parameters

Parameter | Description
--------- | -----------
privateId | This is the Id of the private messaging chat instance.

### Query Parameters

Parameter | Optional | Description
--------- | ---------| ----------
next | true | The timestamp in millisecondsSinceEpoch of the last message, indicates pagination.
limit | true | This can set the number of replies to retrieve at a time, defaults to 20.

### Notes

This call retrieves the history of private messages for any instance of a private message chat.

If end is true, it indicates you have reached the end of the private messages history.

Use the next variable returned to retrieve the next set of messages from the history.


## Create a new private messaging instance


```dart
await postRequest(
      url: 'createPm/$privateMessageId',
      data: {
        'members': ["a_user_id", "another_user_id", "yet_another_user_id"],
      },
    );
```

> The above command returns JSON structured like this:

```json
{
    "error": false,
    "privateId": "example_private_id_chat_instance",
    "exists": true
}
```

### HTTP Request

`POST /createPm/:privateId`


### Route Parameters

Parameter | Description
--------- | -----------
privateId | This is the Id of the private messaging chat instance.

### Query Parameters

Parameter | Description
--------- | -----------
members  | This is a list of user ids, which you want to create the private messaging instance with.

### Notes

This call retrieves will create a new instance, or retrieve one that already exists, if it exists.

If exists returns as true, it will also return the id which is associated with the already created instance.

If exists if false, it will return the id created on the client side and create the new instance.

## Post a new private message

```dart
await postRequest(
      url: 'postPm/$privateId',
      data: {
        'message': {
          'body': body,
          'senderId': senderId,
        },
        'userName': senderName,
        'id': privateMessageId,
      },
    );
```

> The above command returns JSON structured like this:

```json
{
    "error": false
}
```

### HTTP Request

`POST /postPm/:privateId`

### Route Parameters

Parameter | Description
--------- | -----------
privateId | This is the Id of the private messaging chat instance.

### Body Parameters

Parameter | Description
--------- | -----------
id | This is the Id of the the private message (NOT THE INSTANCE), created on the client side.
userName | The name of the user who is creating the private message.
message | This is the object of the private message to be created, consisting of below:
    body | The text body of the new message to be created.
    senderId | The Id of the user who is creating/sending the private message.


### Notes

This call creates a new private message within the private messaging chat instance.

It triggers a notification to all participants within the private messaging chat.

# User groups

## Create user group(s)

- Implement a service to create a list of user groups
  - User group has an  (auto-generated) id, a name and a description
  - There is a built-in group 'admin' with description 'Administrator Group'
  - unique constraint on group name
  - non-null name and description
  
## Read all user groups

Implement a service to read all user groups

## Assign group(s)to user(s)

Implement a service which assigns user/s to group/s. The input payload is something like

```json
{"11":[5, 42, 1], "15": [5]}
```
, where the keys are user id's and the values are group id's.

In java this can be represented as a hash array mapped trie of integer to list of integer.

This webservice is behaves like a map 'put'. It means that if a user '1' is assigned roles '2,3,4', and they already
have roles assigned that are not in the list '2,3,4', those roles should first be removed from the user.

The service is transactional. The whole modify operation should succeed, or be discarded.


## Enhance the get all users service and user object

It should now also contain the user groups.

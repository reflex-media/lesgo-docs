# Firebase Admin Service

Lesgo! comes with a service wrapper of `firebase-admin` plugin

## Initialization

```js
import FirebaseAdmin from "lesgo/services/FirebaseAdminService";

const serviceAccount = require("path/to/serviceAccountKey.json");

const firebaseAdmin = new FirebaseAdmin({
	serviceAccount,
	projectName: "databaseName"
});
```

## Accessing Firebase Admin Client

If ever needed you can also access the service instance directly through `firebaseAdmin.app`

## Available methods

### firebaseAdmin.getAllUsers

This will fetch all users from firebase

```js
// Get at most 10 users starting from 
await firebaseAdmin.getAllUsers(10, "zUECpHuP5a63T9DNt64TeTrxqfV1WvM5IVe8cYZ9E9tSbhFOrw3QK2VPXJiLiYZo4dne5naMKnPPPYrUkUaYaJgPzfaV0gVj0EEdkiJvsdlreEShuogIs4zbdlLtqPQ8");

/**
[
	{
		uid: "ly7vYiwWlpE1kAMw34mfPPkqiqaA",
		email: "jon@test.com",
		password: "jon1234",
		username: "jonsnow",
		role: "user",
		lastSignInTime: null,
		creationTime: "Mon, 08 Jul 2019 01:07:01 GMT"
	},
	{
		uid: "8zaRodq7KcGfXh6Dg3r5ub3svBgM",
		email: "arya@test.com",
		password: "arya1234",
		username: "aryastark",
		role: "user",
		lastSignInTime: "Sat, 27 Jul 2019 01:07:01 GMT",
		creationTime: "Thu, 25 Jul 2019 01:07:01 GMT"
	},
	...
]
*/
```

### firebaseAdmin.createUser

This allows you to create a new Firebase Authentication user

```js
await firebaseAdmin.createUser({
	uid: "ly7vYiwWlpE1kAMw34mfPPkqiqaA",
	email: "jon@test.com",
	password: "jon1234",
	username: "jonuser"
});

/**
{
	uid: "ly7vYiwWlpE1kAMw34mfPPkqiqaA",
	email: "jon@test.com",
	password: "jon1234",
	username: "jonuser",
	role: "user",
	lastSignInTime: null,
	creationTime: "Mon, 08 Jul 2019 01:07:01 GMT"
}
*/
```

### firebaseAdmin.deleteUser

This allows deleting existing users by their `uid`

```js
await firebaseAdmin.deleteUser("qSE2pP4lr0Dfn0DfcDbeY6rqlwjd");
```

### firebaseAdmin.delete

This will render this app unusable and frees the resources of all associated services.

```js
await firebaseAdmin.delete();
```


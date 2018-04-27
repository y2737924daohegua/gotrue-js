# gotrue-js

[![Build Status](https://travis-ci.org/netlify/gotrue-js.svg?branch=master)](https://travis-ci.org/netlify/gotrue-js)

This is a JS client library for the [GoTrue](https://github.com/netlify/gotrue) API.

It lets you signup and authenticate users and is a building block for constructing
the UI for signups, password recovery, login and logout.

## Installation

```
yarn add gotrue-js
```

## Usage

```js
import GoTrue from 'gotrue-js'

const auth = new GoTrue();

auth.signup(email, password).then(
  response => console.log("Confirmation email sent"),
  error => console.log("Error during signup: %o", error.msg)
);

auth.confirm(token).then(
  user => console.log("Logged in as %s", user.email),
  error => console.log("Failed to log in: %o", error)
);
// Use `auth.confirm(token, true)` to save to localStorage

auth.login(email, password).then(
  user => console.log("Logged in as %s", user.email),
  error => console.log("Failed to log in: %o", error);
)
// Use `auth.login(email, password, true)` to save to localStorage

auth.requestPasswordRecovery(email).then(
  response => console.log("Recovery email sent"),
  error => console.log("Error sending recovery mail: %o", error)
);

auth.recover(token).then(
  user => console.log("Logged in as %s", user.email),
  error => console.log("Failed to verify recover token: %o", error)
);

const user = auth.currentUser()

// Users updating email requires an email confirmation step
user.update({email: newEmail, password: newPassword}).then(
  user => console.log("Updated user"),
  error => console.log("Failed to update user: %o", error)
);

user.jwt().then(
  token => console.log("Current token: %s", token),
  error => console.log("Failed to get token: %o", error)
);

user.logout().then(
  response => console.log("User logged out"),
  error => console.log("Failed to logout user: %o", error)
);

const admin = user.admin // User must have the 'admin' role

admin.list(audience).then(
  users => console.log("List of users: %o", users),
  error => console.log("Failed to get list of users: %o", error)
)

const user = { id: 1234 }
admin.getUser(user).then(
  user => console.log("User object: %o", user),
  error => console.log("Failed to retrieve user: %o", error)
)

// Admins updating email does not require a confirmation step
const attributes = {email: newEmail, password: newPassword}
admin.updateUser(user, attributes).then(
  user => console.log("Updated user: %o", user),
  error => console.log("Failed to update user: %o", error)
)

admin.createUser(email, password, attributes = {}).then(
  user => console.log("Created user user: %o", user),
  error => console.log("Failed to create user: %o", error)
)

admin.deleteUser(user).then(
  user => console.log("Deleted user user: %o", user),
  error => console.log("Failed to delete user: %o", error)
)
```


## API

### `import GoTrue from "gotrue-js"`

Import Gotrue class.

### `auth = new GoTrue([opts])`

Create a new GoTrue instance.

Optional `opts` include:

```js
{
  APIUrl: '/.netlify/identity',
  audience: "", // Set the X-JWT-AUD header TODO: write about why you would set this
  setCookie: fase // tells the API to set a cookie TODO: What is this for?
}
```

### `auth.signup(email, password)`

Create a new user with the provided email and password.  Returns a promise that contains a response object.

Example response object: 

```js
{ id: '3d4dbf6f-0ef5-4377-8dc0-8aa89b385fc0',
  aud: '',
  role: '',
  email: 'example@gmail.com',
  confirmation_sent_at: '2018-04-27T22:36:59.636416916Z',
  app_metadata: { provider: 'email' },
  user_metadata: null,
  created_at: '2018-04-27T22:36:59.632133283Z',
  updated_at: '2018-04-27T22:37:00.061039863Z' }
```

If an error occurs during the request, the promise may be rejected with an Error, `HTTPError`, `TextHTTPError`, or `JSONHTTPError`.  See [micro-api-client-lib error types](https://github.com/netlify/micro-api-client-lib#class-httperror-extends-error).

### `auth.confirm(token)`

### `auth.login(email, password)`

### `auth.requestPasswordRecovery(email)`

### `auth.recover(recoveryToken)`

### `user = auth.crrentUser()`

Return the current user object.

### `user.update( attributes )`

### `user.jwd()`

### `user.logout()`

### `admin = auth.admin()`

### `admin.list(audience)`

### `admin.getUser(userObject)`

### `admin.updateUser(userObject, attributes)`

### `admin.createUser(email, password, [attributes])`

### `admin.deleteUser(userObject)`



## See also

- [gotrue]()
- [netlify-identity-widget]()
- [micro-api-client-library]()
- [Netlify identity docs]()

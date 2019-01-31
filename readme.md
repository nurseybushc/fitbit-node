# Fitbit-Node

An API client library for Fitbit written in Node.js.

## Usage
1. `npm install fitbit-node`
1. (In your JS file) `var FitbitApiClient = require("fitbit-node");`

## API

#### `new FitbitApiClient({clientId: "YOUR_CLIENT_ID", clientSecret: "YOUR_CLIENT_SECRET", apiVersion: "1.2"})`
Constructor. Use the `clientId` and `clientSecret` provided to you when you registered your application on [dev.fitbit.com](http://dev.fitbit.com/).
The `apiVersion` is by default 1.2

#### `getAuthorizeUrl(scope, redirectUrl, [prompt], [state])`
Construct the authorization URL. This is the first step of the OAuth 2.0 flow. Returns a string. When this string containing the authorization URL on the Fitbit site is returned, redirect the user to that URL for authorization. The `scope` (a string of space-delimitted scope values you wish to obtain authorization for) and the `redirectUrl` (a string for the URL where you want Fitbit to redirect the user after authorization) are required. See the [Scope](https://dev.fitbit.com/docs/oauth2/#scope) section in Fitbit's API documentation for more details about possible scope values. See the [Authorization Page](https://dev.fitbit.com/docs/oauth2/#authorization-page) section in Fitbit's API documentation for more details about possible `prompt` and `state` values. 

#### `getAccessToken(code, redirectUrl)`
After the user authorizes your application with Fitbit, they will be forwarded to the `redirectUrl` you specified when calling `getAuthorizationUrl()`, and the `code` will be present in the URL. Use this to exchange the authorization code for an access token in order to make API calls. Returns a promise.

#### `refreshAccessToken(accessToken, refreshToken, [expiresIn])`
Refresh the user's access token, in the event that it has expired. The `accessToken` and `refreshToken` (returned as `refresh_token` alongside the `access_token` by the `getAccessToken()` method) are required. The `expiresIn` parameter specifies the new desired access token lifetime in seconds. Returns a promise.

#### `AccessTokenStatus(accessToken)`
Check the status of `accessToken` that is passed in. When active = 1 response body looks like { active: 1, scope: '{HEARTRATE=READ, SETTINGS=READ, PROFILE=READ, SOCIAL=READ, NUTRITION=READ, ACTIVITY=READ, SLEEP=READ, WEIGHT=READ, LOCATION=READ}',
  clientId: { id: <clientId> }, userId: { id: <userId> }, tokenType: 'access_token', exp: 28798, iat: 1515628997000 }. When Active = 0, Response body looks like { active: 0 }.(See [example.js](https://github.com/nurseymybush/fitbit-node/blob/master/example.js). Returns a promise.

#### `revokeAccessToken(accessToken)`
Revoke the user's access token. The `accessToken` to be revoked is required. Returns a promise.

#### `get(path, accessToken, [userId], [extraHeaders])`
Make a GET API call to the Fitbit servers. (See [example.js](https://github.com/lukasolson/fitbit-node/blob/master/example.js) for an example.) Returns a promise.

#### `post(path, accessToken, data, [userId], [extraHeaders])`
Make a POST API call to the Fitbit servers. Returns a promise.

#### `put(path, accessToken, data, [userId], [extraHeaders])`
Make a PUT API call to the Fitbit servers. Returns a promise.

#### `delete(path, accessToken, [userId], [extraHeaders])`
Make a DELETE API call to the Fitbit servers. Returns a promise.

## Custom HTTP Headers

Some Fitbit API calls (such as [adding subscriptions](https://dev.fitbit.com/docs/subscriptions/#adding-a-subscription)) accept an optional HTTP header.  The `get`, `post`, `put`, and `delete` functions accept an optional parameter with an object with HTTP headers to handle these kind of calls (`extraHeaders` above).  The default `Authorization` header is merged with the provided parameter, if it exists.



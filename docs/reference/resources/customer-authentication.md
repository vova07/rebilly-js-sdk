# Customer Authentication <small>`:::js api.customerAuthentication`</small>

> Member of [`RebillyAPI`][goto-rebillyapi]

Authenticate your customers within your Rebilly integration. This feature is useful for creating self-service portals for your customers. 

## getAuthOptions
<div class="method"><code><strong>getAuthOptions</strong>() -> <span class="return">{Member}</span></code></div>

Get global customer authentication options.

**Example**

```js
const options = await api.customerAuthentication.getAuthOptions();
console.log(options.fields.credentialTtl);
```


**Returns**

A member exposing the authentication options fields.

Type [`Member`][goto-member]


**API Spec**

See the [detailed API spec][1]{: target="_blank"} for all payload fields and response data.

## updateAuthOptions
<div class="method"><code><strong>updateAuthOptions</strong>({<span class="prop">data</span>}) -> <span class="return">{Member}</span></code></div>

Update the global customer authentication options. You can modify the TTL (in seconds) for the credentials, authorization tokens and reset password tokens, or the password regular expression pattern use for validation.

**Example**

```js
// first set the properties for the authentication options
const data = {
    passwordPattern: null,
    credentialTtl: 10,
    authTokenTtl: 20,
    resetTokenTtl: 30
};

const options = await api.customerAuthentication.updateAuthOptions({data});
console.log(options.fields.credentialTtl);
```

**Returns**

A member exposing the updated custom field fields.

Type [`Member`][goto-member]


**API Spec**

See the [detailed API spec][2]{: target="_blank"} for all payload fields and response data.



## getAllAuthTokens

<div class="method"><code><strong>getAllAuthTokens</strong>({<span class="prop">limit</span><span class="optional">opt</span>, <span class="prop">offset</span><span class="optional">opt</span>}) -> <span class="return">{Collection}</span></code></div>

Get a collection of authentication tokens created for your customers. Each entry will be a member.

!!! info "Token Generation"
    Tokens are generated by the [customerAuthentication.login][goto-login] method.


**Example**

```js
// all parameters are optional
const firstCollection = await api.customerAuthentication.getAllAuthTokens();

// alternatively you can specify one or more of them
const params = {limit: 20, offset: 100}; 
const secondCollection = await api.customerAuthentication.getAllAuthTokens(params);

// access the collection items, each item is a Member
secondCollection.items.forEach(token => console.log(token.fields.username));
```

**Parameters**


--8<----- "reference/resources/shared/paged-get-all.md"


**Returns**

A collection of authentication tokens.

Type [`Collection`][goto-collection]


**API Spec**

See the [detailed API spec][3]{: target="_blank"} for all payload fields and response data.

## verify
<div class="method"><code><strong>verify</strong>({<span class="prop">token</span>}) -> <span class="return">{Member}</span></code></div>

Verify the validity of an authentication token. If valid the response will contain all the token fields.


**Example**

```js
const token = 'dcf6e32f2daee457a1db8ce5fdfbe200';
const verification = await api.customerAuthentication.verify({token});
// if the the token is valid then no error will be thrown
console.log(verification.reponse.status) // 200
```

**Returns**

A member exposing the token fields.

Type [`Member`][goto-member]


**API Spec**

See the [detailed API spec][4]{: target="_blank"} for all payload fields and response data.

## login
<div class="method"><code><strong>login</strong>({<span class="prop">data</span>}) -> <span class="return">{Member}</span></code></div>

Login a customer into Rebilly and retrieve the authentication token being generated. You must first [create a credential][goto-create-cred] for a customer.

Optionally you can provide the expired time of the session using `expiredTime` as a `datetime` in the future.

!!! info "Similar Account Method"
    This method is similar to [account.signIn][goto-account-login] but unlike it, it requires to be authorized in the API and will not return a JWT session token.
    
> See [customerAuthentication.createCredential][goto-create-cred]

> See [customerAuthentication.logout][goto-logout]


**Example**

```js
const data = {
    username: 'foobar',
    password: 'fuubar'
    
    // optionally you can define an `expiredTime` to 
    // limit the duration of the session token
    
    //expiredTime: '2017-09-18T19:17:39Z'
};
const session = await api.customerAuthentication.login({data});
console.log(session.fields.token);
```

**Returns**

A member exposing the token fields.

Type [`Member`][goto-member]


**API Spec**

See the [detailed API spec][5]{: target="_blank"} for all payload fields and response data.

## logout
<div class="method"><code><strong>logout</strong>({<span class="prop">token</span>}) -> <span class="return">{Member}</span></code></div>

Logout the customer and invalidate his session token.


**Example**

```js
const token = 'dcf6e32f2daee457a1db8ce5fdfbe200';
const request = await api.customerAuthentication.logout({token});

// the request does not return any fields but
// you can confirm the success using the status code
console.log(request.response.status); // 204
```

**Returns**

An empty member without fields. Check the `response` property to validate the expected status code.

Type [`Member`][goto-member]


**API Spec**

See the [detailed API spec][6]{: target="_blank"} for all payload fields and response data.

## getAllCredentials

<div class="method"><code><strong>getAllCredentials</strong>({<span class="prop">limit</span><span class="optional">opt</span>, <span class="prop">offset</span><span class="optional">opt</span>}) -> <span class="return">{Collection}</span></code></div>

Get a collection of customer credentials. Each entry will be a member.


**Example**

```js
// all parameters are optional
const firstCollection = await api.customerAuthentication.getAllCredentials();

// alternatively you can specify one or more of them
const params = {limit: 20, offset: 100}; 
const secondCollection = await api.customerAuthentication.getAllCredentials(params);

// access the collection items, each item is a Member
secondCollection.items.forEach(credential => console.log(credential.fields.customerId));
```

**Parameters**


--8<----- "reference/resources/shared/paged-get-all.md"


**Returns**

A collection of customer credentials.

Type [`Collection`][goto-collection]


**API Spec**

See the [detailed API spec][7]{: target="_blank"} for all payload fields and response data.

## getCredential
<div class="method"><code><strong>getCredential</strong>({<span class="prop">id</span>}) -> <span class="return">{Member}</span></code></div>

Get a customer credential by its `id`.


**Example**

```js
const credential = await api.customerAuthentication.getCredential({id: 'my-first-id'});
console.log(credential.fields.customerId);
```

**Returns**

A member exposing the customer credential fields.

Type [`Member`][goto-member]


**API Spec**

See the [detailed API spec][8]{: target="_blank"} for all payload fields and response data.


## createCredential
<div class="method"><code><strong>createCredential</strong>({<span class="prop">id</span><span class="optional">opt</span>, <span class="prop">data</span>}) -> <span class="return">{Member}</span></code></div>

Create a credential for a customer. Optionally provide a specific `id` to use, or let Rebilly generate one.

!!! tip "Customer Login"
    Use credentials to allow your customers to login to Rebilly directly through your website and self-serve.

**Example**

```js
// first set the required properties for the new credential
const data = {
    username: 'foobar',
    password: 'fuubar',
    customerId: 'foobar-0001'
    
    // optionally you can define an `expiredTime` to 
    // limit the duration of the credential
    
    //expiredTime: '2017-09-18T19:17:39Z'
};

// the ID is optional
const firstCredential = await api.customerAuthentication.createCredential({data});

// or you can provide one
const secondCredential = await api.customerAuthentication.createCredential({id: 'my-second-id', data});
```

**Returns**

A member exposing the created customer credential fields.

Type [`Member`][goto-member]


**API Spec**

See the [detailed API spec][9]{: target="_blank"} for all payload fields and response data.

## updateCredential
<div class="method"><code><strong>updateCredential</strong>({<span class="prop">id</span>, <span class="prop">data</span>}) -> <span class="return">{Member}</span></code></div>

Update a credential for a customer using its `id`.

**Example**

```js
// define the values to update
const data = {
    username: 'foobar',
    password: 'hell0'
};

const secondCredential = await api.customerAuthentication.updateCredential({id: 'my-second-id', data});
```

**Returns**

A member exposing the update customer credential fields.

Type [`Member`][goto-member]


**API Spec**

See the [detailed API spec][9]{: target="_blank"} for all payload fields and response data.

## deleteCredential
<div class="method"><code><strong>deleteCredential</strong>({<span class="prop">id</span>}) -> <span class="return">{Member}</span></code></div>

Delete a customer credential by using its `id`.  

**Example**

```js
const request = await api.customerAuthentication.deleteCredential({id: 'my-second-key'});

// the request does not return any fields but
// you can confirm the success using the status code
console.log(request.response.status); // 204
```


**Returns**

An empty member without fields. Check the response property to validate the expected status code.

Type [`Member`][goto-member]


**API Spec**

See the [detailed API spec][10]{: target="_blank"} for all payload fields and response data.

## getAllResetPasswordTokens

<div class="method"><code><strong>getAllResetPasswordTokens</strong>({<span class="prop">limit</span><span class="optional">opt</span>, <span class="prop">offset</span><span class="optional">opt</span>}) -> <span class="return">{Collection}</span></code></div>

Get a collection of password reset tokens. Each entry will be a member.

!!! info "Token ID"
    The `token` field of each member is used as the `id` for this entity.


**Example**

```js
// all parameters are optional
const firstCollection = await api.customerAuthentication.getAllResetPasswordTokens();

// alternatively you can specify one or more of them
const params = {limit: 20, offset: 100}; 
const secondCollection = await api.customerAuthentication.getAllResetPasswordTokens(params);

// access the collection items, each item is a Member
secondCollection.items.forEach(token => console.log(token.fields.token));
```

**Parameters**


--8<----- "reference/resources/shared/paged-get-all.md"


**Returns**

A collection of password reset tokens.

Type [`Collection`][goto-collection]


**API Spec**

See the [detailed API spec][11]{: target="_blank"} for all payload fields and response data.

## getResetPasswordToken
<div class="method"><code><strong>getResetPasswordToken</strong>({<span class="prop">id</span>}) -> <span class="return">{Member}</span></code></div>

Get a password reset token by its `id`.


**Example**

```js
const token = await api.customerAuthentication.getResetPasswordToken({id: 'my-first-id'});
console.log(token.fields.credential);
```

**Returns**

A member exposing the password reset token fields.

Type [`Member`][goto-member]


**API Spec**

See the [detailed API spec][12]{: target="_blank"} for all payload fields and response data.

## createResetPasswordToken
<div class="method"><code><strong>createResetPasswordToken</strong>({<span class="prop">data</span>}) -> <span class="return">{Member}</span></code></div>

Create a password reset token for a specific `credential`. Reset tokens are not created for a customer but for a credential attached to it.

!!! info "Token ID"
    The `token` field is used as the `id` for this entity.

> See [customerAuthentication.createCredential][goto-create-cred]

**Example**

```js
// first set the required properties for the new credential
const data = {
    username: 'foobar',
    password: 'fuubar',
    // the `credential` expects 
    // the customer credential's ID
    credential: 'foobar-0001'
    
    // optionally you can define an `expiredTime` to 
    // limit the duration of the reset token
    
    //expiredTime: '2017-09-18T19:17:39Z'
};

const resetToken = await api.customerAuthentication.createResetPasswordToken({data});
```

**Returns**

A member exposing the created password reset token fields.

Type [`Member`][goto-member]


**API Spec**

See the [detailed API spec][13]{: target="_blank"} for all payload fields and response data.

## deleteResetPasswordToken
<div class="method"><code><strong>deleteResetPasswordToken</strong>({<span class="prop">id</span>}) -> <span class="return">{Member}</span></code></div>

Delete a password reset token by using its `id`. 

!!! info "Token ID"
    The `token` field is used as the `id` for this entity.

**Example**

```js
const request = await api.customerAuthentication.deleteResetPasswordToken({id: 'my-second-key'});

// the request does not return any fields but
// you can confirm the success using the status code
console.log(request.response.status); // 204
```


**Returns**

An empty member without fields. Check the response property to validate the expected status code.

Type [`Member`][goto-member]


**API Spec**

See the [detailed API spec][14]{: target="_blank"} for all payload fields and response data.

[goto-rebillyapi]: ../rebilly-api
[goto-collection]: ../types/collection
[goto-member]: ../types/member
[goto-file]: ../types/file
[goto-login]: #login
[goto-account-login]: ./account#signin
[goto-logout]: #logout
[goto-create-cred]: #createcredential
[1]: https://rebilly.github.io/RebillyAPI/#tag/Customer-Authentication/paths/~1authentication-options/get
[2]: https://rebilly.github.io/RebillyAPI/#tag/Customer-Authentication/paths/~1authentication-options/put
[3]: https://rebilly.github.io/RebillyAPI/#tag/Customer-Authentication/paths/~1authentication-tokens/get
[4]: https://rebilly.github.io/RebillyAPI/#tag/Customer-Authentication/paths/~1authentication-tokens~1{token}/get
[5]: https://rebilly.github.io/RebillyAPI/#tag/Customer-Authentication/paths/~1authentication-tokens/post
[6]: https://rebilly.github.io/RebillyAPI/#tag/Customer-Authentication/paths/~1authentication-tokens~1{token}/delete
[7]: https://rebilly.github.io/RebillyAPI/#tag/Customer-Authentication/paths/~1credentials/get
[8]: https://rebilly.github.io/RebillyAPI/#tag/Customer-Authentication/paths/~1credentials~1{id}/get
[9]: https://rebilly.github.io/RebillyAPI/#tag/Customer-Authentication/paths/~1credentials~1{id}/put
[10]: https://rebilly.github.io/RebillyAPI/#tag/Customer-Authentication/paths/~1credentials~1{id}/delete
[11]: https://rebilly.github.io/RebillyAPI/#tag/Customer-Authentication/paths/~1password-tokens/get
[12]: https://rebilly.github.io/RebillyAPI/#tag/Customer-Authentication/paths/~1password-tokens~1{id}/get
[13]: https://rebilly.github.io/RebillyAPI/#tag/Customer-Authentication/paths/~1password-tokens/post
[14]: https://rebilly.github.io/RebillyAPI/#tag/Customer-Authentication/paths/~1password-tokens~1{id}/delete

# Gateway Accounts <small>`:::js api.gatewayAccounts`</small>

> Member of [`RebillyAPI`][goto-rebillyapi]

Create and manage gateway accounts for your business. Select from a list of over 60 different gateways and configure them for active use.

A payment gateway is an e-commerce application service provider service that authorizes credit card payments for e-businesses, online retailers, bricks and clicks, or traditional brick and mortar. It is the equivalent of a physical point of sale terminal located in most retail outlets.



## getAll

--8<----- "reference/resources/shared/full-signature.md"

Get a collection of gateway accounts. Each entry will be a member.


**Example**

```js
// all parameters are optional
const firstCollection = await api.gatewayAccounts.getAll();

// alternatively you can specify one or more of them
const params = {limit: 20, offset: 100, sort: '-createdTime'}; 
const secondCollection = await api.gatewayAccounts.getAll(params);

// access the collection items, each item is a Member
secondCollection.items.forEach(gatewayAccount => console.log(gatewayAccount.fields.gatewayName));
```

**Parameters**


--8<----- "reference/resources/shared/full-get-all.md"


**Returns**

A collection of gateway accounts.

Type [`Collection`][goto-collection]


**API Spec**

See the [detailed API spec][1]{: target="_blank"} for all payload fields and response data.

## get
<div class="method"><code><strong>get</strong>({<span class="prop">id</span>}) -> <span class="return">{Member}</span></code></div>

Get a gateway account by its `id`.


**Example**

```js
const gatewayAccount = await api.gatewayAccounts.get({id: 'foobar-001'});
console.log(gatewayAccount.fields.gatewayName);
```


**Returns**

A member exposing the gateway account fields.

Type [`Member`][goto-member]


**API Spec**

See the [detailed API spec][2]{: target="_blank"} for all payload fields and response data.

## create
<div class="method"><code><strong>create</strong>({<span class="prop">id</span><span class="optional" title="optional">opt</span>, <span class="prop">data</span>}) -> <span class="return">{Member}</span></code></div>

Create a gateway account. Optionally provide a specific `id` to use, or let Rebilly generate one. Each gateway has custom configuration options that are provided using the `gatewayConfig` property. See the [API spec][3] for more details.

An additional gateway named `RebillyProcessor` is available in the sandbox mode and will allow you to test particular scenarios with set credit card numbers. See the [Sandbox vs Live Mode][goto-helpjuice] help article for details.

**Example**

```js
// first set the required properties for the new gateway account
const data = {
    gatewayName: 'RebillyProcessor',
    acquirerName: 'RebillyProcessor',
    merchantCategoryCode: 0,
    acceptedCurrencies: ['USD'],
    method: 'payment-card',
    paymentCardSchemes: [
        'Visa', 'MasterCard', 'American Express', 
        'Discover', 'Diners Club', 'JCB'
    ],
    // the gatewayConfig varies for each gateway name, 
    // see the API spec for details
    gatewayConfig: {},
};

// the ID is optional
const firstKey = await api.gatewayAccounts.create({data});

// or you can provide one
const secondKey = await api.gatewayAccounts.create({id: 'my-second-id', data});
```


**Returns**

A member exposing the created gateway account fields.

Type [`Member`][goto-member]


**API Spec**

See the [detailed API spec][3]{: target="_blank"} for all payload fields and response data.

## update
<div class="method"><code><strong>update</strong>({<span class="prop">id</span>, <span class="prop">data</span>}) -> <span class="return">{Member}</span></code></div>

Update a gateway account using its `id`. This method will `patch` the existing values, allowing you to skip gateway credentials.

**Example**

```js
// build data with only the fields you wish to update
const data = {
    paymentCardSchemes: [
            'Visa', 'MasterCard', 'American Express'
    ]
};

const secondKey = await api.gatewayAccounts.update({id: 'my-second-id', data});
```


**Returns**

A member exposing the updated gateway account fields.

Type [`Member`][goto-member]


**API Spec**

See the [detailed API spec][5]{: target="_blank"} for all payload fields and response data.

## delete
<div class="method"><code><strong>delete</strong>({<span class="prop">id</span>}) -> <span class="return">{Member}</span></code></div>

Delete a gateway account by using its `id`. You cannot delete a gateway account that has been used previously.


**Example**

```js
const request = await api.gatewayAccounts.delete({id: 'my-second-key'});

// the request does not return any fields but
// you can confirm the success using the status code
console.log(request.response.status); // 204
```


**Returns**

An empty member without fields. Check the response property to validate the expected status code.

Type [`Member`][goto-member]


**API Spec**

See the [detailed API spec][4]{: target="_blank"} for all payload fields and response data.


## enable
<div class="method"><code><strong>enable</strong>({<span class="prop">id</span>}) -> <span class="return">{Member}</span></code></div>

Enable an inactive gateway account by using its `id`.


**Example**

```js
const gatewayAccount = await api.gatewayAccounts.enable({id: 'foobar-001'});
console.log(gatewayAccount.fields.status);
```


**Returns**

A member exposing the gateway account fields.

Type [`Member`][goto-member]


**API Spec**

See the [detailed API spec][6]{: target="_blank"} for all payload fields and response data.

## disable
<div class="method"><code><strong>disable</strong>({<span class="prop">id</span>}) -> <span class="return">{Member}</span></code></div>

Disable an active gateway account by using its `id`.


**Example**

```js
const gatewayAccount = await api.gatewayAccounts.disable({id: 'foobar-001'});
console.log(gatewayAccount.fields.status);
```


**Returns**

A member exposing the gateway account fields.

Type [`Member`][goto-member]


**API Spec**

See the [detailed API spec][7]{: target="_blank"} for all payload fields and response data.


## getAllDowntimeSchedules

<div class="method">
    <code>
        <strong>getAllDowntimeSchedules</strong>({
        <span class="prop">id</span>,
        <span class="prop">limit</span><span class="optional" title="optional">opt</span>,
        <span class="prop">offset</span><span class="optional" title="optional">opt</span>,
        <span class="prop">filter</span><span class="optional" title="optional">opt</span>
        }) -> <span class="return">{Collection}</span>
    </code>
</div>

Get a collection of gateway account downtime schedules for a gateway `id`. Each entry will be a member.


**Example**

```js
// all parameters are optional except for the `id`
const firstCollection = await api.gatewayAccounts.getAllDowntimeSchedules({id: 'my-gateway');

// alternatively you can specify one or more of them
const params = {id: 'my-gateway', limit: 20, offset: 100};
const secondCollection = await api.gatewayAccounts.getAllDowntimeSchedules(params);

// access the collection items, each item is a Member
secondCollection.items
    .forEach(gatewayAccount => console.log(gatewayAccount.fields.reason));
```

**Parameters**

| Name | Type | Attribute | Description |
| - | - | - | - |
| limit | number | Optional | The amount of members to return per request.<br>Defaults to `100`. |
| offset | number | Optional | Member index from which to start returning results. <br>Defaults to `0`. |
| filter | string | Optional | A list of one or more member fields and their values, used to filter the collection results.<br>Example: `status:active`.<br> See the [filters guide][guide-filters] for more details. |


[guide-filters]: ../../guides/filters.md


**Returns**

A collection of gateway account downtime schedules.

Type [`Collection`][goto-collection]


**API Spec**

See the [detailed API spec][8]{: target="_blank"} for all payload fields and response data.


## getDowntimeSchedule
<div class="method"><code><strong>getDowntimeSchedule</strong>({<span class="prop">id</span>, <span class="prop">downtimeScheduleId</span>}) -> <span class="return">{Member}</span></code></div>

Get a gateway account downtime schedule by its `id` and `downtimeScheduleId` combination.


**Example**

```js
const downtimeSchedule = await api.gatewayAccounts.getDowntimeSchedule({id: 'foobar-001', downtimeScheduleId: 'foobar-202'});
console.log(downtimeSchedule.fields.reason);
```


**Returns**

A member exposing the gateway account downtime schedule fields.

Type [`Member`][goto-member]


**API Spec**

See the [detailed API spec][9]{: target="_blank"} for all payload fields and response data.


## createDowntimeSchedule
<div class="method"><code><strong>createDowntimeSchedule</strong>({<span class="prop">id</span>, <span class="prop">data</span>}) -> <span class="return">{Member}</span></code></div>

Create a gateway account downtime schedule using the gateway's `id`.

**Example**

```js
// first set the required properties for
// the new gateway account downtime schedule
const data = {
    startTime: '2018-08-02T15:13:23Z',
    endTime: '2018-08-02T15:13:23Z'
};

// the gateway ID is required
const secondKey = await api.gatewayAccounts.createDowntimeSchedule({id: 'my-second-id', data});
```


**Returns**

A member exposing the created gateway account downtime schedule fields.

Type [`Member`][goto-member]


**API Spec**

See the [detailed API spec][10]{: target="_blank"} for all payload fields and response data.

## updateDowntimeSchedule
<div class="method"><code><strong>updateDowntimeSchedule</strong>({<span class="prop">id</span>, <span class="prop">downtimeScheduleId</span>, <span class="prop">data</span>}) -> <span class="return">{Member}</span></code></div>

Update a gateway account downtime schedule using a combination of the gateway's `id` and the schedule's `downtimeScheduleId`.

**Example**

```js
// first set the required properties for
// the update gateway account downtime schedule
const data = {
    startTime: '2018-08-02T15:13:23Z',
    endTime: '2018-08-02T15:13:23Z'
};

// the gateway ID is required
const secondKey = await api.gatewayAccounts
    .updateDowntimeSchedule({id: 'my-second-id', downtimeScheduleId: 'schedule-id', data});
```


**Returns**

A member exposing the created gateway account downtime schedule fields.

Type [`Member`][goto-member]


**API Spec**

See the [detailed API spec][11]{: target="_blank"} for all payload fields and response data.

## deleteDowntimeSchedule
<div class="method"><code><strong>deleteDowntimeSchedule</strong>({<span class="prop">id</span>, <span class="prop">downtimeScheduleId</span>}) -> <span class="return">{Member}</span></code></div>

Delete a gateway account downtime schedule by using a combination of the gateway's `id` and the schedule's `downtimeScheduleId`.


**Example**

```js
const request = await api.gatewayAccounts
    .deleteDowntimeSchedule({id: 'my-second-key', downtimeScheduleId: 'schedule-id'});

// the request does not return any fields but
// you can confirm the success using the status code
console.log(request.response.status); // 204
```


**Returns**

An empty member without fields. Check the response property to validate the expected status code.

Type [`Member`][goto-member]


**API Spec**

See the [detailed API spec][12]{: target="_blank"} for all payload fields and response data.


## getAllTimelineMessages

<div class="method">
    <code>
        <strong>getAllTimelineMessages</strong>({
        <span class="prop">id</span>,
        <span class="prop">limit</span><span class="optional" title="optional">opt</span>,
        <span class="prop">offset</span><span class="optional" title="optional">opt</span>,
        <span class="prop">sort</span><span class="optional" title="optional">opt</span>,
        <span class="prop">filter</span><span class="optional" title="optional">opt</span>
        }) -> <span class="return">{Collection}</span>
    </code>
</div>

Get a collection of gateway account timeline messages for a gateway `id`. Each entry will be a member.


**Example**

```js
// all parameters are optional except for the `id`
const firstCollection = await api.gatewayAccounts
    .getAllTimelineMessages({id: 'my-gateway');

// alternatively you can specify one or more of them
const params = {id: 'my-gateway', limit: 20, offset: 100};
const secondCollection = await api.gatewayAccounts.getAllTimelineMessages(params);

// access the collection items, each item is a Member
secondCollection.items
    .forEach(message => console.log(message.fields.eventType));
```

**Parameters**

| Name | Type | Attribute | Description |
| - | - | - | - |
| limit | number | Optional | The amount of members to return per request.<br>Defaults to `100`. |
| offset | number | Optional | Member index from which to start returning results. <br>Defaults to `0`. |
| sort | string | Optional | The member field on which to sort on. Sorting is ascending by default. Use `-` (dash) to make it descending.<br>Example: `createdTime` and `-createdTime`. |
| filter | string | Optional | A list of one or more member fields and their values, used to filter the collection results.<br>Example: `status:active`.<br> See the [filters guide][guide-filters] for more details. |


[guide-filters]: ../../guides/filters.md


**Returns**

A collection of gateway account timeline messages.

Type [`Collection`][goto-collection]


**API Spec**

See the [detailed API spec][13]{: target="_blank"} for all payload fields and response data.


## getTimelineMessage
<div class="method">
    <code>
        <strong>getTimelineMessage</strong>({
        <span class="prop">id</span>,
        <span class="prop">messageId</span>
        }) -> <span class="return">{Member}</span></code></div>

Get a gateway account timeline message by its `id` and `messageId` combination.


**Example**

```js
const message = await api.gatewayAccounts
    .getTimelineMessage({id: 'foobar-001', messageId: 'message-202'});
console.log(message.fields.eventType);
```


**Returns**

A member exposing the gateway account timeline message fields.

Type [`Member`][goto-member]


**API Spec**

See the [detailed API spec][14]{: target="_blank"} for all payload fields and response data.

## createTimelineComment
<div class="method">
    <code>
        <strong>createTimelineComment</strong>({
        <span class="prop">id</span>,
        <span class="prop">data</span>
        }) -> <span class="return">{Member}</span>
    </code>
</div>

Create a timeline comment associated to a gateway account by defining its `id` and `data`. The comment message to be posted must be defined in `message` string attribute of the `data` object.


Mentions to users and references to resources can be set inside the message string. See the [detailed API spec][15]{: target="_blank"} for details.

**Example**

```js
// Create a comment
const firstComment = await api
    .gatewayAccounts.createTimelineComment({id: 'my-gateway-id', data: {message: 'Your comment here'}});

// Using params object, mentions and references
const message = `Example of mentions @user@mydomain.com and references #customers-customer-id`;
const params = {
    id: 'my-gateway-id'
    data: {
        message,
    },
};
const secondComment = await api.gatewayAccounts.createTimelineComment(params);
```


**Returns**

A member exposing the created gateway timeline comment fields.

Type [`Member`][goto-member]


**API Spec**

See the [detailed API spec][15]{: target="_blank"} for all payload fields and response data.


## deleteTimelineMessage
<div class="method">
    <code>
        <strong>deleteTimelineMessage</strong>({
        <span class="prop">id</span>,
        <span class="prop">messageId</span>
        }) -> <span class="return">{Member}</span>
    </code>
</div>

Delete a gateway timeline message by using its `id` and `messageId` combination.


**Example**

```js
const request = await api.gatewayAccounts
    .deleteTimelineMessage({id: 'foobar-001', messageId: 'message-202'});

// the request does not return any fields but
// you can confirm the success using the status code
console.log(request.response.status); // 204
```


**Returns**

An empty member without fields. Check the response property to validate the expected status code.

Type [`Member`][goto-member]


**API Spec**

See the [detailed API spec][16]{: target="_blank"} for all payload fields and response data.


[goto-rebillyapi]: ../rebilly-api
[goto-collection]: ../types/collection
[goto-member]: ../types/member
[goto-helpjuice]: https://help.rebilly.com/35300-rebilly-basics/sandbox-vs-live-mode
[1]: https://rebilly.github.io/RebillyUserAPI/#tag/Gateway-Accounts/paths/~1gateway-accounts/get
[2]: https://rebilly.github.io/RebillyUserAPI/#tag/Gateway-Accounts/paths/~1gateway-accounts~1{id}/get
[3]: https://rebilly.github.io/RebillyUserAPI/#tag/Gateway-Accounts/paths/~1gateway-accounts~1{id}/put
[4]: https://rebilly.github.io/RebillyUserAPI/#tag/Gateway-Accounts/paths/~1gateway-accounts~1{id}/delete
[5]: https://rebilly.github.io/RebillyUserAPI/#tag/Gateway-Accounts/paths/~1gateway-accounts~1{id}/patch
[6]: https://rebilly.github.io/RebillyUserAPI/#tag/Gateway-Accounts/paths/~1gateway-accounts~1{id}~1enable/post
[7]: https://rebilly.github.io/RebillyUserAPI/#tag/Gateway-Accounts/paths/~1gateway-accounts~1{id}~1disable/post
[8]: https://rebilly.github.io/RebillyUserAPI/#tag/Gateway-Accounts/paths/~1gateway-accounts~1{id}~1downtime-schedules/get
[9]: https://rebilly.github.io/RebillyUserAPI/#tag/Gateway-Accounts/paths/~1gateway-accounts~1{id}~1downtime-schedules~1{downtimeId}/get
[10]: https://rebilly.github.io/RebillyUserAPI/#tag/Gateway-Accounts/paths/~1gateway-accounts~1{id}~1downtime-schedules/post
[11]: https://rebilly.github.io/RebillyUserAPI/#tag/Gateway-Accounts/paths/~1gateway-accounts~1{id}~1downtime-schedules~1{downtimeId}/put
[12]: https://rebilly.github.io/RebillyUserAPI/#tag/Gateway-Accounts/paths/~1gateway-accounts~1{id}~1downtime-schedules~1{downtimeId}/delete
[13]: https://rebilly.github.io/RebillyUserAPI/#tag/Gateway-Accounts/paths/~1gateway-accounts~1{id}~1timeline/get
[14]: https://rebilly.github.io/RebillyUserAPI/#tag/Gateway-Accounts/paths/~1gateway-accounts~1{id}~1timeline~1{messageId}/get
[15]: https://rebilly.github.io/RebillyUserAPI/#tag/Gateway-Accounts/paths/~1gateway-accounts~1{id}~1timeline/post
[16]: https://rebilly.github.io/RebillyUserAPI/#tag/Gateway-Accounts/paths/~1gateway-accounts~1{id}~1timeline~1{messageId}/delete

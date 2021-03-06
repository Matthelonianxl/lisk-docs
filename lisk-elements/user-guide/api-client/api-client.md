# Lisk Elements API Client

The Lisk Elements API Client provides a convenient wrapper for interacting with the public API of nodes on the Lisk network.

## `APIClient`

We expose a constructor, which takes an array of nodes, and optionally an options object. For convenience, we provide helper functions for creating API clients on specific networks with a default set of nodes. We recommend using these functions unless you operate your own nodes, or have good reasons to prefer nodes provided by a third party. *If you use the generic constructor, it is your responsibility to ensure that the nodes you specify are on the correct network.*

#### Syntax

```js
new APIClient(nodes, [options])
APIClient.createMainnetAPIClient([options])
APIClient.createBetanetAPIClient([options])
APIClient.createTestnetAPIClient([options])
```

#### Parameters

`nodes`: When calling the constructor directly, a list of nodes must be provided for the API client to connect to as an array of strings.

`options`: Additional configuration for the client can be specified via an optional option object:
- `bannedNodes`: An array of nodes which should not be used (overrides the array of nodes used to initialise the client).
- `client`: An object specifying certain details about your client, which will be included as part of the `User-Agent` header when sending HTTP requests. Available keys are `name`, `version` and `engine`.
- `nethash`: The nethash of the network your client will interact with.
- `node`: The node to use first (overrides the order of the array of nodes used to initialise the client).
- `randomizeNodes`: Whether a random node should be selected after one becomes unreachable. (Default: `true`.)

#### Return value

Instance of `APIClient`.

#### Examples

```js
import lisk from 'lisk-elements';

const client = new lisk.APIClient(['https://node01.lisk.io:443', 'https://node02.lisk.io:443']);
const clientWithOptions = new lisk.APIClient(
    ['https://node01.lisk.io:443', 'https://node02.lisk.io:443'],
    {
        bannedNodes: ['https://my.faultynode.io:443'],
        client: {
            name: 'My Lisk Client',
            version: '1.2.3',
            engine: 'Some custom engine',
        },
        nethash: '9a9813156bf1d2355da31a171e37f97dfa7568187c3fd7f9c728de8f180c19c7',
        node: 'https://my.preferrednode.io:443',
        randomizeNodes: false,
    }
);

const mainnetClient = lisk.APIClient.createMainnetAPIClient();
const testnetClient = lisk.APIClient.createTestnetAPIClient();
const betanetClient = lisk.APIClient.createBetanetAPIClient({ randomizeNodes: false });
```

## Constants

API-specific constants are available via the `APIClient` constructor, and include relevant HTTP methods and lists of default nodes.

#### Examples

```js
import lisk from 'lisk-elements';

lisk.APIClient.constants.GET; // 'GET'
lisk.APIClient.constants.POST; // 'POST'
lisk.APIClient.constants.PUT; // 'PUT'

lisk.APIClient.constants.BETANET_NODES; // Array of default betanet nodes
lisk.APIClient.constants.TESTNET_NODES; // Array of default testnet nodes
lisk.APIClient.constants.MAINNET_NODES; // Array of default mainnet nodes
```

## Methods associated with a resource

Requests to a node are made via the `APIClient` instance’s respective resource, and return a promise. In the case of a response with a status code in the `2xx` range, these promises are fulfilled with a relevant object, otherwise they are rejected with an appropriate error message.

Documentation for each resource can be found on the following pages:
- [Accounts](accounts/accounts.md)
- [Blocks](blocks/blocks.md)
- [Dapps](dapps/dapps.md)
- [Delegates](delegates/delegates.md)
- [Node](node/node.md)
- [Peers](peers/peers.md)
- [Signatures](signatures/signatures.md)
- [Transactions](transactions/transactions.md)
- [Voters](voters/voters.md)
- [Votes](votes/votes.md)

## Methods not associated with a resource

### `initialize`

Initialises the client instance with an array of nodes and an optional configuration object. This is called in the constructor, but can be called again later if necessary. (Note that in practice it is usually easier just to create a new instance.)

#### Syntax

```js
initialize(nodes, [options])
```

#### Parameters

The parameters are the same as for the constructor.

#### Return value

`undefined`

#### Examples

```js
client.initialize(['https://node01.lisk.io:443', 'https://node02.lisk.io:443']);
client.initialize(
    ['https://node01.lisk.io:443', 'https://node02.lisk.io:443'],
    {
        bannedNodes: ['https://my.faultynode.io:443'],
        client: {
            name: 'My Lisk Client',
            version: '1.2.3',
            engine: 'Some custom engine',
        },
        nethash: '9a9813156bf1d2355da31a171e37f97dfa7568187c3fd7f9c728de8f180c19c7',
        node: 'https://my.preferrednode.io:443',
        randomizeNodes: false,
    }
);
```

### `getNewNode`

Selects a random node that has not been banned.

#### Syntax

```js
getNewNode()
```

#### Parameters

n/a

#### Return value

`string`: One of the node URLs provided during intialisation.

#### Examples

```js
const randomNode = client.getNewNode();
```

### `banNode`

Adds a node to the list of banned nodes. Banned nodes will not be chosen to replace an unreachable node.

#### Syntax

```js
banNode(node)
```

#### Parameters

`node`: String URL of the node that should be banned.

#### Return value

`boolean`: `false` if the node is already banned, otherwise `true`.

#### Examples

```js
client.banNode('https://my.faultynode.io:443');
```

### `banActiveNodeAndSelect`

Bans the current node and selects a new random (non-banned) node.

#### Syntax

```js
banActiveNodeAndSelect()
```

#### Parameters

n/a

#### Return value

`boolean`: `false` if the current node is already banned, otherwise `true`.

#### Examples

```js
client.banActiveNodeAndSelect();
```

### `hasAvailableNodes`

Tells you whether all the nodes have been banned or not.

#### Syntax

```js
hasAvailableNodes()
```

#### Parameters

n/a

#### Return value

`boolean`: `false` if all nodes have been banned, otherwise `true`.

#### Examples

```js
const moreNodesNeeded = !client.hasAvailableNodes();
```

### `isBanned`

Tells you whether a specific node has been banned or not.

#### Syntax

```js
isBanned(node)
```

#### Parameters

`node`: String URL of the node to check.

#### Return value

`boolean`: `true` if the node has been banned, otherwise `false`.

#### Examples

```js
const banned = client.isBanned('https://node01.lisk.io:443');
```

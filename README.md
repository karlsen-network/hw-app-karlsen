![LEDGER](https://user-images.githubusercontent.com/4631227/191834116-59cf590e-25cc-4956-ae5c-812ea464f324.png)

## hw-app-karlsen

Ledger Hardware Wallet Karlsen JavaScript bindings.

## API

### Table of Contents

* [Karlsen](#karlsen)
  * [Parameters](#parameters)
  * [Examples](#examples)
  * [getPublicKey](#getaddress)
    * [Parameters](#parameters-1)
    * [Examples](#examples-1)
  * [signTransaction](#signtransaction)
    * [Parameters](#parameters-2)
    * [Examples](#examples-2)

### Karlsen

Karlsen API

#### Parameters

* `transport` **Transport** a transport for sending commands to a
  device

#### Examples

```javascript
import Karlsen from "hw-app-karlsen";
const karlsen = new Karlsen(transport);
```

#### getPublicKey

Get Karlsen Public Key for a BIP32 path.

##### Parameters

* `path` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)**
  a BIP32 path
* `display` **[boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)**
  flag to show display (optional, default `false`)

##### Examples

```javascript
karlsen.getPublicKey("44'/121337'/0'")
```

Returns **[Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)
<[Buffer](https://nodejs.org/api/buffer.html)>** the public key
buffer with chain code.

#### signTransaction

Sign a Karlsen transaction.

##### Parameters

* `transaction` **Transaction** from `src/transaction.js`

##### Examples

```typescript
import Karlsen from 'hw-app-karlsen';
import { TransactionInput, TransactionOutput, Transaction } from 'hw-app-karlsen';

...

const karlsen = new Karlsen(transport);

const txin = new TransactionInput({
    prevTxId: "40b022362f1a303518e2b49f86f87a317c87b514ca0f3d08ad2e7cf49d08cc70",
    value: 1100000,
    addressType: 0,
    addressIndex: 0,
    outpointIndex: 0,
});

const txout = new TransactionOutput({
    value: 1000000,
    scriptPublicKey: "2011a7215f668e921013eb7aac9b7e64b9ec6e757c1b648e89388c919f676aa88cac",
});

// By convention, the second output MUST be the change address
// It MUST set both addressType and addressIndex
const txoutchange = new TransactionOutput({
    value: 90000,
    scriptPublicKey: "2011a7215f668e921013eb7aac9b7e64b9ec6e757c1b648e89388c919f676aa88cac",
});

const tx = new Transaction({
    version: 0,
    changeAddressType: 0,
    changeAddressIndex: 0,
    inputs: [txin],
    outputs: [txout, txoutchange],
});

karlsen.signTransaction(tx);
```

Updates the transaction by filling in the `signature` property of
each `TransactionInput` in the `Transaction` object.

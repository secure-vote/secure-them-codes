# Secure Them Codes!

This is a library / utility for securely downloading larger JS files
in a totally distributed fashion. Provided everything you need for your
app is in the JS file downloaded, you should _never_ have to change your
frontend. It integrates with Ethereum (or any similar network) and will
retrive a reference to the latest version, securely download that file,
then load that dynamically into the browser. It is not as fast as directly
loading JS from a webserver, but it is more secure.

## Overview

The idea is simple enough:

* Have some network like Ethereum
* Load a smart contract on it that allows you to serve an up to date
    reference to your latest code
* Have a network like IPFS where you can reference content via hash
    (note: bittorrent could work here too)
* Deploy your code by first ensuring it's available on IPFS, then record
    the hash in your smart contract

It's easy enough to add streams (like 'beta', 'nightly', etc) and automate
this process.

From a user's perspective they load the minimum to

* display some nice loading screen
* securely fetch the code via Etheruem and IPFS (using multiple public nodes)

## Usage

```javascript
const stc = require('secure-them-codes');
stc(loadingHtml, loader);
```

### What's a Loader Object?

Loader Objects conform to one of the following:

* `{ loader: "ens-ipfs", ref: "code.secvote.eth", extra: "<STREAM>" }`
    Use ENS (Ethereum Name Service) and IPFS, starting with the provided
    ENS name and presuming it points to a `StcReleases.sol` contract.
    `<STREAM>` can be the name of a release stream, or a particular
    hash.
* `{ loader: "ipfs", ref "<MULTIHASH>" }`
    Just load straight from the provided multihash (or CID).
* `{ loader "ens-mango", ref: "code.secure.vote", extra: "<BRANCH>" }`
    Use ENS and [mango] (git via ethereum / IPFS).
    `<BRANCH>` can be a branch name or release or even a particular hash.
* `{ loader "custom", ref: <LOADER_OBJECT>, extra: <ANYTHING YOU LIKE> }`

**Note:** the `extra` param is _always_ optional - each .

#### Loader Objects

Loader objects should look like:

```typescript
{
    toString: () => String,
    runDefault: () => Uint8Array,
    runWExtra: (e: ExtraDataType) => Uint8Array
}
```

The binary produced by `runDefault` or `runWExtra` will be injected into the page.

### From JS Projects:

__todo: instructions for running from JS projects__

## Planned features:

* Add your own data-source layers (e.g. replace Ethereum, or replace IPFS
    individually)

[mango]: https://github.com/axic/mango
# Web3 - Build a Smart Contract in Rust

## 1

### --description--

For this project, you need to create and deploy a Smart Contract using Rust that keeps track of how many users have clicked a button on your web page.

You are started with Rust library consisting of all the unit and integration tests you are expected to pass. The boilerplate already contains the necessary crates to complete the user stories, but you will need to import them.

You will be required to define structs and functions matching those used in the tests. You should only need to work in the `build-a-smart-contract-in-rust/src/lib.rs` file.

**Commands**

- Run all unit tests: `cargo test --lib`
- Run all integration tests: `wasm-pack test --firefox --headless`

_Note: You need to run the tests from within the library directory._

**User Stories:**

- Your smart contract uses `wasm-pack` to compile the Rust code to JavaScript for Nodejs
- Your smart contract exports an `initialise` function that returns `Result<JsValue, JsError>`
  - This function takes no arguments
  - This function returns a `Context` wrapped in a `JsValue`
- Your smart contract exports a `set_click` function that returns `Result<JsValue, JsError>`
  - This function accepts a `JsValue` with the `Context` type
  - This function also accepts a `String` as the second argument which is the address of the user who clicked the button
- Your smart contract exports a `get_contract_account` function that returns `Result<JsValue, JsError>`
  - This function accepts a `JsValue` with the `Context` type
  - This function returns an `Account` wrapped in a `JsValue`
- Your library should export a struct named `Account` with the following fields:
  - `total_clicks` which is a `u64`
  - `clickers` which is a `Vec<String>`
- Your library should export a struct named `Context` with the following field:
  - `base_account` which is an `Account`
- You should deploy your smart contract using `node node/deploy.js <path_to_your_pkg_directory>`
  - You should run `node node/smart-contract.js <contract_id> set_click <clicker_name>` to add at least 3 clickers to the contract

**Useful Resources:**

- `https://www.rust-lang.org/what/wasm` - WebAssembly - Rust Programming Language
- `https://rustwasm.github.io/docs/book/` - RustWASM Book
- `https://developer.mozilla.org/en-US/docs/WebAssembly/Rust_to_wasm` - Compiling from Rust to WebAssembly - WebAssembly | MDN
- `https://rustwasm.github.io/wasm-bindgen/` - wasm-bindgen guide
- `https://rustwasm.github.io/wasm-pack/` - wasm-pack

**Tips:**

- You can wrap a value in a `JsValue` using `JsValue::from_serde`
- You can serialize a `JsValue` using the `.into_serde()` method
- Build your smart contract with `nodejs` as the target

### --tests--

Your smart contract should pass all `lib::tests` unit tests.

```js
const { stdout } = await __helpers.getCommandOutput(
  'cargo test --lib tests',
  __projectLoc
);
assert.match(stdout, /test result: ok/);
```

Your smart contract should pass all `initialise` integration tests.

```js
const { stdout } = await __helpers.getCommandOutput(
  'wasm-pack test --firefox --headless -- --test initialise',
  __projectLoc
);
assert.match(stdout, /test result: ok/);
```

Your smart contract should pass all `set_click` integration tests.

```js
const { stdout } = await __helpers.getCommandOutput(
  'wasm-pack test --firefox --headless -- --test set_click',
  __projectLoc
);
assert.match(stdout, /test result: ok/);
```

Your smart contract should pass all `get_contract_account` integration tests.

```js
const { stdout } = await __helpers.getCommandOutput(
  'wasm-pack test --firefox --headless -- --test get_contract_account',
  __projectLoc
);
assert.match(stdout, /test result: ok/);
```

You should deploy your smart contract using `node node/deploy.js <path_to_your_pkg_directory>`.

```js
const blockchain = JSON.parse(
  await __helpers.getFile(`${__projectLoc}/node/data/blockchain.json`)
);
assert.exists(blockchain, 'Could not find blockchain');
assert.isAtLeast(
  blockchain.length,
  2,
  'Expected blockchain to contain at least 2 blocks'
);
assert.equal(blockchain[1].smartContracts.length, 1);
```

You should add at least 3 different clickers to your contract state.

```js
const blockchain = JSON.parse(
  await __helpers.getFile(`${__projectLoc}/node/data/blockchain.json`)
);
const smartContract = blockchain
  .reverse()
  .find(b => b.smartContracts.length > 0)?.smartContracts?.[0];
assert.exists(smartContract, 'Smart contract not found');
assert.isAtLeast(
  smartContract.state.base_account.clickers.length,
  3,
  'Not enough clickers'
);
assert.isAtLeast(
  smartContract.state.base_account.total_clicks,
  3,
  'Not enough total_clicks'
);
```

### --before-all--

```js
global.__projectLoc = 'build-a-smart-contract-in-rust';
```

## --fcc-end--

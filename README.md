# AutoExe
AutoExe is a helper function that can run code in Code Blocks through various means, i.e. World Code, and other Code Blocks, without having the need to click on the Code Block.

## Description
Global object `exe` has a few methods. These methods allow code to be executed through different means, for example, through World Code. AutoExe can also help by allowing a user to extend their code, as there is a limit for the number of characters in World Code. Using AutoExe and global object `exe`, a user can execute code in code blocks to initialise parts of their World Code.

`exe` also lets the user check whether a specific code block has been executed before. The user may also choose to remove a specific log of a code's execution.

However, the amount of characters in each code block is limited. It is recommended to write code such that it only takes up about three quarters of the screen. You can also use minified code to allow for more things to be executed (code can be minified in multiple websites and sources; my recommendation is [Minify JS](https://minify-js.com "JS Minifier")).

## Installation
AutoExe can be installed into your Bloxd.io world by **copy-pasting the code below into World Code**.

**Callbacks used**: None<br>
**Variables used**: `exe`, `exec`

```js
exec=new Set,exe={run:(pos,y,z)=>{if(Array.isArray(pos)){api.getBlock(pos);let data=api.getBlockData(...pos)?.persisted?.shared?.text;return eval(data),exec.add(JSON.stringify(pos)),data}if("number"==typeof pos&&"number"==typeof y&&"number"==typeof z){api.getBlock(pos,y,z);let data=api.getBlockData(pos,y,z)?.persisted?.shared?.text;return eval(data),exec.add(JSON.stringify([pos,y,z])),data}throw new Error("Invalid arguments inputted. Expected type array, or number for x, y and z")},isExec:(e,r,t)=>{if(Array.isArray(e))return[...exec].includes(JSON.stringify(e));if("number"==typeof e&&"number"==typeof r&&"number"==typeof t)return[...exec].includes(JSON.stringify([e,r,t]));throw new Error("Invalid arguments inputted. Expected type array, or number for x, y and z")},removeLog:(e,r,t)=>{if(Array.isArray(e))exec.delete(JSON.stringify(e));else{if("number"!=typeof e||"number"!=typeof r||"number"!=typeof t)throw new Error("Invalid arguments inputted. Expected type array, or number for x, y and z");exec.delete(JSON.stringify([e,r,t]))}}};
```

## Usage

### Methods
The usable methods of global object `exe` is listed below.
```js
/**
 * Runs a code block at a given position.
 * @param {number | number[]} x - Can also be an array, in which case y and z shouldn't be passed
 * @param {number} [y]
 * @param {number} [z]
 * @returns {string} Code
 */
run(x, y, z)

/**
 * Checks whether a code block has been previously executed.
 * Do note that this method requires set exec to be intialised. If set exec is not initialised or deleted/removed by the user, this method will not work.
 * @param {number | number[]} x - Can also be an array, in which case y and z shouldn't be passed
 * @param {number} [y]
 * @param {number} [z]
 * @returns {boolean}
 */
isExec(x, y, z)

/**
 * Removes the execution log of a code block at a given position.
 * Do note that this method requires set exec to be intialised. If set exec is not initialised or deleted/removed by the user, this method will not work.
 * @param {number | number[]} x - Can also be an array, in which case y and z shouldn't be passed
 * @param {number} [y]
 * @param {number} [z]
 * @returns {void}
 */
removeLog(x, y, z)
```

### Use Case
AutoExe can be combined with callback `tick` to execute code in Code Blocks at the first tick (after the chunk is loaded). Example:
```js
// ... Code
tick = (ms) => {
  if (!exe.isExec(0, 0, 0)) {
    exe.run(0, 0, 0)
  }
}
```

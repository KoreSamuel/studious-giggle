# studious-giggle

## done
### [is-sorted](https://github.com/dcousens/is-sorted) 
> A small module to check if an Array is sorted.

```
/**
 * 两个参数，一个数组 array，一个 comparator。
 * Array.isArray() 判断参数是否是数组，遍历数组，使用comparator(array[i - 1], array[i]) 判断，若大于 0，则返回 false，否则返回 true
 */
function defaultComparator (a, b) {
  return a - b
}

module.exports = function checksort (array, comparator) {
  if (!Array.isArray(array)) throw new TypeError('Expected Array, got ' + (typeof array))
  comparator = comparator || defaultComparator

  for (var i = 1, length = array.length; i < length; ++i) {
    if (comparator(array[i - 1], array[i]) > 0) return false
  }

  return true
}
```
### [array-first](https://github.com/jonschlinkert/array-first) 
> Get the first element or first n elements of an array.

```
/**
 * 两个参数，一个数组 array, 一个返回数量 num，默认为 1.
 * 校验参数，是否为数组，数组长度。
 * 截取前 num 个元素返回
 */
var isNumber = require('is-number');
var slice = require('array-slice');

module.exports = function arrayFirst(arr, num) {
  if (!Array.isArray(arr)) {
    throw new Error('array-first expects an array as the first argument.');
  }

  if (arr.length === 0) {
    return null;
  }

  var first = slice(arr, 0, isNumber(num) ? +num : 1);
  if (+num === 1 || num == null) {
    return first[0];
  }
  return first;
};
```
### [array-last](https://github.com/jonschlinkert/array-last)
> Return the last element or last n elements in an array. Faster than `.slice`

```
/**
 * 返回数组最后一个或n个元素
 * 同样两个参数
 * 新建n空间空数组res，依次将后n个元素放入新数组，最后返回res
 * 未判断arr.length < n, 会出现 last([1, 2, 3, 4], 5) => [undefined, 1, 2, 3, 4]
 */
'use strict'
var isNumber = require('is-number');

module.exports = function last(arr, n) {
  if (!Array.isArray(arr)) {
    throw new Error('expected the first argument to be an array');
  }

  var len = arr.length;
  if (len === 0) {
    return null;
  }

  n = isNumber(n) ? +n : 1;
  // n = isNumber(n) ? arr.length < +n ? arr.length : +n : 1; 增加数组长度和n比较，抛出错误或取数组长度
  if (n === 1) {
    return arr[len - 1];
  }

  var res = new Array(n);
  while (n--) {
    res[n] = arr[--len];
  }
  return res;
};
```
### [arr-flatten] (https://github.com/jonschlinkert/arr-flatten)
> Recursively flatten an array or arrays. This is the fastest implementation of array flatten.

```
/**
 * 数组扁平化
 * 遍历数组，递归调用，判断 Array.isArray
 */
'use strict'
module.exports = function (arr) {
  return flat(arr, []);
};

function flat(arr, res) {
  var i = 0, cur;
  var len = arr.length;
  for (; i < len; i++) {
    cur = arr[i];
    Array.isArray(cur) ? flat(cur, res) : res.push(cur);
  }
  return res;
}
```
### [dedupe](https://github.com/seriousManual/dedupe)
> easy deduplication of array values

```
/**
 * 数组去重
 * @params {Array} client 需要处理的数组
 * @params {Function} hasher 处理元素的函数，为保证hash key的唯一性
 */
'use strict'

function dedupe (client, hasher) {
    hasher = hasher || JSON.stringify

    const clone = []
    const lookup = {}

    for (let i = 0; i < client.length; i++) {
        let elem = client[i]
        let hashed = hasher(elem)

        if (!lookup[hashed]) {
            clone.push(elem)
            lookup[hashed] = true
        }
    }

    return clone
}

module.exports = dedupe
```

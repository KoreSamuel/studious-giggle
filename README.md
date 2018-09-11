# studious-giggle

## done
### [is-sorted](https://github.com/dcousens/is-sorted)
> A small module to check if an Array is sorted.

```javascript
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

```javascript
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

```javascript
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
### [arr-flatten](https://github.com/jonschlinkert/arr-flatten)
> Recursively flatten an array or arrays. This is the fastest implementation of array flatten.

```javascript
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

```javascript
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
### [array-range](https://github.com/mattdesl/array-range)
> creates a new array with given range

```javascript
/**
 * 支持一个参数和两个参数,返回前闭后开的区间 `[...)`
 * newArray(5) => [0, 1, 2, 3, 4]
 * newArray(1, 5) => [1, 2, 3, 4]
 */
module.exports = function newArray(start, end) {
    var n0 = typeof start === 'number',
        n1 = typeof end === 'number'

    if (n0 && !n1) {
        end = start
        start = 0
    } else if (!n0 && !n1) { // 这个步骤和下面 `|` 操作感觉有点重复
        start = 0
        end = 0
    }

    start = start|0
    end = end|0
    var len = end-start
    if (len<0)
        throw new Error('array length must be positive')

    var a = new Array(len)
    for (var i=0, c=start; i<len; i++, c++)
        a[i] = c
    return a
}
```
### [arr-diff](https://github.com/jonschlinkert/arr-diff)
> Returns an array with only the unique values present in all given arrays using strict equality for comparisons.

```javascript
/**
 * 参数数量不限，可以为多个数组
 * 返回第一个数组于剩余数组不共有的元素
 * diff([1, 2, 3, 4, 4], [5, 2, 7], [1, 2], [3]) => [4, 4]
 */
'use strict';

module.exports = function diff(arr/*, arrays*/) {
  var len = arguments.length;
  var idx = 0;
  while (++idx < len) {
    arr = diffArray(arr, arguments[idx]);
  }
  return arr;
};

function diffArray(one, two) {
  if (!Array.isArray(two)) { // 第二个参数不是数组的时候，返回第一个数组
    return one.slice();
  }

  var tlen = two.length
  var olen = one.length;
  var idx = -1;
  var arr = [];

  while (++idx < olen) { // 双重循环对比，不相同则存起来，相同则跳过
    var ele = one[idx];

    var hasEle = false;
    for (var i = 0; i < tlen; i++) {
      var val = two[i];

      if (ele === val) {
        hasEle = true;
        break;
      }
    }

    if (hasEle === false) {
      arr.push(ele);
    }
  }
  return arr;
}
```
### [filled-array](https://github.com/sindresorhus/filled-array)
> Returns an array filled with the specified input

```javascript
/**
 * @param {Function|Any} item 填充函数或值，如果是函数，则需要调用参数 index，传入的count，以及填充的数组
 * @param {Number} n 填充个数
 * filledArray('a', 3) => ['a', 'a', 'a']
 * filledArray(i => i * 2, 3) => [0, 2, 4]
 */
'use strict';
module.exports = function (item, n) {
	var ret = new Array(n);
	var isFn = typeof item === 'function';

	if (!isFn && typeof ret.fill === 'function') {
		return ret.fill(item);  // es6
	}

	for (var i = 0; i < n; i++) {
		ret[i] = isFn ? item(i, n, ret) : item;
	}

	return ret;
};

```
### [map-array](https://github.com/parro-it/map-array)
> Map object keys and values into an array.

```javascript
/**
 *
 */
'use strict';
const map = require('map-obj');

function mapToArray(obj, fn) {
	let idx = 0;
	const result = map(obj, (key, value) =>
		[idx++, fn(key, value)]
	);
	result.length = idx;
	return Array.from(result);
}

module.exports = mapToArray;
```
### [in-array](https://github.com/jonschlinkert/in-array)
> Return true if a value exists in an array. Faster than using indexOf and won't blow up on null values.

```javascript
/**
 * emmmm...看起来比较常规的实现呢，就是一个遍历比对，存在返回true，不存在返回false
 * issue里有人做过benchmark测试，数组长度大于5后，的确比indexOf快
 */
'use strict';

module.exports = function inArray (arr, val) {
  arr = arr || [];
  var len = arr.length;
  var i;

  for (i = 0; i < len; i++) {
    if (arr[i] === val) {
      return true;
    }
  }
  return false;
};
```
### [unordered-array-remove](https://github.com/mafintosh/unordered-array-remove)
> Efficiently remove an element from an unordered array without doing a splice

```javascript
/**
 * 将数组最后一位移出来去占需要删除的位置,返回删除的位置的值
 * 不过这样最终打乱了原数组的排序
 */
module.exports = remove

function remove (arr, i) {
  if (i >= arr.length || i < 0) return
  var last = arr.pop()
  if (i < arr.length) {
    var tmp = arr[i]
    arr[i] = last
    return tmp
  }
  return last
}
```
### [swap-array](https://github.com/michaelzoidl/swap-array)
> Swaps the index / position of an array

```javascript
/**
 * 交换数组指定两个位置的值
 * 为不改变元素组，新开辟空间并重新赋值
 */
export default (Arr, Caller, Target) => {
  let Instance = Arr.constructor();
  let Stash = Arr;

  let InstanceType = Array.isArray(Instance) ? 'array' : typeof Instance;

  // Check types and throw err if no arr is passed
  if(InstanceType !== 'array') throw '[ERR] SwapArray expects a array as first param';

  // Copy the Arr-Content into new Instance - so we don't overwrite the passed array
  Stash.map((s, i) => Instance[i] = s);

  // Update indexes
  Instance[Caller] = Instance.splice(Target, 1, Instance[Caller])[0];

  return Instance;
}
```
### [mirrarray](https://github.com/johnwquarles/mirrarray)
> NPM module for creating a keymirror object from an array of strings

```javascript
/**
 * mirrarray(['this', 'that', 'another']);
 * {
 *   this: 'this',
 *   that: 'that',
 *   another: 'another'
 * }
 */

```
### [group-array](https://github.com/doowb/group-array)
> Group array of objects into lists.

### [pad-left](https://github.com/jonschlinkert/pad-left)
> Left pad a string with zeros or a specified string. Fastest implementation

```javascript
/**
 * var pad = require('pad-left');
  pad(  '4', 4, '0') // 0004
  pad( '35', 4, '0') // 0035
  pad('459', 4, '0') // 0459
 */
'use strict';

var repeat = require('repeat-string');

module.exports = function padLeft(str, num, ch) {
  str = str.toString();

  if (typeof num === 'undefined') {
    return str;
  }

  if (ch === 0) {
    ch = '0';
  } else if (ch) {
    ch = ch.toString();
  } else {
    ch = ' ';
  }

  return repeat(ch, num - str.length) + str;
};
```

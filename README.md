# util-project

```

(function (window) {

    var utils = {};
    window.utils = utils;

    utils.isNull = isNull;
    utils.isBoolean = isBoolean;
    utils.isString = isString;
    utils.isNumber = isNumber;
    utils.isRegExp = isRegExp;
    utils.isDate = isDate;
    utils.isError = isError;
    utils.isArray = isArray;
    utils.isObject = isObject;

    utils.random = random;
    utils.debounce = debounce;
    utils.getUUID = getUUID;


    // Date时间相关
    utils.formatDate = formatDate;
    utils.relative = relative;


    // 字符串相关
    utils.trim = trim;
    utils.trimLeft = trimLeft;
    utils.trimRight = trimRight;
    utils.reverse = reverse;
    utils.humanizeNumber = humanizeNumber;
    utils.pad = pad;
    utils.padLeft = padLeft;
    utils.padRight = padRight;

    // 数组相关
    utils.contains = contains;
    utils.some = some;
    utils.every = every;
    utils.uniquelize = uniquelize;


    utils.getScrollTop = getScrollTop;
    utils.getScrollLeft = getScrollLeft;
    utils.encode = encode;
    utils.decode = decode;
    utils.query = {
        parse: parse,
        stringify: stringify
    };
    utils.Emitter = Emitter;


    utils.clone = clone;

    var toString = Object.prototype.toString;
    var getObjectTypeString = function (x) {
        return toString.call(x).slice(8, -1);
    }

    //  is 方法部分

    function isNull(value) {
        return value === null;
    }

    function isBoolean(value) {
        return value === true || value === false;
    }

    function isString(value) {
        return typeof value === "string";
    }

    function isNumber(value) {
        return typeof value === "number";
    }

    function isRegExp(value) {
        return getObjectTypeString(value) === "RegExp";
    }

    function isDate(value) {
        return getObjectTypeString(value) === "Date";
    }

    function isError(value) {
        return getObjectTypeString(value) === "Error";
    }

    function isArray(value) {
        return getObjectTypeString(value) === "Array";
    }

    function isObject(value) {
        return getObjectTypeString(value) === "Object";
    }


    /**
     * 生成随机数的方法
     * 
     * @param {any} min 生成随机数的最小值
     * @param {any} max 生成随机数的最大值
     * @returns  返回生成的随机数
     */
    function random(min, max) {
        return Math.floor(Math.random() * (max - min + 1) + min);
    };

    //  字符串相关的方法

    /**
     * 去掉字符串两边的空格
     * 
     * @param {any} str 
     * @returns 
     */
    function trim(str) {
        if (str.trim) return str.trim();
        return str.replace(/^\s*|\s*$/g, '');
    }

    /**
     * 去掉字符串左侧的空格
     * 
     * @param {any} str 
     * @returns 
     */
    function trimLeft(str) {
        if (str.trimLeft) return str.trimLeft();
        return str.replace(/^\s*/, '');
    }


    /**
     * 去掉字符串右侧的空格
     * 
     * @param {any} str 
     * @returns 
     */
    function trimRight(str) {
        if (str.trimRight) return str.trimRight();
        return str.replace(/\s*$/, '');
    }


    /**
     * 
     * 反转一个字符串
     * @param {any} str 
     * @returns 
     */
    function reverse(str) {

        var symbols = /([\0-\u02FF\u0370-\u1DBF\u1E00-\u20CF\u2100-\uD7FF\uDC00-\uFE1F\uFE30-\uFFFF]|[\uD800-\uDBFF][\uDC00-\uDFFF]|[\uD800-\uDBFF])([\u0300-\u036F\u1DC0-\u1DFF\u20D0-\u20FF\uFE20-\uFE2F]+)/g;
        var surrogates = /([\uD800-\uDBFF])([\uDC00-\uDFFF])/g;

        /**
         * Reverse a string
         *
         * @param {String} str
         * @return {String}
         */

        function reverse(str) {
            // Step 1: deal with combining marks and astral symbols (surrogate pairs)
            str = str
                .replace(symbols, function ($0, $1, $2) { return reverse($2) + $1; })
                .replace(surrogates, '$2$1');

            // Step 2: reverse the code units in the string
            var out = '';
            var i = str.length;
            while (i--) {
                out += str.charAt(i);
            }

            return out;
        }

        return reverse(str);

    }


    /**
     * 转换字符串为对用户友好的字符串
     * 1000000.99 -> 1,000,000.99
     * 
     * humanize(1000); // => '1,000'
     * humanize(1000.55, { delimiter: '.', separator: ',' }); // => '1.000,55' 
     * 
     * @param {any} n 
     * @param {any} options 
     * @returns 
     */
    function humanizeNumber(n, options) {
        options = options || {};
        var d = options.delimiter || ',';
        var s = options.separator || '.';
        n = n.toString().split('.');
        n[0] = n[0].replace(/(\d)(?=(\d\d\d)+(?!\d))/g, '$1' + d);
        return n.join(s);
    }


    /**
     * Pad `str` to `len` with optional `c` char,
     * favoring the left when unbalanced.
     *
     * @param {String} str
     * @param {Number} len
     * @param {String} c
     * @return {String}
     * @api public
     */
    function pad(str, len, c) {
        c = c || ' ';
        if (str.length >= len) return str;
        len = len - str.length;
        var left = Array(Math.ceil(len / 2) + 1).join(c);
        var right = Array(Math.floor(len / 2) + 1).join(c);
        return left + str + right;
    }


    /**
     * 
     * Pad `str` left to `len` with optional `c` char.
     * 
     * @param {any} str 
     * @param {any} len 
     * @param {any} c 
     * @returns 
     */
    function padLeft(str, len, c) {
        c = c || ' ';
        if (str.length >= len) return str;
        return Array(len - str.length + 1).join(c) + str;
    }

    /**
     * Pad `str` right to `len` with optional `c` char.
     * 
     * @param {any} str 
     * @param {any} len 
     * @param {any} c 
     * @returns 
     */
    function padRight(str, len, c) {
        c = c || ' ';
        if (str.length >= len) return str;
        return str + Array(len - str.length + 1).join(c);
    }



    // 数组相关方法的扩展

    /**
     * 判断arr是否包含元素o
     * @memberOf array
     * @name contains
     * @function
     * @param {Array} arr
     * @param {Obejct} o
     * @return {Boolean}
     */
    function contains(arr, o) {
        return (indexOf(arr, o) > -1);
    }

    /**
     * 数组中有一个为真,就返回真
     * 
     * @param {any} arr 
     * @param {any} fun 
     * @returns 
     */
    function some(arr, fun) {
        var len = arr.length;
        if (typeof fun != "function") {
            throw new Error("type error");
        }

        var thisp = arguments[2];
        for (var i = 0; i < len; i++) {
            if (i in arr && fun.call(thisp, arr[i], i, arr)) {
                return true;
            }
        }

        return false;
    }


    /**
     * 数组中所有的项都为真,就返回真
     * 
     * @param {any} arr 
     * @param {any} fun 
     * @returns 
     */
    function every(arr, fun) {
        var len = arr.length;
        if (typeof fun != "function") {
            throw new Error("type error");
        }
        var thisp = arguments[2];
        for (var i = 0; i < len; i++) {
            if (i in arr && !fun.call(thisp, arr[i], i, arr)) {
                return false;
            }
        }
        return true;
    }

    /**
     * 数组去重
     * 
     * @param {any} arr 
     * @returns 
     */
    function uniquelize(arr) {
        var result = [];
        for (var i = 0, len = arr.length; i < len; i++) {
            if (!contains(result, arr[i])) {
                result.push(arr[i]);
            }
        }
        return result;
    };


    //  和 dom元素相关的方法

    /**
     * 获取当前文档的上边已卷动的宽度
     * Returns the top scroll value of the document 
     * @method getDocumentScrollTop
     * @memberOf dom
     * @param {HTMLDocument} document (optional) The document to get the scroll value of
     * @return {Number}  The amount that the document is scrolled to the top
     */
    function getScrollTop(el) {
        var scrollTop;
        if (el) {
            scrollTop = el.scrollTop;
        } else {
            scrollTop = Math.max(document.documentElement.scrollTop, document.body.scrollTop);
        }
        return scrollTop || 0;
    };


    /**
     * 获取当前文档的左边已卷动的宽度
     * Returns the left scroll value of the document 
     * @method getDocumentScrollLeft
     * @memberOf dom
     * @param {HTMLDocument} document (optional) The document to get the scroll value of
     * @return {Number}  The amount that the document is scrolled to the left
     */
    function getScrollLeft(el) {
        var scrollLeft;
        if (el) {
            scrollLeft = el.scrollLeft;
        } else {
            scrollLeft = Math.max(document.documentElement.scrollLeft, document.body.scrollLeft);
        }
        return scrollLeft || 0;
    };



    // Date 时间相关的方法

    function relative(date, other) {

        var second = 1000;
        var minute = 60 * second;
        var hour = 60 * minute;
        var day = 24 * hour;
        var week = 7 * day;
        var year = day * 365;
        var month = year / 12;

        /**
         * Return `date` in words relative to `other`
         * which defaults to now.
         *
         * @param {Date} date
         * @param {Date} other
         * @return {String}
         * @api public
         */
        function relative(date, other) {
            other = other || new Date;
            var ms = Math.abs(other - date);

            if (ms < second) return '';

            if (ms == second) return 'one second';
            if (ms < minute) return Math.ceil(ms / second) + ' seconds';

            if (ms == minute) return 'one minute';
            if (ms < hour) return Math.ceil(ms / minute) + ' minutes';

            if (ms == hour) return 'one hour';
            if (ms < day) return Math.ceil(ms / hour) + ' hours';

            if (ms == day) return 'one day';
            if (ms < week) return Math.ceil(ms / day) + ' days';

            if (ms == week) return 'one week';
            if (ms < month) return Math.ceil(ms / week) + ' weeks';

            if (ms == month) return 'one month';
            if (ms < year) return Math.ceil(ms / month) + ' months';

            if (ms == year) return 'one year';
            return Math.round(ms / year) + ' years';
        }

        return  relative(date, other);
    }



    /**
     * 日期时间格式化方法
     * 
     * @param {any} dateStr 时间实例
     * @param {any} formatStr  格式
     * @returns 格式化之后的字符串
     */
    function formatDate(dateStr, formatStr) {

        var date = new Date(dateStr);

		/*   
		函数：填充0字符   
		参数：value-需要填充的字符串, length-总长度   
		返回：填充后的字符串   
		*/
        var zeroize = function (value, length) {
            if (!length) {
                length = 2;
            }
            value = new String(value);
            for (var i = 0, zeros = ''; i < (length - value.length); i++) {
                zeros += '0';
            }
            return zeros + value;
        };

        return formatStr.replace(/"[^"]*"|'[^']*'|\b(?:d{1,4}|M{1,4}|yy(?:yy)?|([hHmstT])\1?|[lLZ])\b/g, function ($0) {
            switch ($0) {
                case 'd': return date.getDate();
                case 'dd': return zeroize(date.getDate());
                case 'ddd': return ['Sun', 'Mon', 'Tue', 'Wed', 'Thr', 'Fri', 'Sat'][date.getDay()];
                case 'dddd': return ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'][date.getDay()];
                case 'M': return date.getMonth() + 1;
                case 'MM': return zeroize(date.getMonth() + 1);
                case 'MMM': return ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'][date.getMonth()];
                case 'MMMM': return ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'][date.getMonth()];
                case 'yy': return new String(date.getFullYear()).substr(2);
                case 'yyyy': return date.getFullYear();
                case 'h': return date.getHours() % 12 || 12;
                case 'hh': return zeroize(date.getHours() % 12 || 12);
                case 'H': return date.getHours();
                case 'HH': return zeroize(date.getHours());
                case 'm': return date.getMinutes();
                case 'mm': return zeroize(date.getMinutes());
                case 's': return date.getSeconds();
                case 'ss': return zeroize(date.getSeconds());
                case 'l': return date.getMilliseconds();
                case 'll': return zeroize(date.getMilliseconds());
                case 'tt': return date.getHours() < 12 ? 'am' : 'pm';
                case 'TT': return date.getHours() < 12 ? 'AM' : 'PM';
            }
        });
    }

    /**
     * 对给定的字符串加密
     * 
     * @param {any} code 
     * @returns 返回加密后的字符串
     */
    function compileStr(code) {
        var c = String.fromCharCode(code.charCodeAt(0) + code.length);
        for (var i = 1; i < code.length; i++) {
            c += String.fromCharCode(code.charCodeAt(i) + code.charCodeAt(i - 1));
        }
        return escape(c);
    }

    /**
     * 对compileStr加密后的字符串进行解密
     * 
     * @param {any} code 
     * @returns 
     */
    function uncompileStr(code) {
        code = unescape(code);
        var c = String.fromCharCode(code.charCodeAt(0) - code.length);
        for (var i = 1; i < code.length; i++) {
            c += String.fromCharCode(code.charCodeAt(i) - c.charCodeAt(i - 1));
        }
        return c;
    }

    /**
     * 对字符串做编码
     * 
     * @param {any} str 
     * @returns 
     */
    function encode(str) {
        try {
            return encodeURIComponent(str);
        } catch (e) {
            return str;
        }
    };

    /**
     * 对编码后的字符串解码 
     * 
     * @param {String} str
     * @return {String}
     */
    function decode(str) {
        try {
            return decodeURIComponent(str.replace(/\+/g, ' '));
        } catch (e) {
            return str;
        }
    }

    /**
     * 将url中的查询字符串序列化成对象
     * 
     * @param {any} str 
     * @returns 
     */
    function parse(str) {
        var pattern = /(\w+)\[(\d+)\]/;
        if ('string' != typeof str) return {};

        str = str.trim();
        if ('' == str) return {};
        if ('?' == str.charAt(0)) str = str.slice(1);

        var obj = {};
        var pairs = str.split('&');
        for (var i = 0; i < pairs.length; i++) {
            var parts = pairs[i].split('=');
            var key = decode(parts[0]);
            var m;

            if (m = pattern.exec(key)) {
                obj[m[1]] = obj[m[1]] || [];
                obj[m[1]][m[2]] = decode(parts[1]);
                continue;
            }

            obj[parts[0]] = null == parts[1]
                ? ''
                : decode(parts[1]);
        }

        return obj;
    };


    /**
     * 将对象序列化成查询字符串
     * 
     * @param {any} obj 
     * @returns 
     */
    function stringify(obj) {
        if (!obj) return '';
        var pairs = [];

        for (var key in obj) {
            var value = obj[key];

            if (isArray(value)) {
                for (var i = 0; i < value.length; ++i) {
                    pairs.push(encode(key + '[' + i + ']') + '=' + encode(value[i]));
                }
                continue;
            }

            pairs.push(encode(key) + '=' + encode(obj[key]));
        }

        return pairs.join('&');
    }


    /**
     *   Emitter  类的实现开始
     *   ===============================================================================
     */

    /**
     * Initialize a new `Emitter`.
     *
     * @api public
     */

    function Emitter(obj) {
        if (obj) return mixin(obj);
    };

    /**
     * Mixin the emitter properties.
     *
     * @param {Object} obj
     * @return {Object}
     * @api private
     */

    function mixin(obj) {
        for (var key in Emitter.prototype) {
            obj[key] = Emitter.prototype[key];
        }
        return obj;
    }

    /**
     * Listen on the given `event` with `fn`.
     *
     * @param {String} event
     * @param {Function} fn
     * @return {Emitter}
     * @api public
     */

    Emitter.prototype.on =
        Emitter.prototype.addEventListener = function (event, fn) {
            this._callbacks = this._callbacks || {};
            (this._callbacks['$' + event] = this._callbacks['$' + event] || [])
                .push(fn);
            return this;
        };

    /**
     * Adds an `event` listener that will be invoked a single
     * time then automatically removed.
     *
     * @param {String} event
     * @param {Function} fn
     * @return {Emitter}
     * @api public
     */

    Emitter.prototype.once = function (event, fn) {
        function on() {
            this.off(event, on);
            fn.apply(this, arguments);
        }

        on.fn = fn;
        this.on(event, on);
        return this;
    };

    /**
     * Remove the given callback for `event` or all
     * registered callbacks.
     *
     * @param {String} event
     * @param {Function} fn
     * @return {Emitter}
     * @api public
     */

    Emitter.prototype.off =
        Emitter.prototype.removeListener =
        Emitter.prototype.removeAllListeners =
        Emitter.prototype.removeEventListener = function (event, fn) {
            this._callbacks = this._callbacks || {};

            // all
            if (0 == arguments.length) {
                this._callbacks = {};
                return this;
            }

            // specific event
            var callbacks = this._callbacks['$' + event];
            if (!callbacks) return this;

            // remove all handlers
            if (1 == arguments.length) {
                delete this._callbacks['$' + event];
                return this;
            }

            // remove specific handler
            var cb;
            for (var i = 0; i < callbacks.length; i++) {
                cb = callbacks[i];
                if (cb === fn || cb.fn === fn) {
                    callbacks.splice(i, 1);
                    break;
                }
            }
            return this;
        };

    /**
     * Emit `event` with the given args.
     *
     * @param {String} event
     * @param {Mixed} ...
     * @return {Emitter}
     */

    Emitter.prototype.emit = function (event) {
        this._callbacks = this._callbacks || {};
        var args = [].slice.call(arguments, 1)
            , callbacks = this._callbacks['$' + event];

        if (callbacks) {
            callbacks = callbacks.slice(0);
            for (var i = 0, len = callbacks.length; i < len; ++i) {
                callbacks[i].apply(this, args);
            }
        }

        return this;
    };

    /**
     * Return array of callbacks for `event`.
     *
     * @param {String} event
     * @return {Array}
     * @api public
     */

    Emitter.prototype.listeners = function (event) {
        this._callbacks = this._callbacks || {};
        return this._callbacks['$' + event] || [];
    };

    /**
     * Check if this emitter has `event` handlers.
     *
     * @param {String} event
     * @return {Boolean}
     * @api public
     */

    Emitter.prototype.hasListeners = function (event) {
        return !!this.listeners(event).length;
    };

    /**
     *   Emitter 类的实现结束
     *   ===============================================================================
     */



    /**
     * Clones objects.
     *
     * @param {Mixed} any object
     * @api public
     */
    function clone(obj) {

        if (isObject(obj)) {
            var copy = {};
            for (var key in obj) {
                if (Object.prototype.hasOwnProperty.call(obj, key)) {
                    copy[key] = clone(obj[key]);
                }
            }
            return copy;
        }

        if (isArray(obj)) {
            var copy = new Array(obj.length);
            for (var i = 0, l = obj.length; i < l; i++) {
                copy[i] = clone(obj[i]);
            }
            return copy;
        }


        if (isRegExp(obj)) {
            var flags = '';
            flags += obj.multiline ? 'm' : '';
            flags += obj.global ? 'g' : '';
            flags += obj.ignoreCase ? 'i' : '';
            return new RegExp(obj.source, flags);
        }

        if (isDate(obj)) {
            return new Date(obj.getTime());
        }

        return obj;

    }


    /**
     * 
     * 函数节流
     * @param {any} func 
     * @param {any} wait 
     * @param {any} immediate 
     * @returns 
     */
    function debounce(func, wait, immediate) {
        var timeout, args, context, timestamp, result;
        if (null == wait) wait = 100;

        function later() {
            var last = Date.now() - timestamp;

            if (last < wait && last >= 0) {
                timeout = setTimeout(later, wait - last);
            } else {
                timeout = null;
                if (!immediate) {
                    result = func.apply(context, args);
                    context = args = null;
                }
            }
        };

        var debounced = function () {
            context = this;
            args = arguments;
            timestamp = Date.now();
            var callNow = immediate && !timeout;
            if (!timeout) timeout = setTimeout(later, wait);
            if (callNow) {
                result = func.apply(context, args);
                context = args = null;
            }

            return result;
        };

        debounced.clear = function () {
            if (timeout) {
                clearTimeout(timeout);
                timeout = null;
            }
        };

        debounced.flush = function () {
            if (timeout) {
                result = func.apply(context, args);
                context = args = null;

                clearTimeout(timeout);
                timeout = null;
            }
        };

        return debounced;
    };


    /**
     * 获取随机的编码字符串
     * 
     * @param {any} length 
     * @returns 
     */
    function getUUID(length) {
        /**
         * Base 64 characters
         */

        if (!length) {
            length = 32;
        }

        var BASE64 = '0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz-_';

        /**
         * Make a Uint8Array into a string
         *
         * @param {Uint8Array}
         * @returns {String}
         * @api private
         */
        function tostr(bytes) {
            var r, i;

            r = [];
            for (i = 0; i < bytes.length; i++) {
                r.push(BASE64[bytes[i] % 64]);
            }

            return r.join('');
        }

        /**
         * Generate an unique id
         *
         * @param {Number} The number of chars of the uid
         * @api public
         */
        function uid(length) {
            if (typeof window != 'undefined') {
                if (typeof window.crypto != 'undefined') {
                    return tostr(window.crypto.getRandomValues(new Uint8Array(length)));
                } else {
                    var a = new Array(length);
                    for (var i = 0; i < length; i++) {
                        a[i] = Math.floor(Math.random() * 256);
                    }
                    return tostr(a);
                }
            } else {
                var crypto = require('cryp' + 'to'); // avoid browserify polyfill
                try {
                    return tostr(crypto.randomBytes(length));
                } catch (e) {
                    // entropy sources are drained
                    return tostr(crypto.pseudoRandomBytes(length));
                }
            }
        }

        return uid(length);

    }




})(window);



```

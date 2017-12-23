# Now

To get the current date and time, just call moment() with no parameters.

> Moment.js creates a wrapper for the Date object.

~~~js
var now = moment();
~~~

# String

For consistent results parsing anything other than ISO 8601 strings, you should use `String + Format`

If a string does not match any of the above formats and is not able to be parsed with Date.parse, `moment#isValid `will return false.

~~~js
var time = moment('123abc').isValid(); // false
~~~

# display format

[display format](http://momentjs.com/docs/#/displaying/format/)

~~~js
var now = moment().format('YYYY-MM-DD HH:mm:ss');
~~~

# Query

~~~js
moment('2010-10-20').isBefore('2010-10-21'); // true
// year month week day hour minute second
moment('2010-10-20').isBefore('2010-12-31', 'year'); // false
moment('2010-10-20').isBefore('2011-01-01', 'year'); // true
moment('2010-10-20').isSame('2010-10-20'); // true
moment('2010-10-20').isSame('2009-12-31', 'year');  // false
moment('2010-10-20').isSame('2010-01-01', 'year');  // true
moment('2010-10-20').isSame('2010-12-31', 'year');  // true
moment('2010-10-20').isSame('2011-01-01', 'year');  // false
moment('2010-01-01').isSame('2011-01-01', 'month'); // false, different year
moment('2010-01-01').isSame('2010-02-01', 'day');   // false, different month
moment('2010-10-20').isAfter('2010-10-19'); // true
moment('2010-10-20').isAfter('2010-01-01', 'year'); // false
moment('2010-10-20').isAfter('2009-12-31', 'year'); // true
moment('2010-10-20').isSameOrBefore('2010-10-21');  // true
moment('2010-10-20').isSameOrBefore('2010-10-20');  // true
moment('2010-10-20').isSameOrBefore('2010-10-19');  // false
moment('2010-10-20').isSameOrBefore('2009-12-31', 'year'); // false
moment('2010-10-20').isSameOrBefore('2010-12-31', 'year'); // true
moment('2010-10-20').isSameOrBefore('2011-01-01', 'year'); // true
moment('2010-10-20').isSameOrAfter('2010-10-19'); // true
moment('2010-10-20').isSameOrAfter('2010-10-20'); // true
moment('2010-10-20').isSameOrAfter('2010-10-21'); // false
moment('2010-10-20').isSameOrAfter('2011-12-31', 'year'); // false
moment('2010-10-20').isSameOrAfter('2010-01-01', 'year'); // true
moment('2010-10-20').isSameOrAfter('2009-12-31', 'year'); // true
moment('2010-10-20').isBetween('2010-10-19', '2010-10-25'); // true
moment('2010-10-20').isBetween('2010-01-01', '2012-01-01', 'year'); // false
moment('2010-10-20').isBetween('2009-12-31', '2012-01-01', 'year'); // true
~~~

> instantMessage

~~~js
let time = '';
let MsgTimeStamp = item.MsgTimeStamp * 1000;
if (moment().isSame(MsgTimeStamp, 'day')) {
  time = moment(MsgTimeStamp).format('HH:mm');
} else if (moment().subtract(1, 'day').isSame(MsgTimeStamp, 'day')) {
  time = '昨天';
} else {
  time = moment(MsgTimeStamp).format('MM月DD号');
}
~~~          

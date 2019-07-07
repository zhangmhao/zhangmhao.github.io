---
layout: post
title:  "How to format time with timezone?"
date:   2019-07-03 19:51:05 +0800
categories: English,Javascript
---

One of my recent projects need to launch at various countries with different timezone, And it has a calendar to show users checkin footprint which related to date format issue.

For example. If we use system timezone to format date, Then a user from Thailand(GMT+7) visit our page at `2019-07-03 23:00`，the format date would be 2018-07-03. But at the same time if a user from Singapore(GMT+8) visit the same page, the format date would be 2018-07-04, because Singapore is one hour early than Thailand, So for people in Singaport, the time was actually  `2019-07-04 00:00:00`. 

So In order to make sure that people from different timezones will have the same calendar when they visit the same page. It means that we should format the date with the timezone of a specific country instead of the user's system's.

And The method to solve this problem is quite simple and intuitive. Below is the code exmaple.

```
function formatDateWithTimezone(timestamp, timezoneOffset = null) {
  const date = new Date(timestamp);
  const localOffset = date.getTimezoneOffset() * 60000;
  const utc0Timestamp  =  timestamp + localOffset;
  const alignTimestamp = utc0Timestamp   + timezoneOffset * 60000;
  return formatDate(alignTimestamp, 'YYYY-MM-DD');
}
```

First we use getTimezoneOffset() to get the user's actual timezone offset, For example, the timezone of Singaport is GMT+8, so it's timezone offset is -480 

```
  const localOffset = date.getTimezoneOffset() * 60000;
```

Next, Let's use our original timestamp to add `date.getTimezoneOffset() * 60000`,  After this correction we will get a new timestamp which is GMT+0.

```
const utc0Timestamp = timestamp + localOffset;
```

Now comes to the final step, let's add the timezone offset of the specific country that we need to the new timestamp.For example, If a user come from Thailand,  then the timezone is GMT+7,  so it's timezone offset will be 7 * 60 = 420. After format this new timestamp, you will get the date you want.

```
const alignTimestamp = utc0Timestamp + timezoneOffset * 60000;
formatDate(alignTimestamp, 'YYYY-MM-DD');
```

Use this method, You will always get the same format date no matter where you are or what you timezone is.


---
date: 2020-05-05 07:43:40
layout: post
title: "Reflection Journal#7: MomentJS in Flask"
subtitle:
description:
image: https://miro.medium.com/max/918/1*s_yRzdt5-339dfl5r_AasQ.png
optimized_image:
category: code
tags:
    - reflection
    - flask
    - python
author:
paginate: false
---

A few things to note about the popular JavaScript library, MomentJS. **MomentJS** is a lightweight JavaScript date library for parsing, validating, manipulating, and formatting dates. MomentJS makes it easier for us to deal with datetime format. Now but it's a JavaScript library, what if I want to use it for Python? For Flask app, of course you can just import momentjs using **CDN** link and use it straight in the code. But think about some case when you really need it for the backend.

### Timezone issues

Python has a built in package for dealing with dates and times. However, the format is not that really good. Given example below, at the time I run this command

```py
from datetime import datetime
print(datetime.now())

# output -> 2020-05-05 17:53:38.158771
```

Forget about if you're living in the other side of the world and someone also lives in the other side of the world, you're both executing this command at the same time and wonder what happen to the datetime. Let's all forget about it. 

The main issue is, this format looks horrible. You can't just display this format to the users with tons of numbers and they don't even understand what half of the string means. As it turns out, I decided to extend the python moment library with of course some more functions and features that it doesn't have.

This entire package is imported inside my template which you can find [here](https://github.com/polowis/Pandoru_template). You can use it with backend and also frontend. 

### The magic

Things get really easy with the package. 

```py
from app.framework.util.moment.core import moment

# Format day
moment.date("12-13-2018").format("YYYY-M-D")
# output: 2018-12-13

```

Use it with the frontend
```html
{% raw %}
<h1>{{moment.date("12-13-2018").format("YYYY-M-D")}}<h1>
{% endraw %}
```

Now, it offers almost everything you can think of. Including all features in moment package with some addition
```py
moment.date('2017-09-28T21:45:23Z').format('L')
# output: "09/28/2017"

moment.date('2017-09-28T21:45:23Z').format('LL')
# output: "September 28, 2017"

moment.date('2017-09-28T21:45:23Z').format('LLL')
# output: "September 28, 2017 2:45 PM"

moment.date('2017-09-28T21:45:23Z').format('LLLL')
# output "Thursday, September 28, 2017 2:45 PM"


# create object
moment.date("2020-05-05")


moment.date("2016-09-28T21:45:23Z").fromNow()
# output 05 years 17 months 8 hours ago

# Create a moment with a specified strftime format
moment.date("2020-05-05", "%m-%d-%Y")

# Moment uses the awesome dateparser library behind the scenes
moment.date("2020-05-05")

# Create a moment with words in it
moment.date("May 18, 2019")

# Create a moment that would normally be pretty hard to do
moment.date("2 weeks ago")

# Create a future moment that would otherwise be really difficult
moment.date("2 weeks now")

# Create a moment from the current datetime
moment.now()

# The moment can also be UTC-based
moment.utcnow()

# Create a moment with the UTC time zone
moment.utc("2020-05-05")

# Create a moment from a Unix timestamp
moment.unix(1355875153626)

# Create a moment from a Unix UTC timestamp
moment.unix(1355875153626, utc=True)

# Return a datetime instance
moment.date(2012, 12, 18).date

# We can do the same thing with the UTC method
moment.utc(2012, 12, 18).date

# Create and format a moment using Moment.js semantics
moment.now().format("YYYY-M-D")

# Create and format a moment with strftime semantics
moment.date(2012, 12, 18).strftime("%Y-%m-%d")

# Update your moment's time zone
moment.date(datetime(2012, 12, 18)).locale("US/Central").date

# Alter the moment's UTC time zone to a different time zone
moment.utcnow().timezone("US/Eastern").date

# Set and update your moment's time zone. 
moment.now().locale("US/Pacific").timezone("US/Eastern")

# In order to manipulate time zones, a locale must always be set or
# you must be using UTC.
moment.utcnow().timezone("US/Eastern").date

# You can also clone a moment, so the original stays unaltered
now = moment.utcnow().timezone("US/Pacific")
future = now.clone().add(weeks=2)

```

What else?

```py
# Customize your moment by chaining commands
moment.date(2012, 12, 18).add(days=2).subtract(weeks=3).date

# Imagine trying to do this with datetime, right?
moment.utcnow().add(years=3, months=2).format("YYYY-M-D h:m A")

# You can use multiple keyword arguments
moment.date(2012, 12, 19).add(hours=1, minutes=2, seconds=3)

# And, a similar subtract example...
moment.date(2012, 12, 19, 1, 2, 3).subtract(hours=1, minutes=2, seconds=3)

#we can also replace values
moment.now().replace(hours=5, minutes=15, seconds=0).epoch()

# And, if you'd prefer to keep the microseconds 
moment.now().replace(hours=5, minutes=15, seconds=0).epoch(rounding=False)

# Years, months, and days can also be set
moment.now().replace(years=1984, months=1, days=1, hours=0, minutes=0, seconds=0)

# Also, datetime properties are available
moment.utc(2012, 12, 19).year == 2012

moment.now().seconds

# We can also manipulate to preferred weekdays, such as Monday
moment.date(2012, 12, 19).replace(weekday=1).strftime("%Y-%m-%d")

# Or, this upcoming Sunday
moment.date("2012-12-19").replace(weekday=7).date

# We can even go back to two Sundays ago
moment.date(2012, 12, 19).replace(weekday=-7).format("YYYY-MM-DD")

```

I'm also extending it with more functions such as ```isBefore```, ```isSame```, ```isAfter```, ```isDate```

---
title: Comparing Dates and Times with Python 3
date: 2021-01-15
---

When comparing dates and times in Python, there are a couple things you must get right:
- Timezones
- UTC offset-awareness
- Variable/object type

This post will cover:
- Getting the current date/time in a different timezone in Python 3
- Comparing dates and times in Python 3
- Avoiding the *"TypeError: can't compare offset-naive and offset-aware datetimes"* error by creating offset-aware *datetime* objects
- Converting date and time *strings* into Python *datetime* objects
- Converting Python *datetime* objects into date and time *strings*
- Using the *datetime* and *pytz* Python libraries
- RSS feed date/time format specification

## Timezones
I just pulled data from an RSS feed. The RSS feed lists each blog post along with the date and time the post was published.

RSS feeds are required to give dates in one of two formats:
- Sat, 30 Jan 2021 09:30:00 +0000
- Sat, 30 Jan 2021 09:30:00 GMT

Notice, the only difference between the two formats above is in the timezone specification - *+0000* versus *GMT*.

The *+0000* is called the **UTC offset**. UTC is a time standard that never changes due to daylight savings time, and has a UTC offset of *+0000*. Every other timezone has a UTC offset based on how many hours different it is than UTC. For example, Eastern Standard Time (New York City time) has a UTC offset of *-0500*. That means Eastern Standard Time is five hours behind UTC time.

It is typically simpler for a computer to use the UTC offset of a timezone, but most people know their timezone by its abbreviation, like *EST*.

The creator of an RSS feed can choose which timezone format to provide. That means programs that read RSS feeds need to account for both formats.

This can cause some confusion if you're new to working with dates or times or datetimes. The best way for me to show you the problem is with some code.

## The problematic code
<pre>from datetime import datetime

rssFeedDateString = "Sat, 30 Jan 2021 09:30:00 GMT"
rssFeedDate = datetime.strptime("Sat, 30 Jan 2021 09:30:00 GMT", "%a, %d %b %Y %H:%M:%S %Z")
print(rssFeedDate)

currentDate = datetime.now()
print(currentDate)

if currentDate > rssFeedDate:
    print("Current date is later")
else:
    print("RSS feed date is later")</pre>
    
The code imports from the *datetime* Python library. Then, we get our RSS feed date which will come from the RSS feed as a string. We convert the RSS feed date string to a datetime object so that Python can do a datetime comparison on it. Then, we *print* it just to show what it's set to. Then we get the current date and time, and assign it to the *currentDate* variable. And finally, we compare the two dates in an *if-else*. 
    
I executed that code on *Sat, 30 Jan 2021 06:45:00 EST*. At that time, the time in the GMT timezone was *Sat, 30 Jan 2021 11:45:00 GMT*.

If you ignore the timezone specification, and just compare the dates and times, you would think the *rssFeedDate* is later than the *currentDate*.
- *rssFeedDate* = Sat, 30 Jan 2021 09:30:00
- *currentDate* = Sat, 30 Jan 2021 06:45:00

However, the *rssFeedDate* is in the GMT timezone while the *currentDate* is in the EST timezone. If we set both dates to the EST timezone, we get:
- *rssFeedDate* = Sat, 30 Jan 2021 04:30:00 EST
- *currentDate* = Sat, 30 Jan 2021 06:45:00 EST

There you can see that the *rssFeedDate* is actually earlier than the *currentDate* when we adjust for the timezone difference.

## The bad result
Python doesn't make that adjustment automatically, so when I run the above code, the result is:
<pre>2021-01-30 09:30:00
2021-01-30 06:45:00.895164
RSS feed date is later</pre>

That can be a real problem if you need to determine whether a blog post was posted in the past hour. According to the Python results, the blog post won't be posted until a couple hours from now.

## The solution to the timezone problem
We can fix the timezone issue by specifying the UTC offset and converting the dates to UTC. It takes some doing, but I will explain everything. Here is the code:

<pre>from datetime import datetime
import pytz

rssFeedDateString = "Sat, 30 Jan 2021 09:30:00 GMT"
rssFeedDate = datetime.strptime(rssFeedDateString, "%a, %d %b %Y %H:%M:%S %Z")
last3ofDateString = rssFeedDateString[-3:]
rssFeedTimezone = pytz.timezone(last3ofDateString)
rssFeedDateUTCOffset = rssFeedDate.astimezone(rssFeedTimezone).strftime('%z')
rssFeedDateNoTimezone = rssFeedDate.strftime('%a, %d %b %Y %H:%M:%S')
rssFeedDateStringUTCOffset = rssFeedDateNoTimezone + " " + rssFeedDateUTCOffset
finalRSSFeedDate = datetime.strptime(rssFeedDateStringUTCOffset, "%a, %d %b %Y %H:%M:%S %z")
print(f"RSS feed date: {finalRSSFeedDate}")

utc = pytz.utc
currentDate = datetime.now(utc)
print(f"Current date: {currentDate}")

if currentDate > finalRSSFeedDate:
    print("Current date is later")
else:
    print("RSS feed date is later")</pre>
    
I think the best way to explain this is to explain it line-by-line.

<code>from datetime import datetime</code>
- Imports the library we need for handling dates/times

<code>import pytz</code>
- Imports the *pytz* library for handling time zones

<code>rssFeedDateString = "Sat, 30 Jan 2021 09:30:00 GMT"</code>
- The string that we pull from the RSS feed

<code>rssFeedDate = datetime.strptime(rssFeedDateString, "%a, %d %b %Y %H:%M:%S %Z")</code>
- Using the *datetime* library function, *strptime*, to convert the RSS feed date *string* into a *datetime* object, then assign to *rssFeedDate* variable

<code>last3ofDateString = rssFeedDateString[-3:]</code>
- Getting only the last 3 characters of our RSS feed date *string*, and assigning to *last3ofDateString* variable
- This contains the timezone code from the RSS feed date, *GMT*

<code>rssFeedTimezone = pytz.timezone(last3ofDateString)</code>
- Passing the timezone code, *GMT*, to the *pytz* library function, *timezone*, and then assign the return value to *rssFeedTimezone* variable
- The *timezone* function returns the timezone code in a *datetime* object rather than what we had, which was a *string*

<code>rssFeedDateUTCOffset = rssFeedDate.astimezone(rssFeedTimezone).strftime('%z')</code>
- Converts the *rssFeedTimezone* (which is a *datetime* object of *GMT*) into a UTC offset value (which is a *string* object of (*+0000*), and assigns the value to the *rssFeedDataUTCOffset* variable

<code>rssFeedDateNoTimezone = rssFeedDate.strftime('%a, %d %b %Y %H:%M:%S')</code>
- Removes the timezone code, *GMT*, from the RSS feed date
- This is so that we can replace it with the UTC offset value, *+0000*, so Python properly compares the RSS feed date with the current date
- The *strftime* function takes a *datetime* object and returns a *string*, thus efffectively converting *datetime* to *string*

<code>rssFeedDateStringUTCOffset = rssFeedDateNoTimezone + " " + rssFeedDateUTCOffset</code>
- Attaches the UTC offset value to the RSS feed date
- This is a *string* object, so the next line is to convert it to a *datetime* object again so we can compare it to the current date

<code>finalRSSFeedDate = datetime.strptime(rssFeedDateStringUTCOffset, "%a, %d %b %Y %H:%M:%S %z")</code>
- We use the *strptime* function to convert the new RSS feed date string, which now includes the UTC offset, to a *datetime* object so we can compare it to the current date
- We now have a UTC offset-aware RSS feed date

<code>utc = pytz.utc</code>
- This creates a UTC timezone object
- You can create timezone objects for other timezones like this: *est = pytz.timezone('EST')* or *est = pytz.timezone('Eastern')*

<code>currentDate = datetime.now(utc)</code>
- Gets the current date in the UTC timezone and assigns it to the *currentDate* variable
- We now have a UTC offset-aware current date

<code>if currentDate > finalRSSFeedDate:
    print("Current date is later")
else:
    print("RSS feed date is later")</code>
- Compares the current date to the date the RSS feed was posted
- Both dates are now UTC offset-aware and will be compared based on that

### The successful result
When we run the new code, we can see that both dates have been converted to the UTC timezone, and they show UTC offset values (*+00:00*). Instead of comparing the current date in EST to the RSS feed date in GMT, we are comparing UTC to UTC. And now we have a much better result!

<pre>RSS feed date: 2021-01-30 09:30:00+00:00
Current date: 2021-01-30 13:36:11.180536+00:00
Current date is later</pre>

That is the easiest way I could find to get the comparison right, and believe me I searched! Here are some resources I found helpful. Happy hacking!

- https://pypi.org/project/pytz/
- https://docs.python.org/3/library/datetime.html#strftime-and-strptime-behavior
- https://stackabuse.com/converting-strings-to-datetime-in-python/
- https://stackoverflow.com/questions/5537876/get-utc-offset-from-time-zone-name-in-python
- https://stackoverflow.com/questions/8142364/how-to-compare-two-dates
- https://www.timeanddate.com/time/gmt-utc-time.html
- https://stackoverflow.com/questions/796008/cant-subtract-offset-naive-and-offset-aware-datetimes
- https://time.is/EST
- https://www.ustrem.org/en/articles/rss-date-format-rfc822-en/
- https://en.wikipedia.org/wiki/UTC_offset
- https://en.wikipedia.org/wiki/List_of_UTC_time_offsets
- https://www.geeksforgeeks.org/get-current-time-in-different-timezone-using-python/
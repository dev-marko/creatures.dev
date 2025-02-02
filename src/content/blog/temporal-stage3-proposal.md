---
title: 'Exploring the Temporal Proposal: Modernizing Date and Time Handling in JavaScript'
updateDate: 2023-09-19
publishDate: 2023-09-19
image: 'chaos-1.jpg'
tags: ['javascript', 'proposal', 'tc39']
author: paul-valladares
canonicalUrl: 'https://www.paulvall.dev/posts/temporal-proposal'
excerpt:
  'Taking a look at the Temporal proposal, which aims to modernize date and time handling in JavaScript.'
---

## Introduction

The handling of dates in ECMAScript has long been a source of frustration for developers. In response to this, the Temporal proposal was born. Temporal is a global object, similar to Math, that introduces a modern date and time API to the ECMAScript language. To dive deeper into the challenges associated with the existing Date object and the motivations behind Temporal, take a look at this informative article: [Fixing JavaScript Date](https://maggiepint.com/2017/04/11/fixing-javascript-date-web-compatibility-and-reality/).

## How Temporal Solves Date and Time Challenges

Temporal addresses these date and time challenges by offering:

- **User-Friendly APIs:** Temporal provides easy-to-use APIs for various date and time computations, making complex operations more accessible.

- **Time Zone Support:** It boasts first-class support for all time zones, including daylight-saving time (DST) adjustments, ensuring accurate and reliable arithmetic across different regions.

- **Immutability:** Temporal enforces immutability, meaning that once a date or time object is created, it cannot be altered. This helps prevent bugs stemming from unintended modifications.

- **Strict String Parsing:** Temporal allows you to parse date and time strings in a strictly specified format, ensuring consistency in data handling.

- **Non-Gregorian Calendars:** Temporal even goes beyond the Gregorian calendar, accommodating different calendar systems, broadening its usability.

Additionally, Temporal introduces separate ECMAScript classes for date-only, time-only, and other specialized use cases. This separation enhances code readability and minimizes errors that often arise from assumptions about time zones or values.

## Getting Started with Temporal

For those eager to embark on their Temporal journey, a cookbook is available to guide you through the fundamentals and intricacies of Temporal. It serves as a valuable resource for developers of all levels, offering practical examples and insights. [Explore the Temporal Cookbook](https://tc39.es/proposal-temporal/docs/cookbook.html) to kickstart your Temporal exploration.

## Understanding Temporal API Documentation

The Temporal API follows a consistent naming convention, with types that start with "Plain" (e.g., `Temporal.PlainDate`, `Temporal.PlainTime`, and `Temporal.PlainDateTime`) for objects without an associated time zone. However, converting between these types and exact time types (`Temporal.Instant` and `Temporal.ZonedDateTime`) can introduce ambiguity due to time zones and DST. Temporal's API empowers developers to configure how this ambiguity is resolved.

To delve deeper into essential concepts such as exact time, wall-clock time, time zones, DST, and handling ambiguity, consult the comprehensive [Temporal API Documentation](https://tc39.es/proposal-temporal/docs/index.html).

### Temporal.Now: Instant Gratification

Temporal introduces a powerful feature known as Temporal.Now, offering real-time insights into the current system time and time zone. Developers can leverage this feature to access:

- `Temporal.Now.instant()`: Retrieve the current system exact time.
- `Temporal.Now.timeZoneId()`: Obtain the current system time zone.
- `Temporal.Now.zonedDateTime(calendar)`: Fetch the current date and wall-clock time in the system time zone and a specified calendar.
- `Temporal.Now.zonedDateTimeISO()`: Access the current date and wall-clock time in the system time zone using the ISO-8601 calendar.
- `Temporal.Now.plainDate(calendar)`: Obtain the current date in the system time zone and a specified calendar.
- `Temporal.Now.plainDateISO()`: Retrieve the current date in the system time zone using the ISO-8601 calendar.
- `Temporal.Now.plainTimeISO()`: Retrieve the current wall-clock time in the system time zone using the ISO-8601 calendar.
- `Temporal.Now.plainDateTime(calendar)`: Retrieve the current system date/time in the system time zone. Note that the resulting object does not remember its time zone, making it unsuitable for deriving other values in time zones that observe Daylight Saving Time (DST).

Here's a quick example:

```js
console.log('Initialization complete', Temporal.Now.instant());
// Example output:
// Initialization complete 2021-01-13T20:57:01.500944804Z
```

### Temporal.Instant: Capturing Fixed Points in Time

A `Temporal.Instant` represents a fixed point in time, devoid of calendar or location considerations. For instance, it can represent a moment like July 20, 1969, at 20:17 UTC. If you need human-readable local calendar dates or clock times, Temporal suggests using a combination of `Temporal.TimeZone` and `Temporal.Calendar` to obtain a `Temporal.ZonedDateTime` or `Temporal.PlainDateTime`.

```js
const instant = Temporal.Instant.from('1969-07-20T20:17Z');
instant.toString(); // => '1969-07-20T20:17:00Z'
instant.epochMilliseconds; // => -14182980000
```

### Temporal.ZonedDateTime: Navigating Time Zones and Calendars

A `Temporal.ZonedDateTime` is a versatile date/time object that considers time zones and calendars, representing real-world events from the perspective of specific regions on Earth. Whether it's December 7th, 1995, at 3:24 AM in US Pacific time within the Gregorian calendar, or any other scenario, Temporal.ZonedDateTime has you covered.

```js
const zonedDateTime = Temporal.ZonedDateTime.from({
  timeZone: 'America/Los_Angeles',
  year: 1995,
  month: 12,
  day: 7,
  hour: 3,
  minute: 24,
  second: 30,
  millisecond: 0,
  microsecond: 3,
  nanosecond: 500
}); // => 1995-12-07T03:24:30.0000035-08:00[America/Los_Angeles]
```

The broad capabilities of Temporal.ZonedDateTime make it a powerful combination of Temporal.TimeZone, Temporal.Instant, and Temporal.PlainDateTime.

### Temporal.PlainDate: A Date Without Time or Time Zone

A Temporal.PlainDate object encapsulates a calendar date without any association with a specific time or time zone. For example, it can represent August 24th, 2006.

```js
const date = Temporal.PlainDate.from({ year: 2006, month: 8, day: 24 });
date.year; // => 2006
date.inLeapYear; // => false
date.toString(); // => '2006-08-24'
```

Additionally, `Temporal.PlainDate` can be converted to partial dates like `Temporal.PlainYearMonth` and `Temporal.PlainMonthDay`.

### Temporal.PlainTime: Time Without Date or Time Zone

A `Temporal.PlainTime` object represents a wall-clock time without any connection to a specific date or time zone. Think of it as a way to represent times like 7:39 PM.

```js
const time = Temporal.PlainTime.from({
  hour: 19,
  minute: 39,
  second: 9,
  millisecond: 68,
  microsecond: 346,
  nanosecond: 205
});
time.second; // => 9
time.toString(); // => '19:39:09.068346205'
```

### Temporal.PlainDateTime: Date and Time Without Time Zone

A `Temporal.PlainDateTime` elegantly represents a combination of calendar date and wall-clock time without tying itself to a specific time zone. It's perfect for scenarios like December 7th, 1995, at 3:00 PM in the Gregorian calendar.

```js
const dateTime = Temporal.PlainDateTime.from({
  year: 1995,
  month: 12,
  day: 7,
  hour: 15
});
const dateTime1 = dateTime.with({
  minute: 17,
  second: 19
});
```

While `Temporal.PlainDateTime` is a versatile choice, it's essential to note that for use cases requiring precise time zone adjustments, especially concerning Daylight Saving Time (DST), consider using `Temporal.ZonedDateTime`.

### Temporal.PlainYearMonth: Focusing on the Year and Month

Sometimes, you need to express a date without specifying the day component. Temporal.PlainYearMonth serves this purpose, making it suitable for scenarios like "the October 2020 meeting."

```js
const yearMonth = Temporal.PlainYearMonth.from({ year: 2020, month: 10 });
yearMonth.daysInMonth; // => 31
yearMonth.daysInYear; // => 366
```

### Temporal.PlainMonthDay: A Date Without a Year

`Temporal.PlainMonthDay` represents dates without a year component, making it ideal for expressing events like "Bastille Day is on the 14th of July."

```js
const monthDay = Temporal.PlainMonthDay.from({ month: 7, day: 14 });
const date = monthDay.toPlainDate({ year: 2030 });
date.dayOfWeek; // => 7
```

### Temporal.Duration: Measuring Time Intervals

For measuring time intervals, Temporal introduces `Temporal.Duration`. It's the go-to choice when dealing with date/time arithmetic or calculating differences between Temporal objects.

```js
const duration = Temporal.Duration.from({
  hours: 130,
  minutes: 20
});
duration.total({ unit: 'second' }); // => 469200
```

### Temporal.TimeZone: Managing Time Zone Translations

`Temporal.TimeZone` is your companion for managing IANA time zones, UTC offsets, and UTC itself. It facilitates the conversion from UTC to local date/time and helps find offsets for specific `Temporal.Instant` moments.

```js
const timeZone = Temporal.TimeZone.from('Africa/Cairo');
timeZone.getInstantFor('2000-01-01T00:00'); // => 1999-12-31T22:00:00Z
timeZone.getPlainDateTimeFor('2000-01-01T00:00Z'); // => 2000-01-01T02:00:00
timeZone.getPreviousTransition(Temporal.Now.instant()); // => 2014-09-25T21:00:00Z
timeZone.getNextTransition(Temporal.Now.instant()); // => null
```

### Temporal.Calendar: Embracing Different Calendar Systems

While most code aligns with the ISO 8601 calendar, Temporal accommodates various calendar systems. It's important to note that dates are associated with `Temporal.Calendar` objects to perform calendar-related operations.

```js
const cal = Temporal.Calendar.from('iso8601');
const date = cal.dateFromFields({ year: 1999, month: 12, day: 31 }, {});
date.monthsInYear; // => 12
date.daysInYear; // => 365
```

By navigating the comprehensive documentation and utilizing the powerful features of Temporal, developers can revolutionize their date and time handling in JavaScript. Temporal brings clarity, precision, and reliability to date-related operations, making it an invaluable addition to the ECMAScript ecosystem.

## Conclusion

In conclusion, Temporal represents a significant leap forward in addressing the long-standing date and time challenges in ECMAScript. With its user-friendly APIs, time zone support, immutability, and comprehensive documentation, Temporal equips developers to conquer complex date and time scenarios with confidence. As you explore Temporal's capabilities and integrate it into your projects, you'll discover a new level of precision and clarity in date and time handling.

## References

- [Temporal Proposal Documentation](https://tc39.es/proposal-temporal/docs/index.html)
- [Temporal GitHub Repository](https://github.com/tc39/proposal-temporal)
- [Fixing JavaScript Date](https://maggiepint.com/2017/04/11/fixing-javascript-date-web-compatibility-and-reality/).

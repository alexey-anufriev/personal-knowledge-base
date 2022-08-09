# Localization

## Common

Locale is a geographical, political, or cultural region.

&#x20;In Java, a locale can be represented by a language code in lowercase, or a language and country code, with language in lowercase and country in uppercase. Locale cannot be constructed from a single parameter that joins both components.

Java starts out by looking for a properties file with the requested locale. If it doesn't find it then it moves onto the locale with just a language code. If it doesn't find it, then it moves onto the default locale.

`Locale` has `getDefault()` and `setDefault()` methods for working with the default locale.

There are two categories for locales: `Category.DISPLAY` (to represent the default locale for displaying user interfaces) and `Category.FORMAT` (used to represent the default locale for formatting dates, numbers, and/or currencies).

## `DateTimeFormatter`

| Symbol | Meaning                    | Presentation | Examples                                      |
| ------ | -------------------------- | ------------ | --------------------------------------------- |
| G      | era                        | text         | AD; Anno Domini; A                            |
| u      | year                       | year         | 2004; 04                                      |
| y      | year-of-era                | year         | 2004; 04                                      |
| D      | day-of-year                | number       | 189                                           |
| M/L    | month-of-year              | number/text  | 7; 07; Jul; July; J                           |
| d      | day-of-month               | number       | 10                                            |
| g      | modified-julian-day        | number       | 2451334                                       |
| Q/q    | quarter-of-year            | number/text  | 3; 03; Q3; 3rd quarter                        |
| Y      | week-based-year            | year         | 1996; 96                                      |
| w      | week-of-week-based-year    | number       | 27                                            |
| W      | week-of-month              | number       | 4                                             |
| E      | day-of-week                | text         | Tue; Tuesday; T                               |
| e/c    | localized day-of-week      | number/text  | 2; 02; Tue; Tuesday; T                        |
| F      | day-of-week-in-month       | number       | 3                                             |
| a      | am-pm-of-day               | text         | PM                                            |
| h      | clock-hour-of-am-pm (1-12) | number       | 12                                            |
| K      | hour-of-am-pm (0-11)       | number       | 0                                             |
| k      | clock-hour-of-day (1-24)   | number       | 24                                            |
| H      | hour-of-day (0-23)         | number       | 0                                             |
| m      | minute-of-hour             | number       | 30                                            |
| s      | second-of-minute           | number       | 55                                            |
| S      | fraction-of-second         | fraction     | 978                                           |
| A      | milli-of-day               | number       | 1234                                          |
| n      | nano-of-second             | number       | 987654321                                     |
| N      | nano-of-day                | number       | 1234000000                                    |
| V      | time-zone ID               | zone-id      | America/Los\_Angeles; Z; -08:30               |
| v      | generic time-zone name     | zone-name    | Pacific Time; PT                              |
| z      | time-zone name             | zone-name    | Pacific Standard Time; PST                    |
| O      | localized zone-offset      | offset-O     | GMT+8; GMT+08:00; UTC-08:00                   |
| X      | zone-offset 'Z' for zero   | offset-X     | Z; -08; -0830; -08:30; -083015; -08:30:15     |
| x      | zone-offset                | offset-x     | +0000; -08; -0830; -08:30; -083015; -08:30:15 |
| Z      | zone-offset                | offset-Z     | +0000; -0800; -08:00                          |
| p      | pad next                   | pad modifier | 1                                             |
| '      | escape for text            | delimiter    |                                               |
| ''     | single quote               | literal      | '                                             |
| \[     | optional section start     |              |                                               |
| ]      | optional section end       |              |                                               |
| #      | reserved for future use    |              |                                               |
| {      | reserved for future use    |              |                                               |
| }      | reserved for future use    |              |                                               |

## `MessageFormat`

```
int planet = 7;
String event = "a disturbance in the Force";

String result = MessageFormat.format(
    "At {1,time} on {1,date}, there was {2} on planet {0,number,integer}.",
    planet, new Date(), event);

// At 12:30 PM on Jul 3, 2053, there was a disturbance in the Force on planet 7.


int fileCount = 1273;
String diskName = "MyDisk";
Object[] testArgs = {new Long(fileCount), diskName};

MessageFormat form = new MessageFormat(
    "The disk \"{1}\" contains {0} file(s).");

System.out.println(form.format(testArgs));

// The disk "MyDisk" contains 1,273 file(s).
```

## `Properties`

`String getProperty(String key, String defaultValue)`\
`Object get(Object key)`

Getters return `null` if no value found.

## `ResourceBundle`

`String getString(String key)` - `MissingResourceException` – if no object for the given key can be found.

## `LocalDate`

Cannot be formatted to time:

```
var d = LocalDate.of(2021, 4, 18);
System.out.println(d.format(DateTimeFormatter.ISO_LOCAL_DATE_TIME));
// UnsupportedTemporalTypeException
```

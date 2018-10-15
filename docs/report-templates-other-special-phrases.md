---
Title: Report Templates - Other Special Phrases
---

There are several other parse phrases processed by the interpreter:

-   `#Balance_Date#` - Returns the date for which report is
    processing. The output string is in the following format: "for date
    **DD.MM.YYYY**", where **DD.MM.YYYY** is the day, month and year
    according to the **PHD** server's local settings;

-   `#Balance_Date32#` - Returns timestamp in **Unix** time
    (**INT32**);

-   `#Balance_Date64#` - Returns timestamp in **Windows** file time
    (**INT64**);

-   `#month_total_days#` - Returns the number of the last day of
    the month according to `balance_date`. The output data is 2
    symbol string, which can be 28, 29, 30 or 31;

-   `#month_current_day#` - Returns the current day number
    according to `balance_date`. The output is a 1 or 2 symbol
    string, representing a number between 1-31.

-   `#week_start_on_X#` - where "**X**" is one of the following
    (***Sunday***, ***Monday***, ***Tuesday***, ***Wednesday***,
    ***Thursday***, ***Friday***, ***Saturday***). This special phrase
    sets beginning of the week. If omitted ***Monday*** is considered as
    the beginning of the week.

-   `#week_day_on_dXhY#` - returns OLE Automation date. Where **X**
    are days and **Y** are hours forward since prev week last day **0h.**

    **Constraints**

-   Avoid using tags with names which includes mathematical signs like
    (+,-,\*). The slash "**/**", backslash "**\\**" and "**-**"
    symbols must be escaped respectively with "**\[slash\]**\",
    \"**\[backslash\]**\" and "**\[minus\]**". ;

-   Data fetches for times in the future will not be processed;

-   Data fetches for times grater than requested report date shifted
    forward with 1 day will not be processed.
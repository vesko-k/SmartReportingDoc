---
Title: Report Templates - Operators
---

The interpreter maintains the basic mathematical operations: addition,
subtraction, multiplication and division with their priorities. So if
the user wants to add the values of two tags, the parse will look like
`$$FIELD+FIELD`.

__Example:__

` $$phd#snapshot#day_val_d0_h12#TI301_63.PV + phd#snapshot# day_val_d0_h12#TI301_64.PV`

Where `TI301_63.PV` and `TI301_64.PV` are the names of the tags, which values have to be added.

A field can be a constant number.

__Example:__

`$$phd#snapshot#day_val_d1_h12#TI301_63.PV+ 152.34`

This line will return as a result the sum of the values of the tag
`TI301_63.PV` and the number `152.34`.

Using formulas in the template is also allowed but visual basic scripts
are not.
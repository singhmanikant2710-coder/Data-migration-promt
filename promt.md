Hi Geoff,

Thanks for flagging. This looks like the same root cause as the 1-30
DelinquentID issue: the CSV parser is auto-detecting CompCallCode as a
number, so "01E0" is read as scientific notation. I'll fix the monthly
upload so CompCallCode is always read as text, and I'll check whether
xlsx upload is supported.

Note: the 8/31 data already loaded will still hold the converted values
(e.g. "010"), so once the fix is in, John will need to re-upload the 8/31
file. I'll confirm as soon as it's ready.

Thanks,
Manikant

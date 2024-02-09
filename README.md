# DVLARegCheck
RegEx expression function for UK DVLA Vehicle Registration Plate checks.


The regular expressions in the file `isValidReg` were authored by Zion Fox in late 2018 during an assignment for Manchester Metropolitan University.
Although this regex function above isn't perfect, it's the best and likely most accurate one we have available as it has been discussed with the DVLA in detail to be as accurate as possible.

They were coined after searches on the internet were providing results, but they were too simple and would allow completely invalid registration plates to pass.

Example simple for current style plate: `"[A-Z]{2}[0-9]{2}[A-Z]{3}"` __(<- Do Not use this expression!)__

This would allow illegal characters, and therefore licence plates, to pass the test, and in the real world this wouldn't be allowed.


## Regex Definition:
http://dvlaregistrations.direct.gov.uk/help/website-usage.html?question=style_reg

DVLA define UK registration plates into four groups:
* Current style: AA## AAA
* Prefix style: A### AAA
* Suffix style: AAA ###A
* Dateless style #### AA or AA #### or ### AAA or AAA ### [or AAA #### (NI only)]

The following regular expression is designed to cover all of these styles, and alternatives. These are split into groups contained with parenthesis, and separated by pipes.
Each group starts with `^` and ends in `$`, these are to signify the start or end of the test string, or lines within the string.

---

### Group one: Current Style
* regex literal: `^[A-HJ-PR-Y]{2}(?!00|01|50)[0-9]{2} ?[A-HJ-PR-Z]{3}$`
* Date Range: September 2001 to September 2049

Current style registration plates are two letters ranging from A to H, J to P, and R to Y, followed by two numbers, followed by three letters ranging from A to H, J to P, R to Z. The numbers are the year marker.

__DVLA disallow any letters to be I or Q, but specifically the leading letters to not be Z. The numbers also cannot be 00, 01, or 50.__

The disallowed numbers use a technique called negative lookahead (contained in a subgroup), which will check the next characters for a match, and discard the result if it does.

---

### Group two: Prefix Style
* regex literal: `^[A-HJ-NPR-Y](?!0)[0-9]{1,3} ?[A-HJ-PR-Y]{3}$`
* Date range: August 1983 to August 2001

Prefix style registration plates are one letter, one to three numbers, and three letters. The first letter is the year marker.

Similar to the current style of registration plates, __the DVLA disallow some characters, mostly I, Q and Z from all characters, but also the year marker disallows O from being shown.
Also, the number section must not start with a 0.__

---

### Group three: Suffix Style
* regex literal: `^[A-HJ-PR-Y]{3} ?(?!0)[0-9]{1,3}[A-HJ-NPR-Y]$`
* Date range: February 1963 to July 1983

Suffix style registration plates are three letters, one to three numbers, and one letter. The last letter is the year marker.

Same restrictions as the Prefix style, but the whole format is reversed, where the year marker is at the end of the plate, not the start.

---

### Group four: Dateless Style
* regex literal: `(^[0-9]{1,4} [A-HJ-PR-Y]{1,2}$)|(^[0-9]{1,3} [A-HJ-PR-Y]{1,3}$)|(^[A-HJ-PR-Y]{1,2} [0-9]{1,4}$)|(^[A-HJ-PR-Y]{1,3} [0-9]{1,3}$)|(^[A-HJ-PR-Y][A-PR-Z]{2} ?[0-9]{1,4}$)`
* Date range: up to February 1963

Dateless style registration plates in England, Scotland and Wales are one to three/four numbers, and three/two letters. The format can be reversed, with the letters first and numbers last. Maximum number of characters is 6, the minimum is 2.

Northern Ireland limitations are only three letters, followed by one to four numbers. Letter range is unknown, but likely still restricts I (for being too similar to 1), and Q (for being used for date undetermined), however their example shows a Z (usually disallowed for being too similar to 2).

Therefore I have assumed that I and Q are disallowed, but Z is allowed, but not as the first character.

---

Since being in contact with the DVLA for information on this topic:
* They have confirmed to me that the letter Q is used whenever the vehicle's date cannot be determined.
  * I have decided to disallow this to be a case, as the vast majority of vehicles on the road will have a date of manufacture.
* In Nothern Ireland, Z and I are used in the plate, but never as the first letter. They must be the second or third letters.
 

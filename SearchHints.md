##Library - Quick Start Search tips##
*  Simply enter a word or words in the box and click on Search to conduct a free text search
*  By default, there is an "OR" relationship between the words in a free text search
*  Enclose a word or phrase in double quotes “xxxxx” to find the exact  word or phrase. For example, "water temperature"
*  To search only a specific metadata field, precede the search term(s) with the field name and a colon. For example, title:"temperature profile"

## Search Terms  ##
A query is broken up into terms and operators. There are two types of terms: Single Terms and Phrases. A Single Term is a single word such as air or quality. A Phrase is a group of words surrounded by double quotes such as "air quality". Multiple terms can be combined together with Boolean operators to form a more complex query. 

***Examples:***

*  Searching for **air** could result in 35 hits (items contain the word air)
*  Searching for **quality** results in 123 hits (items contain the word quality)
*  Searching for **air quality** (without quotes) results in 148 hits (items contain the words air or quality or both)
*  Searching for **air AND quality** results in 10 hits (results contain both words air and quality)
*  Searching for **"air quality"** (with quotes) results in 7 hits (items contain the words air and quality directly after each other)
*  Searching for **title:air** results in 5 hits (items contain the word air in the title)
*  Searching for **title:quality** results in 14 hits (items contain the word quality in the title)
*  Searching for **+title:air +title:quality** or **title:"air quality"** results in 2 hits (both items contain both words air and quality in the title)

***Special Characters***

The Geoportal Server supports escaping special characters that are part of the query syntax. The current list special characters are: 

     + - && || ! ( ) { } [ ] ^ " ~ * ? : \
 
To escape these character use a backslash character '\' before the special character. For example to search for items that contain the scale  **1:250k** use the query: **1\:250k**.

***Wildcard Searches***

The Geoportal Server supports single and multiple character wildcard searches within single terms (not within phrase queries).

Caution: You cannot use a * or ? symbol as the first character of a search.

*  To perform a single character wildcard search use the question mark character '?'. The single character wildcard search looks for terms that match that with the single character replaced. For example, to search for **'well' or 'wells'** you can use the search: **well?**
*  To perform a multiple character wildcard search use the asterisk  '\*' character. Multiple character wildcard searches look for 0 or more characters. For example, to search for **'test', 'tests' or 'tester'**, you can use the search: **test\*** . 
*  You can also use the wildcard searches in the middle of a term as for **'text' or 'test'**: **te?t**

***Proximity Searches***
The Geoportal Server supports finding words are a within a specific distance away. To make a proximity search use the tilde character '~' at the end of a Phrase. For example to search for **air** and **quality** within 10 words of each other in a document use the search: **air quality~10**

***Boosting a Term***
The Geoportal Server provides the relevance level of matching documents based on the terms found. To boost a term use the caret character '^' with a boost factor (a number) at the end of the term you are searching. The higher the boost factor, the more relevant the term will be. Boosting allows you to control the relevance of a document by boosting its term. 

For example, if you are searching for **air quality** and you want the term **air** to be more relevant, boost it using the ^ symbol along with the boost factor next to the term. You would type: **air^4 quality**. This will make documents with the term air appear more relevant. 

You can also boost Phrase Terms as in the example: **"air quality"^4 "water quality"**. By default, the boost factor is 1. Although the boost factor must be positive, it can be less than 1 (e.g. 0.2)

***Boolean Operators***
Boolean operators allow terms to be combined through logic operators. The Geoportal Server supports **AND, +, OR, NOT** and **-** as Boolean operators.

*Note:  Boolean operators must be ALL CAPS*

*  The OR operator is the default conjunction operator. This means that if there is no Boolean operator between two terms, the OR operator is used. The OR operator links two terms and finds a matching document if either of the terms exist in a document. This is equivalent to a union using sets. The symbol || can be used in place of the word OR.
*  The AND operator matches documents where both terms exist anywhere in the text of a single document. This is equivalent to an intersection using sets. The symbol && can be used in place of the word AND.
*  The + or required operator requires that the term after the + symbol exist somewhere in a field of a single document.
*  The NOT operator excludes documents that contain the term after NOT. This is equivalent to a difference using sets. The symbol '!' can be used in place of the word NOT. *Note: The NOT operator cannot be used with just one term.*

***Grouping***
The Geoportal Server supports using parentheses to group clauses to form sub queries. This can be very useful if you want to control the boolean logic for a query. For example: **(air OR water) AND quality** will find documents containing the words air or water and the word quality.

***Field Grouping***
The Geoportal Server supports using parentheses to group multiple clauses to a single field. For example: title:(air OR water) finds items that contain the words air or water in the title.

 
##Map - Quick Start Search tips##
*  You may search for data whose spatial extent lies anywhere in the world, intersects with the displayed map area, or falls entirely within the displayed map area.
*  Click-and-drag on the map to center it, then double-click or use the scroll wheel on the mouse to zoom in and out to set your region of interest, and/or use the '+' or '-'  zoom tools in the upper left corner of the map.
*  You can also draw a bounding box around the area of interest using the ‘pencil’ tool in the upper left corner of the map. Click on the tool then at one corner of the intended box, drag while holding the mouse button down. Release mouse button to complete the box. The map will auto zoom to that chosen area. Results will display over the map or click on Arrow bar on the right side of the screen to open display.
*  You can reset the extent using the ‘world’ icon in the upper left corner of the map.
*  The bottom layers icon can be used to turn on or off the results displayed on the screen by checking/unchecking the Search Results box.
*  You can also enter search terms in the Search: box to narrow your search within the map extent by typing in the search term and hitting Enter on your keyboard. 

For more information on how to specifically leverage Lucene search syntax for powerful searching in your Geoportal, please see the [Lucene website](http://lucene.apache.org/core/2_9_4/queryparsersyntax.html).


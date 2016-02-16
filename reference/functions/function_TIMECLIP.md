---
title: "TIMECLIP"
layout: "function"
isPage: "true"
link: "/warpscript/functions"
desc: "Clip a Geo Time Series to only retain ticks that are within a given time range"
categoryTree: ["reference","functions"]
oldPath: ["30-functions","50-gts","9100-function_TIMECLIP.html.md"]
category: "reference"
---
 

The `TIMECLIP` function takes a GTS or a list of GTS and clip each GTS to only retain ticks that are within a given time range.

This function consumes two parameters from the top of the stack and then expects to find a single GTS instance or a list of GTS instances on the top of the stack.

According to the type of parameters, there are two syntax for defining the time range :

- A start and end timestamps in [ISO8601](http://en.wikipedia.org/wiki/ISO_8601) format, *i.e.* **YYYY-MM-DDTHH:MM:SS.SSSSSSZ**. 

- An end timestamp in microseconds since the Unix epoch and a duration in microsecondes, if you want to define a time range with a spefific duration before a timespamp (for example to keep only the values from the last minute).


## Example ##

Initial stack:

    TOP: {"c":"test name","l":{"label0":"42","label1":"foo"},"a":{},"v":[[100,10],[200,9],[300,8],[400,7],[500,6],[1100,10],[1200,9],[1300,8],[1400,7],[1500,6]]}


WarpScript commands:

    1250                  // Origin timestamp
    250                   // Duration
    TIMECLIP  

Stack: 

    {"c":"test name","l":{"label0":"42","label1":"foo"},"a":{},"v":[[1100,10],[1200,9]]}

## Let's play with it ##

{% raw %}
<warp10-warpscript-widget backend="{{backend}}"  exec-endpoint="{{execEndpoint}}">// Create a new GTS with ten values separated by a pause 
NEWGTS 
'test name'
RENAME
{ 'label0' '42' 'label1' 'foo' }
RELABEL
100  NaN NaN NaN 10 ADDVALUE
200  NaN NaN NaN  9 ADDVALUE
300  NaN NaN NaN  8 ADDVALUE
400  NaN NaN NaN  7 ADDVALUE
500  NaN NaN NaN  6 ADDVALUE
1100  NaN NaN NaN 10 ADDVALUE
1200  NaN NaN NaN  9 ADDVALUE
1300  NaN NaN NaN  8 ADDVALUE
1400  NaN NaN NaN  7 ADDVALUE
1500  NaN NaN NaN  6 ADDVALUE
1250                  // Origin timestamp
250                   // Duration
TIMECLIP  
</warp10-warpscript-widget>
{% endraw %}    

{% raw %}
<warp10-warpscript-widget backend="{{backend}}"  exec-endpoint="{{execEndpoint}}">// Create a new GTS with ten values separated by a pause (boundary in ISO8601)
NEWGTS 
'test name'
RENAME
{ 'label0' '42' 'label1' 'foo' }
RELABEL
1000  NaN NaN NaN 10 ADDVALUE
2000  NaN NaN NaN  9 ADDVALUE
3000  NaN NaN NaN  8 ADDVALUE
4000  NaN NaN NaN  7 ADDVALUE
5000  NaN NaN NaN  6 ADDVALUE
11000  NaN NaN NaN 10 ADDVALUE
12000  NaN NaN NaN  9 ADDVALUE
13000  NaN NaN NaN  8 ADDVALUE
14000  NaN NaN NaN  7 ADDVALUE
15000  NaN NaN NaN  6 ADDVALUE
12500 ISO8601          // End Timestamp in ISO8601
11000 ISO8601          // Start Timestamp in ISO8601
TIMECLIP  
</warp10-warpscript-widget>
{% endraw %}  

## Unit test ##

{% raw %}
<warp10-warpscript-widget backend="{{backend}}"  exec-endpoint="{{execEndpoint}}">// Create a new GTS with ten values separated by a pause
NEWGTS 
'test name'
RENAME
{ 'label0' '42' 'label1' 'foo' }
RELABEL
100  NaN NaN NaN 10 ADDVALUE
200  NaN NaN NaN  9 ADDVALUE
300  NaN NaN NaN  8 ADDVALUE
400  NaN NaN NaN  7 ADDVALUE
500  NaN NaN NaN  6 ADDVALUE
1100  NaN NaN NaN 10 ADDVALUE
1200  NaN NaN NaN  9 ADDVALUE
1300  NaN NaN NaN  8 ADDVALUE
1400  NaN NaN NaN  7 ADDVALUE
1500  NaN NaN NaN  6 ADDVALUE
1250                  // Origin timestamp
250                   // Duration
TIMECLIP
VALUES SIZE 2 == ASSERT      // 2 ticks in range
'unit test successful'
</warp10-warpscript-widget>
{% endraw %}     

{% raw %}
<warp10-warpscript-widget backend="{{backend}}"  exec-endpoint="{{execEndpoint}}">// Create a new GTS with ten values separated by a pause (boundary in ISO8601)
NEWGTS 
'test name'
RENAME
{ 'label0' '42' 'label1' 'foo' }
RELABEL
1000  NaN NaN NaN 10 ADDVALUE
2000  NaN NaN NaN  9 ADDVALUE
3000  NaN NaN NaN  8 ADDVALUE
4000  NaN NaN NaN  7 ADDVALUE
5000  NaN NaN NaN  6 ADDVALUE
11000  NaN NaN NaN 10 ADDVALUE
12000  NaN NaN NaN  9 ADDVALUE
13000  NaN NaN NaN  8 ADDVALUE
14000  NaN NaN NaN  7 ADDVALUE
15000  NaN NaN NaN  6 ADDVALUE
12500 ISO8601          // End Timestamp in ISO8601
11000 ISO8601          // Start Timestamp in ISO8601
TIMECLIP
VALUES SIZE 2 == ASSERT      // 2 ticks in range
'unit test successful'
</warp10-warpscript-widget>
{% endraw %}        
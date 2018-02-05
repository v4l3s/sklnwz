+++
title="Interview with Parsa Rahimi: Why and How I Made a Calendar Generator"
date="2018-01-24"
image="https://www.wunderlist.com/blog/introducing-the-wunderlist-calendar-app-for-outlook-on-iphone-ipad-android/"
+++
Below is an interview with Parsa Rahimi, creater of the Calendar App that has helped students deal with the rotating 8 day - 5 classes schedule. 

**Introduction**
----------------

On September 19th, I published a [web app](https://parsuli.net/projects/asg/).
What does it do? It takes the schedule of a student at my school, looks up which *day* it is and based on that, it creates a set of events for that day. This might sound a little confusing so let me break it up into pieces.

 

**My​ Schedule System**
----------------------

When I first moved to my current school, the schedule was normal. Every day you’d have a set of classes and they would rotate throughout the week. We had seven classes total and they were the ones that looped. Every class got repeated exactly three times with the exception of period 7 which would usually be a *useless* class such as Film or P.E.. Here is a chart to demonstrate what it
used to be like:

| **Monday** | **Tuesday** | **Wednesday** | **Thursday** | **Friday** |
|------------|-------------|---------------|--------------|------------|
| Period 1   | Period 5    | Period 2      | Period 6     | Period 3   |
| Period 2   | Period 6    | Period 3      | Period 7     | Period 4   |
| Period 3   | Period 7    | Period 4      | Period 1     | Period 5   |
| Period 4   | Period 1    | Period 5      | Period 2     | Period 6   |

 

Last year however, they decided it was time to change things a bit. Thus, they
came up with the **Alphabet-Day System 1.0**. I believe I’m the only who calls it that but that’s not without good reason. I call it the Alphabet-Day System because suddenly we stopped caring about weekdays. Instead you had five new days called “A-Day, B-Day, C-Day, D-Day, E-Day”. This system is called a five-day rotating schedule. Basically, it removes the problem of holidays. Where in your typical school schedule you have certain classes on certain days of the week, here you have them on certain labeled days. So now if there is a holiday on a Friday, the next Monday starts off with the classes you were supposed to have on Friday and everything shifts off by one. This makes it so you get an even
distribution of your classes. The downside to it? You have to keep track of which *day* it is and if you get it wrong you bring the wrong materials or you study for the wrong exam. To be fair, our school did make a dynamic iCal containing all this information but they never advertised it as far as I know.


This year, the made things even worst. They introduced the newly polished…
**Alphabet-Day System 2.0** also known as an *eight*-day rotating schedule. It’s practically the same thing with more features no one asked for – like the upgrade from the iPhone 6s to the iPhone 7. Some of these features are:

-   A five period day with inconsistent class durations

    -   An eight day schedule which spanned across a week and a half

    -   Eight periods instead of seven.

    -   A *blue moon* class where every week, a class gets repeated four times
        instead of three.

    -   Seniors ending a class at a different time than other grades.

This was the last straw, I was about to start the IB, my brain would be filled
with other things, I didn’t have the time to keep track of what day it was and what classed I’d have the following day. I decided it was time for a solution.

 

**The Ideas**
-------------

### The Table


I had never worked with calendars in my entire life. I hated them. I am not an organized person so I never saw any use in them. In fact I ignored the calendar solution for a bit while making this app so let’s start there.

My first idea was to parse the information from the school provided calendar and feed that into an HTML table. The idea was something like this (in not-so-pseduocode):

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
A-Day = [Period 1, 2, 3, 4, 5]
B-Day = [Period 6, 7, 8, 1, 2]
...
H-Day = [...]

var today = check today's date in schoolcalendar.ics and get event name that contains "-Day";

get array with the same name as variable today /*using eval()*/

take that array and insert the data into the table.
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The result would look something like this:

| **A-Day** |
|-----------|
| Period 1  |
| Period 2  |
| Period 3  |
| Period 4  |
| Period 5  |

Now this idea would have probably worked. In fact the initial stages are on my Codepen project, commented out. There were some problems with this solution though. Firstly, I wanted to make this code functional for anyone. I wanted every student and teacher at our school to be able to use this program. 
Secondly, this was being designed to work online, with people being required to open up their browser every time in order to see what they had that day. That would mean that I had to use cookies to keep the classes saved and that was beyond my scope. Next, this would only show the schedule for today, not for any time a user wanted it to. Lastly, this was being made with the goal of satisfying my needs and mine only. It was too personalized to fit my schedule and my lifestyle. As a matter of fact, the way I planning on using this was not in a browser but on my phone. I was planning on displaying the table of my lock screen during the time I was at school so I could check it at a glance. The only way one can achieve this (as far as I know) on an iPhone is if they are jailbroken. I was and am jailbroken but I know very few others who are.

 

### The Calendar

Idea number two – and the final idea – was to do the same as before except that now I create an empty calendar, loop through the entire school calendar file, checking one by one if they contain “-Day” and if they do, store which *day *it is as well as the real world date for said event. Then I hard coded the time table so that on day DD-MM-YYYY the code creates 6 events: “X-Day”, “Class 1 for day X”, “Class 2 for day X”, “Class 3 for day X”, “Class 4 for day X”, “Class 5 for day X”. Not-so-pseudocode:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
A-Day = [Period 1, 2, 3, 4, 5]
B-Day = [Period 6, 7, 8, 1, 2]
...
H-Day = [...]

calendar = newCalendar()

for events in schoolcalendar.ics
    if eventName() contains "-Day"
        today = eventName()
        date = eventDate()
    else
        break
    schedule = eval(today)
    create event on day date with title today at time t  
    create event on day date with schedule[0] today at time t 
    create event on day date with schedule[1] today at time t 
    create event on day date with schedule[2] today at time t     
    create event on day date with schedule[3] today at time t
    create event on day date with schedule[4] today at time t

download calendar
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You might notice in the pseudocode that I use some functions which do not exist in JavaScript. That’s because I used two free libraries from GitHub:
[ical.js](https://github.com/mozilla-comm/ical.js/wiki) (for the parsing of the school calendar) and [ics.js](https://github.com/nwcell/ics.js) (for the creation of the new calendar and it’s events). For the latter, I had to make my own changes to it so the calendar would download in the format I wanted to it as well as added an alarm to all my events. You can find my patch [here](https://github.com/par5ul1/ics.js/blob/patch-1/ics.deps.min.js) (I just
changed the ics.deps.min.js as it had the dependencies included).

 

**Beta Testing & Debugging**
----------------------------

As is bound to happen with any sort of program, errors surface. One of the ways I found these errors was through the help of *Beta-Testers*. Before e-mailing the entire school about my program, I had to make sure that it was right and that no large issues were present.

Some of the problems I found myself. For one, the calendar did not work on
Google Calendar. After having done a bit of research, I quickly figured out a
patch which had to do with the way the ics file was formatted. Next, Blob. The way the file is generated and pushed for download is through something called Blob. Frankly, I am still unsure what it is so I will not try to explain it but you can find more about it [here](https://developer.mozilla.org/en-US/docs/Web/API/Blob). The problem with Blob is that it does not work on mobile safari, therefore people couldn’t download a calendar straight from their phones and install it. Unfortunately, I could not fix this even after having tried various solutions so I opted to create a mini-tutorial on how to get the file on a computer and e-mail it.

As far as beta testing goes, a friend of mine got the calendar a week before
launch so he could try it and let me know what he thought about it. Thankfully, this friend of mine was a senior and he realized something. Seniors have a thing informally knows as *DP-Flex.* In short, every period eight class they rotate through their one-seven classes. If the class for that day happens to be a higher level, they go to it. Otherwise, they have Independent Study. The
solution wasn’t too hard but I won’t go into detail here as this post is already
long enough. Instead, I wanna talk about one big issue which arose a few weeks ago and it only affected late adopters of the calendar:

 

### Daylight Savings

Apparently, I made some changes to the header of the ics after having launched and that got rid of timezones. So now, after the clocks moved back one hour, school starts at 07:55 instead of 08:55. This annoyed me a lot. It affected me personally because it reminded me about class changes at the wrong time. I ended up fixing it by changing one line of ics.js.

 

 

**Some Last Words**
-------------------

### Open-Source

Soon, I am planning on polishing up the code and creating a free library out it such that people can look for events and do things with their information. I
honestly see very little use out of it but if I could use it for something,
what’s to say someone else doesn’t need this. Once done, I will edit the post with a link to it in case anyone is interested.

 

### How do I feel the final result?

I am happy I made this. I mean, there are superficial benefits to me making the program such as the fact that it helps me know my classes or the recognition it got me at school for a while but one of the most important effects of me taking on this challenge is the experience I got out it. I learnt so much about how calendars work, about how difficult it is to make sure the program runs smoothly, how to troubleshoot and update the program once bugs are found and so much more. As the saying goes:

>   It's not about the destination; it's about the journey.


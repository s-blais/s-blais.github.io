---
layout: post
title:      "Taming the Tinker Urge"
date:       2020-01-14 19:04:56 +0000
permalink:  taming_the_tinker_urge
---


I'm rapidly closing in on submission of my Sinatra project – and as eager as I am to put it to bed, it feels like a good time to blog about my experience with the process. I'm not setting out to present a well-crafted narrative here, rather more of a short collection of observations in no particular order. 

I'm quite happy with the overall concept I came up with for the project. It meshes easily with the project object-relationship requirements. It also practically begs to be extended and developed further; while implementing it, I kept saying "Oh, and it could have this! And it could do this!" (I think that's a good sign that it's a good concept). It may not be commercially viable, but it is cool that it lends itself so easily to extension. Alas, I have resisted those urges along the way, as the key to ever actually finishing this dang thing is to focus on meeting the minimum viable product (MVP) for the project. 

One aspect of this project that I'm enjoying is playing with the ERB substitution and scripting tags. It's, dare I say it, *fun* devising different ways to access and display data and elements in a way that is useful to the user. For example, a site-wide footer whose content changes depending on `logged_in?` status. Also, buttons and links on page views that appear only when applicable. And so on. Again, it's easy to get caught up in "ooh! ooh!" diversions, and I've had to reluctantly climb out of a few rabbit holes in order to return to forward momentum. Stick to the MVP. Well, mostly.

My CSS is a bit rusty, however that good ol' frenemy is reasserting its place in my life as I flesh out my stylesheet. I had to dig to find that single lesson that had an external stylesheet linked to the layout file; I certainly wasn't going to lazily pile it all into the layout head like I've done in previous lessons. And again, I've had to cap my style tinkerings to the bare minumum (which for me means making it look merely good enough that I don't cringe), because I could play with it all indefinitely. That's where the "enemy" part of "frenemy" comes in. CSS loves to say "forget about everything else and play with me all day."

Oh, the commits! So many commits. I'm glad I familiarized myself with VS Code's built-in Github tools, it's pretty slick (well, compared to command-line), and allows me to easily separate my commits if I get carried away and make changes to too many different things. Commit the page view ERB logic changes, then commit the stylesheet changes, then commit the controller changes, click, click, click. The suggested commit guidelines in the lesson text have encouraged me to change my approach to coding somewhat; I'm learning to control the scope of what I'm working on at any given moment, knowing that the task at hand needs to be well-described in 50 characters or less with the commit message. Rather than detour into a different aspect of the project that happens to capture my attention at any given moment, I make a note to take care of it later. 

I implemented the "bonus" functionality of using Rack-Flash to alert a user that they've tried creating a new user with an existing username. Not bad! Then thought it would be cool to remind a user that they're already logged in if they try to access the login page. Then decided to alert a new user with a welcome message when they've successfully created a new user. Then... okay, let's calm down and not go alert-crazy, so we can get this project done. Tame the tinker urge!

Shortly, I'll clean up a few things, record my walkthrough, complete my lesson, and move on to the Rails section!

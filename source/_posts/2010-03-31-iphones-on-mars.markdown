---
author: Peter
comments: true
date: 2010-03-31 12:42:06
layout: post
slug: iphones-on-mars
title: iPhones on Mars
wordpress_id: 413
categories:
- Conferences
- Eclipse
- iPhone
- USA
---

[EclipseCon 2010](http://www.eclipsecon.org/2010/) is over and as always has been a great chance to meet up with fellow committers and users of the software we build. [Xtext](http://www.xtext.org) has received a tremendous amount of attention: not only did we deliver several talks and tutorials on Xtext, but also did Xtext get mentioned in a number of talks we were not involved in. Xtext even received the [Eclipse Community Award](http://www.eclipse.org/org/press-release/20100322_awardswinners.php) in the category "Most Innovative New Feature of Eclipse Project".
<!-- more -->
I presented on Xpand, the template engine we use in Xtext. Of course - as I've demonstrated in my 12 minutes lightning talk - it can be used to generate anything that can be expressed with text, so you can use it in your project, too! I uploaded my slides to Slideshare, [feel free to browse them](http://www.slideshare.net/peterfriese/xpand-eclipsecon-2010).

Right after my talk, [Heiko](http://www.heikobehrens.net) showed how Xtext and Xpand can be used to generate native iPhone applications - something that he and I are currently working on as a part of our day-to-day job at [itemis](http://www.itemis.com). Heiko's slides [also are available on Slideshare](http://www.slideshare.net/HeikoB/mdsd-on-iphone-eclipsecon-2010). 

Something which - originally - wasn't related to code generation or iPhones at all was the [e4 Mars Rover Challenge](http://www.eclipse.org/community/e4RoverMars/challenge.php). In this challenge, the goal was to control a Mars rover (built with [LEGO Mindstorms](http://mindstorms.lego.com)) in a Mars-like environment by using an Eclipse e4 based client and a set of server components hosted on [Amazon EC2](http://aws.amazon.com/ec2/). In order to win, you either needed to score the highest score or improve the Eclipse e4 client. It was left to the developer's fantasy how to improve the client. In order to score the most points, you needed to drive the rover to certain places in the Mars arena and present one of two "tools" highlighted on the rover - see the [rules of the challenge](http://www.eclipse.org/community/e4RoverMars/howtoplay.php).

On the first day, I played some rounds with the basic e4 client (you needed to install it on your local machine) and scored quite OK. In the evening, after I dropped off my laptop in my room, it occurred to me that without my computer, I wouldn't be able to take the challenge that evening. "Wouldn't it be awesome to use my iPhone to control the Mars Rover", I thought? Compared to a laptop, an iPhone is a very small device so you can take it with you where ever you go. Also, as the Mars Rover Challenge was setup to be a completely distributed system, with the command server being hosted "in the cloud", delivering an image of the Mars arena to the clients, I should be able to connect to the rover from anywhere on the world (as long as I have access to the internet).

When I mentioned my thinking to [Heiko](http://www.twitter.com/HBehrens), he was equally thrilled as I was and we immediately set out to take the new challenge of writing an iPhone client for the Mars Rover.

We discussed quite a few interaction patterns we wanted to try out:



	
  1. Using buttons (much like the cursor pad on a keyboard) to "joystick" the rover

	
  2. Using a touch interface to control the rover: use pinching to rotate the rover and pointing to make it move in a certain direction

	
  3. Using the accelerometer to be able to control the Rover by tilting the iPhone



Heiko took on the job of evaluating the human interaction interface, whereas I was busy implementing the interface between the iPhone app and the command server backend. The command server of the Mars Rover is implemented as a RESTful webservice, so I could use plain HTTP calls to send commands to the server and to retrieve telemetry and imagery data. At first, I used blocking calls and later replaced them with asynchronous calls - sometingh which I'll liekly blog about the next days, so stay tuned. In order to test my code, I implemented a very simple button-based UI. With this UI, I was able to score 2702 points - not too bad!



[![iPhone Mars Rover Basic](http://farm3.static.flickr.com/2700/4478823448_963fe43271_m.jpg)](http://farm3.static.flickr.com/2700/4478823448_963fe43271.jpg)



After just two nights of development time (we wanted to attend the sessions and also had to man the itemis booth), we finally had a slick iPhone app that was able to control the Mars Rover by using the accelerometer. In the nght just before we had to hand in our submission, we shot a video of us driving the Rover. You can watch this video on the [iPhone Mars Rover website](http:/www.iphonemarsrover.com).

Here is a little diagram outlining the controls of the application:



[![iPhone Mars Rover](http://farm5.static.flickr.com/4060/4473119658_105d444c56_m.jpg)](http://farm5.static.flickr.com/4060/4473119658_105d444c56.jpg)



We knew right from the start that we wouldn't be able to match the competition criteria (the contest rules clearly state that the client has to be Eclipse e4-based in order to qualify). However, we still were eager to work on this project, as it fit very well with our current obsession with all things iPhones.

While it certainly is a fun idea to use an iPhone app to control a LEGO Mindstorms robot, we would not recommend using this approach for controlling real robots - especially if they're some 55 million km away on Mars! Signal latency from Mars to Earth is said to be around 15 to 20 minutes, which suggests to use a move-oriented approach to drive the rover.

Nevertheless it has been an exciting and fun project for Heiko and me. By the way, we're not only developing iPhone apps for fun, but also for profit: we're spearheading the [brand-new mobile development division of itemis](http://www.itemis.com/itemis-ag/portfolio/language=en/29470/business-applications-for-mobile-devices), so I'm pretty sure we will blog about mobile technology soon.

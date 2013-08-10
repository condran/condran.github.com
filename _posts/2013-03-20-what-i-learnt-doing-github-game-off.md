---
layout: post
image: /img/zombiehead.png
title: What I Learnt creating Zombie Head for GitHub Game Off 2012
summary: Thinking of programming in a game competition? I was able to get a simple game working with Cocos2D using HTML and Javascript. Here are some lessons learnt during GitHub Game Off last November.
---


Game Off is a game development competition that requires you to create a playable game using web based technologies. This was not limited only the newer HTML5 (browser only) technology, but also allowed plugins such as Unity3D and Adobe Flash to be used to play the game.

I learnt many things about making a game, the most surprising was that I could use plain HTML & Javascript. Browsers have come a long way. I also learnt quite a bit about open source in general. The good thing about joining up to a big group project like this is that you can involve yourself as little or as much as you want, but the fact that there is other people all trying to achieve the same thing can be very motivating. I highly recommend anyone to join up whether it be a game jam or something else. Let me share some of my insights.

## 1. Being busy is no excuse
One thing that is the most important resource we never have enough of: *time*. The game off couldn’t have come at a worse month for me, but it was something I really wanted to do because it would be a small win. I played around creating some artwork and some basic ideas one night, but then wasn’t able to work on it again until about two weeks before the game off finished. Obviously, you need to evaluate if it’s a short term push or if you are about to over-commit yourself.

I found the best way was to work a small bit each day, perhaps not more than an hour. On weekends I could commit a bit more time, but the small 30 minutes - 1 hour each day kept me motivated.

## 2. Small achievements are highly motivating
This was actually one of the reasons that I wanted to do the game off in the first place. However, I found that breaking down the problem into manageable smaller goals and completing those also motivated me to keep going. Also, as I said above working a little bit each day kept me involved and engaged.

Like any software project limiting the scope of what you deliver largely determines if it will be completed or not. So I was mindful of completing the overall project while making sure I was happy with the quality.

## 3. It’s amazing what you can do with a deadline looming
A firm deadline is a commitment and for most people breaking commitments are not something to do lightly. This is when we start prioritising, stop procrastinating, and start getting motivated to complete what we set out to achieve.

I broke down the game into some milestones into stages I was happy to complete. Divide and conquer is the  key. This is what I aimed to do:

 * Just complete one scene of the game
 * Simplify the game to be one head rolling (no bodies), and a way to kill the heads
 * Only once I was satisfied with the basic gameplay, add some particle effects
 * Lastly, add the sound (it was the one thing I hadn’t done before, and was willing to finish without)

## 4. When the going gets tough, take a deep breath and meditate
Well, maybe not meditate but don’t get too anxious. Perhaps this comes with experience in programming within the corporate world but don’t let time pressures rush what you are doing. The worst thing you can do is stress out over some buggy code. Fortunately when the code was pissing me off I could dive into some artwork.

## 5. The best way to learn is by doing
While I am very interested in illustration, I am just a novice at it. So this game was as much an opportunity for me to practice illustrating as well as creating a game. Thankfully these days there are so many tutorials out there on how to do things and achieve various effects that it just took time and patience to get things looking somewhat decent.

## 6. Stay close to what you know
The library I used was Cocos2D-html which is a part of Cocos2Dx. I used this as I was already familiar with Cocos2D for iPhone, but I had never used Cocos2D-html before. Now, there are a lot of similar classes, methods, and even techniques in creating a game for both engines so it wasn’t too bad. The documentation was a bit sparse, but armed with some other open source examples I was able to understand the differences and use my knowledge of the iOS based library.

## 7. Cocos2D-html is a great way to prototype
Creating a good game is as much about getting some fundamental game mechanics and testing some gaming ideas. While you can quickly get a game going in cocos2d for iPhone, coding in javascript and loading the browser seems much faster. Perhaps the best advantage of using cocos2d-html is that you can reuse all the images and even most of the code when transferring your game to iPhone or iPad.

I didn’t know this at the time, but cocos2d-html5 can also covert your game into iPhone. So once you have a hit prototype, you can convert to iOS. You can also get people to play test in the browser and get some early feedback. You can read a good tutorial on how to make cross platform games with cocos2dx from this [Ray Wenderlich tutorial](http://www.raywenderlich.com/32970/how-to-make-a-cross-platform-game-with-cocos2d-javascript-tutorial-getting-started).

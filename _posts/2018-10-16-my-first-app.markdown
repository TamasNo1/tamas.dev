---
layout: post
title:  "My first mobile app"
date:   2018-10-15 01:00:00 +0100
categories: dev mobile javascript react reactnative
---

Man, _finishing_ a project is the hardest part. I'm sure you're familiar with the excitement you get when you have that crazy idea and you spend nights and nights coding until you actually crack it and make things work. After that point suddenly the project is not interesting anymore and you slowly start abandoning it, thinking about more exciting problems to solve or new techs to try out. This is why I haven't released anything, even thought I've spent hours and hours on all sorts of side projects.

This time is different. This time I have released my side project.

But let's start at the beginning. I really wanted to get into mobile app development. Not necessarily because of the apps themselves, but to build an infrastructure to power them. I envisioned a use case where millions of concurrent users are actively doing their thing using a super/hyperscalabe network and resources. Of course as with many thing, I had to think realistically and start small. I had my first exposure to mobile web apps years ago, using the then popular [Meteor][meteor] framework. It was quick and dirty. Or just dirty. Fast forward circa 5 years and after so many trends & frameworks, I had something that might worth investing some time into: [React native][react-native]. React native promises everything I'm currently looking for: steep learning curve, fast prototyping and being able to compile to native code for all major platforms using a single codebase. I bought into this promise since both react and react-native are open source technologies built by a tech giant (Facebook) who managed to give it a proper licensing (after a small scandal). At the time of writing both react and react-native are under the MIT License (check them [here][react-license] and [here][react-native-license]). Also, both have a very active and growing online community with great tooling (such as [Expo][expo]) and libraries (like [Redux][redux]). Not to mention the absolute dominance of JavaScript and the infinite amount of resources available for it online. Frameworks can come and go, but JavaScript will stay.

The idea I had in mind was an old game but with a new twist: ~~Minesweeper~~ ~~Bombsweeper~~ BOMBSWPR with multiplayer support! The new idea here was to let players play against each other or together against the clock. I also wanted to create a "survival" mode where the board is infinite and the player can play as long as they like. As with many projects I started small: react (not react-native) tutorial. Understanding the concepts, DOM, ES6, Redux, etc.. After I had a working POC, I invested some company money (thank you [Plentific][plentific]) into learning a bit about react-native by buying these two courses on Udemy: [The Complete React Native and Redux Course][udemy-react-complete] and [React Native: Advanced Concepts][udemy-react-advanced]. Parallel to the courses I started working on the project, mostly focusing on getting an MVP out. I defined the MVP as something people can actually play with but only providing the bare minimum.

By the time I finished the "Advanced Concepts" course, I was mostly done with the MVP. As I mentioned above, it was really hard to keep working on this project after the early excitement and the fun part was over. I had 5-6 new ideas I really wanted to work on, but I knew I had to focus on this game and finish it first. Honestly, if there's this exciting new tech everyone's talking about or you have a revelation that could be your billion dollar idea, how could you resist not getting involved and keep focused on this project? Well the solution for me was simple: write the ideas down. Every time I had an idea or read about a new tech, I simply wrote it down. You might say "But Tamas, you'll never going to go back to those notes". Maybe you're right. Maybe I'll just find something new and ignore everything I wrote down. However I think this method could work. First of all, I had this amazing idea, I wrote it down, phew, no need to worry, won't be forgotten. It's funny how the excitement dies down a little once I start writing things down (which is good because I can focus again). Also once written down, I can revisit these ideas any time if something comes to my mind. Let's say 3 days after I randomly had this though how could I make this non-implemented idea better, I just need to add it to my notes. This also gives me a chance of thinking the use cases over and see if the things I thought really make sense, or my brain just wanted to come up with _something_.

But long story short, I managed to bring my app into a shape that allows people to play with it and gives me opportunity to slowly but surely add all the ideas I had in mind. After some metadata nonsense and learning a bit about publishing, Google seems to be happy with my app, so ladies and gents, please welcome my first app: [BOMBSWPR][bombswpr]

-T

[meteor]: https://www.meteor.com/
[plentific]: https://plentific.com/
[expo]: https://expo.io/
[bombswpr]: https://play.google.com/store/apps/details?id=com.boros2me.bombswpr
[redux]: https://redux.js.org/
[react-native]: https://facebook.github.io/react-native/
[react-license]: https://github.com/facebook/react/blob/master/LICENSE
[react-native-license]: https://github.com/facebook/react-native/blob/master/LICENSE
[udemy-react-complete]: https://www.udemy.com/share/1001rgBEcZeVtXQ3w=/
[udemy-react-advanced]: https://www.udemy.com/share/10000aBEcZeVtXQ3w=/

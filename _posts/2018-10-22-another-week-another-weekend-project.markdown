---
layout: post
title:  "Another week, another weekend project"
date:   2018-10-22 08:00:00 +0100
categories: dev javascript react
---

Have you ever felt that what you're doing is a complete waste of your time? That such things should be automated? Well, I got that feeling this week when I was putting together some test data for our demo team _manually_. This had to change. Now, of course the internet is full of realistic data-generator sites (you just need to google "realistic data generator"), however where's the fun in using an existing solution? I decided to put together something similar, however making it a pure client-side application. The benefits of making it such is that it's blazing fast, safe to use (no information leaves the browser) and it'll also help me to gain some more experience with [React][react].

Luckily I had a basic idea of how it should be implemented. What I wanted to have was a realistic data-generator app that would give users the ability to add/remove columns as they like, select the data type for each column and save the results to a CSV file. Since I wanted it to be more than just a "Lorem ipsum" generator, I gave [faker.js][faker-js] a try. The functionality was pretty similar to it's python sister [Faker][faker] we use at [Plentific][plentific]. Unfortunately it lacks [Hungarian localization][hungarian], but I guess it's just something I have to live without. However it does support lots of data types, which are more then plenty for my project to start with. For the CSV export I picked the excellent [PapaParse][papaparse] library (I love the Q&A style feature presentation on the homepage). I also wanted to save/export/import the configuration so people can share them with each other, but for these I just used `localStorage` and `JSON`.

So there was the plan.

I wish I could tell about all the crazy adventures I had while prototyping, but the project went pretty smoothly (almost). I created a new React application using the good old [create-react-app my-app][create-react], I `yarn add`-ed all the libraries needed and started coding. The first prototype went out fairly quickly, purely a functional prototype with 0 design. Then I wanted to add some styling using something that just works out of the box so I picked [Bootstrap][bootstrap]. It took me a while to figure out the exact way of how to integrate it with react, but at the end it was as simple as installing both [react-bootstrap][react-bootstrap] and Bootstrap. The trouble I had was around versioning because simply running `yarn add bootstrap` will install version 4, however `react-bootstrap` only supports Bootstrap version 3. Luckily the solution was as easy as pinning the version with `yarn add bootstrap@3.3.7`. Once I sorted out the styling issues (I'm a backend guy but that'll become obvious when you look at the site) I just wanted to replace the favicon and add a logo for which I used [hatchful][hatchful]. I never used them before, but they generated a free logo and sent all the different resolution resources in an email (nice one). Once that was done I just had to buy a domain name, configure DNS, build & upload the site. But that should be enough for now, I'll write a separate post about this bit of the process (spoiler: it involves AWS Route 53, S3, CloudFront and Let's encrypt). To be continued...

In the meanwhile, feel free to play around with the tool, hope you'll find it useful: [https://datagen.xyz][datagen]
 
-T

[react]: https://reactjs.org/
[papaparse]: https://www.papaparse.com/
[create-react]: https://github.com/facebook/create-react-app
[faker]: https://github.com/joke2k/faker
[faker-js]: https://github.com/Marak/Faker.js
[plentific]: https://plentific.com/
[hungarian]: https://www.youtube.com/watch?v=2YYM209GJoE
[bootstrap]: https://getbootstrap.com/
[react-bootstrap]: https://react-bootstrap.github.io/
[hatchful]: https://hatchful.shopify.com/
[datagen]: https://datagen.xyz

---
layout: post
title:  "Building NOW TV for the 3rd time"
date:   2014-10-19
tags: crosstv, angular, nowtv
---
I've had a great opportunity since starting my graduate career with Sky, my 2 year rotational programme has let me experience different sides of the software development process as a tester, business analyst and where I've finally landed as a developer.

#### The Platform Team
I started at NOW TV within the Platform team; providing a set of APIs which client teams use to display content and apply business logic on their respective platforms. My role with this team was as a tester, responsible for building the automated suite of API tests to ensure regression doesn't occur, proudly (although my code quality has greatly improved...) they're still in use to this day. For anyone interested they are built in Cucumber JVM.

In my 3rd rotation at Sky I became a technical analyst for the same team, this time however I was responsible for gathering requirements translating them into technical requirements and essentially bridging the gap between the developers and product teams. This allowed the developers to get on and do what they do best, develop and not sit in meetings about something that may or may not happen.

I'll admit this ranked incredibly highly against what I now do on a daily basis (develop), I thoroughly enjoyed working with product to refine their ideas and requirements, create user stories, arrange the stories based on dependancies and come up with plans based on deadline, I am however not a fan of the question "How long will it take?" - it just doesn't seem very agile... I've digressed, the long and short of it if I wasn't a developer I am sure my career would've gone down this route.

#### The Watch Team
With my final rotation on the graduate scheme I joined the web team, now called the watch team [watch.nowtv.com](http://watch.nowtv.com/movies/) (NOW TV has a split web architecture). Our team's sole purpose was to build the logged in customer experience on the web, we spend most of our time building content discovery tools, like the movies explorer I recently posted about. We're also the main portal for managing your account, everything from signing up to cancelling.

I class this as the 2nd time I've built NOW TV, this ties into the split architecture of the web I mentioned about, when we decided to split customer and prospect experiences into separate websites we also took the opportunity to transition to a single page application (SPA). This was an entire re-write of the application using Angular to bring the majority of it all client side. I think we have achieved a great deal given the timescales of doing the entire re-write in only a few months.

That said there is still a large portion of legacy JavaScript code which powers payment and play out, the journey here will be long and hard, its a brittle area which is heavily intertwined. We run weekly sprints so where possible when we tackle migrating legacy code we aim to have it out within a week, no customer appreciates no updates to a live site so this presents a set of challenges, which is taking longer than we’d like to overcome.

#### The Cross TV Team
However my challenges have now changed, I’ve recently transitioned to a new client team. My team now builds LG, PlayStation 3/4 and EE TV. We also plan to bring on more HTML5 capable devices soon. Up until recent most of NOW TV has individual teams to build each application, as a team we will now build a single framework which runs on multiple devices, it also needs to be future proof, its likely this framework will exist within NOW TV for a few years.

All the developers in the team bring a specific set of skills, mine lie within the vast knowledge I’ve gained over the years of NOW TV, I am extremely familiar with all the APIs. We’re also building the framework in Angular and I have over a year and a half’s worth of experience in this area now. I am also passionate about researching how to optimise web applications, considering some of the devices our app will run on may be less powerful than an iPhone or Android device we need to have incredibly efficient CSS, HTML and JavaScript.

Although we’re not too worried about slow connections here, lets face it if you can stream 720p then I don’t think you will have a problem loading the web app, but we need to be careful with memory management and squeeze where we can, JSON parse is slow, so we don’t want to parse what we won’t be using and we need to cache in app where appropriate to keep the experience smooth and not feel like we are constantly fetching from the server, there will be multiple other challenges we will face along the way, I'll post about some and how we over came them.

For now though I am just starting this new journey...

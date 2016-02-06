---
layout: post
title:  "Evaluating React at NOWTV"
date:   2015-09-25
tags: react, nowtv
---
Re-evaluating our software and framework choices on a regular basis allows us to be a dynamic and flexible player in the video streaming space. I've seen it far too often at Sky, we build large systems; traditional MVC monolith apps (we're slowly moving away), thus we're stuck with outdated technologies, due to the sheer amount of effort in migration to a new stack. Team productivity becomes restricted leading to stagnation and ultimately a loss of passion.

Software engineers produce their best work when they fall in love with a product, combined with a set of technologies they enjoy working with, it becomes more than simply churning out functionality. Passions kick in and we produce high quality software that meets and hopefully customers expectations.

### Criteria for evaluation

Rebuilding the account management area of NOWTV gave us the ability to re-evaluate our existing toolset. I'm a firm believer in picking the right tool for the job, we'll run a spike and evaluate a set of technologies against a small set of criteria.

1. Testability
2. Modularity
3. Scalability & Maintenance
4. Training & Market

We don't have the resource to re-evluate every month, we need to establish longevity; without building years worth of work in it. When I take common past use-cases and I came up with the above criteria and answer them by writing code or exploration of the topic.

#### Testability
How easy is the framework when it comes to testing? A great set of unit tests combined with a comprehensive set of functional tests will make any engineers life easier.

Units are incredibly quick to run, ability to mock dependancies and test in isolation is crucial here. Does the framework provides built in tools to enable this? In the case of React, we're using ES6 vanilla Javascipt, unit testing at this level is trivial, we need to look closer at React, Facebook created it's own test framework Jest to help, but you needn't be restricted by this as helpers are built into the library, use the test framework of your choice in essence.

We needn't discuss functional testing here, it's an important reminder to ourselves. Are there new tools that deal with async and single page applications better than the tool we currently use?

#### Modularity
Code reuse is important for a large organisation, we have multiple teams and they may have cross cutting areas, we want a consistent look and feel for the customer but we don't want to build it 3 times in differing ways in the same language.

Therefore we've opted for module and component level sharing, module sharing isn't the most complicated, but a framework that enables engineers to thing in a module sense certainly helps. React leaves this down to the engineer, it only concerns itself with the view however offers opinions on structure such as Flux.

UI components can be shared to provide the consistent look and feel, with React this is trivial. Creating decoupled components that can be used by multiple teams, still it requires an engineer to understand how to compose a decoupled component.

#### Scalability & Maintenance
Performance, can is scale? can we still find stuff 4 months on, will it become a pain to maintain. In essence we don't want to repeat the past when it comes to monolith MVCs.

Striving for lean and nimble the modularity/testing aspects and considerations in part answer this question. Whilst the remaining partly relies upon our engineers ability to create well structured applications and code that fit architectual patterns. This will achieve not only our ability to scale, but also the effort required for future maintenance.

Whilst frameworks can assist here, React itself leaves the pattern decision mainly down to the engineer. Although Flux is a common association when React is mentioned.

This can sometimes be a deal breaker for myself, if we can get a small application to scale then

#### Training & Market
I placed this in the criteria more for myself as a lead engineer, I don't expect this to come to mind for most engineers. But it becomes an important factor when a project relying on this technology choice has a tight deadline, we may need to hire in skills to train our existing engineers.

We certainly don't want engineers rushing to learn and make wrong decisions especially if those decisions are core to an applications architecture, decisions made at level could cause issues for years to come. So a few questions I ask myself:

 - How easy is it to pick up this technology?
 - Does the job market already have these skills?
 - Are training courses available?

A team of 2 pairs needs 4 pairs to reach optimum velocity, we need 4 software engineers with the relevant skills, an empty or competitive hiring market can make this complicated and lengthy. Can we get experienced Javascript developers and provide trainning? Do we have experts already within the company; can we run bootcamps? Where are the external experts? Can we get them in?

Each question leads to a potential set of new ones, eventually cumulating in a final outcome. Where React is concerned, the job market was small and competitive, but the learning curve is minimal and we can train people. In a matter of months we'll have the skills internally to provide bootcamps, external training and courses would be available not long after that. Buzz around new technologies tend to draw in talented engineers in to create well defined courses.

#### React in production
React satisfied our criteria and became a new technology added to our expanding toolset, in the coming months we'll push our first customer facing React application.

**Update:** We're now investigating React Native to build our next generation of iPhone and Android applications.

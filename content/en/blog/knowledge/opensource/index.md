---
title: "Finishing Open-Source Projects"
linkTitle: "Finishing Open-Source Projects"
date: 2019-10-15
description: >
  Take one last stride to cross the finish line and wrap up your OS project!

---
Have you ever started an open-source project, dived right into the code, discovered new API features that you loved, fiddled around with the build process, and then take a little break and never come back to it?

If that sounds familiar, this article is for you. 

In April, I set out to write Milkman, an extensible postman replacement that allows me to finally have the features always missing in Postman. In general, Postman and Milkman are applications that help test web APIs by sending/organizing requests. Milkman is a JavaFX application without many dependencies, having a smaller footprint, for example, than electron apps, and is optimized for fast start-up times. The codebase now is about 12.5K code lines big, and most of it was written in a time span of ~ 20 days in April, normally working around 4–5 hours per day. Since then, I've spent about one day a week, 2–4 hours, fixing reported issues and/or adding new features.

Now, writing a large application like this without falling into the usual traps of losing motivation requires some best practices that I would like to discuss today. I am not writing about coding style, architectures that scale, or how to allow your initial prototype to grow — these are topics for another day.

The biggest problem with open-source projects is always motivation — how to keep up the motivation and how to not lose interest and then stop? All of the practices mentioned below will help you maintain motivation and produce a mature (and hopefully open-source) project.

# Productivity
Being productive helps you spend time on what you actually want to work on, as well as spend time on what you find fun instead of producing churn. It also helps you to create a lot of features within a small amount of time.

# A Commit a Day/Week
This is along the lines of "Don’t Break The Chain" and might be a well-known motivational technique. As I had some spare time in April, I could spend each day working on Milkman. Later on, I could only spend one day a week developing, but I could still achieve a lot in that time.

It was important to me to not skip a day, and even if it was only 30–60 minutes, simply adding some sentences to the readme was better than nothing. Sometimes, I was not in the mood to work much, but as soon as I started, I wanted to finish that one feature, and by the end, I would spend 2–3 hours instead.

# Planning That One Day
As I only have that one day a week, I want to spend it as efficiently as possible. Therefore, I am planning the things I want to do on that day earlier, maybe taking some notes on the train ride. That way, when I fire up my IDE on that day, I am already ready to go and don’t have to think about what to do.

Also, I always plan my days by features. I wanted to finish this or that feature on that day. This also means that I had to estimate how long a feature would take and sometimes split it into smaller ones.

The third thing I planned for those days was the prioritization of the list of features that I wanted to implement. Each day, there was a must-have feature set and a nice-to-have feature set. After finishing the must-haves, I would normally work ~3 hours anyway. I could then decide to go on with the nice-to-have features or just stop there without any bad feelings. Most often, I picked one or two nice-to-haves as well.

# Easy Release Process
I wanted to be able to spit out new releases/features in the blink of an eye. I could spend some time streamlining the build process so that it is was not in my way. If I always spent time doing tedious steps to release Milkman, this would be a barrier to even get started adding a new feature. Right now, I only have to type ‘mvn deploy’ and the whole application gets packaged (containing a bundled JRE, an executable for Windows, etc.), and uploaded to the GitHub release page automatically. I can go away and get a coffee meanwhile.

Also, I set up a CI that provides access to nightly builds so people can even get development versions of the product.

Although it is just one command, it still takes time, and one day, I may be able to further optimize it and put all handling of this into GitHub Actions so that I can trigger it once and continue working on something else.

# Minimal Tests
This one might be a bit controversial, and I would never advise it (I always write a lot of tests normally), but I have to be honest — this was one of the drivers for being productive. I did not test any obvious functionality. It is a desktop application, and if something does not work, I will see it while using it. I did set up some tests, but they mainly test some of the more complicated stuff (postman collection import or garbage collector quirks in JavaFx).

This one might bite me later though. If I want to change things, I might accidentally break something without realizing it. I am still not sure if the trade-off is worth it; let’s see what the future brings. Up until now, I did not run into many issues because of this.

# Focus
Keeping my focus on the goal I wanted to achieve helped me stay effective and develop only what helps solving the problem. Because of the nature of applications, there is a lot of work to be done to finally see some minimum viable product (MVP). Iterating on this is then easier and you can see your progress. To optimize that, the MVP should be as small as possible, so you can see results very early on and get motivated by the outcome and iterations (return of invest, so to say).

# Clear Thread
When building the MVP for Milkman, I was solely concentrating on the clear thread. No design, no optimization, nothing that diverted me from contributing to the happy case use case of my application. The result was even less than an MVP but still a working application with persistence, a “usable” UI and something that executed requests.

# No Sidetracking
One thing that was really important was to always reflect on the work that I was currently working on. Is it contributing to the feature that I am currently working on? Yes? Good. No? Either stop it or at least timebox it (to 30 minutes/1 hour. This needs discipline) and drop it rigorously if it did not work out. I also needed to accept loose ends and non-perfect code. I needed to implement some hacks here and there to get things done. At some point later, when those hacks proved to be an obstacle, they got removed. I could obviously spend all day refactoring the whole codebase, but this does not contribute to my current goal.

# No Experiments
Normally, I start a hobby project with the goal of solving a problem but also learning something new (otherwise, it is called ‘work’ :D). Now, for this project, I avoided any experiment. I wanted to learn Kotlin or Go, maybe look into graph databases or marry Spring and JavaFx for a nicer development experience. No. The whole codebase of Milkman is boring plain old Java, nothing special. But that is what I am most productive in. I did not want to spend time finding solutions to problems introduced by my lack of knowledge about a different language or tool.

# Scope
Writing a replacement for Postman gives me a pretty good plan of what features I wanted to have but also what features I don’t want to implement. This helped me with defining the next features that I wanted to work on and leave out unnecessary stuff.

Additionally, I defined a set of general requirements before starting development, such as extensibility or fast startup time. All the features/code written was evaluated if it fits those requirements. I did this, again, to always produce stuff that contributes to some of the goals of Milkman.

# No Bugs
This does not mean to write code without bugs (especially, if there are not many tests), but I don’t accept discovered bugs to linger in my code (maybe with “FIXME” close by or a GitHub issue). If I would, they would pile up and rot my development experience. Nobody likes fixing bugs, but that’s why they are always in the must-have feature set planned for a day (see “Planning That One Day”).

# Sharing on Social Media
Finally, I would like to shortly talk about social media. Getting feedback is great, so you might want to use Twitter or even Twitch for live-coding to get feedback and also a pat on the back for your hard work, which is always a great feeling (GitHub stars, anyone? :D).

I chose not to because I think that this might lead to the wrong kind of motivation. I develop Milkman to solve my own needs. I use it day-by-day and fix whatever issue I discover. Having people talk about it is a nice side-effect and I love for people to use Milkman, but that is not the reason for me to go on. It has to be a product that YOU use, but I guess, that is clear anyway.

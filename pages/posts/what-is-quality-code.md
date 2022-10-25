---
  type: posts
  title: What is Quality code?
  date: 2018-09-11
---
  
I was asked a few questions recently about what exactly the term “quality code” means. It took me a couple of days thinking about books I’ve read and things I’ve learned from failures in the past, but I figured it out.

If you are expecting an article full of rules like “Don’t repeat yourself” (DRY) and SOLID principles, then you will be disappointed. From my experience, quality code just isn’t that simple—but I’ll outline some steps that will take you in the right direction. 

## What You Should Know to Write Quality Software

This has been written about many, many times. My recommendations for the best books to read on the subject appear later in this article; they go into a lot more detail than I’ll be able to convey in this post.

After much trial and error, I found that following these steps, in order of most importance to least, resulted in the best end product with the most maintainable code. Don’t skip to the coding (fun!) part—that is of the least importance. Creating a useful solution is much more important, no matter what the code looks like..

### What You Don’t Code Is More Important Than What You Do Code

A vital ingredient of quality code is a minimum of new code. The more code you write, the more potential bugs you could introduce. The ideal scenario is to not write any code at all: Try to use an off-the-shelf solution and don’t use that new popular framework if you don’t need to. There are many benefits to frameworks, but you need to be aware of the trade-offs, the biggest of which is added complexity.

### Set a Goal
Make sure you have a quantifiable goal. What are you trying to achieve with your solution and how do you know if it has been achieved? Always keep this goal in mind.

Think about how your solution fits into the bigger picture. You can dig a beautiful tunnel, but if it’s in the wrong place, it’s just a big hole.

### Target a Persona

This tactic has been used for ages by user experience (UX) designers, but you can get in on the action too!

A persona is an example of a typical user of your solution. When making any decisions, this tactic focuses the thought process on which solution would bring the most value to that particular user. This can be a powerful technique to turn individual developers into a team with a common mindset.

Speak to the users of your code, speak to co-workers, speak to random people on the street, show them sketches of what you are thinking about. Tear them up and have another go. It’s very easy to fall into the trap of believing users and developers think the same way. Decisions are often made based on how the developers think, not the users of the system, which may be, and often is, completely different. 

Flesh out an idea of a specific person that will use your solution. Give them a job, an age, a name, hobbies, likes, dislikes—the more specific, the better.

Draw a picture of them and put it above your monitor. For every decision, ask yourself, “What would Molly like to happen in this situation?”

This method has the benefit of reducing cognitive load. I hate making decisions, and with this method, I don’t need to. Your persona does all the decision-making. Code is only quality when it is useful to the person using it.

### Define the Structure of the Software in as Much Detail as Possible

Agile has ruined us all. It’s a great concept, but big corporations have corrupted it. Waterfall was the preferred software development methodology of the 1990s, which led to years of defining requirements, which is great—but after all those years, the requirements made no sense anymore when the users started using the system. 

Then Agile came along with the core belief that software should be designed and created in small iterations by multi-disciplinary teams and delivered to the users as soon as an increment is complete. The management then went to a two-day Agile course, became “experts,” and exclaimed, “No need for requirements anymore, no need for design! All hail Agile.”

What that meant for us developers is more pressure. But never fear, you can do something about it. Push back, only go ahead with a project that everyone involved understands. It doesn’t have to be written down in fine detail, but the entire solution has to be understood by everyone involved before any coding starts.

### Tests, Tests, and More Tests!

I don’t believe anything is correct until I test it. Test everything until you know what you should and should not test. Test with real data. Pretend you are your persona. You will thank me later. Test, code, refactor. It’s the only way to create the most maintainable code.

### The More Reviews, the Better

Get in the mindset that the code you are writing is not yours; that code belongs to your company or your client, and you shouldn’t be offended when people say you don’t own it. 

More reviews is better; constant reviews with pair programming will bring good quality. If you can stomach it, reviews with mob programming can yield even better quality. Get as many people to view your code as possible. Review each others’ codes; the many outweigh the few.

### Quality Code Is Maintainable

Readability and maintainability trump performance. Best practices don’t cover all scenarios; they are suggestions on how to clean up the majority of situations. 

You can take DRY so far that the code is almost unreadable. Keep maintenance in mind at all times—can the common function you wrote really be used in multiple places with no side effects? Your best weapon here is common sense, or pair with someone who has common sense.

## How Do I Measure the Quality of my Software? 
Now that we know what goes into quality software, we can measure it. This can be split into two sections: 1) Does the solution achieve what I want? and 2) Is it still maintainable?

### Measuring How Well the Solution Is Achieving Your Goal
This is hard to measure, but it’s probably the most important metric. Does your intended audience want to use your software and does it create value for them? The only way to measure this is to track interest, adoption, and successful use of your solution.

This can happen before you even start writing code. Describe your idea on Twitter; ask your intended users what issues they are having; show them drawings; note the size of the market and the interest you are getting over other products. Create a signup page; use your existing contacts and get emails of everyone interested in the new service, and see who signs up for beta testing.

As soon as possible, get something out there for your interested parties to use. Make it as simple as possible, but provide something they want or need. You will know if it provides no value if only your mum uses it. You’ll know you’re providing lots of value if everyone’s impatient and waiting for features. 

### Measuring the Quality of Your Code Base 

Now we know what quality looks like, it can be measured and maintained using a continuous integration (CI) pipeline with stages covering each quality point. The output of the pipeline varies depending on the tools available and the time available. No one metric will show you how healthy the code is. The start of the project is a great time to discuss what is important to the project and your business as a whole.

A beneficial side effect of setting up your pipeline before coding is that you avoid rewrites due to either misunderstanding of the target hardware or misunderstanding of what the user wants. It’s very costly to change something, both in time and pride. 

You may think your project is immune to this, and that it’s best to get some code out there and organize the pipeline later, but in most cases, later means never. It is very hard to bolt on any of these elements after even a short amount of time, and most issues can be solved almost immediately if you get the pipeline done first.

There are many blog posts by CI providers such as Codeship, Travis, and GitLab on how to implement each of the following stages in every language imaginable, but, at a minimum, you must cover:
- Code linting
- Automated unit/integration/end-to-end tests including coverage
- Security tests
- Static code analysis
- Accessibility tests
- Performance tests
- Formatting

The stages can be simple and hacked together first, but it’s important something is in place and some time is set aside to improve the coverage. Most importantly, fix bugs as soon as they come up: You are more likely to refactor in a maintainable way if the context of the code is still fresh in your mind.

## Legacy Code

Most, if not all, roles involve working with legacy code. You would have to be very lucky to work on only greenfield projects for your entire career.

### What Is Legacy Code?

I think the book The Pragmatic Programmer summed it up the best: “legacy code is any code that has already been written.” It may not be the official definition, but it is the one that is understood by most software developers when the words “legacy code” are mentioned.

### Improving Quality in Legacy Code

If it functions as designed and no enhancements are required, then leave it. If no additional value can be seen in it, any redesign will be shelved at some point and the mess will just get worse.

If enhancements are needed, first check if all the original functionality is actually needed and is of some value. The book Refactoring: Improving the Design of Existing Code by Martin Fowler details probably the best way to deal with a legacy system. 

In summary, make sure test coverage is up to standard, then make small incremental changes. It may be best to build an extension that integrates with the legacy system using a more maintainable solution, as displayed in the book, The Phoenix Project: A Novel About IT, DevOps, and Helping Your Business Win by Gene Kim, Kevin Behr, and George Spafford

With GraphQL, this approach is becoming much easier to achieve, integrating with legacy application programming interfaces. The code can gradually change over to this more maintainable codebase. In my experience, this approach works much better than a full rewrite on all but the most simple of projects.

### No Time, Too Much to Do, and Other Excuses

If you are pressured into rushing something or you feel like you don’t have time for any of the points raised, that’s OK. Just make sure you are aware of the trade-off you are making.  The less maintainable a system is, the longer you will be supporting it, and the worse you look as a developer. Build all of this into your estimates. Your client may balk, but hold your ground. Once you produce systems that do what they are supposed to do and allow quick turnaround of features, they will see the value. Whoever wants you to do this work is looking at you as the expert!

### Quality Code is a Journey, Not a Destination

Quality is difficult to get right; the software industry is still figuring this stuff out and there isn’t one clear path to one destination. Quality is a scale on which you need to actively decide where you want to sit and seriously consider the trade-offs. Don’t worry if you feel you are on the low quality end of the scale; it just means you have loads of opportunity to improve. Small improvements over a long time make a huge difference. 

## Book Recommendations

None of these books individually cover everything you need to know. Most focus on either compiling the best solution or writing the code correctly. Both types of books tout their view as the most important, but in reality, both are required to get to that mythical nirvana of quality code.

### UX (Including Personas) 

* The Inmates Are Running the Asylum: Why High-Tech Products Drive Us Crazy and How to Restore the Insanity by Alan Cooper

### Language Agnostic Points on Writing Maintainable Code

* Clean Code: A Handbook of Agile Software Craftsmanship by Robert C. Martin
* Code Complete: A Practical Handbook of Software Construction by Steve McConnell
* The Pragmatic Programmer by Andrew Hunt and David Thomasa full rewrite on all but the most simple of projects.
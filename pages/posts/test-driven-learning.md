---
  type: posts
  title: 'Test-Driven Learning: A Better Way to Learn Any Programming Language'
  date: 2019-08-14
---
  

Learning from your mistakes isn’t a new concept. Scottish author Samuel Smiles wrote in 1862 “We learn wisdom from failure much more than from success.” 

![Portrait of Samuel Smiles](https://upload.wikimedia.org/wikipedia/commons/3/3e/Samuel_Smiles_by_Sir_George_Reid.jpg)

The view has been popularized recently in software development by teams applying the DevOps and Agile methodologies of producing small improvements iteratively. If a feature doesn’t work as expected, it can be scrapped; it is a concept known as “fail fast.”

“Fail fast, learn fast” is the main premise of Jez Humble’s book Continuous Delivery: Reliable Software Releases through Build, Test, and Deployment Automation. In the context of this book,  Jez is referring to building reliable production software by releasing as often as possible. Any failures should be small and cause little impact, with the ability to rollback to the last working version and learn what exactly went wrong. 

We can apply the same concept to learning. Fail as often as you can, and learn as much as you can from those failures.

## Learn to Code by Doing

Schools have taught us to learn the facts first and  then, only when we know what we are doing, apply the skills in practice. We go to school, college, or university and then off to work with all the knowledge we have gained. 

This idea has been extended to video-learning sites, such as Pluralsight or Udemy, which are great, but a little boring. I learn best by doing, and only when I have struggled with a subject, do I ever find books or videos on the subject interesting. 

In the 16th century, before schools existed, people learned by doing. A child would be an apprentice, learning from a master until they were good enough to perform the job by themselves. The apprentice would practice repeatedly with most of the output getting thrown away. But more important than the output was what they learned from their mistakes. When they became a master, they had years of mistakes that they now knew how to avoid and techniques for all kinds of scenarios.

We are going to use the same repetitive practicing technique to learn to code a new language by creating tests for different problems and solving them in a variety of ways. Only when we don’t know how to write a test or figure out the solution, do we need to look in a book or at course material—do first, learn as you go.

## Applying Test-Driven Development to Learning


Test-driven development (TDD) wasn’t my idea; it originated from a range of techniques called extreme programming in the 1990s to help improve software quality in development teams. The core idea of test-driven development is:

Create the smallest failing test possible for code you are planning to write.
* Run the test and fail.
* Write the code until the test passes.
* Refactor the code bit by bit, and keep running the tests until the code is maintainable and readable.

The main benefits we are using from TDD is the refactor step. Once the tests have been created with well-defined inputs and outputs, the solution can take many forms. This refactoring step is really useful when learning to understand how built-in language features work and how different ways of approaching the same solution lead to the same output. Another benefit is motivation; it’s addictive watching the tests go from red (failing) to green (pass!). It’s like a kind of game.

The idea of learning through TDD isn’t new either. While learning JavaScript I was introduced to this approach via freeCodeCamp, where from the first lesson you are required to pass failing tests to complete a level. I also recently started learning Ruby and was introduced to Koans via the Edgecase Ruby Koans site. 

The idea behind freeCodeCamp and Ruby Koans is to present a long list of failing tests for you to fix. This approach of fixing tests one by one is ideal if you are just starting out. You don’t need to write tests yourself, which sometimes sucks the fun out of learning.

## Learning Through Testing Promotes a Deeper Understanding


Early in my career I survived by searching through Stack Overflow, looking at the code already written in the codebase and randomly trying code snippets to see if they would work. That was fun but with some major drawbacks: Some of the code I was writing had unintended side effects. Fixing it involved more searching and more hacks upon hacks. My shortcuts actually caused me more work, and everything took longer and made me hate seeing a tester walk toward my desk with yet another bug.

It wasn’t only my project that suffered. When applying for new jobs, I was able to answer superficial questions that I found on the internet, but when the interviewer probed further, I didn’t have deep enough knowledge to answer any further questions. These problems could have been avoided had I been practicing test-driven learning.

The main benefit of approaching learning by testing is a deeper understanding of the code you are writing, how to interact with library functions, and what output can be expected in different scenarios. 

The refactoring is the fun, experimenting part of the process; the same problem can be solved in a multitude of different ways. Experimentation allows your brain to play with an idea and provides a greater understanding of the limits and cool features of the language and where to use them. 

I have been practicing TDD for quite a few years, but when I recently needed to learn Ruby, I thought I would try out learning through testing. 

My process was:

1. Read the bare minimum about a concept.
2. Write a test for that concept.
3. Test the limits of the concept by refactoring multiple times with different solutions.

Each solution offers advantages and trade-offs. Writing down each solution adds a new tool to your toolbelt, so when you do come up against a situation where you need an algorithm, you have a collection of solutions that you now have a deep understanding of because you have already struggled with the concepts.

## Learning Through Testing Helps You After You’ve Learned the Basics

I’ve found that once you have the basics, there are other benefits of learning this way. I am much more likely to use test-driven development when writing production code; if I get in the habit while learning, I just continue on when applying it in practice. I also have a sandbox for working out difficult problems. If I have an issue with some code buried deep in a code base, isolating the problem usually speeds up the diagnosis, and if not, it’s much easier to paste that isolated code into Stack Overflow!
Learning Ruby and RSpec Through Testing

Most programming languages have lists of items, normally named an “array.” This is where I normally start when learning a new language, as most likely I will be using the items in the list frequently. The method I will be creating to demonstrate test-driven learning will return the first two items from an array. I’ll first create the test that will call the method and then create the method to satisfy the test. I’ll be using Ruby and RSpec for the following examples.

## Write the First Test

Make the first test really simple; you are learning a lot from this short test, so there’s no need to complicate things. 

If you want to try out this method, you will need to install:

Rspec: https://rspec.info/
Ruby: https://www.ruby-lang.org/en/

The principles can be applied to any technology that has a testing framework, so coding along is not required.

Gist link: https://gist.github.com/peterjgrainger/4cce0bbd1777b7bbe4ca9f145242fa0d

```
spec/shorten_array_spec.rb

require 'shorten_array.rb'

describe ShortenArray do
 it 'returns the first 2 items from the array' do
  best_array_ever = %w[best array ever]
  result = ShortenArray.first_two_items(best_array_ever)
  expect(result).to eq %w[best array]
 end
end
```

If all went correctly you should have achieved your first fail! 

```
An error occurred while loading ./spec/examples/array.rb.
Failure/Error: require 'examples/shorten_array.rb'

LoadError:
 cannot load such file -- examples/shorten_array.rb
# ./spec/examples/array.rb:1:in `require'
# ./spec/examples/array.rb:1:in `<top (required)>'
No examples found.


Finished in 0.00025 seconds (files took 0.14376 seconds to load)
0 examples, 0 failures, 1 error occurred outside of examples
```

We haven’t written the method yet to satisfy the test, so the fail here is expected and proves to us that we have set up the testing framework correctly. Failure here is an important part of the process; you will see these same errors pop up when you are coding in the future, and it will speed up your development if you remember how you fixed it.

We also learned from writing this test:

* How to reference a method in a different file.
* How to call a method.
* How to write an assertion (expect ...).
* How to create an array.

All that from just writing the test!

## Pass the Test

Write the simplest code you can think of to satisfy the test. This is where training materials come in handy, Or even better, use the language API. Ruby’s API documentation is quite easy to read but maybe a little intimidating at first.

Gist link: https://gist.github.com/peterjgrainger/8bb6085422e30e49aa2bcf05b50950cf

```
lib/shorten_array.rb

module ShortenArray
 def self.first_two_items(array)
  [array[0], array[1]]
 end
end
```

This should result in a pass:

```
.

Finished in 0.00448 seconds (files took 0.11038 seconds to load)
1 example, 0 failures
```

The passed test lets us know that we satisfied the requirements set out in the test. In the process we learned:

* How to define a module.
* How to define a method on that module.
* How to pass an argument to the method.
* How to reference a value in an array, by index.
* How to return a value from a method.

## Give Test-Driven Learning a Shot

Learning this way is fun! I love coding, and if I can code while I’m learning, it makes me much happier.

Learning through testing includes lots of failure. Failure is important to learning, and you may not get that experience by reading a book or taking a course alone. It’s much better to fail and learn from it early than to fail when it matters.

Test-driven learning is a great way to get a deeper understanding of the language you are trying to learn. You will be learning because you need to pass the next test rather than learning dry facts and trying to apply them when you need them.

This approach might not be for everyone, but give it a try. Whether you like it or not, you will probably learn something new.

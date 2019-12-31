---
title: Go's Date Formatting - yay or nay?
date: "2018-12-30"
draft: false
---

Lately I've been looking for excuses to write more and more Go, both on personal stuff (mostly for fun) and on smaller projects at work. I don't know enough about low level languages, multithreading, or concurrency to make informed judgements of why Go is better than say, Rust, or C++. For me, Go seems to strike a good balance between giving the programmer control over minute details and maintaining the readability of the resulting code. I particularly enjoy how the language enforces decent code style through `gofmt`, the compiler's refusal to execute if unused variables or imports are found, and the implied exporting of structs and functions if the first letter of their names are capitalised. The rigidity of the Go pipeline and tooling, instead of stifling creativity, enable the programmer to focus on the logic rather than the plumbing.

That being said, I was mildly delighted to discover (while attempting day 4 of [Advent of Code](https://adventofcode.com/) that Go has a novel way of representing date formats. What are date formats? Chances are you would've come across the infamous `strftime` [directives](https://devhints.io/strftime); what appears to be the de-facto standard of date format representations across many languages, and rightfully so being the UNIX standard. It's the standard with all the percentage signs and letters that denote the whereabouts of each part of the date in a given string. However, Go chooses a different approach.

Go's `time` package provides a [Parse()](https://golang.org/pkg/time/#Parse) function that accepts a `layout` and `value`, and returns a `time.Time` type storing the result of the interpretation. Rather than implementing the `strftime` standard, the package expects you to supply this weird `layout` parameter to tell the compiler how your input string is formatted. The value of this is a permutation of Go's *reference time* that matches the date string you want to parse.

Say whaaa? That's right! Go defines a specific date/time in history - the *reference time* - and uses the uniqueness of the values of the various components in that date to try and determine the format of your own date string. The complete reference date documented is `Mon Jan 2 15:04:05 -0700 MST 2006` which exposes most of the components necessarily for 99% of date parsing needs (milliseconds appear to be missing). A bit strange? Let's run through an example:

Let's say you want to parse the string `2018-12-30 10:30pm` into a nice `time.Time` variable. You'd look at the reference date and pick out the parts that match your input - in this case we'd end up with `2006-01-02 3:04pm`. Now Go understands how to parse your string! Chuck that into `time.Parse()` and you're good to go!

I reckon it's pretty neat; a little counter-intuitive at first simply because a popular standard already exists, but leads to code that's easier to comprehend without context switching to a `strftime` reference. Provided enough commentary exists within the code to explain how Go's parsing works! What do you guys think?

# Creating a document-based application with Mate #

The code for this application is available at [/trunk/examples/documentbased/](http://mate-examples.googlecode.com/svn/trunk/examples/documentbased/). Both the code and this document was written by [Theo Hultberg/Iconara](http://blog.iconara.net/).

Jelle van Wieringen has expanded the application to include saving to Amazon SimpleDB, you can find a live demo at http://www.otemba.org/examples/flex/TheoHultberg/Main.html, and the code at https://otemba.svn.sourceforge.net/svnroot/otemba/flex/ExampleDocumentbased.

# Introduction #

Initially my goal when writing this application was to show how to work with the new LocalEventMap feature of Mate, but it evolved to something a little bigger. The application is a work in progress, but in its current form it shows how to

  * create a document-based application
  * design an application that uses Mate without the application code being aware of it
  * support undo
  * work with bindings effectively

I also aim to expand it to show how to

  * test user interfaces
  * handle navigation

## Getting started ##

If you have written applications with Mate before and consider yourself an experienced Flex developer, I recommend diving straight into the code, and come back and read the walk-though or the definitions if you're new to the PresentationModel pattern or not sure what I mean by "document-based".

The code contains a lot of explanations that you won't find here because they are more relevant in a context, so I really recommend looking at the code first, or at least have it open as you read this.

Whatever you do, don't read this from start to finish, if you get bored with a lengthy explanation of something, just skip ahead, you don't have to read all of it to understand.

## Assumptions & things ##

I assume that you are somewhat familiar with Mate, and I assume that you know Flex. This is not an introduction to Flex programming nor to Mate. I have tried to write this application as I would write any application, even though this one is smaller. Nothing is simplified just because its is an example, if it is not complete it is because the application is too small to need it (but in that case I should expand it so that I could show how to do those things too).

There are two things of note that are not included: navigation and server side integration. In the case of navigation the reason is simply that the application only has one state, but that could be changed, and it could even be argued that which document is the current is an application state (in which case navigation is implemented, just not explicitly). In the case of server side integration there is the problem of having a server to integrate to. There is an empty event handler where documents should be saved, but to implement that would mean that the application would be harder to just compile and try out, you would need to set up a server to be able to try all the features. I can write another example application that shows server side interaction, but this one will likely not have it ever.

I foresee that someone will ask something like "does this example show best practice for working with Mate?". I don't like the term "best practice", it is usually used by the clueless to give weight to bad ideas. No, this is not best practice, this is just how I write applications. I have tried to not do anything that I wouldn't do in a "real" application and I think that most of what I do is good practice, but YMMV.

## Some definitions ##

Before continuing to the next section you may want to read explanations of a few concepts:

  * [What do I mean by document-based?](DocumentBased.md)
  * [The Presentation Model pattern](PresentationModel.md)

[continue](DocumentBasedExampleWalkthrough.md)
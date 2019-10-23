from: src[url:https://indepth.dev/level-up-your-reverse-engineering-skills/]

What I cannot [re]create, I do not understand
The quote above belongs to Richard Feyman, one of the greatest scientists and physicists of our time. What he meant is that, starting with a blank piece of paper and the knowledge already in his mind, he could take any theoretical result and re-derive it. Feynman thought that ability was the true marker of understanding something.

I’m a firm believer of the need to know the fundamentals myself. Having a solid grasp on existing solutions to common problems is an absolute necessity for someone to come up with new solutions to the problems. You have to know how to solve every problem that’s been solved in the field you’re working in.

However, there’s a challenge — where do you find this kind of knowledge? In the fast-paced world we’re living in now, authors of technologies have almost no time to write documents providing insights into the fundamentals. So what’s the answer? Well, I advocate for reverse-engineering.

I’m known for reverse-engineering Angular. But Angular is not the only framework that I’ve looked at in-depth. I’ve explored Vue.js, Webpack, jQuery and lots of other web libraries and frameworks. I’m now going through React. And, I believe I’ve gained enough insights to share with you and help you get started with reverse-engineering.

To me, the process of reverse-engineering is the magic of discovering something new. It’s the state of being excited about new findings and thinking like a hacker: always being curious. I hope this guide will help your mind get to the same state.

I’ve split my findings into two articles. This article outlines guidelines and principles that I use when reverse-engineering. The second article shows a practical application of these principles through the actual process of reverse-engineering a small part of React. It also demonstrates a few interesting debugging techniques to accelerate your reverse-engineering efforts.

But first, let’s start with why you would want to engage in the reverse-engineering.

I work as a developer advocate at ag-Grid. If you’re curious to learn about data grids or looking for the ultimate Angular/React/Vue data grid solution, give it a try with our guide “Get started in 5 minutes” guide. I’m happy to answer any questions you may have. And follow me to stay tuned!
The WHY#

Let’s face it, reverse-engineering is hard work. It’s time consuming and usually requires a substantial knowledge base. So why bother?

Most people believe that the primary goal of reverse-engineering is to enhance your knowledge about a technology to find a better job. And since the lifespan of modern technologies is pretty short, it doesn’t make sense to invest time to dive too deep.

You will almost certainly obtain an excellent understanding of any technology by going through its source code. But, that is only one of the many benefits you will gain through reverse engineering.

As you go through the source code, you’ll become familiar with the new design patterns to solve common problems which you can then reuse at work. I experienced it first hand many times. For example, when I reverse-engineered the Angular Router I learned how to lazy load components and modules which helped me build a plugin-based platform.

You’ll learn many new features of a language. When I was reverse-engineering Angular I learned about monomorphism in JavaScript. You’ll learn the underlying platform’s API since most technologies use it heavily. You’ll find new things to learn and see their practical usage instead of just reading about it in a book and guessing where you could use it.

And if you decide to share your findings with the community, it will help build your public profile. This is a win-win situation because by helping others you help yourself. My success story started with Angular-In-Depth(AiD) when I decided to start writing about my findings. Since then AiD has grown into the biggest Angular publication and significantly helped me start speaking at conferences and find a perfect job at ag-Grid. By spending time and learning a technology in-depth you’re also demonstrating your problem solving skills, determination and curiosity. These are the qualities that innovative companies look for in candidates.

You’ll also get conformable with reading existing code and exploring new code bases. Robert C. Martin, commonly known as Uncle Bob, estimates that the ratio of time spent reading versus writing is well over 10 to 1.We are constantly reading old code as part of the effort to write new code. This is what all developers start with when they begin working on an existing project. And through practicing reverse-engineering you’ll have an edge here.

As you can see, reverse-engineering will make you a better engineer.

Required knowledge to help with reverse-engineering

Let’s first take a look at the things that you need to know to make the process of reverse-engineering easier and faster. Spend time to learn and master them. It’s a lot to know and most likely you won’t have that knowledge now. No worries. Set aside one or two hours a day for deliberate learning and make a goal to become an expert in these areas. You’ll get there.

Solid knowledge of the underlying platform#

The first requirement to successful reverse-engineering is to know the underlying platform APIs. If we’re talking about frameworks and libraries that run on the web, here are the things you need a good grasp of:JavaScript, DOM API and browser API.

Having a solid understanding of JavaScript doesn’t mean you know what a closure is or how this is resolved. Solid understanding means you have to know most of the advanced stuff like property descriptors (used by Vue.js),proxy objects or bit masks (used by Angular).

When talking about the DOM and browser APIs, knowing how to create and append a DOM node or execute a callback asynchronously is not enough.You need to know what will happen if you re-append an existing child node or how browsers work with unknown elements. Learn about existing APIs to make an HTTP request and its intricacies like when XHR fail callback is executed.

To master JavaScript I recommend Axel Rauschmayer’s books. To master the DOM and browsers API I recommend MDN web docs and Google developers web updates. And, if you can learn to read specs, that of course is the ideal source: EcmaScript for JavaScript and WHATWG for browser environment.

Debugging tools#

It’s essential to know your favorite browser’s Developer Tools from the inside out. My favorite browser is Chrome. So when in Chrome, you should know:

what the $0 means when you type it in a console
how to work with conditional breakpoints
how to pause before an exception
how to skip part of the code or step out of current function
how to find a particular text in the loaded sources etc.
The best resource on Chrome Dev Tools is of course Google’s Tools for web developers.

Common design patterns and general architectural concepts#

Sometimes technologies use common design patterns. So it is beneficial to know them. For example, Webpack relies on patterns of async JS execution implemented by the async library. All frameworks and libraries before ES modules used the UMD packaging format for distribution. As you explore more and more frameworks and libraries, you’ll begin to recognize common patterns and it will help you move through the code faster.

Concepts relevant to a particular technology#

It also helps if you know the concepts used by a framework or library. For example, before reverse-engineering a modern framework you should know what a component is. Sometimes you can pick up these concepts from the documentation, sometimes from an in-depth article or a design document. Read everything you can find before you start exploring the code. Read about new concepts as you discover them in code. I suggest reading material with concrete implementation details, not the significantly simplified variant for general audience. Conference talks are usually not the best way to learn concrete details. Design docs on Github and advanced articles are much more helpful.

Guidelines for exploring sources

Read a follow-up article to see how I used the guidelines to go through React’s sources. I suggest you note down the bits of knowledge that you discover as you move along. Later, you’ll be able to piece it all together and see the bigger picture.

Identify the part of a technology to focus on#

The most common question I get asked is where to where to start, where to put the debugger statement in a code base. Well, you can figure that out from your goal. Before you start reverse-engineering, you should always have an idea of what part of a technology you want to understand. For example, when reverse-engineering Angular or React, I first wanted to understand change detection. That was the part of the framework I needed to focus on. Due to my knowledge of modern change detection process, I knew that change detection is about synchronizing changes from a component instance to DOM nodes. So I needed to find where these frameworks store references to the created DOM nodes. That was the goal.

Think like a scientist#

I believe that the scientific method, which involves observation, formulating hypotheses (assumptions) based on such observations and experimental testing is the most effective method of knowledge acquisition.It is also the model I use when reverse-engineering. Here are the basic steps:

Make an observation and form a hypothesis.
Make a prediction based on the hypothesis.
Test the prediction.
Once you’ve got the results, use them to make new hypotheses and predictions. Iterate until you’ve figured out the part you focused on.

Use inference to form a hypothesis and make a prediction. Inference is using observation and background to reach a logical conclusion. For example, if you see someone eating a new food and he or she makes a face, then you infer he does not like it. Or if someone slams a door, you can infer that she is upset about something.

Besides giving you a structured approach to reverse-engineering, I believe assumptions (hypotheses) and validations create anchors in your memory that help you retain what your have learned for longer periods and retrieve them when needed.

Switch between debugging, exploring implementations in the sources and reading comments#

Reverse-engineering is not only about reading sources. In fact, I only spend about 20% of my time checking a particular implementation detail in the source code. I spend about 70% of the time debugging a sample application.That’s why I believe that having a good command of debugging tools is indispensable to make the process of reverse-engineering efficient. The remaining 10% of the time I spend reading comments in the source code or explanations of concepts discovered in the code. Usually, comments are a lot more helpful than anything you can find online, so never disregard them as unimportant.

Use callstack to construct the application flow#

As you move forward with debugging and put breakpoints at different locations in code, routinely check the callstack. Seeing the order of function calls will give you an idea of the application flow. Often it will also help you locate the functions with the relevant functionality in the sources.

Don’t get discouraged by getting your hypothesis wrong#

Get ready to have most of your assumptions proven wrong. That’s a normal and expected process. Sometimes it means you should spend more time on building your background knowledge. But often it just means that the patterns implemented by a framework or library are novel.

It’s not to say that you won’t get frustrated. You will. But, keep your eye on the prize and overcome these frustrations and move forward. By having something wrong, you’ve just learned something new.

Allow yourself some time to think about what you’ve found#

In the book “Mind for numbers” Barbara Oakley talks about two alternating states of mind — focused mode and diffused mode. Both approaches are essential for studying something new. Focused mode involves a direct approach to solving problems using rational, sequential and analytical approaches. Diffused mode allows us to suddenly gain a new insight on a problem we’ve been struggling with and is associated with “big-picture” perspectives. Diffuse mode is what happens when you relax your attention and just let your mind wander. So don’t just sit for hours on end in front of your computer, regularly take short breaks and think about what you discovered. I just walk around my apartment. And I do that when I’m doing any creative work, like writing, not just reverse-engineering.

Start with acquiring sources and setting up a sample application#

To reverse-engineer a framework or library, you’re going to need its sources and a sample application with the technology. Nowadays most frameworks and libraries are hosted on Github, so go ahead and just clone the repository. You always want to explore a particular version, so go to the “releases” tab in the repository and check the latest release. Now once you get the repository cloned, checkout this release by running git checkout tags/[version].

The second step is to set up a sample application. Always try to put together the simplest setup possible. Avoid code bundlers like webpack and CLI tools provided by modern frameworks. In my experience they significantly complicate the debugging experience. An ideal setup is a plain HTML page with the code loaded from CDN, for example, unpkg.com. Just make sure that the version of a framework or library matches the version checked out in the sources.

A word about luck

As with everything in life, luck plays its part. As you repeatedly go through the same part of functionality, you may stumble upon a new comment that will make a certain concept clear; or a function call with a descriptive name that you haven’t noticed before. That happens to me a lot. So I suggest you go multiple times with the debugger through the piece of code that you don’t understand. Or come back to it later. Don’t leave it off completely just because you don’t understand it now. There’s a good chance next time you’re going through it you’ll find something new because you’ve gained some new insights.

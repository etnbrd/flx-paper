
We agree with the reviewers that the incomplete evaluation and implementation of the compiler weaken this paper.
The current implementation still needs assistance from the developer to fix some mistakes and to identify asynchronism.
The mistakes directly results from the early state of development. They are not a scientific limitations of the idea.
Therefore, they are not impacting the actual improvement of productivity a developer would gain with this solution.
The identification of asynchronism, however, is an interesting engineering limitation.
As asynchronism is inherent to the execution engine, and not the language, the detection needs to be implemented at the level of the execution engine, or be carried by the developer who knows the execution engine asynchronism.
We chose to develop a compiler and not a Javascript interpreter.
Hence we need the developer to identify the asynchronism.
We agree with the reviewers, this interaction with the developer would possibly impact the developer productivity if the compiler was to be released as is.
However, with informations from the execution engine about the asynchronism of specific functions, the compiler would not require any interaction with the developer.

We agree with the reviewers that the literature already addressed the parallelization of loops constructs of imperative languages.
However, we strongly insist on the novelty of our approach.
To our knowledge, there is no literature on the parallelization of event-loops.
The event-loop paradigm is used in Javascript execution engines, as well as in many other languages and frameworks.
It is gaining popularity with web servers like nginx, and frameworks like Python Twisted and Ruby EventMachine.
The choice of Javascript is motivated by its popularity in the web development community, and the uniqueness of its implementations.
Usually, event-loops are implemented inside the language, using execution engine concurrence primitives, such as threads.
But to our knowledge, the Javascript execution engines are the only execution engine relying directly on an event-loop to provide concurrency.
This particularity is not limited to Javascript.
Event-loop based execution engines could be implemented for any language supporting higher-order functions, such as Ruby or Python.

These languages allow the development team to incrementally optimize performances.
In the early stages of development, the team releases an MVP and iterates over it to improve features and performances.
However, the monolithic design of these languages eventually limits the performance improvements.
At this point, the team needs to switch to a different programming model.
As we argued in the introduction, this switch is an economical risk.
Our compiler intends to improve the scalability of the initial languages without asking the developer to annotate its code for parallelism.
We argue that languages immediately exposing parallelism to the developers are inherently more difficult to develop with.
Therefore, these languages are not gathering a community and are in rupture with the early stages of development of a web application.
Our solution intends to avoid this rupture in the incremental development, hence avoiding the economical risks this rupture brings.

We motivate in the next paragraphs the novelty of our approach.
A regular loop, as studied in the literature, iterates over the same instructions.
It is generally parallelizable when iterating over an array.
But this parallelization is embedded in the synchronous execution.
This linear, synchronous organization of the execution limits the overall speedup, as stated by the Amdahl's law.

Alternatively, the event-loop is not bounded by any synchronous execution, as it is inside the execution engine.
The iterations are independently executed.
They have different call stacks.
We leverage this particular situation to parallelize each iteration of the event-loop.
Moreover, as the event-loop executes different functions, or callbacks, at each iteration, the different functions are more likely to access distinct memory regions.
This increases the likeliness of independence and parallelization.

Finally, because we focus on streaming web application, the different callbacks executed by the event-loop are organized as a pipeline.
For each input user request, the first callback triggers the execution of the second, which triggers the execution of the third, and so on until the last callback responds to the user.
We propose to parallelize the execution of these callbacks.
Each callback is isolated to form a stage in a parallel pipeline through which flows the stream of input request.
To our knowledge, there is no literature presenting this approach.
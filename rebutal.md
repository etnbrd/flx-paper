
We aggree with the reviewer that the evaluation is still incomplete, and the implementation is still in early stage of development weakening this paper.
Moreover, we aggree with the reviewer that the litterature has already widely addressed the parallelisation of loops as exposed in the imperative programming model.
However, we strongly insist on the novelty of our approach.
To our knowledge, there is no work on the parallelization of event-loops.
Event-loop are used in Javascript execution engines, but as well as in many other languages and frameworks, and is increasingly gaining popularity in the web development community with web servers like nginx, and frameworks like Python Twisted and Ruby Event Machine.
The choice of Javascript is motivated by its popularity in the web development community, and the uniqueness of its implementations.
Indeed, it is, to our knowledge, the only execution engine based on an event-loop, and not the other way around, an event-loop implemented as a framework inside the language.
But this particularity is not limited to Javascript, such execution engine could be implemented for any language supporting higher-order functions, such as Ruby or Python.
Moreover, with the recent introduction of the instruction await in Python, such execution engines could tend to generalize at even greater extents.

A regular loop, as studied in the literature, iterates over the same instructions.
It is generally parallelizable when iterating over an array.
But the parallelization of said loop is bounded by the synchronous execution.
It needs to wait for all the parallel iterations to finish, before continuing the synchronous execution.
The parallelization is also bounded inside a single thread of synchronous execution.

Alternatively, the event-loop is not bounded by any synchronous execution, as it is inside the execution engine itself.
Every iteration is completely independent from another.
They are executed on distinct call stacks.
We leverage this particular situation to parallelize each iteration.
Moreover, as the event-loop executes different functions, or callback, at each iteration, the different functions are more likely to access distinct memory regions.
This increase the likeliness of independence, and hence of parallelization.

Finally, because we focus on streaming real-time web application, the different callbacks executed by the event-loop are regularly solicited, and they are organized as a pipeline.
The first callback pushing the execution of the second, which pushes the execution of the third, and so on.
Each callback is isolated to form a stage in a parallel pipeline through which flows the stream of input request.

This idea is backed by an analysis of the organization of the memory of a web application in the Node.js execution engine.
And we present in the third section the insights required to continue exploiting the idea further.
Indeed, the interdependencies between the different callback is the main limitation of the implementation of this idea.


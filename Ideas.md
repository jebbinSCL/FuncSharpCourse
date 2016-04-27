# Things that could be covered

- WPF application
- Dapper / Nhibernate / Entity Framework
- Type providers  
  - SQL, JSON, XML etc
- Parsing / generation of data exchange files
  - Json.Net, Linq.Xml
- Creating your own type provider?
- MBrace
- Akka.NET
- Reactive / Lazy
  - (Promises/Async) / (Ienumerable/Seq) / Events / Observables
- Libraries with other collections
  - Async seq? Fsharp.data
- Monads / Computational Expressions
- Railroad programming (Find correct name)
- Quotations
- Pattern Matching/ Active patterns
- Pipelining
- Function composition
- Other useful operators
- Pure functions
- immutable data
- Recursion and Tail Recursion
  - http://stackoverflow.com/questions/2867514/how-can-i-implement-a-tail-recursive-list-append/2867646#2867646
- Pure functions, Immutable data and implementing Time Travel
- Hot swapping?
  - http://elm-lang.org/blog/interactive-programming is it possible in F#?
  - https://github.com/gaearon/redux-devtools

- Misunderstandings and clarifications addressing them
  - https://codewords.recurse.com/issues/six/immutability-is-not-enough
    - The author of this article had a misconception that by using functional programming and immutable data structures, ordering wouldn't matter.
      Maybe better to say that he believed that it would allow him to arbitrarily order the execution of functions which are meant to act on the same data.
      When he executed the functions in the wrong order, he got the wrong results. His functions were not commutative :
        See comment : https://news.ycombinator.com/item?id=11395942
        And comment : https://news.ycombinator.com/item?id=11396248
      He also tried applying all the functions on the same input state, returning an diff of their changes, and then merging their actions to give the output state.
      He points out that this has the issue that each function will not see each other's changes until the next frame (Note: go back add info to make it clear he is talking about animation).

      The true problem here is that the execution order matters. This doesn't change no matter what programming paradigm you use. There is always going to be a required ordering that will give the expected result.
      It is only possible for functions to be executed in an arbitrary order if their inputs are truly independent. In that case, we have code that is trivially parallelizable (For example, if we can change from Array.map to Array.Parallel.map without any other code changes, and the only effect is a change in performance).

      So, is there a way for us to encode this required ordering into our code so that it is much more difficult (or impossible?). If we look at the problem above in a slightly different way, we can say that each function implicitly expects their input data to be in a certain state. At the moment, the only place this is represented is the overall output, and the correct ordering of the functions. We want the requirements of each function function to be explicit.

      To do this, we can use the type system. Specifically discriminated unions. ... TODO ...

      .... TODO ....

      Congratulations, you've made a state machine.

      Misconception: Functional programming and immutable data structures will allow you to arbitrarily order functions, and will remove programmer errors to do with incorrect ordering of code.
      Reality: All code has dependencies. If functions not commutative (chaining order doesn't change result) then you can't use a random order and reasonably expect a correct result. Also, commutative functions are rare, so attempting to achieve that would be difficult. However, we can still reduce / remove programmer errors to do with code ordering by using the type system to create a state machine. This allows us to encode our expectations for each transition between states for the program.

    - This leads to a similar misconception, which is, (in shortened form):
        TODO Find better quote about pure functions, immutable data, and trivially parallelizable code https://news.ycombinator.com/item?id=11396173

      Reality: The full statement should be the following: (Improve statement to make sure there are no flaws) Pure functions with independent inputs are trivially parallelizable.
      
    - Another misconception: F# code will remove the need for nulls.
      Reality: F# is dependent on .NET and has been built with the benefit of interfacing with other .net languages and libraries. Because of this, you may need to deal with nulls whenever you deal with non F# code, but there are a number of tools to help make it easier. And the convention is to isolate the interfacing code as much as possible, so null checks are only in certain areas of your code.

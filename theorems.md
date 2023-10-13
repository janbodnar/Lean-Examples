# Theorems 


There are two modes in Lean to prove theorems: the *tactic mode* and the *term mode*.
We can switch into *tactic mode* at any point using the keyword `by`. 
  
The basic tactics either run an inference rule forwards or backwards (for example,  convert hypotheses A and B into A∧B or convert goal A∧B  into two goals A  
and B  with same hypotheses), apply a lemma (~ function application), split up a  
lemma about some inductive type into a case for each constructor, and so on. Basic  tactics may succeed or fail depending upon the context in which they are applied.  More advanced tactics are like little functional programs that run the basic tactics,  perform pattern matching over the terms in the goal and/or assumptions, make choices  based on the success or failure of tactics, and so forth. More advanced tactics deal  with arithmetic and other specific kinds of theories. 

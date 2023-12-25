Ryo Novoa
Net ID anovoa
Student ID 31582858

Setting: 2-D Blocks World. In which an AI system builds structures with children's blocks on a table.
    An example of a block structure (call it a "springboard"):
                _____ _________________
               |     |                 | (CUBE A) (CUBE B)
               |  D  |        H        | (CUBE C) (CUBE D)
               |_____|_________________| (BAR G)  (BAR H)
               |     |     |     |       (VERTICAL G)
               |     |     |  C  |       (HORIZONTAL H)
               |     |     |_____|       (ON G *TABLE*)
               |     |     |     |       (ON A *TABLE*)
               |  G  |     |  B  |       (NEAR A G); "A is one unit to the right
               |     |     |_____|        of G; valid only for blocks on *TABLE*
               |     |     |     |       (ON B A)
               |     |     |  A  |       (ON C B),
    *TABLE*____|_____|_____|_____|_______(ON H C); This is "symmetrical"-ON

Assumptions: We have exactly 6 unit "cubes" -- {A, B, C, D, E, F} -- and 2 "bars" -- {G, H} -- of length 3. 

I implemented the following functions in this assignment:
store-fact. Takes a fact (e.g. (ON B C)) and the *FACTS-HT* hashtable as its parameters. Loads fact into *FACTS-HT* as its own fact with the value T for "true," and also as a list item according to its keyword (e.g. ON, NEAR, etc.).

remove-fact. Takes a fact (e.g. (ON B C)) and the *FACTS-HT* hashtable as its parameters. Removes the fact from *FACTS-HT* hashtable by setting it to nil, then removes the fact as a list item from the corresponding list according to its keyword.

store-facts. Takes a list of facts and the *FACTS-HT* hashtable as its parameters. Loads each fact from list into *FACTS-HT* using a mapping function that calls store-fact.


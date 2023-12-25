Ryo Novoa
Net ID anovoa
Student ID 31582858

Note: All tasks have been completed to specifications. Code provided by student tzhou was used and, in some cases, modified in order to meet the project requirements.

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

Here, we are asked to implement "spatial question answering" for Blocks World structures. We do this by coding an interactive "Blocks World agent" that begins by asking the user "What would you like me to build?" and, in response to the user's specification, tries to produce a construction which satisfies the given specification. Then, the agent will repeatedly prompt the user with the following query: "Do you have a spatial question?"
Unless the user says NO to halt the process, it tries to answer the user's question. Questions are posed declaratively, e.g., (SUPPORTS C H) means "Does block C directly support bar H?"
Beside SUPPORTS questions, the user can also ask ON, HORIZONTAL, VERTICAL, and HELPS-SUPPORT questions.

Assumptions: We have exactly 6 unit "cubes" -- {A, B, C, D, E, F} -- and 2 "bars" -- {G, H} -- of length 3.

Task 1: Write a function (defun bw-agent () ...) which will prompt the user with the question "What would you like me to build?"
The user then supplies a goal structure specification, i.e., a list in which all properties should be true. The goal specification will have variables [names beginning with a question mark, e.g., (ON ?X *TABLE*)] for all blocks, and the only individual constant in the goal description will be *TABLE*. 
Assuming that the construction succeeds without prompting an error message (in that process also filling in the *FACTS-HT* and *SUPPORTS* hash tables, and the *BLOCKS-AT-LEVEL-I* array), bw-agent should then print out, on separate lines,
- the list of block bindings used,
- the construction steps, and
- the instantiated specification (i.e., the user's goal specification with variables replaced by constants).
bw-agent then asks, "Do you have a spatial question?" The user then either prints NO, terminating the interaction, or supplies a question in one of the forms (where X, Y are constants): (SUPPORTS X Y), (ON X Y), (HORIZONTAL X), (VERTICAL X), OR (HELPS-SUPPORT X Y). 
If the queried property is found to be true, bw-agent should respond with, e.g., "Yes, block C SUPPORTS block H" or "Yes, block H is HORIZONTAL."
After the query has been answered, bw-agent goes back to asking "Do you have a spatial question?" until the user answers NO.


Below are transcripts of demonstrations for task 1.

EXAMPLE 1
What would you like me to build? ((CUBE ?X1) (CUBE ?X2) (CUBE ?X3) (CUBE ?X4) (BAR ?Y1)  (BAR ?Y2) (VERTICAL ?Y1) (HORIZONTAL ?Y2) (ON ?Y1 *TABLE*) (ON ?X1 *TABLE*) (NEAR ?X1 ?Y1) (ON ?X2 ?X1) (ON ?X3 ?X2) (ON ?X4 ?Y1) (ON ?Y2 ?X3))

You entered: ((CUBE ?X1) (CUBE ?X2) (CUBE ?X3) (CUBE ?X4) (BAR ?Y1)  (BAR ?Y2) (VERTICAL ?Y1) (HORIZONTAL ?Y2) (ON ?Y1 *TABLE*) (ON ?X1 *TABLE*) (NEAR ?X1 ?Y1) (ON ?X2 ?X1) (ON ?X3 ?X2) (ON ?X4 ?Y1) (ON ?Y2 ?X3))

Confirm goal structure description (Y/N): Y

User input confirmed.

Goal structure description: ((CUBE ?X1) (CUBE ?X2) (CUBE ?X3) (CUBE ?X4) (BAR ?Y1)  (BAR ?Y2) (VERTICAL ?Y1) (HORIZONTAL ?Y2) (ON ?Y1 *TABLE*) (ON ?X1 *TABLE*) (NEAR ?X1 ?Y1) (ON ?X2 ?X1) (ON ?X3 ?X2) (ON ?X4 ?Y1) (ON ?Y2 ?X3))
Bindings: ((?Y1 G) (?Y2 H) (?X1 A) (?X2 B) (?X3 C) (?X4 D))
Steps: ((TURN-VERTICAL G) (PUT-ON G *TABLE*) (PUT-NEAR A G) (PUT-ON B A)
        (PUT-ON C B) (PUT-ON D G) (PUT-ON H C))
Particularized description: ((CUBE A) (CUBE B) (CUBE C) (CUBE D) (BAR G)
                             (BAR H) (VERTICAL G) (HORIZONTAL H) (ON G *TABLE*)
                             (ON A *TABLE*) (NEAR A G) (ON B A) (ON C B)
                             (ON D G) (ON H C))

Do you have a spatial question? (HELPS-SUPPORT A C)
Yes, block A HELPS SUPPORT block C.
Do you have a spatial question? (SUPPORTS *TABLE* G)
Yes, *TABLE* supports block G.
Do you have a spatial question? (HORIZONTAL H)
Yes, block H is HORIZONTAL.
Do you have a spatial question? (VERTICAL A)
No, block A is not VERTICAL.
Do you have a spatial question? (ON H C)
Yes, block H is ON block C.
Do you have a spatial question? NO
Terminating the loop.

EXAMPLE 2
What would you like me to build? sldkfjslkdj                   

You entered: sldkfjslkdj

Confirm goal structure description (Y/N): N                             

What would you like me to build? ((ON ?X *table*) (NEAR ?Y ?X))

You entered: ((ON ?X *table*) (NEAR ?Y ?X))

Confirm goal structure description (Y/N): Y

User input confirmed.

Goal structure description: ((ON ?X *table*) (NEAR ?Y ?X))
Bindings: ((?X A) (?Y B))
Steps: ((PUT-ON A *TABLE*) (PUT-NEAR B A))
Particularized description: ((ON A *TABLE*) (NEAR B A))

Do you have a spatial question? (ON A *TABLE*)
Yes, block A is ON block *TABLE*.
Do you have a spatial question? NO
Terminating the loop.

Task 2: Write the function (defun support-chain (X Y) ...). X, Y are expected to be distinct names of blocks, or X could be *TABLE*. It's assumed that the global *SUPPORTS* hash table has been filled when this is called. The output should be a support chain -- a sequence of block names (or possibly an initial *TABLE*) starting with X and ending with Y, where each block (partially or fully) supports the next one in the chain.
   ex.
     HHHHHH    SOME SUPPORT-CHAINS:
     GG  CC    (SUPPORT-CHAIN *TABLE* H) = (*TABLE* G H), (*TABLE* A B C H)
     GG  BB    (SUPPORT-CHAIN A C) = (A B C)
     GG  AA    (SUPPORT-CHAIN G H) = (G H)
   TTTTTTTTTT

    Example 2
    `````````
     HHHHHH    SOME SUPPORT-CHAINS:
     DD  EEFF  (SUPPORT-CHAIN *TABLE* H) = (*TABLE* A C D H), (*TABLE* B G E H)
     CCGGGGGG  (SUPPORT-CHAIN B F) = (B G F)
     AA  BB    (SUPPORT-CHAIN B D) = NIL [this leads to a "No" for HELPS-SUPPORT]
  TTTTTTTTTTTT

Below are transcripts of demonstrations for task 2.

EXAMPLE 1
What would you like me to build? ((CUBE ?X1) (CUBE ?X2) (CUBE ?X3) (CUBE ?X4) (BAR ?Y1)  (BAR ?Y2) (VERTICAL ?Y1) (HORIZONTAL ?Y2) (ON ?Y1 *TABLE*) (ON ?X1 *TABLE*) (NEAR ?X1 ?Y1) (ON ?X2 ?X1) (ON ?X3 ?X2) (ON ?X4 ?Y1) (ON ?Y2 ?X3))

You entered: ((CUBE ?X1) (CUBE ?X2) (CUBE ?X3) (CUBE ?X4) (BAR ?Y1)  (BAR ?Y2) (VERTICAL ?Y1) (HORIZONTAL ?Y2) (ON ?Y1 *TABLE*) (ON ?X1 *TABLE*) (NEAR ?X1 ?Y1) (ON ?X2 ?X1) (ON ?X3 ?X2) (ON ?X4 ?Y1) (ON ?Y2 ?X3))

Confirm goal structure description (Y/N): Y

User input confirmed.

Goal structure description: ((CUBE ?X1) (CUBE ?X2) (CUBE ?X3) (CUBE ?X4) (BAR ?Y1)  (BAR ?Y2) (VERTICAL ?Y1) (HORIZONTAL ?Y2) (ON ?Y1 *TABLE*) (ON ?X1 *TABLE*) (NEAR ?X1 ?Y1) (ON ?X2 ?X1) (ON ?X3 ?X2) (ON ?X4 ?Y1) (ON ?Y2 ?X3))
Bindings: ((?Y1 G) (?Y2 H) (?X1 A) (?X2 B) (?X3 C) (?X4 D))
Steps: ((TURN-VERTICAL G) (PUT-ON G *TABLE*) (PUT-NEAR A G) (PUT-ON B A)
        (PUT-ON C B) (PUT-ON D G) (PUT-ON H C))
Particularized description: ((CUBE A) (CUBE B) (CUBE C) (CUBE D) (BAR G)
                             (BAR H) (VERTICAL G) (HORIZONTAL H) (ON G *TABLE*)
                             (ON A *TABLE*) (NEAR A G) (ON B A) (ON C B)
                             (ON D G) (ON H C))

Do you have a spatial question? NO
Terminating the loop.

TEST CASES FOR SUPPORT-CHAIN FUNCTION
Support chain for A to C
Expected: (A B C)
Generated: (A B C)

Support chain for *TABLE* to H
Expected: (*TABLE* A B C H)
Generated: (*TABLE* A B C H)

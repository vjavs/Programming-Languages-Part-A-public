Contributed by Charilaos Skiadas.

**Notes on the material:**

- Three key ways of creating "compound" types, make sure to understand them.
- Tuples are just records.
- Datatype bindings are used to create one-of types: extremely powerful and flexible (for example can build trees).
- A datatype binding creates functions (or constants) for each of the constructors.
- Note the keyword ```datatype``` is not used when writing values of that type.
- Type synonyms offer alternative names for complicated types (e.g., type date = int*int*int).
- Pattern matching rocks! Enough said.
- Using ```val p = e``` where ```p``` is a pattern (like ```(x,y,z)``` or ```{ x= x, y = y }``` can be a very effective way to get values out of a tuple or record.
- Lists and options are really just datatypes (worth understanding how that works). Practice by creating your own "list" type.
- Nested patterns can be very expressive.
- Pay attention to the use of the wildcard ```_``` for parts of a pattern we don't care to name.
- Raising/handling exceptions is similar to datatypes and pattern-matching.
- Appending to the end of a list is relatively expensive.

**Notes on the assignment:**

- It might be a good idea, while they are fresh in your mind, to rewrite the first homework's problems using pattern-matching.
- Read the instructions very, very carefully.
- Make sure to download the provided sml file, it contains code you need.
- The provided code file has separate locations for where to put your answers to question 1's parts and where to put your answers to question 2's parts. Make sure to note and respect those locations, it will make it easier for you and for your peers if the type definitions needed for question 2 are close to the functions of question 2.
- Use the provided function ```same_string``` instead of simple equality to compare strings. It will help make the reported types of the functions closer to the "expected" types.
- Especially for the later parts of the assignment, study the types defined in the provided file. Make sure you understand them.
- Some of the questions require that you use specific functions/forms. Don't miss those instructions!
- You do not need to specify types for your function arguments or return values. SML will infer them for you.
- Your function types might appear to differ from those requested, and that may be okay (see "type inference" and "type synonyms" videos).
- Do NOT, under any circumstances, use ```hd```, ```tl```, ```null```, ```valOf```, ```isSome```, ```#1```, ```#2```, ```#3```, .... Use pattern matching instead.
- Often avoid nested case expressions in favor of "nested patterns" (see triples example in "nested patterns" video).
- Use tuple patterns to match against multiple things.
- Use wildcards for matches you don't care to identify.
- List order in the answer does not matter unless specifically stated in the problem.
- Read very carefully how the "preliminary score" for the card game is computed. It is easy to miss.
- Integer division rounds down. So 1 div 2 = 0, ~1 div 2 = ~1.
- "Hands" are allowed to have the same card more than once.
- ```all_same_color``` for an empty list and a list with one card is "vacuously" true: there are not cards with different colors.
- ```sum_cards``` for an empty list should be 0.
- ```officiate``` and ```officiate_challenge``` should not end the game simply because the card list is empty. There might still be ```Discard``` moves left for example. The game ends on a ```Draw``` move from an empty card list.
- The ```officiate``` helper method is a good place to use nested patterns.
- The wording of the ```careful_player``` challenge problem requirements is a bit tricky. There are 4 requirements:
1. If held-card value is more than 10 points behind the goal, you must draw. (It is guaranteed to be safe, make sure you understand why.)
2. If your held-card value is 10 points or less behind the goal, then you have a choice on your algorithm as to whether you should draw or not. But you must ensure that you do not exceed the goal. So you can either say "No I won't draw" or you can say "let me sneak a peak into the next card, and if it is safe to draw it then I will (which is sort of cheating, but valid from the assignment's point of view)" or something in-between. These are all valid approaches. But you must make sure that the held-card value never exceeds the goal. And you also must make sure to follow 4 below.
3. If your score is 0, you must not make any more moves.
4. If your score is not 0, but you can reach a zero by discarding a card and then drawing a card, this must be done. In order to achieve this, your algorithm will need to look ahead and see what the next card is.
5. Your code may choose to discard cards if it wants to, or do any other things it wants almost, as long as it satisfies conditions 1-4 above. So say your held-card value is 6 points behind the goal and the next card would be a 2. Then you have some leeway in what to do. You can choose to discard a card, you can choose to just stop, you can choose draw pretending you have looked ahead at that 2. (If the next card was a 5-6 though, note case 4 above.)
- Watch out with nested case expressions -- you may need parentheses. This does not arise too much because often nested patterns are a better choice, but nested case expressions do still make sense in some situations. The problem arises in something like this:
```
fun silly xs =
    case xs of
	 [] => ""
       | x :: [] => case helper_function x of
			NONE => ""
		      | SOME i => ""
       | x :: xs' => "" (* Error related to this line. It is a branch of the *inner* case *)
```
So you have an inner case expression, but then there is an extra clause at the end that corresponds to the outer case expression. SML reads that as:
```
fun silly xs =
    case xs of
	 [] => ""
       | x :: [] => case helper_function x of
			NONE => ""
		      | SOME i => ""
		      | x :: xs' => "" (* Error related to this line. It is a branch of the *inner* case *)
```
which is wrong (it's not what you intended and it doesn't type-check). You need to wrap the inner case expression in parentheses:
```
fun silly xs =
    case xs of
	 [] => ""
       | x :: [] => (case helper_function x of
			NONE => ""
		      | SOME i => "")
       | x :: xs' => ""
```

# Criss Cross Puzzle Generator

## Assignment

This task ia about writting an app that should generate criss-cross puzzle from a list of unuque words.

Generated criss-cross should be presented graphically and textually.

For example given a list of words:

- Мул, Сом, Енот, Крот, Лемур,

- Койот, Панда, Тукан, Мустанг, Гиппопотам

The criss cross puzzle for the given list of words might be looked as followed:

    М У Л   К
    У   Е Н О Т
    С О М   Й       П
    Т   У   О       А
    А   Р   Т У К А Н
    Н           Р   Д
    Г И П П О П О Т А М
                Т

    B     S A C K F U L
    A     E       A   A
    G R A N D S O N   M
          D   Q   G A B
    B A R O Q U E   B
    E     F   A     L
    W     F O W L   E
    A   E   A   A
    R E D I R E C T
    E   I   S   E   P
    D A F T   O D O R S
        Y           O

The correctly generated puzzle means that there are no words that are not in the puzzle.

## The Solution

Application should make two steps to resolve the task:
    
- s.1 Generate all complete variants of criss-cross.

*c.1 Complete variant assumes usage of all the words from the list*.

- s.2 Select the most appropriate variant.

*c.2 The best variant should take minimal footprint*.

### s.1 The followed actions generates all complete variants from the list:

- a.0 Take a word from the list and put it on a field horizontally. It creates a new _variant_ of a criss-cross. 
  
- a.1 Take every placed word from a unsolved variant as current _working_ word.

- a.2 Find all the words with intersections with the working word from the one out of the unplaced words list. If there is no intersection with the working word, go to a.1.</br>Check every selected intersection. Below there is an example of the intersections of two words. Vertically the working word БАРМАГЛОД is placed and all intercsectons with selected word АБРАКАДАБРА marked by \* and listed under the scheme.  

            0 1 2 3 4 5 6 7 8 9 0
            А Б Р А К А Д А Б Р А
        0 Б   *             *
        1 А *     *   *   *     *
        2 Р     *             *
        3 М
        4 А *     *   *   *     *
        5 Г
        6 Л
        7 О
        8 Д             *
        (0, 1) (0, 8), (1, 0) (1, 3) (1, 5) (1, 7) (1, 10), (2, 2) (2, 9), (4, 0) (4, 3) (4, 5) (4, 7) (4, 10), (8, 6)

- a.3 For an every selected word check if it possible to set the word on an intersected position. Tests should be provided according to followint rules:

  - *r.1 there shouldn't be an overlapping with previous intersections linked to the same word*

        X Z X      (bad)         X Z X      (good)       X X Z        (good)
        Y Z Y Y                    Z                         Z Y Y Y
          Z                      Y Z Y Y                     Z

  - *r.2 there shouldn't be any touch with previously placed words*

        V X Z      (bad)         V X Z      (good)
        V   Z                    V   Z
        V Y Z Y Y                V   Y Y Y Y

All testing word should successfully passed followed conditions:

  - c.1 There are should be no letters under selected word from placed words except the working one at the intersection point

  - c.2 There are no touch with the placed words on the begin and the end of the selected word in the directions of the selected word

  - c.3 There are no touch with placed words from sides opposed the selected word direction excepth the working word and only on the tested intersection point

- a.4 If testing succeded the word is placed on a field and new iteration starts from a.2 for every placed word.

- a.5 If the free words list is empty, the variant is finished.

- a.6 If there is no intersections between placed and the words from free words list it is a deadended incomplete variant.

In general the generated variant looks like the followed expression:

    CC(n) = CC(n - 1) + free_words[]

Iterations continue until free_words[] is empty or there is no acceptable intersections between placed words on CC and free_words. There will be a three of variants started from the initial words list.

### s.2 Selection of the best variant

It's clear that the best variant has the smallest footprint used by words.

### Programm objects

The programm's objects taxonomy is short:

- o.1 **CrissCross** - the criss-cross object itself. It generates the puzzle from a list of given words. It gets a list of Variant and choose from them the best one.

  - o.2 **Variant** - the one single state of single puzzle generation step. It holds the current puzzle Field, the list of placed words and the list of words to be placed. Every next steps generates a tree of new Variants or the salvation is stops either by success or by error if it's impossible to place any word is left in the free_words list.

    - o.3 **Field** - field under the criss-cross. It defines the range of cells used by current criss-cross.</br> The Field could grow in any direction. If it grows up and left, the origins of the placed word should be moved onto a new position. For example: if the field grows up for 2 rows, the y positon of every words increased on 2 also.
  
    - o.4 **Word** - the single placed word with intersections list, origin position on the field, direction of the word ( horizontal or vertical ).
  
      - o.5 **Intersection** -- the single intersection which linked two words with positions of intersection for every word.


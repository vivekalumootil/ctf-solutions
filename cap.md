# Buckeye CTF: rev/cap





## Solution

Upon downloading the file cap.c from the challenge, we notice that it is a C file. However, all of the usual keywords and values have been substituted with slang via macros.

Our goal is to determine the meaning of each of the macros to be able to run this encrypted program. The following list explains how we find each of the macros.

```
1. fr is ;. Notice how most lines end in fr.
2. finna is { and tho is }. See the indentation of the program's lines.
3. lookin is =. There are various instances of lookin between a variable and a value. 
4. poppin is for, ongod is (, af is ). Some lines are obvious for loops.
5. lit is i and cap is 0. For loops usually start with int i = 0. 
6. tryna is if. The structure of statements in kinda function suggests this. 
7. be is ==. Observe if statements.
8. playin is ++. See for loops.
9. downbad is --. See "val downbad fr". 
10. respectfully is do, boutta is while. See the do-while loop in kinda.
11. lowkey is <, highey is >. See loops and if statements. 
12. val is *. We reason this based on how val appears before variables and is also an operator.
13. clean is char. val is a C-string, so clean should be char.
14. like is or. This makes sense given code like "i == 5 like i == 6". 
15. lackin is -. It is clearly and operator and the word itself sugests subtraction.
16. wack is /. Division is the only operator left. 
17. bussin is 1. We make an educated guess based on the fact that *(x+1) is missing.
18. drip is case and sus is switch, approximately. 
19. no is !. Self explanatory.
20. yeet is [, rn = ]. See relevant line in kinda.
21. sheeesh is printf. It is clearly printing out strings in the two lines it appears. 
22. bruh is ,. Notice how it appears between inputs of functions. 
23. deadass is return. Notice the 'deadass 0;' at the end of the main function. 

```
Once we run the program, it prints the flag, buckeye{7h47_5h17_mf_bu551n_n0_c4p}. Fitting, isn't it?

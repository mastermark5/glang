# glang
Created by Mark Aldrich. The g language is similar to Assembly, but includes more modern aspects, such as variables.

### Syntax
There are 3 types of symbols: operators, operands, and separators. Operators are built in (see below). Operands can be either variables, registers, or literals. Separators separate things passed to an operator. Example: ```@jumpz $v :: 3```

Operators start with "@".
Variables start with "$".
Registers start with "#".
Comments start with ";" and must take up a whole line. Example: ```; This is a comment```
Separators are "::".
The current line is "~".
The line after the current line is "^" (used for functions).

Compound expressions are NOT allowed. Example: ```@print $v + $f``` is invalid.

These are the operators: ("value" refers to a literal, variable, or register.)

	- @init (variable)
		Tells the interpreter that you will be using this variable in the future.
		While you do not need to @init a variable, you should @init variables you
		plan to use heavily in the future to make accesses to them more efficient.
		Example:
```
; Beginning of program
@init $var
; Tell the compiler you will be using this variable heavily later
...
@+ $var :: 1 :: $var
```

	- @store (variable) :: (value)
		Stores a value in a variable.

	- @print (value)
		Prints a value to the console.

	- @printc (value)
		Prints a value to the console as a Unicode character.

	- @prints (string)
		Prints a string literal to the console. NOTE: @prints does not follow the normal syntax. 
		It prints all remaining characters on the line. Example: @prints Hello world! will print "Hello world!"
		and @prints $v will print "$v".

	- @printl (string)
		Prints a string literal and a new line to the console. 
		NOTE: follows the same irregular syntax as @prints.

	- @newline
		Prints a new line to the console.

	- @+ (value) :: (value) :: (variable)
		Adds the two values and store the result in variable.

	- @- (value1) :: (value2) :: (variable)
		Subtracts value2 from value1 and stores the result in variable.

	- @* (value) :: (value) :: (variable)
		Multiplies the two values and store the result in variable.

	- @/ (value1) :: (value2) :: (variable)
		Divides value1 by value2 and stores the result in variable.

	- @jump (value)
		Jumps to value as a line number.

	- @jumpz (value1) :: (value2)
		Jumps to value2 if value1 is 0.

	- @jumpnz (value1) :: (value2)
		Jumps to value2 if value1 is not 0.

	- @jumpp (value1) :: (value2)
		Jumps to value2 if value1 is positive.

	- @jumpn (value1) :: (value2)
		Jumps to value2 if value1 is negative.

	- @equal (value1) :: (value2) :: (variable)
		If value1 is equal to value2, store 1 in variable, else, store 0 in variable.

	- @push (value)
		Push value onto the stack.

	- @pop (variable)
		Pop the value on the stack into variable.

	- @func and @endfunc
		Used to declare a function. To the interpreter, all this means is to skip all code between this tag and the matching @endfunc tag the first time it encounters it. Example of how a function should be declared:
			@store $func_square :: ^
			; Takes 1 number
			; Returns squared number
			@func
				; Pop in parameter and return location (Return location it always popped first, the rest of the parameters are then popped in reverse order)
				@pop $func_square_ret
				@pop $func_square_num
				
				; Multiply the number by itself and store the result in the same variable
				@* $func_square_num :: $func_square_num :: $func_square_num
				
				; Push the result
				@push $func_square_num
				
				; Return from the function
				@jump $func_square_ret
			@endfunc
		And to call it:
			; Push number
			@push 5
			
			; Call function
			@call $func_square
			
			; Pop result into a variable
			@pop $squared
			
			; Print result (should be 25)
			@print $squared
### Hello world!
A "Hello world!" program can be written in g farily easily.
```@printl Hello world!```

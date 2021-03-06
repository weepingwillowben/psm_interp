COMMANDS:
	INTERPRETING .psm:
		PATTERN: python3.4 [interpreter/compiler] [filename.psm]

		INTERPRETERS/COMPILERS:
			GUI_interp.py
				Brings up a GUI that displays errors and warnings
				on the command line.
			text_interp.py
				A text based debugger that has much of the same
				functionality as the GUI
			make_x86.py
				A compiler that turns the "filename.psm" file into a
				"filename.s" which can be compiled with lib.s on gcc to
				produces a native executable that runs on the hardware.

	COMPILING/RUNNING x86:
		GENERATE lib.s:
			gcc -S lib.c
		ASSEBLE filename.s:
			gcc -o filename.out filename.s lib.s
		RUN filename.out:
			./filename.out

FILES:

	GUI_interp.py: contains the tkinter interface
	text_interp.py: contains the text interface
	make_x86.py: contains code that uses parsed information to build the x86 file
	instructions.py: contains execution procedures and x86 information for each psm instruction
	platform_check.py: gives information about which platform you have to the GUI and the x86 maker for platform specific issues
	psm_parser.py: parses a file into instructions and label data and gives errors for incorrect syntax
	run_core.py: a framework for runtime errors and warnings used by the instructions
	state_holders.py: classes that keep track of data and debugging instruction flow. They are they are inhereted by interface specific classes.
	lib.c: a file that allows for native execution of the x86 generated by make_x86.py

LANGUAGE:

	Design due to [Jim Fix](http://people.reed.edu/~jimfix/).

	Registers: rAX, rBX, rCX, rDX, rSI, rDI, rBP, rSP

	These are all 64 bit registers, which means that any overflow will be truncated.

	In the below rS means source and rD means destination
	rS can be a register or a constant, rD can only be a register
	constants which can fit into 32 bits except for register assignment which
	allows for 64 bit constants.

	Instructions:
		All instructions must have exactly 4 spaces or a tab
		behind it.

		push rS         "push"
		rD = pop        "pop"

		out rS          "output"
		rD = in         "input"
		rD = rand       "generate random number"

		rD = rS         "assignment"
		rD = -rD        "negate"
		rD = ~rD        "invert"
		rD += rS        "add accumulate"
		rD -= rS        "subtract accumulate"
		rD &= rS        "and ..."
		rD |= rS        "or ..."
		rD ^= rS        "xor ..."
		rD ++           "increment"
		rD --           "decrement"
		rD *= rS		"integer multiply"
		rD /= rS		"integer divide"
		rD %= rS        "integer modulus"

		c has to a constant less than 64
		rD >>= c		"shift arthmatic right"
		rD <<= c		"shift arthmatic left"

		mem[rD] = rS   "register-to-memory store"
		rD = mem[rD]   "memory-to-register load"

		mem[rD+offset] = rS "register-to-memory store with offset"
		rD = mem[rD+offset] "memory-to-register load with offset"

		goto LABEL          "(unconditional) jump"
		if rD op rS goto LABEL "conditional jump"

		op is one of <, >, <=, >=, ==, !=

		call LABEL   "subroutine call"
		return       "return from subroutine"

	Label creation:
		Labels cannot have any spacing before them.

		LABEL:			"create label"

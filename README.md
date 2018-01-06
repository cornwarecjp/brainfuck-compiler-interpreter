Brainfuck-interpreter-compiler
==============================

An optimizing Brainfuck compiler/interpreter.

Usage
=====

	bf.py program.bf
	bf.py --debug program.bf
	bf.py --compile program.bf program.c

Without options: run program.bf, interpreted

--debug: run program.bf, interpreted; after program termination,
print a memory dump.

--compile: compile program.bf to C-code; save this in program.c.

Note that, in interpreted mode, the current implementation reads all input in
one go, before the start of the program; this does not enable any kind of
interactivity. A small change in the code might change this behavior.

Memory elements are one-byte (8-bit) values; their values wrap when
incrementing/decrementing. All memory elements have initial value zero.
The memory pointer is wrapping: when moving past one end of the memory, it ends
up at the opposite end of the memory.

Limits
======

bf.py observes limits on the following:
* program size
* memory size
* number of instructions executed before program termination

These limits can all be changed at will by editing bf.py.

Program size is the size of the .bf program file, including any non-Brainfuck
characters.

Memory size is the number of elements in the memory made available to the
Brainfuck program.

The number of instructions is limited to prevent infinite loops in use cases
where those are unwanted. The limitation is placed on the number of *optimized*
instructions, which may be lower than the number of instructions in the
original Brainfuck code

Optimizations
=============

The following optimizations are applied:

* Consecutive increments or decrements of memory values or of the memory
  pointer are merged together into a single addition/subtraction
* Target positions of jump instructions are pre-computed, so that, on execution,
  there is no need to walk through the code to find the proper position to
  jump to.
* The very common pattern in Brainfuck, where one memory location is decremented
  in a loop until it is zero, and zero or more other memory locations are
  incremented/decremented in the same loop, is merged into a single instruction.
  This optimizes, for instance, common implementations of set-to-zero,
  move-value, copy-value, add, subtract and multiply-with-constant.



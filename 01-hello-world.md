# 01 - Hello, World!

1. Edit hello.cpp and write a `main` that prints Hello World.
2. Compile and run it 

# Source Code

File: hello.cpp
```
#include <iostream>

int main() {
  std::cout << "Hello, world!\n";
  return 0;
}
```

## Notes

- `iostream` is part of the C++ standard library, and gives you (amongst other things) access to input, output and error streams
- Eg, `std::cin`, `std::cout`, `std::cerr`
- `<<` is the stream operator.  More on that later. 
  - `<<` can be chained.  Equivalent code: `std::cout << "Hello" << ", world!" << std::endl;`


# Compile, link and run

```
# Compile and link in one step:
clang++ -std=c++23 hello.cpp -o hello

# Run:
./hello

#=> Hello, World!
```

A few notes on what's happening:

- clang++ is the C++ driver (vs clang for C). It links the C++ standard library automatically.
- -std=c++23 pins the language standard. You could use c++17 or c++20 too — c++23 is the current stable modern target.
- The single command above both compiles and links. Later we'll separate those steps to understand what each produces.
- return 0 from main is technically optional in C++ (the compiler inserts it), but explicit is fine for clarity.

Want to try separating compile and link into two steps next, to see the object file in between?

# Extra: Compile and link in two steps

```
# Step 1 — Compile to an object file (no linking yet):
clang++ -std=c++23 -c hello.cpp -o hello.o

# Step 2 — Link the object file into an executable:
clang++ hello.o -o hello

# Run:
./hello
```

What each step produces and why it matters:

- -c tells the compiler to stop after compilation — don't link. You get hello.o, a binary object file containing machine code but with unresolved references (e.g., std::cout isn't wired up yet).
- The second clang++ invocation sees .o files and knows to invoke the linker. It pulls in the C++ standard library and resolves all those references, producing the final executable.

# Extra: Peeking inside the binaries

You can inspect what's inside the object file with: `nm hello.o`

That shows the symbol table — you'll see your main defined, and std::cout-related symbols marked as undefined (U), waiting for the linker to resolve them.

After linking, run `nm hello` and compare — those U symbols are now resolved.

This two-step model is how real builds work at scale: compile each .cpp independently (parallelizable), then link everything together once at the end.

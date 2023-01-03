# Alcatraz
Alcatraz is a x64 binary obfuscator that is able to obfuscate various different pe files including: 
- .exe
- .dll
- .sys

# Overview
- [Alcatraz](#alcatraz)
- [Features](#features)
    + [Obfuscation of immediate moves](#obfuscation-of-immediate-moves)
    + [Control flow flattening](#control-flow-flattening)
    + [ADD mutation](#add-mutation)
    + [Anti disassembly](#anti-disassembly)
    + [Import obfuscation](#import-obfuscation)
    + [Opaque predicates](#opaque-predicates)
    + [Mixed boolean arithmetic](#mixed-boolean-arithmetic)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>


# Features
In the following showcase all features (besides the one being showcased) are disabled.
### Obfuscation of immediate moves
If an immediate value is moved into a register, we obfuscate it by applying multiple bitwise operations. Let's take a look at the popular function `_security_init_cookie`.  
Before:
![imgbefore](images/const_before.PNG)
After:
![imgafter](images/const_after.PNG)
### Control flow flattening
By removing the tidy program structure the compiler generated and putting our code into new generated blocks, we increase the complexity of the program. Lets take this simple function `main` as example:  
![imgmain](images/flatten_function.PNG)  
If we throw this into IDA 7.6 the decompiler will optimize it:  
![imgmainnoobf](images/flatten_func_noobf.PNG)  
Now let's flatten its control flow and let IDA analyze it again:  
![imgmainobf](images/flatten_func_obf.PNG)  
As you can see, the complexity increased by a lot even though I only show a small portion of the generated code. If you want to know what the cfg looks like take a look:
![imgmaincfg](images/flatten_func_cfg.PNG)  
### ADD mutation
If a register (eg. RAX) is added to another register (eg. RCX) we will mutate the instruction. This means that the syntax changes but not the semantic.
The instruction `ADD RCX, RAX` can be mutated to:  
```asm
push rax
not rax
sub rcx, rax
pop rax
sub rcx, 1
```
If you want to learn more about mutation take a look at [perses](https://github.com/mike1k/perses).
### Entrypoint obfuscation
If the PE file is a .exe (.dll support will be added) we will create a custom entrypoint that decrypts the real one on startup (!!! doesn't work when beeing manual mapped).  
![imgmaincfg](images/customentry.PNG)  
### Anti disassembly
balbalbalba
### Import obfuscation
There is no "proper" IAT obfuscation at the moment. The 0xFF anti disassembly trick takes care of it at the moment. Proper implementation is planned here:  
[iat.cpp](Alcatraz/obfuscator/misc/iat.cpp)
### Opaque predicates
balbalbalba
### Mixed boolean arithmetic
balbalbalba

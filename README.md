# vim-s-asm (currently in development)
This repo is heavily based on Shirk/vim-gas and is forked from such project.
The .vim syntax file in this repo contains essentially the same funcionality 
as the original gas.vim file but with added instructions and edited syntax 
supporting GNU Assembly files. 
This started with a Operating Systems project create a kernel entry point 
but with the lack of syntax support for .S assembly files was troublesome. 
This repo aims to fix some of the similar issues you may run into!

This vim bundle adds advanced syntax highlighting for GNU as (AT&T).

This file defines an (almost) complete syntax for GNU as assembler. 

My motivation in writing this was the lack of a complete, working syntax for this 
common assembler. 

For now the basic as directives as well as the Intel / AMD, ARM (thumb) and AVR instruction sets are included.  
Opcodes supporting a size suffix are recognized plain as well as suffixed with b/w/l/q. 

## What does it look like?

![screenshot here..](https://raw.githubusercontent.com/Shirk/vim-gas/master/sample.png)

## Supported instructions a per version 0.16: 

- X86 common instruction set (8086 - 686) 
- Katmai Streaming SIMD instructions (SSE -- a.k.a. KNI, XMM, MMX2) 
- Introduced in Deschutes but necessary for SSE support 
- XSAVE group (AVX and extended state) 
- Generic memory operations 
- New MMX instructions introduced in Katmai 
- AMD Enhanced 3DNow! (Athlon) instructions 
- Willamette SSE2 Cacheability Instructions 
- Willamette MMX instructions (SSE2 SIMD Integer Instructions) 
- Willamette Streaming SIMD instructions (SSE2) 
- Prescott New Instructions (SSE3) 
- VMX Instructions 
- Extended Page Tables VMX instructions 
- Tejas New Instructions (SSSE3) 
- AMD SSE4A 
- New instructions in Barcelona 
- Penryn New Instructions (SSE4.1) 
- Nehalem New Instructions (SSE4.2) 
- AMD SSE5 instructions 
- Intel SMX 
- Geode (Cyrix) 3DNow! additions 
- Intel AES instructions 
- Intel AVX AES instructions 
- Intel AVX instructions 
- Intel AVX2 instructions
- Intel Carry-Less Multiplication instructions (CLMUL) 
- Intel AVX Carry-Less Multiplication instructions (CLMUL) 
- Intel Fused Multiply-Add instructions (FMA) 
- VIA (Centaur) security instructions 
- AMD XOP, FMA4 and CVT16 instructions (SSE5) 
- Systematic names for the hinting nop instructions 
- ARM (Thumb) + updated instructions
- AVR

You can set different highlighting for each set listed above by adding a line like this to you .vimrc: 

```vim
  hi def link gasOpcodeX86_PENT_SSE  <some_highlighting> 
```

## per-file opcode enable / disable 

Since version 0.6 it is possible to disable sets of opcodes by either CPU they where introduced in or by their extension name. 
This is done by setting a special variable recognized by gas.vim to the list of disabled opcodes. 

E.g. 

```vim
" globally disable all x86_64 opcodes 
:let g:gasDisableOpcodes='x86_64' 


" disable SSE and SSE2 for this buffer 
let b:gasDisableOpcodes='sse sse2' 
```

Disabled opcodes will be highlighted as errors. 

recognized CPU groups:

           186, 286, 386, 3862, 486, 8086, amd, future, ia64, katmai, nehalem, 
            p6, pentium_m, pentium, prescott, sandybridge, x64, x642, x86_64 

recognized extension groups: 

           base, 3dnow, mmx, mmx2, sse, sse2, sse3, ssse3, sse4.1, sse4.2, sse4a, 
           sse5, avx, fma, vmx 


I hope this syntax file will be useful to other people 
  \- but don't blame me if it doesn't suit your needs. 

## CPP preprocessor macros

GAS allows code to be processed by the same preprocessor used for C,C++ code. 
So directives like `#include`, `#define` etc. will be highlighted using the C 
syntax highlighter if available. 

This behavior can be turned off by setting the `gasDisablePreproc` option 
either globally in your .vimrc or on a per-buffer basis:

```vim
" globally disable preprocessor macro detection
:let g:gasDisablePreproc=1 


" disable only for this buffer 
let gasDisablePreproc=1 
```

## Installation

This syntax file can either be installed manually, by using pathogen or vundle.

### My Manual Installation Method
I was running into issues with my ASM files recognizing the s-asm.vim syntax
file. A hackjob was done in order to force this to work. I edited the ~/.vimrc
file to create a new filetype and assign the s-asm.vim syntax file to the newly
created filetype. Here are the steps:

If you have used a plugin manager previously, there is likely this folder 
~/.vim/bundle. If not, follow the below cmds; if so, skip cmd1
```
$ mkdir ~/.vim/bundle &&
cd ~/.vim/bundle &&
git clone git@github.com:akielaries/vim-s-asm.git
```
If your ASM files automatically recognize s-asm.vim as the default syntax file,
great, if not, add the following to your ~/.vimrc file.
```
if has("autocmd")
    " this sets files with .S extension to new filetype on creation and read
    autocmd BufNewFile,BufRead *.S set filetype=s-asm
    " points s-asm.vim syntax file as default for newly created filetype
    autocmd! Syntax s-asm source ~/.vim/bundle/akielaries/vim-s-asm/syntax/s-asm.vim
endif " has("autocmd")"
```
Finally reload your .vimrc file 
(from within your .vimrc file type:)
```
:source %
```
And this should enable the syntax on all .S files.
There will be more changes to include setting this syntax file as the default
for ASM files!
# Manual Installation Method 2
Download gas.vim and copy the file to .vim/syntax/ in your home folder. 
Add this line to the end of your file: 
```vim
/* vim: ft=gas : 
*/ 
```

## Installation with Git & Pathogen

- Install [pathogen](http://www.vim.org/scripts/script.php?script_id=2332) into `~/.vim/autoload/` and add the
   following line to your `~/.vimrc`:

        call pathogen#infect()

- Make a clone of the `vim-gas` repository:

        $ mkdir -p ~/.vim/bundle
        $ cd ~/.vim/bundle
        $ git clone https://github.com/Shirk/vim-gas

- OR use git submodules:

        $ git submodule add https://github.com/Shirk/vim-gas.git bundle/vim-gas
        $ git submodule init

## Installation with Vundle

- Install and setup [vundle](https://github.com/VundleVim/Vundle.vim) then add this to your `~/.vimrc` inside
   the `vundle#begin()` / `vundle#end()` block:

        Plugin shirk/vim-gas

- Restart your vim and call `VundleInstall`

## License ##

BSD (3 clause) - see LICENSE for details

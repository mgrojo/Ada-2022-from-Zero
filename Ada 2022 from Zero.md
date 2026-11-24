# Ada 2022 from Zero

An entry-level programming course in 16 lessons and 53 exercises, written for Ada 2022. All 107 programs on this page went through GNAT 13.3, and every terminal shows exactly what the compiler and the program printed. Stuck on a word? It's in the [glossary](#glossary).

**0 of 53 done**

[Start here: Setup](#0)

[Lesson 1: Hello, world](#1)

[Lesson 2: Types and variables](#2)

[Lesson 3: Arithmetic](#3)

[Lesson 4: Input](#4)

[Lesson 5: Decisions](#5)

[Lesson 6: Loops](#6)

[Lesson 7: Your own types](#7)

[Lesson 8: Arrays](#8)

[Lesson 9: Subprograms](#9)

[Lesson 10: Strings](#10)

[Lesson 11: Records](#11)

[Lesson 12: Packages](#12)

[Lesson 13: Errors](#13)

[Lesson 14: Contracts](#14)

[Lesson 15: Project](#15)

[Glossary](#glossary)

# 0

## Get a compiler running

Six steps, one command at a time. At the end you'll have built and run your first program.

You write Ada in plain text files ending in `.adb`. The GNAT compiler reads that file, checks it against the rules of the language, and if it passes, produces an *executable*: a program you run from the terminal. When it doesn't pass, GNAT tells you the file, line and column of each problem.

Do the steps in order. Copy one box, paste it into the terminal, press Enter, and check the result matches what the step says before moving on. Pick your computer first; the steps change to match.

Your computer

### 1Open a terminal and install the compiler

On Linux

Open a terminal: press **Ctrl+Alt+T**, or find Terminal (Konsole on KDE) in your apps menu. To paste into it, press **Ctrl+Shift+V**; plain Ctrl+V doesn't paste in most Linux terminals.

Run the line for your distribution. It asks for your password, and nothing appears while you type it. That's normal.

Debian, Ubuntu, Mint

```
sudo apt install gnat gprbuild
```

Fedora

```
sudo dnf install gcc-gnat
```

Arch, Manjaro, Garuda

```
sudo pacman -S gcc-ada
```

Then check it worked:

Terminal

```
gnatmake --version
```

**You should see** a first line like `GNATMAKE 13.3.0`. Any version from 13 up should work.

On Windows

On Windows you'll use [MSYS2](https://www.msys2.org), a free toolkit that gives you a Linux-style terminal and the GCC compilers, Ada included. Once it's set up, every command in this course works exactly as written.

- Download the installer from [msys2.org](https://www.msys2.org): the file named `msys2-x86_64-` followed by a date. It needs 64-bit Windows 10 or newer.
- Run it, keep the suggested folder (`C:\msys64`), and click Finish. A terminal window opens.
- From now on, open that terminal from the Start menu as **MSYS2 UCRT64**. Not "MSYS2 MSYS" or the other MSYS2 entries: only UCRT64 can see the compiler you're about to install.
- To paste into it, right-click and choose Paste, or press **Shift+Insert**. Ctrl+V doesn't paste there.

In the UCRT64 terminal, install the Ada compiler:

MSYS2 UCRT64

```
pacman -S mingw-w64-ucrt-x86_64-gcc-ada
```

It lists what it will install and asks `Proceed with installation? [Y/n]`. Press Enter. Then check:

MSYS2 UCRT64

```
gnatmake --version
```

**You should see** a first line starting `GNATMAKE` with a version number, 16 or newer.

Already use WSL? Ubuntu inside WSL works too: switch this page to Linux and follow that tab in your Ubuntu terminal.

The programs in this course were tested on Linux with GNAT 13.3. These Windows install steps follow MSYS2's own instructions rather than a test run here, so if one fails, the exact message is what to search for.

On Mac

Open Terminal: press **Cmd+Space**, type `Terminal`, press Return. Paste with **Cmd+V**.

First install Apple's command line tools, which include the linker GNAT needs. A window pops up: click Install and wait for it to finish. If it says they're already installed, carry on.

Terminal

```
xcode-select --install
```

Next, install [Alire](https://alire.ada.dev), Ada's package manager, with the [GetAda](https://www.getada.dev) installer. Answer its questions with the defaults:

Terminal

```
curl --proto '=https' -sSf https://www.getada.dev/init.sh | sh
```

Quit Terminal (Cmd+Q) and open it again, so it can find the new `alr` command. Then have Alire install the compiler, and add the folder it lands in to your PATH (the list of places Terminal looks for commands):

Terminal

```
alr install gnat_native gprbuild
echo 'export PATH="$HOME/.alire/bin:$PATH"' >> ~/.zprofile
```

Quit and reopen Terminal once more, then check:

Terminal

```
gnatmake --version
```

**You should see** a first line starting `GNATMAKE` with a version number.

The programs in this course were tested on Linux with GNAT 13.3. These Mac install steps follow Alire and GetAda's own instructions rather than a test run here, so if one fails, the exact message is what to search for.

### 2Make a folder for the course

Terminal

```
mkdir -p ~/ada-course
cd ~/ada-course
pwd
```

**You should see** `pwd` print a path ending in `/ada-course`. That's the folder you're "in". Every command in this course runs from here, so whenever you open a new terminal, run `cd ~/ada-course` first.

On Linux

In your file manager, it's `ada-course` inside your home folder.

On Windows

In File Explorer, it's `C:\msys64\home\`*your user name*`\ada-course`.

On Mac

In Finder, choose Go → Home, and it's the `ada-course` folder there.

### 3Create hello.adb with the program in it

This is the program. Press its Copy button:

hello.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Hello is
begin
   Put_Line ("Hello, world!");
   Put ("Put leaves the cursor here... ");
   Put_Line ("Put_Line ends the line.");
   New_Line;
   Put_Line ("That blank line came from New_Line.");
end Hello;
```

Now put it in a file called `hello.adb` inside `ada-course`. The terminal way works the same everywhere: run the command below. Nothing seems to happen; it's waiting for you. Paste the program (with the paste keys from step 1), press **Enter**, then press **Ctrl+D** to finish and save.

Terminal

```
cat > hello.adb
```

On Linux

Prefer a text editor? Kate, GNOME Text Editor or VS Code all work: open a new file, paste, choose Save As, go into your home folder, then `ada-course`, and name it exactly `hello.adb`.

On Windows

Prefer Notepad? Paste the program, choose File → Save As, go to the folder from step 2, set "Save as type" to **All files**, and name it `hello.adb`. Without All files, Notepad quietly saves `hello.adb.txt`.

On Mac

Use the terminal way on a Mac. TextEdit saves formatted text by default and can swap straight quotes for curly ones, which Ada rejects. If you do use TextEdit, choose Format → Make Plain Text first and turn off Edit → Substitutions → Smart Quotes.

### 4Check the file is really there

Terminal

```
ls
cat hello.adb
```

**You should see** `ls` list `hello.adb`, and `cat` print the whole program starting with `with Ada.Text_IO;`. If `cat` prints nothing, the file is empty: go back to step 3. If it prints the commands from this page instead of the program, you pasted the wrong box.

### 5Compile it

Terminal

```
gnatmake -q -gnat2022 -gnata hello.adb
```

**You should see** nothing at all. With `-q`, silence means success. Run `ls` and you'll now also find your program, plus `hello.o` and `hello.ali`, which are leftovers from the build. If you see lines with `error:`, look them up in the table below.

On Linux

The program is the file called `hello`.

On Windows

On Windows the program is called `hello.exe`. You still run it as `./hello`.

On Mac

The program is the file called `hello`.

### 6Run it

Terminal

```
./hello
```

**You should see** the three lines at the bottom of this terminal. It's the same session as steps 5 and 6, captured from a real run:

Terminal

```
$ gnatmake -q -gnat2022 -gnata hello.adb
$ ./hello
Hello, world!
Put leaves the cursor here... Put_Line ends the line.

That blank line came from New_Line.
```

Done. Mark the checkpoint below and go to lesson 1, which explains every line of what you just ran.

### If something went wrong

| You see | What it means | Fix |
| --- | --- | --- |
| `command not found: gnatmake` or `fish: Unknown command: gnatmake` | GNAT isn't installed, or the terminal can't find it | Step 1 |
| Windows: `bash: gnatmake: command not found` after installing | You're in a different MSYS2 terminal, not UCRT64 | Open **MSYS2 UCRT64** from the Start menu |
| Mac: `zsh: command not found: alr` | Terminal hasn't picked up GetAda's change yet | Quit Terminal with Cmd+Q and open it again |
| Mac: `zsh: command not found: gnatmake` after `alr install` | The PATH line didn't take effect, or `alr install` failed | Reopen Terminal. If it still fails, run `alr toolchain --select`, press Enter to accept the defaults, and ask on the [Ada Forum](https://forum.ada-lang.io) how to reach that toolchain |
| Mac: `xcrun: error: invalid active developer path` | Apple's command line tools are missing | `xcode-select --install` |
| Mac: `ld: library not found for -lSystem` | A known clash between some Alire compilers and macOS 26 Tahoe | Use the fix in [this Ada Forum thread](https://forum.ada-lang.io/t/unusual-workaround-required-to-get-ada-programs-to-build-on-an-m1-mac-running-macos-tahoe-26-3-1/4234) |
| `gnatmake: "hello.adb" not found` | There's no `hello.adb` in the folder you're in | Run `pwd` and `ls`, then steps 2 and 3 |
| `hello.adb:1:01: error: compilation unit expected` | The file doesn't start with Ada code. Usually it's empty, or holds something other than the program | `cat hello.adb` to see what's in it, then redo step 3 |
| `./hello: No such file or directory` or `fish: Unknown command: ./hello` | Compiling failed, so the program was never made | Fix the compile error first, then step 5 again |
| `error: ... is an Ada 2022 feature` | `-gnat2022` was left off | Copy the step 5 command exactly |
| `hello: command not found` | You typed `hello`; the shell only looks in system folders | Type `./hello`: the `./` means "in this folder" |

### What the compile command means

| Piece | What it does |
| --- | --- |
| `gnatmake` | Compiles your file and anything it depends on, then builds the program |
| `-q` | Quiet: print nothing unless something is wrong |
| `-gnat2022` | Use the Ada 2022 language. GNAT 13 defaults to Ada 2012, so this matters |
| `-gnata` | Turn on contract checks (lessons 9 and 14). Off by default, so always include it while learning |
| `hello.adb` | The file to build. The program comes out named `hello` |
| `gnatclean hello` | Deletes the `.o` and `.ali` leftovers when you want a tidy folder |

One rule trips everyone once: the file name must be the program's name in lower case. `procedure Hello` lives in `hello.adb`; `procedure Fizz_Buzz` in `fizz_buzz.adb`. Every later lesson follows the same routine: save the file, step 5 with the new file name, step 6 with the new program name.

Later, when you want projects and libraries: [Alire](https://alire.ada.dev)'s `alr init --bin hello` makes a project; add `[build-switches]` and `"*".ada_version = ["-gnat2022"]` to its `alire.toml`, then `alr run`. Any editor works for writing code. VS Code with AdaCore's Ada extension adds highlighting and shows errors as you type.

#### Remember

- One command at a time; check each result
- Work inside `~/ada-course`
- Save, compile, run: `gnatmake -q -gnat2022 -gnata name.adb` then `./name`
- Silence after `gnatmake -q` means it worked

#### Common mistakes

- Running the compile before the file is saved
- Saving the file somewhere other than the folder you're in
- Pasting several boxes at once: things run out of order
- Typing `hello` instead of `./hello`
- Windows: using an MSYS2 terminal other than UCRT64

### Exercises

Checkpoint

#### Hello, world runs

Follow steps 1 to 6. When `./hello` prints `Hello, world!` and the two lines after it, mark this done.

Hint

Most first-time failures are an empty `hello.adb`, or a terminal sitting in a different folder from the file. Step 4 catches both.

If it doesn't work

Find the exact message you're seeing in the table above. If it isn't there, run these three and compare: `pwd` should end in `ada-course`, `ls` should list `hello.adb`, and `head -1 hello.adb` should print `with Ada.Text_IO; use Ada.Text_IO;`. The first one that doesn't match is the step to redo.

# 1

## Hello, world

The shape every Ada program shares, and how to read the compiler when it says no.

This is the program you built in setup. Here's what each part does.

hello.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Hello is
begin
   Put_Line ("Hello, world!");
   Put ("Put leaves the cursor here... ");
   Put_Line ("Put_Line ends the line.");
   New_Line;
   Put_Line ("That blank line came from New_Line.");
end Hello;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata hello.adb
$ ./hello
Hello, world!
Put leaves the cursor here... Put_Line ends the line.

That blank line came from New_Line.
```

Read it top to bottom. `with Ada.Text_IO;` asks for the standard library package that does text input and output. `use Ada.Text_IO;` lets you write `Put_Line` instead of the full `Ada.Text_IO.Put_Line`. (Both get a full explanation below.) Then comes the main procedure: its name, the word `is`, `begin`, the statements, and `end Hello;` with the name repeated.

- Text in double quotes, like `"Hello, world!"`, is a *string literal*: text typed straight into the program, used exactly as written. The quotes only mark where it starts and ends, so `"Hi"` is two characters. Without quotes, a word is a name the program looks up (lesson 2), not text. To put a quote inside, write it twice: `"She said ""hi"""` prints `She said "hi"`. A string literal must fit on one line.
- A *statement* is one instruction for the computer to carry out, like `Put_Line ("Hello, world!");`. Statements run one after another, top to bottom, and every one ends with a semicolon.
- A *procedure* is a named list of statements. Every program has a main procedure, here `Hello`, and running the program means running its statements from `begin` to `end`.
- The words in bold in this page's code, like `procedure`, `is`, `begin` and `end`, are *keywords* (Ada calls them reserved words). Each has a fixed meaning, so you can't use one as a name.
- Ada ignores letter case: `PUT_LINE` and `put_line` work too. The convention is `Mixed_Case_With_Underscores`.
- Two dashes start a comment: `-- the compiler ignores this`.

### Where Put_Line comes from: with and use

`Put_Line` isn't built into Ada the way `begin`, `if` and `loop` are. It's an ordinary procedure, written in Ada by the compiler's makers, kept in a *package*: a named collection of related procedures, types and constants. The Ada standard itself defines a set of these packages (Annex A of the Ada Reference Manual), and every Ada compiler must supply them. So they're already on your computer, installed along with GNAT. There's nothing to download.

The standard packages are arranged like folders. At the top is a package called `Ada`, which holds nothing of its own and just groups the rest: `Ada.Text_IO` for reading and writing text, `Ada.Float_Text_IO` for printing decimals (lesson 3), `Ada.Strings` for working with text (lesson 10), and many more. Each dot steps one level in:

`Ada.Text_IO.Put_Line`

1. `Ada` the package at the top of the standard library
2. `Text_IO` a package inside Ada: text input and output
3. `Put_Line` a procedure inside Text_IO

So `Ada.Text_IO.Put_Line` is a *full name*: "the procedure Put_Line, in the package Text_IO, in the package Ada". The two lines at the top of your program decide how you're allowed to refer to it:

| Line | What it does | Leave it out and |
| --- | --- | --- |
| `with Ada.Text_IO;` | Gives your program access to the package. The compiler reads the package's list of contents to check every call you make, and the build joins the package's code to your program. | The package doesn't exist as far as your program is concerned. Nothing from it works |
| `use Ada.Text_IO;` | A shortcut: lets you write short names like `Put_Line` instead of `Ada.Text_IO.Put_Line`. | You write full names instead. The program works the same |

Think of `with` as getting a library card and `use` as putting the book on your desk. You can't put a book on your desk without the card: `use` on its own fails, with GNAT reporting `missing "with Ada.Text_IO;"`.

With `with` but no `use`, the short name fails. Look at where GNAT says `Put_Line` is: lines 566 and 555 of a file called `a-textio.ads`. That file is the package. (Your line numbers may differ with a different GNAT version.)

short_only.adb

```ada
with Ada.Text_IO;

procedure Short_Only is
begin
   Put_Line ("Hello");
end Short_Only;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata short_only.adb
short_only.adb:5:04: error: "Put_Line" is not visible
short_only.adb:5:04: error: non-visible declaration at a-textio.ads:566
short_only.adb:5:04: error: non-visible declaration at a-textio.ads:555
gnatmake: "short_only.adb" compilation error
```

The full name works with no `use` at all:

full_names.adb

```ada
with Ada.Text_IO;

procedure Full_Names is
begin
   Ada.Text_IO.Put_Line ("No use clause here,");
   Ada.Text_IO.Put_Line ("so every name is written in full.");
end Full_Names;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata full_names.adb
$ ./full_names
No use clause here,
so every name is written in full.
```

And with no `with`, GNAT has never heard of `Put_Line`, and suggests the lines you probably forgot:

no_with.adb

```ada
procedure No_With is
begin
   Put_Line ("Hello");
end No_With;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata no_with.adb
no_with.adb:3:04: error: "Put_Line" is undefined
no_with.adb:3:04: error: possible missing "with Ada.Text_IO; use Ada.Text_IO"
gnatmake: "no_with.adb" compilation error
```

Simple types like `Integer`, `Boolean` and `String` need no `with`. They live in a package called `Standard` that every program can always see. `Text_IO` belongs to the standard library rather than the core language, so you ask for it.

### Under the hood (optional)

You can open the file GNAT pointed at. GNAT keeps the standard library's source code in a folder called `adainclude`. This shows where it is on your computer:

Terminal

```
$ gnatls -v | grep adainclude
   /usr/lib/gcc/x86_64-linux-gnu/13/adainclude
```

`Ada.Text_IO` is the file `a-textio.ads` in that folder; GNAT shortens library file names, and `a-` means "inside package Ada". This finds `Put_Line` in it. Put the folder your own `gnatls` printed in place of mine:

Terminal

```
$ grep -n -A2 "procedure Put_Line" /usr/lib/gcc/x86_64-linux-gnu/13/adainclude/a-textio.ads
555:   procedure Put_Line
556-     (File : File_Type;
557-      Item : String)
--
566:   procedure Put_Line
567-     (Item : String)
568-   with
```

There are two `Put_Line`s. The first also takes a `File` and writes to that file; the second writes to the screen, and that's the one you've been calling. Ada picks between them by looking at what you pass in, which is called *overloading*. The `with` on line 568 starts a contract, a promise about what the procedure does (lesson 14 reads this one): the same word doing a different job.

Building runs three tools, which `-q` normally hides. Leave it off to watch:

Terminal

```
$ gnatmake -gnat2022 -gnata hello.adb
x86_64-linux-gnu-gcc-13 -c -gnat2022 -gnata hello.adb
x86_64-linux-gnu-gnatbind-13 -x hello.ali
x86_64-linux-gnu-gnatlink-13 hello.ali
```

- **Compile** (`gcc -c`): turns `hello.adb` into `hello.o`. This is when GNAT reads `a-textio.ads` and checks that each call matches one of the declarations. It doesn't need the package's code yet, only its list of contents.
- **Bind** (`gnatbind`): reads `hello.ali`, GNAT's record of what your program `with`s, and checks every piece fits together.
- **Link** (`gnatlink`): joins `hello.o` with the already-compiled library code, which ships with GNAT in an `adalib` folder next to `adainclude`, to make the program `hello`.

The `W` lines in `hello.ali` are your `with` clauses as GNAT recorded them: this program needs `Ada` and `Ada.Text_IO`.

Terminal

```
$ grep "^W" hello.ali
W ada%s			ada.ads			ada.ali
W ada.text_io%s		a-textio.adb		a-textio.ali
```

On your system the tools may be named plain `gcc`, `gnatbind` and `gnatlink`, without the prefix and version number. They're the same tools.

### Put, Put_Line and New_Line

All three print to the terminal. The difference is where they leave the *cursor*, the spot where the next character will appear. Think of a typewriter: you can type more on the same line, or return the carriage to start a new one.

| Statement | Prints text? | Ends the line? |
| --- | --- | --- |
| `Put ("Hi");` | Yes | No. The cursor waits right after `Hi` |
| `Put_Line ("Hi");` | Yes | Yes. The cursor moves to the start of the next line |
| `New_Line;` | No | Yes. If the line was empty, you see a blank line |

Watch the cursor (the yellow block) as these six statements run one after another:

Statement

Screen afterwards

1`Put ("Hi");`

Hi

2`Put (" there");`

Hi there

3`New_Line;`

Hi there

4`Put_Line ("Bye");`

Hi there Bye

5`New_Line;`

Hi there Bye

6`Put ("End");`

Hi there Bye End

Statement 3 ends a line that already has text, so nothing looks different except the cursor moving down. Statement 5 ends a line that has nothing on it, so it leaves a blank line. That's the part that confuses people: `New_Line` doesn't print a blank line, it ends the current line, and an ended empty line *looks* blank.

**Put** keeps adding to the same line:

put_demo.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Put_Demo is
begin
   Put ("One ");
   Put ("two ");
   Put ("three");
end Put_Demo;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata put_demo.adb
$ ./put_demo
One two three
```

The program ends without finishing its line, so GNAT finishes it at exit. That's why the terminal's next prompt still starts on a fresh line.

**Put_Line** prints, then ends the line, so every call gets its own line:

put_line_demo.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Put_Line_Demo is
begin
   Put_Line ("One");
   Put_Line ("two");
   Put_Line ("three");
end Put_Line_Demo;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata put_line_demo.adb
$ ./put_line_demo
One
two
three
```

**New_Line** prints nothing; it only ends lines. `New_Line (2)` ends two at once:

new_line_demo.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure New_Line_Demo is
begin
   Put ("Name: Ada");
   New_Line;            --  ends the "Name" line
   Put_Line ("Job: Analyst");
   New_Line;            --  this line is empty, so it shows as a gap
   Put_Line ("Hours: 40");
   New_Line (2);        --  two line endings: two gaps
   Put_Line ("Done");
end New_Line_Demo;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata new_line_demo.adb
$ ./new_line_demo
Name: Ada
Job: Analyst

Hours: 40


Done
```

`Put_Line ("X");` does exactly the same as `Put ("X"); New_Line;`. Use `Put` when you want to build one line out of several pieces, like a prompt the user types after (lesson 4).

### Reading an error

GNAT reports problems as `file:line:column: error: message`. Fix the first one first: a single missing semicolon can cause a pile of confused messages after it. Exercise 1.2 lets you practice.

#### Remember

- `with` gives access to a package; `use` lets you skip its name
- Full names always work: `Ada.Text_IO.Put_Line`
- `procedure Name is begin ... end Name;`
- `Put` stays on the line, `Put_Line` ends it, `New_Line` ends it without printing

#### Common mistakes

- Missing `;` at the end of a statement
- `use` without `with`, or short names with only `with`
- Single quotes around text: `'x'` is one character, text needs `"..."`
- `end` name not matching the procedure name
- `Put_Line` where you meant `Put`: the line ends early, and a `New_Line` after it then shows as an extra blank line

### Exercises

Exercise 1.1

#### Business card

Print a card with a border: a name, a job title and a city, each on its own line inside a box made of `+`, `-` and `|`. The right edge must line up.

Target output

```
$ ./card
+--------------------------+
|  Ada Lovelace            |
|  Analyst, Note G         |
|  London                  |
+--------------------------+
```

Hint

Make every line the same number of characters. Count the spaces before the closing `|`.

Solution

card.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Card is
begin
   Put_Line ("+--------------------------+");
   Put_Line ("|  Ada Lovelace            |");
   Put_Line ("|  Analyst, Note G         |");
   Put_Line ("|  London                  |");
   Put_Line ("+--------------------------+");
end Card;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata card.adb
$ ./card
+--------------------------+
|  Ada Lovelace            |
|  Analyst, Note G         |
|  London                  |
+--------------------------+
```

Exercise 1.2

#### Fix the bugs

This program has three mistakes. Without changing what it prints, fix it so it compiles. Compile it first and let GNAT point at them.

Target output

```
$ ./greet_me
Good morning
Have a nice day
```

greet_me.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Greet_Me is
begin
   Put_Line ("Good morning")
   Put_Line ('Have a nice day');
end Greet;
```

Hint

The error at line 5 column 29 is the end of the first `Put_Line`. Look at the quote marks on line 6 and at the name after `end`.

Solution

Terminal

```
$ gnatmake -q -gnat2022 -gnata greet_me.adb
greet_me.adb:5:29: error: missing ";"
greet_me.adb:6:14: error: strings are delimited by double quote character
greet_me.adb:7:01: error: "end Greet_Me;" expected
gnatmake: "greet_me.adb" compilation error
```

A missing semicolon, single quotes around a string, and `end Greet` where the procedure is called `Greet_Me`. Fixed:

greet_me.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Greet_Me is
begin
   Put_Line ("Good morning");
   Put_Line ("Have a nice day");
end Greet_Me;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata greet_me.adb
$ ./greet_me
Good morning
Have a nice day
```

Exercise 1.3

#### Put and New_Line only

Print the target output below without using `Put_Line` at all: only `Put` and `New_Line`.

Target output

```
$ ./roses
Roses are red,
violets are blue.

Ada checks your types
so bugs don't get through.
```

Hint

Each line is a `Put` followed by a `New_Line`. The gap needs one more `New_Line` on a line with nothing on it.

Solution

roses.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Roses is
begin
   Put ("Roses are red,");
   New_Line;
   Put ("violets are blue.");
   New_Line;
   New_Line;
   Put ("Ada checks your types");
   New_Line;
   Put ("so bugs don't get through.");
   New_Line;
end Roses;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata roses.adb
$ ./roses
Roses are red,
violets are blue.

Ada checks your types
so bugs don't get through.
```

Two `New_Line`s in a row after `violets are blue.`: the first ends that line, the second ends an empty line, which shows as the gap.

Exercise 1.4

#### No use clause

Open the `hello.adb` you made in setup, delete the line `use Ada.Text_IO;`, and make it compile again without putting that line back. The output must stay exactly the same.

Target output

```
$ ./hello
Hello, world!
Put leaves the cursor here... Put_Line ends the line.

That blank line came from New_Line.
```

Hint

Compile it first and read the errors: each one is a short name that's no longer visible. Give each its full name.

Solution

hello.adb

```ada
with Ada.Text_IO;

procedure Hello is
begin
   Ada.Text_IO.Put_Line ("Hello, world!");
   Ada.Text_IO.Put ("Put leaves the cursor here... ");
   Ada.Text_IO.Put_Line ("Put_Line ends the line.");
   Ada.Text_IO.New_Line;
   Ada.Text_IO.Put_Line ("That blank line came from New_Line.");
end Hello;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata hello.adb
$ ./hello
Hello, world!
Put leaves the cursor here... Put_Line ends the line.

That blank line came from New_Line.
```

Every `Put`, `Put_Line` and `New_Line` gets `Ada.Text_IO.` in front. That's why most programs add the `use` line.

# 2

## Types and variables

Every value has a type, and in Ada the type decides nearly everything about it. Types first, then the variables that hold them.

### What a type is

A type is a kind of value. It settles two things: which values are allowed, and what you can do with them. `18`, `3.89`, `True`, `'J'` and `"Welcome"` are five different kinds of value, and Ada treats each one differently.

| Type | Allowed values | What you can do | Example |
| --- | --- | --- | --- |
| `Integer` | Whole numbers. On GNAT: −2,147,483,648 to 2,147,483,647, about ±2.1 billion (more at the end of this lesson) | `+ - * / mod`, compare | `182_400` |
| `Natural` | Integers from 0 up | Same as Integer | `0` |
| `Positive` | Integers from 1 up | Same as Integer | `1` |
| `Float` | Numbers with a fractional part | `+ - * /`, compare | `1.82` |
| `Boolean` | Only `True` and `False` | `and`, `or`, `not`, compare | `True` |
| `Character` | One letter, digit, space or symbol | Compare, convert to and from its number | `'J'` |
| `String` | Text: a row of Characters | Join with `&`, take pieces, compare | `"Welcome"` |

The Example column holds literals (lesson 1), and literals have types too. `182_400` is a whole number, `1.82` is a number with a fraction, `'J'` in single quotes is a Character, and `"Welcome"` in double quotes is a String. So `'J'` and `"J"` are different types: one character, versus text that happens to be one character long.

Here's what that means in practice. The same symbols do different things depending on the type. (Ignore `'Image` for now: it turns a value into text so it can be printed, and it's explained further down.)

same_symbols.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Same_Symbols is
   Two      : constant Integer := 2;
   Ten      : constant Integer := 10;
   Two_Text : constant String  := "2";
   Ten_Text : constant String  := "10";
begin
   Put_Line ("Integer 2 + 2:  " & Integer'Image (Two + Two));
   Put_Line ("String 2 & 2:    " & (Two_Text & Two_Text));
   Put_Line ("Integer 2 < 10:  " & Boolean'Image (Two < Ten));
   Put_Line ("String 2 < 10:   " & Boolean'Image (Two_Text < Ten_Text));
   Put_Line ("Character 65:    " & Character'Val (65));
end Same_Symbols;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata same_symbols.adb
$ ./same_symbols
Integer 2 + 2:   4
String 2 & 2:    22
Integer 2 < 10:  TRUE
String 2 < 10:   FALSE
Character 65:    A
```

- `+` on Integers adds. `&` on Strings joins, so `"2" & "2"` is `22`, not 4.
- `<` on Integers compares size: 2 is less than 10. On Strings it compares like a dictionary, first character first. `1` comes before `2`, so `"10"` sorts before `"2"`, and `"2" < "10"` is `FALSE`.
- The computer stores every Character as a number. `Character'Val (65)` is `'A'`: the type is what says to treat 65 as a letter.

When an operation doesn't exist for a type, the program doesn't compile. Strings have no `+`:

add_text.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Add_Text is
   Two_Text : constant String := "2";
begin
   Put_Line (Two_Text + Two_Text);
end Add_Text;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata add_text.adb
add_text.adb:6:23: error: there is no applicable operator "+" for type "Standard.String"
gnatmake: "add_text.adb" compilation error
```

`Standard.String` in the message is String's full name. It lives in package `Standard`, the one every program can see (lesson 1).

This is what people mean when they say Ada is *strongly typed*. Every value has exactly one type. Every variable keeps the type it was declared with. And Ada never turns one type into another behind your back: the compiler checks all of it before the program runs, so a whole class of mistakes never reaches the person using it. Lesson 3 shows how to convert on purpose.

### The family of types

#### Single values (scalar)

- **`Integer`, `Natural`, `Positive`** whole numbers
- **`Float`** numbers with a fraction
- **`Boolean`** true or false
- **`Character`** one character
- **Your own** lists of named values, and number ranges (lesson 7)

#### Groups of values (composite)

- **`String`** a row of Characters (lesson 10)
- **Arrays** a row of values of any one type (lesson 8)
- **Records** values of different types bundled together (lesson 11)

Ada's own words for the two sides are *scalar* and *composite*. One surprise for later: Boolean and Character aren't special. Each is just a list of named values, the same kind of type you'll make yourself in lesson 7.

### Variables: named boxes for values

A variable is a named box that holds one value of one type. You choose the type when you declare it, and it can never hold anything else.

variables.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Variables is
   Age      : Integer   := 34;
   Height   : Float     := 1.82;
   Initial  : Character := 'J';
   Is_Ready : Boolean   := True;
   Greeting : constant String := "Welcome";
begin
   Put_Line (Greeting & ", " & Initial & ".");
   Put_Line ("Age:" & Age'Image);
   Age := Age + 1;
   Put_Line ("Next year:" & Age'Image);
   Put_Line ("Height:" & Height'Image);
   Put_Line ("Ready? " & Is_Ready'Image);
end Variables;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata variables.adb
$ ./variables
Welcome, J.
Age: 34
Next year: 35
Height: 1.82000E+00
Ready? TRUE
```

Variables are declared between `is` and `begin` as `Name : Type := starting value;`. `:=` means "store this". It is not the same as `=`, which asks "are these equal?". A `constant` gets its value once and can never change, which the compiler enforces.

### Choosing names

The names you make up, for variables, constants, procedures and types, are called *identifiers*. Ada's rules for them:

- Letters, digits and underscores only: `Total_Miles`, `Line_2`.
- Start with a letter. `2nd_Place` isn't allowed; `Second_Place` is.
- No spaces. Join words with an underscore, but never two underscores in a row, and never one at the end.
- Not a keyword. `Begin`, `End` and `Type` are taken.
- Capitals don't matter: `Age`, `AGE` and `age` are the same name. Pick one spelling and stick to it.

Here are three broken names in one program:

bad_names.adb

```ada
procedure Bad_Names is
   2nd_Place    : Integer := 2;
   Total Amount : Integer := 0;
   Begin        : Integer := 1;
begin
   null;
end Bad_Names;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata bad_names.adb
bad_names.adb:2:04: error: identifier may not start with digit
bad_names.adb:3:04: error: declaration expected
bad_names.adb:4:16: error: statement expected
bad_names.adb:7:01: error: "end;" expected for "begin" at line 5
gnatmake: "bad_names.adb" compilation error
```

Only the first message names the real problem. For `Total Amount`, GNAT reads `Total`, expects a colon, finds another word, and can only say it expected a declaration. `Begin` is the keyword `begin`, so GNAT decides the declarations have ended and the statements have started, and every message after that comes from the confusion. When messages stop making sense, fix the first one and compile again.

Good names say what a value means. `Miles_Driven` beats `M` or `X2`, because you'll read your code far more often than you write it.

`&` joins strings and characters. `X'Image` turns a value into text; the apostrophe introduces an *attribute*, a built-in fact about something. Writing `Age'Image` straight on a variable is new in Ada 2022; older code says `Integer'Image (Age)`.

Look closely at the output. `'Image` puts a space in front of numbers that aren't negative (room for a minus sign), writes Floats in scientific notation (`1.82000E+00` means 1.82 × 10⁰) and spells `True` in capitals. Lesson 3 shows nicer number formatting.

Underscores in numbers are only for readability: `182_400` is `182400`.

### How big can an Integer be?

It depends on the compiler, which is why the numbers can look vague. The Ada standard only sets a minimum: every compiler's `Integer` must cover at least −32,767 to 32,767, which is what fits in 16 bits. Compilers are free to go bigger, and GNAT uses 32 bits, which gives about ±2.1 billion. You never need to memorize either: ask Ada with `Integer'First` and `Integer'Last`.

limits.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Limits is
   type Big_Count is range 0 .. 10 ** 12;   --  your own range (lesson 7)

   Count : Integer := Integer'Last;
begin
   Put_Line ("Smallest Integer:" & Integer'First'Image);
   Put_Line ("Largest Integer: " & Integer'Last'Image);
   Put_Line ("Bits used:       " & Integer'Size'Image);
   Put_Line ("Largest Long_Long_Integer:" & Long_Long_Integer'Last'Image);
   Put_Line ("Largest Big_Count:" & Big_Count'Last'Image);

   Count := Count + 1;   --  one past the largest Integer
   Put_Line ("You will never see this line.");
end Limits;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata limits.adb
limits.adb:14:19: warning: value not in range of type "Standard.Integer" [enabled by default]
limits.adb:14:19: warning: Constraint_Error will be raised at run time [enabled by default]
$ ./limits
Smallest Integer:-2147483648
Largest Integer:  2147483647
Bits used:        32
Largest Long_Long_Integer: 9223372036854775807
Largest Big_Count: 1000000000000

raised CONSTRAINT_ERROR : limits.adb:14 overflow check failed
```

- `Integer'First` and `Integer'Last` are the smallest and largest Integers on this compiler. 32 bits can hold 4,294,967,296 different values. Zero takes one of the non-negative slots, which is why the negative side reaches one further.
- `Long_Long_Integer` is a bigger built-in type, 64 bits on GNAT, for numbers past 2 billion.
- `Big_Count` is a type declared by stating the range it needs. You say which values make sense and the compiler picks a size that holds them. Lesson 7 covers this properly.
- Adding 1 to the largest Integer doesn't quietly wrap around to a big negative number, as it often does in C. Ada stops the program with `Constraint_Error` and names the line. Constraint_Error is one of Ada's *exceptions*: signals that something went wrong while the program was running. You'll meet it often; lesson 13 shows how to catch exceptions and carry on.

GNAT even warned before the program ran, because it could work out that `Count` already held the largest Integer. Usually it can't: when a number comes from the user (lesson 4) or from a calculation, the check while the program runs is what catches it.

#### Remember

- A type = which values are allowed + what you can do with them
- `Name : Type := Value;`
- `:=` stores, `=` compares
- `X'Image` gives text, `&` joins text

#### Common mistakes

- Using a variable before giving it a value. GNAT warns, but the value is garbage
- Counting past about 2.1 billion in an Integer: use `Long_Long_Integer` or your own range type
- `"J"` where you meant `'J'`: a one-letter String is not a Character
- `"Age: " & Age` without `'Image`: you can't join a number to a string
- Assigning to a `constant`

### Exercises

Exercise 2.1

#### Describe a vehicle

Declare four variables of four different types that describe a vehicle (year, mileage, tank size, has a trailer). Print each one. Then add 1,500 to the mileage and print it again.

Target output

```
$ ./truck
Year: 2014
Miles: 182400
Tank (gal): 2.60000E+01
Trailer? FALSE
After the trip: 183900
```

Hint

Only the mileage changes, so the other three can be `constant`.

Solution

truck.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Truck is
   Year        : constant Integer := 2014;
   Miles       : Integer := 182_400;
   Tank        : constant Float := 26.0;
   Has_Trailer : constant Boolean := False;
begin
   Put_Line ("Year:" & Year'Image);
   Put_Line ("Miles:" & Miles'Image);
   Put_Line ("Tank (gal):" & Tank'Image);
   Put_Line ("Trailer? " & Has_Trailer'Image);
   Miles := Miles + 1_500;
   Put_Line ("After the trip:" & Miles'Image);
end Truck;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata truck.adb
$ ./truck
Year: 2014
Miles: 182400
Tank (gal): 2.60000E+01
Trailer? FALSE
After the trip: 183900
```

Exercise 2.2

#### Swap two values

Start with `A = 3` and `B = 8`. Swap them so `A = 8` and `B = 3`, and print before and after.

Target output

```
$ ./swap
Before: A = 3, B = 8
After:  A = 8, B = 3
```

Hint

`A := B; B := A;` loses the 3. You need a third variable to hold one value for a moment.

Solution

swap.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Swap is
   A    : Integer := 3;
   B    : Integer := 8;
   Temp : Integer;
begin
   Put_Line ("Before: A =" & A'Image & ", B =" & B'Image);
   Temp := A;
   A := B;
   B := Temp;
   Put_Line ("After:  A =" & A'Image & ", B =" & B'Image);
end Swap;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata swap.adb
$ ./swap
Before: A = 3, B = 8
After:  A = 8, B = 3
```

Exercise 2.3

#### Predict the output

Work out on paper what this prints, including the spacing, before you run it.

predict.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Predict is
   A : Integer := 5;
   B : Integer := A;
begin
   A := A + 10;
   B := B * 2;
   A := B - A;
   Put_Line (A'Image & B'Image);
end Predict;
```

Hint

Track both variables line by line. After line 7, A is 15. After line 8, B is 10.

Solution

Terminal

```
$ gnatmake -q -gnat2022 -gnata predict.adb
$ ./predict
-5 10
```

A becomes 15, B becomes 10, then A becomes 10 − 15 = −5. Negative numbers get no leading space, so the output starts with the minus sign.

Exercise 2.4

#### Type mix-up

This program has four type mistakes. Fix each one so it compiles and prints the target output. Read GNAT's messages: each one says which type it expected and which it found.

Target output

```
$ ./mix_up
Wheels: 19
Running? TRUE, initial J
```

mix_up.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Mix_Up is
   Wheels  : Integer   := 18;
   Running : Boolean   := "True";
   Initial : Character := "J";
begin
   Wheels := Wheels + 0.5;
   Put_Line ("Wheels:" & Wheels);
   Put_Line ("Running? " & Running'Image & ", initial " & Initial);
end Mix_Up;
```

Hint

Booleans and Characters don't take double quotes. An Integer can only add whole numbers. And a number can't be joined to a String until it's been turned into text.

Solution

Terminal

```
$ gnatmake -q -gnat2022 -gnata mix_up.adb
mix_up.adb:5:27: error: expected type "Standard.Boolean"
mix_up.adb:5:27: error: found a string type
mix_up.adb:6:27: error: expected type "Standard.Character"
mix_up.adb:6:27: error: found a string type
mix_up.adb:8:21: error: invalid operand types for operator "+"
mix_up.adb:8:21: error: left operand has type "Standard.Integer"
mix_up.adb:8:21: error: right operand has type universal real
mix_up.adb:9:24: error: invalid operand types for operator "&"
gnatmake: "mix_up.adb" compilation error
```

`"True"` is a String, not the Boolean `True`. `"J"` is a String, not the Character `'J'`. `0.5` isn't a whole number, so it can't be added to an Integer: add `1` instead. And `Wheels` needs `'Image` before `&` can join it. Fixed:

mix_up.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Mix_Up is
   Wheels  : Integer   := 18;
   Running : Boolean   := True;
   Initial : Character := 'J';
begin
   Wheels := Wheels + 1;
   Put_Line ("Wheels:" & Wheels'Image);
   Put_Line ("Running? " & Running'Image & ", initial " & Initial);
end Mix_Up;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata mix_up.adb
$ ./mix_up
Wheels: 19
Running? TRUE, initial J
```

Exercise 2.5

#### Pick a type

No code this time. For each piece of information about a delivery truck, choose the best type from this lesson's table, and say why: the number of wheels; the price of a gallon of diesel; whether the engine is running; the license plate; the driver's middle initial; the temperature outside; the dispatch phone number; how many parcels are still on board.

Hint

Ask two questions each time: what values can it take, and what will you do with it? A number you never do maths on is usually text.

Solution

| Information | Type | Why |
| --- | --- | --- |
| Wheels | `Positive` | A whole number, and a truck has at least one |
| Price per gallon | `Float` for now | It has cents. Lesson 11 shows why money is better stored as whole cents |
| Engine running? | `Boolean` | Only two answers: yes or no |
| License plate | `String` | Letters and digits mixed, and you never do maths on it |
| Middle initial | `Character` | Exactly one letter |
| Temperature | `Integer` or `Float` | Can be negative, so not Natural. Float if you need tenths of a degree |
| Phone number | `String` | It's digits, but you never add phone numbers, and a leading 0 would vanish from a number |
| Parcels on board | `Natural` | A whole number that can be 0 at the end of the run, but never negative |

If you picked differently and can say why, that's often fine. Choosing a type is a decision about what the value means, and that's the habit Ada wants you to build.

# 3

## Arithmetic

Ada does maths the way you expect, until you mix types or divide whole numbers.

| Operator | Meaning | Example | Result |
| --- | --- | --- | --- |
| `+ - *` | add, subtract, multiply | `4 * 3` | 12 |
| `/` | divide (with whole numbers, the remainder is dropped) | `7 / 2` | 3 |
| `mod` | remainder, takes the sign of the right side | `-7 mod 2` | 1 |
| `rem` | remainder, takes the sign of the left side | `-7 rem 2` | −1 |
| `**` | power | `2 ** 10` | 1024 |
| `abs` | absolute value | `abs (-5)` | 5 |

Three words you'll see in GNAT's messages. An *expression* is anything that works out to a value: `2 + 3`, `Total_Minutes / 60`, or just `Age`. An *operator* is the symbol that does the work, like `+` or `mod`. The values on each side of it are its *operands*: in `Miles / Days` the operator is `/`, the left operand is `Miles` and the right operand is `Days`.

arithmetic.adb

```ada
with Ada.Text_IO;       use Ada.Text_IO;
with Ada.Float_Text_IO; use Ada.Float_Text_IO;

procedure Arithmetic is
   Total_Minutes : constant Integer := 135;
   Miles         : constant Float   := 312.5;
   Gallons       : constant Float   := 11.2;
   Count         : Integer := 10;
begin
   Put_Line ("135 min =" & Integer'Image (Total_Minutes / 60) & " h"
             & Integer'Image (Total_Minutes mod 60) & " min");
   Put_Line ("7 / 2    =" & Integer'Image (7 / 2));
   Put_Line ("7 rem 2  =" & Integer'Image (7 rem 2));
   Put_Line ("-7 rem 2 =" & Integer'Image ((-7) rem 2));
   Put_Line ("-7 mod 2 =" & Integer'Image ((-7) mod 2));
   Put_Line ("2 ** 10  =" & Integer'Image (2 ** 10));

   Put ("MPG: ");
   Put (Miles / Gallons, Fore => 1, Aft => 2, Exp => 0);
   New_Line;

   Put ("Average of 7 and 2: ");
   Put (Float (7 + 2) / 2.0, Fore => 1, Aft => 1, Exp => 0);
   New_Line;

   Count := @ + 5;   --  Ada 2022: @ means "the variable being assigned"
   Put_Line ("Count:" & Count'Image);
end Arithmetic;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata arithmetic.adb
$ ./arithmetic
135 min = 2 h 15 min
7 / 2    = 3
7 rem 2  = 1
-7 rem 2 =-1
-7 mod 2 = 1
2 ** 10  = 1024
MPG: 27.90
Average of 7 and 2: 4.5
Count: 15
```

`Ada.Float_Text_IO.Put` prints a Float the way people read it. `Fore` is the minimum number of digits before the point, `Aft` the digits after it, and `Exp => 0` turns scientific notation off. Each setting is given by name with `=>`, which reads as "gets": `Aft => 2` means "Aft gets 2". Naming settings means they can't be mixed up. Lesson 9 shows the same thing for procedures you write yourself.

`Count := @ + 5;` uses the Ada 2022 *target name* `@`, which stands for whatever is on the left of `:=`. It saves retyping long names.

### Which part goes first

When an expression has several operators, Ada follows the rules you learned at school: powers first, then multiplying and dividing, then adding and subtracting, and operators of equal rank from left to right. Brackets override all of it.

precedence.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Precedence is
begin
   Put_Line ("2 + 3 * 4   =" & Integer'Image (2 + 3 * 4));
   Put_Line ("(2 + 3) * 4 =" & Integer'Image ((2 + 3) * 4));
   Put_Line ("10 - 4 - 3  =" & Integer'Image (10 - 4 - 3));
   Put_Line ("2 * 3 ** 2  =" & Integer'Image (2 * 3 ** 2));
   Put_Line ("-2 ** 2     =" & Integer'Image (-2 ** 2));
   Put_Line ("(-2) ** 2   =" & Integer'Image ((-2) ** 2));
end Precedence;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata precedence.adb
$ ./precedence
2 + 3 * 4   = 14
(2 + 3) * 4 = 20
10 - 4 - 3  = 3
2 * 3 ** 2  = 18
-2 ** 2     =-4
(-2) ** 2   = 4
```

- `2 + 3 * 4` is 14, because `*` goes before `+`.
- `10 - 4 - 3` works left to right: 6, then 3.
- `-2 ** 2` is −4. The power is worked out first and the minus applied after, as in maths. Write `(-2) ** 2` to square −2.
- When in doubt, add brackets. They cost nothing and make the order obvious to the next reader.

### Dividing whole numbers

When both sides of `/` are whole numbers, Ada does whole-number division, the kind you did at school before decimals: how many times does 2 fit into 7? Three times, with 1 left over. `/` gives you the 3 and `rem` gives you the 1. The left-over part is simply dropped from `/`. Ada doesn't round, so `9 / 10` is 0, even though 0.9 is nearly 1.

**3** full groups of 2: `7 / 2 = 3`**1** left over: `7 rem 2 = 1`

int_div.adb

```ada
with Ada.Text_IO;       use Ada.Text_IO;
with Ada.Float_Text_IO; use Ada.Float_Text_IO;

procedure Int_Div is
   Seven : constant Integer := 7;
   Two   : constant Integer := 2;
   Zero  : Integer := 0;
begin
   Put_Line ("7 / 2   =" & Integer'Image (Seven / Two));
   Put_Line ("7 rem 2 =" & Integer'Image (Seven rem Two));
   Put_Line ("9 / 10  =" & Integer'Image (9 / 10));
   Put_Line ("-7 / 2  =" & Integer'Image ((-Seven) / Two));

   Put ("Float (7 / 2)         = ");
   Put (Float (Seven / Two), Fore => 1, Aft => 1, Exp => 0);
   New_Line;
   Put ("Float (7) / Float (2) = ");
   Put (Float (Seven) / Float (Two), Fore => 1, Aft => 1, Exp => 0);
   New_Line;

   Put_Line ("7 / 0   =" & Integer'Image (Seven / Zero));
end Int_Div;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata int_div.adb
int_div.adb:21:51: warning: division by zero [enabled by default]
int_div.adb:21:51: warning: Constraint_Error will be raised at run time [enabled by default]
$ ./int_div
7 / 2   = 3
7 rem 2 = 1
9 / 10  = 0
-7 / 2  =-3
Float (7 / 2)         = 3.0
Float (7) / Float (2) = 3.5

raised CONSTRAINT_ERROR : int_div.adb:21 divide by zero
```

- `-7 / 2` is −3. Whole-number division chops the fraction off toward zero, whatever the sign.
- `Float (7 / 2)` is 3.0, not 3.5. Ada works from the inside out: `7 / 2` is whole-number division and gives 3, and turning 3 into a Float afterwards is too late. Convert first, `Float (7) / Float (2)`, and the division keeps its fraction: 3.5. This is the most common arithmetic bug for beginners in any language.
- Dividing by zero doesn't give infinity or a garbage number. The program stops with `Constraint_Error` and names the line. GNAT warned at compile time too, because it could see `Zero` was 0; usually it can't.

### rem and mod

Both give a remainder, and for positive numbers they agree. They only differ when a number is negative:

mod_table.adb

```ada
with Ada.Text_IO;         use Ada.Text_IO;
with Ada.Integer_Text_IO; use Ada.Integer_Text_IO;

procedure Mod_Table is
begin
   Put_Line ("   A   A / 3  A rem 3  A mod 3");
   for A in -5 .. 5 loop
      Put (A, Width => 4);
      Put (A / 3, Width => 8);
      Put (A rem 3, Width => 9);
      Put (A mod 3, Width => 9);
      New_Line;
   end loop;
end Mod_Table;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata mod_table.adb
$ ./mod_table
   A   A / 3  A rem 3  A mod 3
  -5      -1       -2        1
  -4      -1       -1        2
  -3      -1        0        0
  -2       0       -2        1
  -1       0       -1        2
   0       0        0        0
   1       0        1        1
   2       0        2        2
   3       1        0        0
   4       1        1        1
   5       1        2        2
```

- `rem` goes with `/`. For any A and B, `(A / B) * B + (A rem B)` equals A again. Its sign follows A, so a negative A gives a negative remainder.
- `mod` always lands between 0 and B − 1 when B is positive. Read down the mod column: 1, 2, 0, 1, 2, 0, straight through zero, like a clock face. Use `mod` for anything that wraps around: hours, days of the week, or checking even and odd when numbers can be negative.

### Mixing types

Many languages quietly turn an Integer into a Float when you mix the two. Ada refuses, because silent conversions hide bugs like `Float (7 / 2)` above. You always decide where a conversion happens:

mix.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Mix is
   Miles   : constant Float   := 312.5;
   Days    : constant Integer := 3;
   Per_Day : constant Float   := Miles / Days;
begin
   Put_Line (Per_Day'Image);
end Mix;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata mix.adb
mix.adb:6:40: error: invalid operand types for operator "/"
mix.adb:6:40: error: left operand has type "Standard.Float"
mix.adb:6:40: error: right operand has type "Standard.Integer"
gnatmake: "mix.adb" compilation error
```

To convert, write the type you want and put the value in brackets after it, like calling a function: `Float (Days)`, `Integer (X)`. Here the fix is `Miles / Float (Days)`.

Converting can change the value, so it's worth knowing exactly how:

conversions.adb

```ada
with Ada.Text_IO;       use Ada.Text_IO;
with Ada.Float_Text_IO; use Ada.Float_Text_IO;

procedure Conversions is
   Whole    : constant Integer := 7;
   Price    : constant Float   := 2.5;
   Negative : Integer := -1;
   Count    : Natural;
begin
   Put ("Float (7)        = ");
   Put (Float (Whole), Fore => 1, Aft => 1, Exp => 0);
   New_Line;
   Put_Line ("Integer (2.4)    =" & Integer (2.4)'Image);
   Put_Line ("Integer (2.5)    =" & Integer (Price)'Image);
   Put_Line ("Integer (2.6)    =" & Integer (2.6)'Image);
   Put_Line ("Integer (-2.5)   =" & Integer (-Price)'Image);
   Put_Line ("Truncation (2.7) =" & Integer (Float'Truncation (2.7))'Image);
   Put_Line ("Floor (-2.5)     =" & Integer (Float'Floor (-Price))'Image);

   Count := Natural (Negative);
   Put_Line ("Count:" & Count'Image);
end Conversions;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata conversions.adb
conversions.adb:20:13: warning: value not in range of type "Standard.Natural" [enabled by default]
conversions.adb:20:13: warning: Constraint_Error will be raised at run time [enabled by default]
$ ./conversions
Float (7)        = 7.0
Integer (2.4)    = 2
Integer (2.5)    = 3
Integer (2.6)    = 3
Integer (-2.5)   =-3
Truncation (2.7) = 2
Floor (-2.5)     =-3

raised CONSTRAINT_ERROR : conversions.adb:20 range check failed
```

- Integer to Float keeps the value: 7 becomes 7.0.
- Float to Integer *rounds* to the nearest whole number. A half rounds away from zero: 2.5 becomes 3 and −2.5 becomes −3. It never just chops off the fraction.
- To chop instead, apply `Float'Truncation` first: 2.7 becomes 2.0, then 2. `Float'Floor` always rounds down, so −2.5 becomes −3.
- Converting into a narrower type is checked. `Natural (-1)` stops the program, because Natural can't hold −1.

Literals are more flexible than variables. A whole-number literal like `2` can become any integer type, and `2.0` any float type, so they fit wherever they're used. But `2` can never be a Float: when the other side is a Float, write `2.0`. GNAT's messages call literals *universal integer* and *universal real*, as in exercise 2.4.

#### Remember

- Whole-number `/` drops the remainder; `rem` gets it
- Convert before dividing: `Float (A) / Float (B)`
- Float to Integer rounds; `Float'Truncation` chops
- `mod` wraps around; `rem` keeps the sign
- `Put (X, Fore => 1, Aft => 2, Exp => 0)`

#### Common mistakes

- `F - 32` when F is a Float: write `32.0`
- `Float (A / B)`: the division happens as whole numbers first
- Expecting `7 / 2` to be 3.5
- Using `rem` to wrap around when numbers can be negative: use `mod`

### Exercises

Exercise 3.1

#### Fahrenheit to Celsius

Convert 98.6 °F to Celsius with C = (F − 32) × 5 ⁄ 9 and print it with one decimal place.

Target output

```
$ ./temp
98.6 F = 37.0 C
```

Hint

Every number in the formula must be a Float literal. Print with `Fore => 1, Aft => 1, Exp => 0`.

Solution

temp.adb

```ada
with Ada.Text_IO;       use Ada.Text_IO;
with Ada.Float_Text_IO; use Ada.Float_Text_IO;

procedure Temp is
   F : constant Float := 98.6;
   C : constant Float := (F - 32.0) * 5.0 / 9.0;
begin
   Put (F, Fore => 1, Aft => 1, Exp => 0);
   Put (" F = ");
   Put (C, Fore => 1, Aft => 1, Exp => 0);
   Put_Line (" C");
end Temp;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata temp.adb
$ ./temp
98.6 F = 37.0 C
```

Exercise 3.2

#### Making change

Given 87 cents, work out the fewest quarters, dimes, nickels and pennies.

Target output

```
$ ./change
Change for 87 cents:
  Quarters: 3
  Dimes:    1
  Nickels:  0
  Pennies:  2
```

Hint

`Left / 25` is how many quarters fit; `Left mod 25` is what's left over. Repeat down the coins.

Solution

change.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Change is
   Cents : constant Integer := 87;
   Left  : Integer := Cents;
   Quarters, Dimes, Nickels : Integer;
begin
   Quarters := Left / 25;
   Left     := Left mod 25;
   Dimes    := Left / 10;
   Left     := Left mod 10;
   Nickels  := Left / 5;
   Left     := Left mod 5;
   Put_Line ("Change for" & Cents'Image & " cents:");
   Put_Line ("  Quarters:" & Quarters'Image);
   Put_Line ("  Dimes:   " & Dimes'Image);
   Put_Line ("  Nickels: " & Nickels'Image);
   Put_Line ("  Pennies: " & Left'Image);
end Change;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata change.adb
$ ./change
Change for 87 cents:
  Quarters: 3
  Dimes:    1
  Nickels:  0
  Pennies:  2
```

Exercise 3.3

#### Seconds to a clock

Turn 7,384 seconds into hours, minutes and seconds.

Target output

```
$ ./clock
 7384 seconds is 2 h 3 min 4 s
```

Hint

There are 3,600 seconds in an hour. The minutes come from what's left after the hours.

Solution

clock.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Clock is
   Total   : constant Integer := 7_384;
   Hours   : constant Integer := Total / 3_600;
   Minutes : constant Integer := (Total mod 3_600) / 60;
   Seconds : constant Integer := Total mod 60;
begin
   Put_Line (Total'Image & " seconds is" & Hours'Image & " h"
             & Minutes'Image & " min" & Seconds'Image & " s");
end Clock;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata clock.adb
$ ./clock
 7384 seconds is 2 h 3 min 4 s
```

Exercise 3.4

#### Judges' average

Three judges give scores of 7, 8 and 8, stored as Integers. Print their average twice: once as a whole number, and once to two decimal places.

Target output

```
$ ./average_score
Whole-number average: 7
Real average: 7.67
```

Hint

For the second, turn the total into a Float *before* you divide, and divide by `3.0`, not `3`.

Solution

average_score.adb

```ada
with Ada.Text_IO;       use Ada.Text_IO;
with Ada.Float_Text_IO; use Ada.Float_Text_IO;

procedure Average_Score is
   A : constant Integer := 7;
   B : constant Integer := 8;
   C : constant Integer := 8;
begin
   Put_Line ("Whole-number average:" & Integer'Image ((A + B + C) / 3));
   Put ("Real average: ");
   Put (Float (A + B + C) / 3.0, Fore => 1, Aft => 2, Exp => 0);
   New_Line;
end Average_Score;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata average_score.adb
$ ./average_score
Whole-number average: 7
Real average: 7.67
```

The whole-number average is 7 because 23 / 3 drops the remainder of 2. Writing `Float ((A + B + C) / 3)` would also print 7.00: the division would already be done.

Exercise 3.5

#### Predict the results

Work out what each line prints before you run it. Every line uses a rule from this lesson.

predict_math.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Predict_Math is
begin
   Put_Line (Integer'Image (17 / 5));
   Put_Line (Integer'Image (17 rem 5));
   Put_Line (Integer'Image ((-17) / 5));
   Put_Line (Integer'Image ((-17) rem 5));
   Put_Line (Integer'Image ((-17) mod 5));
   Put_Line (Integer'Image (Integer (17.0 / 2.0)));
end Predict_Math;
```

Hint

Whole-number division chops toward zero. `rem` takes the sign of the left number, `mod` lands between 0 and 4. Converting a Float to an Integer rounds, and a half rounds away from zero.

Solution

Terminal

```
$ gnatmake -q -gnat2022 -gnata predict_math.adb
$ ./predict_math
 3
 2
-3
-2
 3
 9
```

5 fits into 17 three times with 2 left over. −17 / 5 chops toward zero to −3, and its `rem` keeps the minus sign: −2. `mod` must land between 0 and 4, and −17 = −4 × 5 + 3, so it's 3. 17.0 / 2.0 is 8.5, and converting a half rounds away from zero, to 9.

# 4

## Reading what the user types

Programs get interesting when they react to input. In the terminals below, what the user typed is underlined.

ask.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Ask is
begin
   Put ("What's your name? ");
   declare
      Name : constant String := Get_Line;
   begin
      Put ("How old are you? ");
      declare
         Age : constant Integer := Integer'Value (Get_Line);
      begin
         Put_Line ("Hi " & Name & ". In ten years you'll be"
                   & Integer'Image (Age + 10) & ".");
      end;
   end;
end Ask;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata ask.adb
$ ./ask
What's your name? Ada
How old are you? 36
Hi Ada. In ten years you'll be 46.
```

Two new things here: `Get_Line` reads what the user types, and the `declare ... begin ... end;` blocks around it make a place to keep the answer. `Get_Line` first.

`Get_Line` waits for the user to press Enter and returns everything they typed, as a String. It could be 3 characters or 300, and nobody knows which until it's been typed. That's a problem, because of a rule from lesson 2: an Ada String has a fixed length, set when the String is created.

### The problem declare solves

The usual place to create variables is between `is` and `begin`. But that happens as soon as the procedure starts, before anyone has typed anything. Try it, and Ada wants the length up front:

no_size.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure No_Size is
   Name : String;
begin
   Put ("What's your name? ");
   Name := Get_Line;
   Put_Line ("Hi " & Name);
end No_Size;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata no_size.adb
no_size.adb:4:11: error: unconstrained subtype not allowed (need initialization)
no_size.adb:4:11: error: provide initial value or explicit array bounds
gnatmake: "no_size.adb" compilation error
```

Give it a length and it compiles, but now only an answer exactly 10 characters long fits:

fixed_size.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Fixed_Size is
   Name : String (1 .. 10);
begin
   Put ("What's your name? ");
   Name := Get_Line;
   Put_Line ("Hi " & Name);
end Fixed_Size;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata fixed_size.adb
$ ./fixed_size
What's your name? Ada


raised CONSTRAINT_ERROR : fixed_size.adb:7 length check failed
```

What's needed is to create the String *after* the answer exists, already the right length. That's what a `declare` block does.

### What a declare block is

A declare block is a small box you can open anywhere a statement could go. It has its own declarations and its own statements:

| Part | What goes there |
| --- | --- |
| `declare` | Starts the block |
| declarations | New constants and variables, written just like the ones before a procedure's `begin` |
| `begin` | Ends the declarations and starts the statements |
| statements | The work, which can use the new names |
| `end;` | Closes the block. Its names disappear here |

declare_demo.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Declare_Demo is
   Greeting : constant String := "Hi ";   --  made before begin
begin
   Put ("What's your name? ");
   declare
      Answer : constant String := Get_Line;   --  made now, as long as the answer
   begin
      Put_Line (Greeting & Answer & "!");
      Put_Line ("That's" & Answer'Length'Image & " characters.");
   end;                                     --  Answer is gone after this line
   Put_Line ("Bye.");
end Declare_Demo;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata declare_demo.adb
$ ./declare_demo
What's your name? Ada
Hi Ada!
That's 3 characters.
Bye.
$ ./declare_demo
What's your name? Grace Hopper
Hi Grace Hopper!
That's 12 characters.
Bye.
```

- **Timing.** Declarations are carried out when the program reaches them, top to bottom, just like statements. `Greeting` is made when `Declare_Demo` starts. `Answer` isn't made until the program reaches the `declare`, after the question has been printed and answered. By then `Get_Line` has the finished answer, so `Answer` is created exactly that long: 3 characters in the first run, 12 in the second.
- **Length from the starting value.** A String declared with a starting value takes its length from that value, so you never write the length yourself. `Answer` is also a `constant`, simply because it never needs to change.
- **Scope.** A name declared in a block exists only until that block's `end;`. After it, `Answer` is gone:

out_of_scope.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Out_Of_Scope is
begin
   Put ("What's your name? ");
   declare
      Answer : constant String := Get_Line;
   begin
      Put_Line ("Hi " & Answer & "!");
   end;
   Put_Line ("Bye, " & Answer);
end Out_Of_Scope;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata out_of_scope.adb
out_of_scope.adb:11:24: error: "Answer" is undefined
gnatmake: "out_of_scope.adb" compilation error
```

The part of a program where a name can be used is called its *scope*. Small scopes are a feature, not a nuisance: each name lives right where it's used, nothing later on can pick up a leftover value by mistake, and the memory is handed back when the block ends.

These examples say `Answer` rather than `Name` for a reason: `Ada.Text_IO` has its own `Name`, a function that gives a file's name. Once a block's `Name` was gone, GNAT would find that one instead and complain about a missing argument, which is a far more confusing message.

### Why ask.adb has one block inside another

Declarations can only create things; they can't print a question. In `ask.adb`, the age question has to be printed after the name is read and before the age is read, so the age gets its own block, opened after that `Put`. Putting the second block *inside* the first keeps `Name` alive for the last `Put_Line`, which uses both:

`procedure Ask`

no names of its own

`first declare block`

`Name`: made after the name question is answered

`second declare block`

`Age`: made after the age question is answered. Can also see `Name`, from the box around it

Inner blocks can see the names in the blocks around them; outer blocks can't see inside. Exercise 4.4 shows what goes wrong when two blocks sit side by side instead.

### Turning text into numbers

`Integer'Value` goes the other way from `'Image`: it turns text like `"36"` into the number 36. `Float'Value` does the same for Floats. If the text isn't a number, the program stops with `Constraint_Error`; lesson 13 shows how to catch that and ask again.

When you only need a number, you don't need a block at all: `A := Integer'Value (Get_Line);`. An Integer is the same size whatever its value, so it can be declared up front like any other variable. Only text has the length problem.

Lesson 10 shows the other way to hold text of unknown length: `Unbounded_String`, a string that can grow and shrink.

#### Remember

- `Get_Line` returns the typed line as a String
- `declare ... begin ... end;` creates names in the middle of a program
- Declarations happen when the program reaches them
- A block's names disappear at its `end;`
- `Integer'Value (S)` turns text into a number

#### Common mistakes

- `Name : String;` with no length or starting value
- `String (1 .. 10) := Get_Line`: only a 10-character answer fits
- Using a block's name after its `end;`
- Forgetting to print a prompt with `Put`, so the user doesn't know to type

### Exercises

Exercise 4.1

#### Two-number calculator

Ask for two whole numbers and print their sum, difference and product.

Target output, with typed input underlined

```
$ ./calc
First number: 12
Second number: 5
Sum:        17
Difference: 7
Product:    60
```

Hint

Read each with `Integer'Value (Get_Line)`.

Solution

calc.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Calc is
   A, B : Integer;
begin
   Put ("First number: ");
   A := Integer'Value (Get_Line);
   Put ("Second number: ");
   B := Integer'Value (Get_Line);
   Put_Line ("Sum:       " & Integer'Image (A + B));
   Put_Line ("Difference:" & Integer'Image (A - B));
   Put_Line ("Product:   " & Integer'Image (A * B));
end Calc;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata calc.adb
$ ./calc
First number: 12
Second number: 5
Sum:        17
Difference: 7
Product:    60
```

Exercise 4.2

#### Fuel economy

Ask how many miles were driven and how many gallons were used, then print miles per gallon to one decimal.

Target output, with typed input underlined

```
$ ./fuel
Miles driven: 312.5
Gallons used: 11.2
That's 27.9 miles per gallon.
```

Hint

Use `Float'Value` and `Ada.Float_Text_IO.Put` from lesson 3.

Solution

fuel.adb

```ada
with Ada.Text_IO;       use Ada.Text_IO;
with Ada.Float_Text_IO; use Ada.Float_Text_IO;

procedure Fuel is
   Miles, Gallons : Float;
begin
   Put ("Miles driven: ");
   Miles := Float'Value (Get_Line);
   Put ("Gallons used: ");
   Gallons := Float'Value (Get_Line);
   Put ("That's ");
   Put (Miles / Gallons, Fore => 1, Aft => 1, Exp => 0);
   Put_Line (" miles per gallon.");
end Fuel;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata fuel.adb
$ ./fuel
Miles driven: 312.5
Gallons used: 11.2
That's 27.9 miles per gallon.
```

Exercise 4.3

#### Name tag

Ask for a first name, then a last name, and print them as "Last, First" followed by the total number of characters, counting the comma and the space.

Target output, with typed input underlined

```
$ ./name_tag
First name? Ada
Last name? Lovelace
Lovelace, Ada
That's 13 characters with the comma and space.
```

Hint

The last-name question has to be printed after the first name is read, so you need a block inside a block, like `ask.adb`. `'Length` gives a String's length.

Solution

name_tag.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Name_Tag is
begin
   Put ("First name? ");
   declare
      First : constant String := Get_Line;
   begin
      Put ("Last name? ");
      declare
         Last : constant String := Get_Line;
      begin
         Put_Line (Last & ", " & First);
         Put_Line ("That's" & Integer'Image (Last'Length + First'Length + 2)
                   & " characters with the comma and space.");
      end;
   end;
end Name_Tag;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata name_tag.adb
$ ./name_tag
First name? Ada
Last name? Lovelace
Lovelace, Ada
That's 13 characters with the comma and space.
```

Exercise 4.4

#### Side by side

This program won't compile. Find out why from GNAT's message, and fix it without changing what it prints.

Target output, with typed input underlined

```
$ ./order
Item: bolts
Quantity: 40
Ordered 40 x bolts
```

order.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Order is
begin
   Put ("Item: ");
   declare
      Item : constant String := Get_Line;
   begin
      Put ("Quantity: ");
   end;
   declare
      Qty : constant Integer := Integer'Value (Get_Line);
   begin
      Put_Line ("Ordered" & Qty'Image & " x " & Item);
   end;
end Order;
```

Hint

Where does `Item`'s scope end? Can the second block see it from there?

Solution

Terminal

```
$ gnatmake -q -gnat2022 -gnata order.adb
order.adb:14:49: error: "Item" is undefined
gnatmake: "order.adb" compilation error
```

The two blocks sit side by side, so `Item` is gone by the time the second block runs. Moving the second block inside the first, after the quantity prompt, keeps `Item` alive:

order.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Order is
begin
   Put ("Item: ");
   declare
      Item : constant String := Get_Line;
   begin
      Put ("Quantity: ");
      declare
         Qty : constant Integer := Integer'Value (Get_Line);
      begin
         Put_Line ("Ordered" & Qty'Image & " x " & Item);
      end;
   end;
end Order;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata order.adb
$ ./order
Item: bolts
Quantity: 40
Ordered 40 x bolts
```

# 5

## Making decisions

if, elsif, else and case let a program take different paths.

decide.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Decide is
   Score : constant Integer := 83;
   Day   : constant Integer := 6;
begin
   if Score >= 90 then
      Put_Line ("Grade: A");
   elsif Score >= 80 then
      Put_Line ("Grade: B");
   elsif Score >= 70 then
      Put_Line ("Grade: C");
   else
      Put_Line ("Grade: F");
   end if;

   if Score >= 0 and then Score <= 100 then
      Put_Line ("Score is valid");
   end if;

   case Day is
      when 1 .. 5 => Put_Line ("Weekday");
      when 6 | 7  => Put_Line ("Weekend");
      when others => Put_Line ("Not a day number");
   end case;

   Put_Line ("Result: " & (if Score >= 70 then "pass" else "fail"));
end Decide;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata decide.adb
$ ./decide
Grade: B
Score is valid
Weekend
Result: pass
```

A *condition* is an expression whose value is `True` or `False`, like `Score >= 90`. An `if` checks its conditions in order and runs the first *branch* (the statements under a condition) whose condition is true. `else` catches everything else. Only one branch ever runs. Compare with `= /= < <= > >=`; note that "not equal" is `/=`.

### and, or, not

Conditions combine with `and`, `or` and `not`. This table shows every case:

| A | B | A and B | A or B | not A |
| --- | --- | --- | --- | --- |
| True | True | True | True | False |
| True | False | False | True | False |
| False | True | False | True | True |
| False | False | False | False | True |

`and` is only True when both sides are. `or` is True when at least one side is. `not` flips True and False.

`and` always works out both sides. `and then` stops as soon as the left side is False, because the answer is already known, so the right side never runs. That matters when the right side is only safe if the left side is true:

short_circuit.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Short_Circuit is
   Total : constant Integer := 50;
   Count : Integer := 0;
begin
   if Count /= 0 and then Total / Count > 10 then
      Put_Line ("Average is above 10");
   else
      Put_Line ("and then: the division was skipped");
   end if;

   if Count /= 0 and Total / Count > 10 then
      Put_Line ("Average is above 10");
   end if;
   Put_Line ("You will never see this line.");
end Short_Circuit;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata short_circuit.adb
short_circuit.adb:13:30: warning: division by zero [enabled by default]
short_circuit.adb:13:30: warning: Constraint_Error will be raised at run time [enabled by default]
$ ./short_circuit
and then: the division was skipped

raised CONSTRAINT_ERROR : short_circuit.adb:13 divide by zero
```

`or else` is the same idea for `or`: it stops as soon as the left side is True.

### Ada makes you bracket mixed logic

Does `A and B or C` mean `(A and B) or C` or `A and (B or C)`? Instead of a rule to memorize, Ada rejects it:

and_or.adb

```ada
procedure And_Or is
   Year    : constant Integer := 2028;
   Is_Leap : Boolean;
begin
   Is_Leap := Year mod 4 = 0 and Year mod 100 /= 0 or Year mod 400 = 0;
end And_Or;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata and_or.adb
and_or.adb:5:52: error: mixed logical operators in expression
gnatmake: "and_or.adb" compilation error
```

### case

A `case` picks a branch by comparing one value against lists of *choices*. A choice can be a single value, a range (`1 .. 5` means 1 to 5, both included) or alternatives (`6 | 7`, read "6 or 7"). The compiler checks that every possible value is covered, and an Integer has over four billion of them:

case_missing.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Case_Missing is
   Day : constant Integer := 6;
begin
   case Day is
      when 1 .. 5 => Put_Line ("Weekday");
      when 6 | 7  => Put_Line ("Weekend");
   end case;
end Case_Missing;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata case_missing.adb
case_missing.adb:6:04: error: missing case values: -16#8000_0000# .. 0
case_missing.adb:6:04: error: missing case values: 8 .. 16#7FFF_FFFF#
gnatmake: "case_missing.adb" compilation error
```

GNAT lists the gaps. The odd-looking numbers are written in base 16, Ada's notation for numbers in other bases: `16#8000_0000#` is the smallest Integer, −2,147,483,648, and `16#7FFF_FFFF#` is the largest. The usual fix is a last choice of `when others =>`, which catches every value not already listed.

### Statements and expressions

Everything above is a *statement*: it does something. `(if Score >= 70 then "pass" else "fail")` is different. It's an *if expression*, which works out to a value, here the String "pass" or "fail", so it can go anywhere a value can, such as inside a `Put_Line`. Because it has to produce a value, it needs an `else` (Booleans are the one exception, and can wait). There are case expressions too; the gradebook in lesson 15 uses one.

#### Remember

- A condition is an expression that's True or False
- `and then` skips the right side when the left is False
- `if ... then ... elsif ... else ... end if;`
- `case X is when ... => ... end case;`
- Not equal is `/=`

#### Common mistakes

- `=` where you meant `:=` (or the other way round)
- Writing `else if`; Ada's word is `elsif`
- A `case` that misses values and has no `when others`

### Exercises

Exercise 5.1

#### Even or odd

Read a whole number and say whether it is even or odd. Test it with a negative number too.

Target output, with typed input underlined

```
$ ./even_odd
Number: 7
 7 is odd
$ ./even_odd
Number: -4
-4 is even
```

Hint

A number is even when `N mod 2 = 0`.

Solution

even_odd.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Even_Odd is
   N : Integer;
begin
   Put ("Number: ");
   N := Integer'Value (Get_Line);
   if N mod 2 = 0 then
      Put_Line (N'Image & " is even");
   else
      Put_Line (N'Image & " is odd");
   end if;
end Even_Odd;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata even_odd.adb
$ ./even_odd
Number: 7
 7 is odd
$ ./even_odd
Number: -4
-4 is even
```

Exercise 5.2

#### Largest of three

Read three numbers and print the largest.

Target output, with typed input underlined

```
$ ./largest
A: 4
B: 19
C: 11
Largest: 19
```

Hint

Assume the first is the biggest, then let each of the others challenge it.

Solution

largest.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Largest is
   A, B, C, Big : Integer;
begin
   Put ("A: ");
   A := Integer'Value (Get_Line);
   Put ("B: ");
   B := Integer'Value (Get_Line);
   Put ("C: ");
   C := Integer'Value (Get_Line);

   Big := A;
   if B > Big then
      Big := B;
   end if;
   if C > Big then
      Big := C;
   end if;
   Put_Line ("Largest:" & Big'Image);
end Largest;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata largest.adb
$ ./largest
A: 4
B: 19
C: 11
Largest: 19
```

Once you know about attributes, `Integer'Max (A, Integer'Max (B, C))` does the same in one line.

Exercise 5.3

#### Leap year

A year is a leap year if it divides by 4, except century years, which must divide by 400. Read a year and say which it is. 1900 wasn't, 2000 was.

Target output, with typed input underlined

```
$ ./leap
Year: 1900
 1900 is not a leap year
$ ./leap
Year: 2000
 2000 is a leap year
$ ./leap
Year: 2028
 2028 is a leap year
```

Hint

Divisible by 4 and not by 100, or divisible by 400. Remember the brackets.

Solution

leap.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Leap is
   Year    : Integer;
   Is_Leap : Boolean;
begin
   Put ("Year: ");
   Year := Integer'Value (Get_Line);
   Is_Leap := (Year mod 4 = 0 and Year mod 100 /= 0) or Year mod 400 = 0;
   Put_Line (Year'Image & (if Is_Leap then " is" else " is not")
             & " a leap year");
end Leap;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata leap.adb
$ ./leap
Year: 1900
 1900 is not a leap year
$ ./leap
Year: 2000
 2000 is a leap year
$ ./leap
Year: 2028
 2028 is a leap year
```

# 6

## Loops

Repeating work is what computers are for. Ada has three loop shapes.

loops.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Loops is
   N   : Integer := 1;
   Sum : Integer := 0;
begin
   for I in 1 .. 5 loop
      Put (I'Image);
   end loop;
   New_Line;

   for I in reverse 1 .. 5 loop
      Put (I'Image);
   end loop;
   New_Line;

   while N < 100 loop
      N := N * 2;
   end loop;
   Put_Line ("First power of 2 past 100:" & N'Image);

   loop
      Sum := Sum + 7;
      exit when Sum > 30;
   end loop;
   Put_Line ("Sum:" & Sum'Image);
end Loops;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata loops.adb
$ ./loops
 1 2 3 4 5
 5 4 3 2 1
First power of 2 past 100: 128
Sum: 35
```

- `for I in 1 .. 5 loop` counts through a *range*: `A .. B` means every whole value from A up to B, both included. Each trip through the loop's statements is one *iteration*; this loop makes five. The loop creates `I` itself, and you can't change it. `I` also only exists inside the loop: the same scope rule as lesson 4's blocks. Add `reverse` to count down.
- `while Condition loop` checks before each pass and stops when the condition turns false.
- A bare `loop` runs until an `exit when` fires. Use it when the test belongs in the middle, such as "read a line, stop if it's blank, otherwise process it".

A range only counts up. If A is bigger than B, the range is empty and the loop runs zero times, with no error. To count down, keep the smaller number first and add `reverse`:

empty_range.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Empty_Range is
begin
   Put ("for I in 5 .. 1:        ");
   for I in 5 .. 1 loop
      Put (I'Image);
   end loop;
   New_Line;

   Put ("for I in reverse 1 .. 5:");
   for I in reverse 1 .. 5 loop
      Put (I'Image);
   end loop;
   New_Line;
end Empty_Range;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata empty_range.adb
empty_range.adb:6:15: warning: loop range is null, loop will not execute [enabled by default]
$ ./empty_range
for I in 5 .. 1:        
for I in reverse 1 .. 5: 5 4 3 2 1
```

GNAT noticed the empty range and warned, but the program still compiled and ran. An empty loop is legal; it's just almost never what you meant.

A loop that never meets its stopping condition runs forever. Press Ctrl+C in the terminal to stop it.

Loops can sit inside loops. `Ada.Integer_Text_IO.Put (X, Width => 4)` prints a number padded to 4 characters, which keeps columns straight. Exercise 6.3 uses both.

#### Remember

- `A .. B` includes both ends; empty if A > B
- `for I in A .. B loop`
- `while C loop`
- `loop ... exit when C; ... end loop;`

#### Common mistakes

- A while loop whose variable never changes: it runs forever
- Off-by-one: `1 .. 10` includes both 1 and 10
- Trying to assign to the `for` loop's counter

### Exercises

Exercise 6.1

#### Add up 1 to 100

Use a loop to add every whole number from 1 to 100.

Target output

```
$ ./sum_100
1 + 2 + ... + 100 = 5050
```

Hint

Start a total at 0 and add the loop counter to it each pass. The answer is 5050.

Solution

sum_100.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Sum_100 is
   Total : Integer := 0;
begin
   for I in 1 .. 100 loop
      Total := @ + I;
   end loop;
   Put_Line ("1 + 2 + ... + 100 =" & Total'Image);
end Sum_100;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata sum_100.adb
$ ./sum_100
1 + 2 + ... + 100 = 5050
```

Exercise 6.2

#### FizzBuzz

For 1 to 15: print Fizz for multiples of 3, Buzz for multiples of 5, FizzBuzz for multiples of both, otherwise the number. It's a classic job-interview question.

Target output

```
$ ./fizz_buzz
 1
 2
Fizz
 4
Buzz
Fizz
 7
 8
Fizz
Buzz
 11
Fizz
 13
 14
FizzBuzz
```

Hint

Test for 15 first. If you test 3 first, 15 prints Fizz and never reaches the FizzBuzz branch.

Solution

fizz_buzz.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Fizz_Buzz is
begin
   for I in 1 .. 15 loop
      if I mod 15 = 0 then
         Put_Line ("FizzBuzz");
      elsif I mod 3 = 0 then
         Put_Line ("Fizz");
      elsif I mod 5 = 0 then
         Put_Line ("Buzz");
      else
         Put_Line (I'Image);
      end if;
   end loop;
end Fizz_Buzz;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata fizz_buzz.adb
$ ./fizz_buzz
 1
 2
Fizz
 4
Buzz
Fizz
 7
 8
Fizz
Buzz
 11
Fizz
 13
 14
FizzBuzz
```

The numbers carry `'Image`'s leading space. Lesson 10's `Trim` removes it.

Exercise 6.3

#### Times table

Print a 5 × 5 multiplication table with the columns lined up.

Target output

```
$ ./times_table
   1   2   3   4   5
   2   4   6   8  10
   3   6   9  12  15
   4   8  12  16  20
   5  10  15  20  25
```

Hint

One loop for rows, another inside it for columns. `New_Line` after each row.

Solution

times_table.adb

```
with Ada.Text_IO;         use Ada.Text_IO;
with Ada.Integer_Text_IO; use Ada.Integer_Text_IO;

procedure Times_Table is
begin
   for Row in 1 .. 5 loop
      for Col in 1 .. 5 loop
         Put (Row * Col, Width => 4);
      end loop;
      New_Line;
   end loop;
end Times_Table;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata times_table.adb
$ ./times_table
   1   2   3   4   5
   2   4   6   8  10
   3   6   9  12  15
   4   8  12  16  20
   5  10  15  20  25
```

Exercise 6.4

#### Hailstone numbers

Start at 27. If the number is even, halve it; if odd, triple it and add 1. Count the steps until it reaches 1, and track the highest value it hits on the way.

Target output

```
$ ./hailstone
27 reaches 1 after 111 steps
Highest value along the way: 9232
```

Hint

You don't know how many steps there will be, so use `while N /= 1 loop`.

Solution

hailstone.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Hailstone is
   N     : Integer := 27;
   Steps : Integer := 0;
   Peak  : Integer := N;
begin
   while N /= 1 loop
      N := (if N mod 2 = 0 then N / 2 else 3 * N + 1);
      Steps := @ + 1;
      Peak  := Integer'Max (Peak, N);
   end loop;
   Put_Line ("27 reaches 1 after" & Steps'Image & " steps");
   Put_Line ("Highest value along the way:" & Peak'Image);
end Hailstone;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata hailstone.adb
$ ./hailstone
27 reaches 1 after 111 steps
Highest value along the way: 9232
```

# 7

## Making your own types

Ada's strongest idea: describe what values make sense, and let the compiler and runtime enforce it.

my_types.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure My_Types is
   type Day is (Mon, Tue, Wed, Thu, Fri, Sat, Sun);
   subtype Weekend is Day range Sat .. Sun;
   type Percent is range 0 .. 100;

   Today   : constant Day := Fri;
   Battery : Percent := 95;
begin
   Put_Line ("Today is " & Today'Image);
   Put_Line ("Tomorrow is " & Day'Image (Day'Succ (Today)));
   Put_Line ("Fri is at position" & Day'Pos (Fri)'Image);
   Put_Line ("Is Sat a weekend day? " & Boolean'Image (Sat in Weekend));

   for D in Day loop
      Put (D'Image & " ");
   end loop;
   New_Line;

   Battery := Battery + 5;
   Put_Line ("Battery:" & Battery'Image & "%");
   Battery := Battery + 1;
   Put_Line ("You will never see this line.");
end My_Types;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata my_types.adb
$ ./my_types
Today is FRI
Tomorrow is SAT
Fri is at position 4
Is Sat a weekend day? TRUE
MON TUE WED THU FRI SAT SUN 
Battery: 100%

raised CONSTRAINT_ERROR : my_types.adb:23 range check failed
```

`type Day is (Mon, Tue, ...)` is an *enumeration*: a type whose values are names you choose. You can loop over it, compare values, and ask for `Day'First`, `Day'Last`, `Day'Succ (D)` (next), `Day'Pred (D)` (previous) and `Day'Pos (D)` (position, from 0).

`type Percent is range 0 .. 100` makes an integer type that can only hold 0 to 100. When the program tried to store 101, it stopped with `Constraint_Error` and named the exact line. In many languages that bug would slip through and corrupt data later.

### subtype or new type?

Both of these hold 0 to 100, but they behave very differently:

subtype_vs_type.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Subtype_Vs_Type is
   subtype Score   is Integer range 0 .. 100;
   type    Percent is range 0 .. 100;

   Count  : Integer := 40;
   Mark   : constant Score   := 75;
   Charge : constant Percent := 75;
begin
   Count := Count + Mark;     --  fine: a Score is still an Integer
   Count := Count + Charge;   --  refused: Percent is a type of its own
   Put_Line (Count'Image);
end Subtype_Vs_Type;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata subtype_vs_type.adb
subtype_vs_type.adb:12:19: error: invalid operand types for operator "+"
subtype_vs_type.adb:12:19: error: left operand has type "Standard.Integer"
subtype_vs_type.adb:12:19: error: right operand has type "Percent" defined at line 5
gnatmake: "subtype_vs_type.adb" compilation error
```

A `subtype` is the same type with an extra rule. A `Score` *is* an Integer that must stay between 0 and 100, so it mixes freely with other Integers, and the range is checked whenever a value goes into it. `Natural` and `Positive` are subtypes: `subtype Natural is Integer range 0 .. Integer'Last`.

A `type` declaration makes a brand-new type. `Percent` holds the same numbers, but Ada treats it as unrelated to Integer, so mixing them is refused until you convert on purpose: `Count + Integer (Charge)`. `is new` does the same starting from an existing type:

units.adb

```ada
procedure Units is
   type Celsius    is new Float;
   type Fahrenheit is new Float;
   Inside  : Celsius    := 21.0;
   Outside : constant Fahrenheit := 14.0;
begin
   Inside := Outside;
end Units;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata units.adb
units.adb:7:14: error: expected type "Celsius" defined at line 2
units.adb:7:14: error: found type "Fahrenheit" defined at line 3
gnatmake: "units.adb" compilation error
```

| Use | When | For example |
| --- | --- | --- |
| `subtype` | The values are the same kind of thing, just limited | An exam score is still a number of points |
| new `type` | Mixing them up would be a bug | Celsius and Fahrenheit, meters and feet, dollars and euros |

That's how you stop someone adding Celsius to Fahrenheit: the compiler refuses.

#### Remember

- `type T is (A, B, C);` enumeration
- `type T is range 1 .. 6;` limited integers
- `subtype S is T range ...;` narrows, still mixes
- `'First 'Last 'Succ 'Pred 'Pos`

#### Common mistakes

- `Day'Succ (Sun)`: there's nothing after the last value, so it raises Constraint_Error
- Expecting two different `is new` types to mix: convert explicitly

### Exercises

Exercise 7.1

#### Months that wrap

Make an enumeration of the 12 months. Starting at October, print five months in a row, wrapping from December back to January.

Target output

```
$ ./months
OCT NOV DEC JAN FEB 
```

Hint

`Month'Succ (Dec)` fails. Check for `Month'Last` first and jump to `Month'First`.

Solution

months.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Months is
   type Month is (Jan, Feb, Mar, Apr, May, Jun,
                  Jul, Aug, Sep, Oct, Nov, Dec);
   M : Month := Oct;
begin
   for I in 1 .. 5 loop
      Put (M'Image & " ");
      M := (if M = Month'Last then Month'First else Month'Succ (M));
   end loop;
   New_Line;
end Months;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata months.adb
$ ./months
OCT NOV DEC JAN FEB 
```

Exercise 7.2

#### Rolling sevens

Make `type Die is range 1 .. 6`. Loop over every pair of dice and count how many of the 36 pairs add to 7.

Target output

```
$ ./dice
Sevens: 6 out of 36 rolls
```

Hint

Two nested loops over `Die`. Convert to Integer before adding, since 7 isn't a valid Die value.

Solution

dice.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Dice is
   type Die is range 1 .. 6;
   Sevens : Integer := 0;
   Rolls  : Integer := 0;
begin
   for A in Die loop
      for B in Die loop
         Rolls := @ + 1;
         if Integer (A) + Integer (B) = 7 then
            Sevens := @ + 1;
         end if;
      end loop;
   end loop;
   Put_Line ("Sevens:" & Sevens'Image & " out of" & Rolls'Image & " rolls");
end Dice;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata dice.adb
$ ./dice
Sevens: 6 out of 36 rolls
```

Exercise 7.3

#### Traffic light

Make a `Light` type with Green, Yellow and Red. Run six ticks of a traffic light, printing what each color means and moving to the next color.

Target output

```
$ ./traffic
Red: stop
Green: go
Yellow: slow down
Red: stop
Green: go
Yellow: slow down
```

Hint

A `case` on the light, with each branch printing and setting the next color.

Solution

traffic.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Traffic is
   type Light is (Green, Yellow, Red);
   Now : Light := Red;
begin
   for Tick in 1 .. 6 loop
      case Now is
         when Green =>
            Put_Line ("Green: go");
            Now := Yellow;
         when Yellow =>
            Put_Line ("Yellow: slow down");
            Now := Red;
         when Red =>
            Put_Line ("Red: stop");
            Now := Green;
      end case;
   end loop;
end Traffic;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata traffic.adb
$ ./traffic
Red: stop
Green: go
Yellow: slow down
Red: stop
Green: go
Yellow: slow down
```

The compiler checks that the `case` covers all three colors. Add a fourth color to the type and it will point at this `case` until you handle it.

# 8

## Arrays

An array holds many values of one type, side by side, each with an index.

arrays.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Arrays is
   type Score_List is array (Positive range <>) of Integer;

   Scores : Score_List := [72, 95, 88, 61, 79];
   Total  : Integer := 0;
begin
   Put_Line ("How many:" & Scores'Length'Image);
   Put_Line ("First:   " & Scores (1)'Image);
   Put_Line ("Last:    " & Scores (Scores'Last)'Image);

   for S of Scores loop
      Total := @ + S;
   end loop;
   Put_Line ("Total:" & Total'Image);
   Put_Line ("Average:" & Integer'Image (Total / Scores'Length));

   Scores (2) := 100;
   for I in Scores'Range loop
      Put_Line ("  #" & I'Image & " =" & Scores (I)'Image);
   end loop;

   Put_Line ("Whole array: " & Scores'Image);
   Put_Line ("Sum with 'Reduce:" & Integer'Image (Scores'Reduce ("+", 0)));

   Put ("80 or more:");
   for S of Scores when S >= 80 loop
      Put (S'Image);
   end loop;
   New_Line;
end Arrays;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata arrays.adb
$ ./arrays
How many: 5
First:    72
Last:     79
Total: 395
Average: 79
  # 1 = 72
  # 2 = 100
  # 3 = 88
  # 4 = 61
  # 5 = 79
Whole array: 
[ 72,  100,  88,  61,  79]
Sum with 'Reduce: 400
80 or more: 100 88
```

Three words to know. Each value in an array is an *element*. Its position is its *index*: `Scores (2)` means "the element at index 2". And a value for a whole array written out at once, like `[72, 95, 88, 61, 79]`, is an *aggregate*.

`type Score_List is array (Positive range <>) of Integer;` describes a list of Integers indexed by Positive numbers. The `<>` ("box") means each list decides its own bounds; `Scores` gets 1 to 5 from the five values in its aggregate.

Square brackets for array aggregates are new in Ada 2022. You can also name positions, `[1 => 72, 2 => 95, others => 0]`, and use `[]` for an empty array.

| Attribute | Gives | For Scores |
| --- | --- | --- |
| `'First` | lowest index | 1 |
| `'Last` | highest index | 5 |
| `'Length` | how many elements | 5 |
| `'Range` | the index range, for loops | 1 .. 5 |

- `for S of Scores loop` visits each element. Use `for I in Scores'Range loop` when you need the position.
- `for S of Scores when S >= 80 loop` adds an Ada 2022 *filter*: only matching elements reach the body.
- `Scores'Reduce ("+", 0)` combines every element with `+`, starting from 0 (Ada 2022). Here it works out 0 + 72 + 100 + 88 + 61 + 79, one element at a time, to get 400.
- `Scores'Image` works in Ada 2022, but the compiler chooses the layout; GNAT 13 starts a new line first.

Ada checks every index. Asking for an element that isn't there stops the program, instead of quietly reading whatever memory sits past the end, which is a classic source of security holes in other languages:

bad_index.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Bad_Index is
   type Score_List is array (Positive range <>) of Integer;

   Scores : constant Score_List := [72, 95, 88, 61, 79];
   Which  : Integer := 6;
begin
   Put_Line ("Score 5 is" & Scores (5)'Image);
   Put_Line ("Score 6 is" & Scores (Which)'Image);
end Bad_Index;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata bad_index.adb
bad_index.adb:10:37: warning: value not in range of subtype of "Standard.Integer" defined at line 6 [enabled by default]
bad_index.adb:10:37: warning: Constraint_Error will be raised at run time [enabled by default]
$ ./bad_index
Score 5 is 79

raised CONSTRAINT_ERROR : bad_index.adb:10 range check failed
```

An array can be indexed by any *discrete* type, meaning a type whose values can be counted off one by one: integer types and enumerations, but not Float. That includes your own enumerations: `type Hour_Log is array (Day) of Natural;` gives one slot per day, named `Hours (Mon)`. Exercise 8.3 uses this.

#### Remember

- `type T is array (Positive range <>) of E;`
- `[1, 2, 3]` aggregates
- `for X of A` elements, `for I in A'Range` indexes

#### Common mistakes

- Assuming the first index is 0. It's whatever the declaration says
- Hard-coding `1 .. 5` in loops: use `A'Range` so the loop follows the array

### Exercises

Exercise 8.1

#### Coldest and warmest day

Given a week of temperatures `[41, 38, 52, 47, 35, 50, 44]`, print the lowest and highest and which day each fell on.

Target output

```
$ ./min_max
Coldest: 35 on day 5
Warmest: 52 on day 3
```

Hint

Track the *position* of the lowest and highest so far, not just the values.

Solution

min_max.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Min_Max is
   type Reading_List is array (Positive range <>) of Integer;

   Temps    : constant Reading_List := [41, 38, 52, 47, 35, 50, 44];
   Low_Day  : Positive := Temps'First;
   High_Day : Positive := Temps'First;
begin
   for I in Temps'Range loop
      if Temps (I) < Temps (Low_Day) then
         Low_Day := I;
      end if;
      if Temps (I) > Temps (High_Day) then
         High_Day := I;
      end if;
   end loop;
   Put_Line ("Coldest:" & Temps (Low_Day)'Image & " on day" & Low_Day'Image);
   Put_Line ("Warmest:" & Temps (High_Day)'Image & " on day" & High_Day'Image);
end Min_Max;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata min_max.adb
$ ./min_max
Coldest: 35 on day 5
Warmest: 52 on day 3
```

Exercise 8.2

#### Reverse in place

Reverse `[1, 2, 3, 4, 5, 6]` without making a second array.

Target output

```
$ ./reverse_array
 6 5 4 3 2 1
```

Hint

Swap the first and last, then step both inward until they meet.

Solution

reverse_array.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Reverse_Array is
   type List is array (Positive range <>) of Integer;

   Data : List := [1, 2, 3, 4, 5, 6];
   Lo   : Positive := Data'First;
   Hi   : Positive := Data'Last;
   Temp : Integer;
begin
   while Lo < Hi loop
      Temp      := Data (Lo);
      Data (Lo) := Data (Hi);
      Data (Hi) := Temp;
      Lo := @ + 1;
      Hi := @ - 1;
   end loop;
   for X of Data loop
      Put (X'Image);
   end loop;
   New_Line;
end Reverse_Array;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata reverse_array.adb
$ ./reverse_array
 6 5 4 3 2 1
```

Exercise 8.3

#### Hours by day

Make an array indexed by a `Day` enumeration holding hours worked. Print the week's total, the weekday total, the busiest day, and any overtime past 40 hours.

Target output

```
$ ./week_hours
Week total:   49 h
Weekdays:     45 h
Busiest day: THU
Overtime:     9 h
```

Hint

`subtype Weekday is Day range Mon .. Fri;` then `if D in Weekday`.

Solution

week_hours.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Week_Hours is
   type Day is (Mon, Tue, Wed, Thu, Fri, Sat, Sun);
   subtype Weekday is Day range Mon .. Fri;
   type Hour_Log is array (Day) of Natural;

   Hours : constant Hour_Log :=
     [Mon => 9, Tue => 10, Wed => 8, Thu => 11, Fri => 7, Sat => 4, Sun => 0];

   Week_Total    : Natural := 0;
   Weekday_Total : Natural := 0;
   Busiest       : Day := Day'First;
begin
   for D in Day loop
      Week_Total := @ + Hours (D);
      if D in Weekday then
         Weekday_Total := @ + Hours (D);
      end if;
      if Hours (D) > Hours (Busiest) then
         Busiest := D;
      end if;
   end loop;
   Put_Line ("Week total:  " & Week_Total'Image & " h");
   Put_Line ("Weekdays:    " & Weekday_Total'Image & " h");
   Put_Line ("Busiest day: " & Busiest'Image);
   if Week_Total > 40 then
      Put_Line ("Overtime:    " & Natural'Image (Week_Total - 40) & " h");
   end if;
end Week_Hours;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata week_hours.adb
$ ./week_hours
Week total:   49 h
Weekdays:     45 h
Busiest day: THU
Overtime:     9 h
```

# 9

## Procedures and functions

Name a piece of work once, then use it anywhere. This is how big programs stay readable.

subprograms.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Subprograms is

   function Square (X : Integer) return Integer is (X * X);

   function Max (A, B : Integer) return Integer is
   begin
      if A > B then
         return A;
      else
         return B;
      end if;
   end Max;

   procedure Swap (A, B : in out Integer) is
      Temp : constant Integer := A;
   begin
      A := B;
      B := Temp;
   end Swap;

   procedure Greet (Name : String; Times : Positive := 1) is
   begin
      for I in 1 .. Times loop
         Put_Line ("Hello, " & Name & "!");
      end loop;
   end Greet;

   X : Integer := 3;
   Y : Integer := 8;
begin
   Put_Line ("Square of 7:" & Square (7)'Image);
   Put_Line ("Max of X and Y:" & Max (X, Y)'Image);
   Swap (X, Y);
   Put_Line ("After Swap: X =" & X'Image & ", Y =" & Y'Image);
   Greet ("Ada");
   Greet (Name => "Grace", Times => 2);
end Subprograms;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata subprograms.adb
$ ./subprograms
Square of 7: 49
Max of X and Y: 8
After Swap: X = 8, Y = 3
Hello, Ada!
Hello, Grace!
Hello, Grace!
```

A *subprogram* is a procedure or a function: a named piece of work you write once and use as often as you like. Using one is called *calling* it. The words that go with them:

| Word | Meaning | In this program |
| --- | --- | --- |
| parameter | A name in a subprogram's first line that stands for a value it will be given | `X` in `Square (X : Integer)` |
| argument | The actual value given in a call | `7` in `Square (7)` |
| return value | What a function hands back to whoever called it | 49 |
| local variable | A variable declared inside a subprogram. It's made fresh on every call and disappears when the call ends | `Temp` in `Swap` |

A `function` calculates and `return`s a value, so you use it inside an expression, as in `Square (7)'Image`. A `procedure` does something and is called as a statement on its own, like `Swap (X, Y);`. Both are declared before the `begin` of whatever uses them.

| Mode | Inside the subprogram | Used for |
| --- | --- | --- |
| `in` (default) | read only | information going in |
| `out` | write before you read | results coming back from a procedure |
| `in out` | read and write | changing the caller's variable, like Swap |

`in` really is read-only. Try to change one and GNAT refuses:

in_param.adb

```ada
procedure In_Param is

   procedure Double (X : Integer) is
   begin
      X := X * 2;
   end Double;

   N : Integer := 4;
begin
   Double (N);
end In_Param;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata in_param.adb
in_param.adb:5:07: error: assignment to "in" mode parameter not allowed
gnatmake: "in_param.adb" compilation error
```

If a procedure is meant to change its argument, say so with `in out`, as `Swap` does. Then anyone reading its first line knows a call can change their variable.

`function Square (X : Integer) return Integer is (X * X);` is an *expression function*: a one-line function whose body is a single expression in brackets. Calls can name their parameters, `Greet (Name => "Grace", Times => 2)`, the same `=>` you used for `Put`'s `Fore` and `Aft` in lesson 3. A parameter with a default value can be left out.

### Contracts

A precondition (`Pre`) says what must be true when the function is called; a postcondition (`Post`) says what it promises about its result. Inside a postcondition, `Percent_Of'Result` means "the value this function is about to return". With `-gnata` they're checked on every call:

contracts.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Contracts is

   function Percent_Of (Part, Whole : Natural) return Natural is
     (Part * 100 / Whole)
     with Pre  => Whole > 0 and then Part <= Whole,
          Post => Percent_Of'Result <= 100;

   Done  : constant Natural := 45;
   Total : Natural := 60;
begin
   Put_Line ("45 of 60 is" & Percent_Of (Done, Total)'Image & "%");
   Total := 0;
   Put_Line ("45 of 0 is" & Percent_Of (Done, Total)'Image & "%");
end Contracts;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata contracts.adb
$ ./contracts
45 of 60 is 75%

raised ADA.ASSERTIONS.ASSERTION_ERROR : failed precondition from contracts.adb:7
```

The contract caught the bad call before the division ran, and the message names the contract that failed. Contracts also document intent: a reader knows `Whole` can't be zero without reading the body.

Contracts take some getting used to, so lesson 14 is all about them: who's to blame when one fails, `'Old`, what happens without `-gnata`, why they're not for checking user input, and contracts on types.

#### Remember

- Functions return values, procedures do things
- Modes: `in`, `out`, `in out`
- `with Pre => ..., Post => ...`
- Compile with `-gnata` to check contracts

#### Common mistakes

- Calling a function as a statement on its own line: use its result
- Trying to assign to an `in` parameter
- Forgetting `-gnata`, so contracts are silently skipped

### Exercises

Exercise 9.1

#### Prime finder

Write `function Is_Prime (N : Integer) return Boolean` and use it to print every prime below 50.

Target output

```
$ ./primes
Primes below 50: 2 3 5 7 11 13 17 19 23 29 31 37 41 43 47
```

Hint

A number is prime if it's at least 2 and nothing from 2 up to its square root divides it.

Solution

primes.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Primes is

   function Is_Prime (N : Integer) return Boolean is
      D : Integer := 2;
   begin
      if N < 2 then
         return False;
      end if;
      while D * D <= N loop
         if N mod D = 0 then
            return False;
         end if;
         D := @ + 1;
      end loop;
      return True;
   end Is_Prime;

begin
   Put ("Primes below 50:");
   for N in 1 .. 49 when Is_Prime (N) loop
      Put (N'Image);
   end loop;
   New_Line;
end Primes;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata primes.adb
$ ./primes
Primes below 50: 2 3 5 7 11 13 17 19 23 29 31 37 41 43 47
```

`for N in 1 .. 49 when Is_Prime (N) loop` is the Ada 2022 filter from lesson 8, on a range.

Exercise 9.2

#### Factorial with a contract

Write a recursive `Factorial` (it calls itself: n! = n × (n − 1)!, and 0! = 1). 13! is too big for a 32-bit Integer, so add a precondition that rejects anything above 12.

Target output

```
$ ./factorial_demo
 0! = 1
 1! = 1
 5! = 120
 10! = 3628800
 12! = 479001600
```

Hint

An expression function can call itself: `(if N = 0 then 1 else N * Factorial (N - 1))`.

Solution

factorial_demo.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Factorial_Demo is

   --  13! is too big for a 32-bit Integer, so the contract says so
   function Factorial (N : Natural) return Positive is
     (if N = 0 then 1 else N * Factorial (N - 1))
     with Pre => N <= 12;

   Samples : constant array (1 .. 5) of Natural := [0, 1, 5, 10, 12];
begin
   for N of Samples loop
      Put_Line (N'Image & "! =" & Factorial (N)'Image);
   end loop;
end Factorial_Demo;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata factorial_demo.adb
$ ./factorial_demo
 0! = 1
 1! = 1
 5! = 120
 10! = 3628800
 12! = 479001600
```

Exercise 9.3

#### Low, high and mean

Write a procedure that returns both the lowest and highest values of a list through two `out` parameters, and a function that returns the mean.

Target output

```
$ ./stats
Low: 4   High: 31
Mean: 16.17
```

Hint

A function returns one value, so two results need a procedure with `out` parameters.

Solution

stats.adb

```ada
with Ada.Text_IO;       use Ada.Text_IO;
with Ada.Float_Text_IO; use Ada.Float_Text_IO;

procedure Stats is
   type List is array (Positive range <>) of Integer;

   procedure Low_High (Data : List; Low, High : out Integer) is
   begin
      Low  := Data (Data'First);
      High := Data (Data'First);
      for X of Data loop
         Low  := Integer'Min (Low, X);
         High := Integer'Max (High, X);
      end loop;
   end Low_High;

   function Mean (Data : List) return Float is
      Sum : constant Integer := Data'Reduce ("+", 0);
   begin
      return Float (Sum) / Float (Data'Length);
   end Mean;

   Values : constant List := [12, 7, 31, 18, 4, 25];
   Lo, Hi : Integer;
begin
   Low_High (Values, Lo, Hi);
   Put_Line ("Low:" & Lo'Image & "   High:" & Hi'Image);
   Put ("Mean: ");
   Put (Mean (Values), Fore => 1, Aft => 2, Exp => 0);
   New_Line;
end Stats;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata stats.adb
$ ./stats
Low: 4   High: 31
Mean: 16.17
```

# 10

## Working with text

A String is an array of Characters, so everything from lesson 8 applies, plus a toolbox of text functions.

strings.adb

```ada
with Ada.Text_IO;             use Ada.Text_IO;
with Ada.Strings;             use Ada.Strings;
with Ada.Strings.Fixed;       use Ada.Strings.Fixed;
with Ada.Strings.Unbounded;   use Ada.Strings.Unbounded;
with Ada.Characters.Handling; use Ada.Characters.Handling;

procedure Strings is
   Word : constant String := "Lovelace";
   Log  : Unbounded_String;
begin
   Put_Line ("Length:" & Word'Length'Image);
   Put_Line ("First letter: " & Word (Word'First));
   Put_Line ("Slice 1 .. 4: " & Word (1 .. 4));
   Put_Line ("Upper case: " & To_Upper (Word));
   Put_Line ("How many e's:" & Natural'Image (Ada.Strings.Fixed.Count (Word, "e")));
   Put_Line ("Where is ""lace"":" & Natural'Image (Index (Word, "lace")));
   Put_Line ("Trimmed: [" & Trim (Integer'Image (42), Left) & "]");

   for C of Word loop
      Put (C & " ");
   end loop;
   New_Line;

   Log := To_Unbounded_String ("start");
   Append (Log, ", step 1");
   Log := Log & ", step 2";
   Put_Line (To_String (Log) & " (" & Trim (Length (Log)'Image, Left)
             & " characters)");
end Strings;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata strings.adb
$ ./strings
Length: 8
First letter: L
Slice 1 .. 4: Love
Upper case: LOVELACE
How many e's: 2
Where is "lace": 5
Trimmed: [42]
L o v e l a c e 
start, step 1, step 2 (21 characters)
```

Because a String is an array, `Word (1 .. 4)` is a *slice*, `Word'Length` is its length and `for C of Word` visits each character. A slice keeps its original positions: `Word (5 .. 8)` starts at index 5, so loop with `S'Range` or `S'First`, never a bare 1.

| Package | Useful pieces |
| --- | --- |
| `Ada.Strings.Fixed` | `Index` (find; gives 0 when it isn't there), `Count`, `Trim` (strip spaces from the `Left`, `Right` or `Both` ends), `Head` (cut or pad to a width) |
| `Ada.Characters.Handling` | `To_Upper`, `To_Lower`, `Is_Digit`, `Is_Letter` |
| `Ada.Strings.Unbounded` | `Unbounded_String`, a string that can grow; `Append`, `To_String`, `Length` |

A plain String can't change length. `Name : String := "Bob";` then `Name := "Robert";` raises Constraint_Error. When text grows, as a log or a sentence being built does, use `Unbounded_String`.

`Unbounded_String` is a different type from `String`, so Ada won't mix them without a conversion, just as with Integer and Float:

unbounded_mix.adb

```ada
with Ada.Strings.Unbounded; use Ada.Strings.Unbounded;

procedure Unbounded_Mix is
   Log : Unbounded_String := "start";
begin
   null;
end Unbounded_Mix;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata unbounded_mix.adb
unbounded_mix.adb:4:30: error: expected private type "Ada.Strings.Unbounded.Unbounded_String"
unbounded_mix.adb:4:30: error: found a string type
gnatmake: "unbounded_mix.adb" compilation error
```

`To_Unbounded_String ("start")` fixes it. `To_String` converts back the other way, for example to print one with `Put_Line`. That's why both appear in the example at the top.

The call `Ada.Strings.Fixed.Count` is written out in full on purpose. `Ada.Text_IO` also has something called `Count`, and with both packages `use`d the short name is ambiguous, so GNAT refuses it. Writing the package name settles it.

#### Remember

- Strings are arrays: slices, `'Length`, `'Range`
- `Trim (S, Left)` drops the `'Image` space
- `Unbounded_String` when text changes length

#### Common mistakes

- Assigning a string of a different length to a String variable
- Indexing a slice from 1
- `"` inside a string: write it twice, `"say ""hi"""`

### Exercises

Exercise 10.1

#### Palindromes

Read a word, print it backwards, and say whether it's a palindrome (the same both ways), ignoring capitals.

Target output, with typed input underlined

```
$ ./palindrome
Word: Racecar
Backwards: racecaR
Racecar is a palindrome
$ ./palindrome
Word: Lovelace
Backwards: ecalevoL
Lovelace is not a palindrome
```

Hint

Write `Reversed (S)` that builds a new String of the same range. Compare lower-case versions.

Solution

palindrome.adb

```ada
with Ada.Text_IO;             use Ada.Text_IO;
with Ada.Characters.Handling; use Ada.Characters.Handling;

procedure Palindrome is

   function Reversed (S : String) return String is
      Result : String (S'Range);
   begin
      for I in S'Range loop
         Result (S'Last - I + S'First) := S (I);
      end loop;
      return Result;
   end Reversed;

begin
   Put ("Word: ");
   declare
      Word  : constant String := Get_Line;
      Lower : constant String := To_Lower (Word);
   begin
      Put_Line ("Backwards: " & Reversed (Word));
      if Lower = Reversed (Lower) then
         Put_Line (Word & " is a palindrome");
      else
         Put_Line (Word & " is not a palindrome");
      end if;
   end;
end Palindrome;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata palindrome.adb
$ ./palindrome
Word: Racecar
Backwards: racecaR
Racecar is a palindrome
$ ./palindrome
Word: Lovelace
Backwards: ecalevoL
Lovelace is not a palindrome
```

Exercise 10.2

#### Count the vowels

Read a sentence and count its vowels, taking the vowels to be a, e, i, o and u. (Exercise 10.4 deals with y.)

Target output, with typed input underlined

```
$ ./vowels
Type a sentence: The Analytical Engine weaves algebraical patterns
Vowels: 18
```

Hint

Loop over the characters, lower-case each one, and use a `case` with `'a' | 'e' | 'i' | 'o' | 'u'`.

Solution

vowels.adb

```ada
with Ada.Text_IO;             use Ada.Text_IO;
with Ada.Characters.Handling; use Ada.Characters.Handling;

procedure Vowels is
begin
   Put ("Type a sentence: ");
   declare
      Line  : constant String := Get_Line;
      Count : Natural := 0;
   begin
      for C of Line loop
         case To_Lower (C) is
            when 'a' | 'e' | 'i' | 'o' | 'u' =>
               Count := @ + 1;
            when others =>
               null;
         end case;
      end loop;
      Put_Line ("Vowels:" & Count'Image);
   end;
end Vowels;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata vowels.adb
$ ./vowels
Type a sentence: The Analytical Engine weaves algebraical patterns
Vowels: 18
```

Exercise 10.3

#### Caesar cipher

Shift every letter 3 places along the alphabet (a→d, x→a), keep capitals as capitals, leave other characters alone. Then decode it by shifting back.

Target output

```
$ ./caesar
Plain:   Meet at the Engine, noon.
Secret:  Phhw dw wkh Hqjlqh, qrrq.
Decoded: Meet at the Engine, noon.
```

Hint

`Character'Pos` gives a character's code and `Character'Val` goes back. Count from 'a', add the shift, wrap with `mod 26`.

Solution

caesar.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Caesar is

   function Shift (C : Character; By : Integer) return Character is
      Base : Integer;
   begin
      if C in 'a' .. 'z' then
         Base := Character'Pos ('a');
      elsif C in 'A' .. 'Z' then
         Base := Character'Pos ('A');
      else
         return C;
      end if;
      return Character'Val ((Character'Pos (C) - Base + By) mod 26 + Base);
   end Shift;

   function Encode (S : String; By : Integer) return String is
      Result : String (S'Range);
   begin
      for I in S'Range loop
         Result (I) := Shift (S (I), By);
      end loop;
      return Result;
   end Encode;

   Message : constant String := "Meet at the Engine, noon.";
   Secret  : constant String := Encode (Message, 3);
begin
   Put_Line ("Plain:   " & Message);
   Put_Line ("Secret:  " & Secret);
   Put_Line ("Decoded: " & Encode (Secret, -3));
end Caesar;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata caesar.adb
$ ./caesar
Plain:   Meet at the Engine, noon.
Secret:  Phhw dw wkh Hqjlqh, qrrq.
Decoded: Meet at the Engine, noon.
```

Decoding shifts by −3. `mod` keeps a negative result inside 0 to 25 because it takes the sign of 26. `rem` would go negative and break.

Exercise 10.4

#### Sometimes y

In English, y is sometimes a vowel (gym, happy) and sometimes a consonant (yes, yellow). Extend the vowel counter with a simple rule: a y that starts a word is a consonant, and any other y counts as a vowel.

Target output, with typed input underlined

```
$ ./vowels_y
Type a sentence: Yes, my gym is happy
Vowels: 6
$ ./vowels_y
Type a sentence: Beyond the canyon
Vowels: 7
```

Hint

Loop over the indexes, `for I in Line'Range`, instead of the characters, so you can look at the one before with `Line (I - 1)`. Check `I > Line'First` first, with `and then`, so you never look before the start. `Is_Letter` tells you whether that character is a letter.

Solution

vowels_y.adb

```ada
with Ada.Text_IO;             use Ada.Text_IO;
with Ada.Characters.Handling; use Ada.Characters.Handling;

procedure Vowels_Y is
begin
   Put ("Type a sentence: ");
   declare
      Line  : constant String := Get_Line;
      Count : Natural := 0;
   begin
      for I in Line'Range loop
         case To_Lower (Line (I)) is
            when 'a' | 'e' | 'i' | 'o' | 'u' =>
               Count := @ + 1;
            when 'y' =>
               --  y starting a word ("yes") is a consonant;
               --  anywhere else ("gym", "happy") count it as a vowel
               if I > Line'First and then Is_Letter (Line (I - 1)) then
                  Count := @ + 1;
               end if;
            when others =>
               null;
         end case;
      end loop;
      Put_Line ("Vowels:" & Count'Image);
   end;
end Vowels_Y;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata vowels_y.adb
$ ./vowels_y
Type a sentence: Yes, my gym is happy
Vowels: 6
$ ./vowels_y
Type a sentence: Beyond the canyon
Vowels: 7
```

The first sentence comes out right: e, y, y, i, a and y make 6. The second shows the limit of any rule that only looks at letters. In "beyond" and "canyon" the y sounds like the y in "yes", but it's in the middle of a word, so the rule counts it, giving 7 instead of 5. Whether a y is a vowel depends on how the word is said, and that isn't in the text. Programs that need to get it right use a pronunciation dictionary.

That's a lesson that goes well beyond vowels: a program does exactly what its rule says, so the hard part is often choosing the rule, and knowing where it stops being true.

# 11

## Records

A record groups values of different types that belong together, such as a part's name, count and price.

records.adb

```ada
with Ada.Text_IO;           use Ada.Text_IO;
with Ada.Strings;           use Ada.Strings;
with Ada.Strings.Fixed;     use Ada.Strings.Fixed;
with Ada.Strings.Unbounded; use Ada.Strings.Unbounded;

procedure Records is

   type Part is record
      Name  : Unbounded_String;
      Qty   : Natural;
      Cents : Natural;   --  money in whole cents, never Float
   end record;

   type Part_List is array (Positive range <>) of Part;

   function Money (Cents : Natural) return String is
      Rest : constant Natural := Cents mod 100;
   begin
      return "$" & Trim (Natural'Image (Cents / 100), Left) & "."
             & (if Rest < 10 then "0" else "") & Trim (Rest'Image, Left);
   end Money;

   Bolt : constant Part :=
     (Name => To_Unbounded_String ("Bolt"), Qty => 120, Cents => 15);

   Restocked : constant Part := (Bolt with delta Qty => Bolt.Qty + 80);

   Bin : constant Part_List :=
     [Bolt,
      (To_Unbounded_String ("Washer"), 300, 4),
      (To_Unbounded_String ("Hinge"), 16, 249)];

   Value : Natural := 0;
begin
   Put_Line (To_String (Bolt.Name) & ":" & Bolt.Qty'Image
             & " at " & Money (Bolt.Cents) & " each");
   Put_Line ("After restock:" & Restocked.Qty'Image);

   for P of Bin loop
      Value := @ + P.Qty * P.Cents;
   end loop;
   Put_Line ("Whole bin is worth " & Money (Value));
end Records;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata records.adb
$ ./records
Bolt: 120 at $0.15 each
After restock: 200
Whole bin is worth $69.84
```

Each value inside a record is a *field*; Ada's own word is *component*, which is what GNAT's messages say. Fields are reached with a dot: `Bolt.Qty`. An aggregate, like lesson 8's array ones, fills every field at once, by name (`(Name => ..., Qty => 120, Cents => 15)`) or by position. Named is clearer; the compiler checks you didn't forget one.

`(Bolt with delta Qty => Bolt.Qty + 80)` is an Ada 2022 *delta aggregate*: a copy of `Bolt` with only the listed fields changed. The original stays as it was.

The price is stored as whole cents in a Natural, not as dollars in a Float. A Float stores numbers in binary, and most decimal fractions, 0.10 included, can't be written exactly in binary, just as 1/3 can't be written exactly in decimal. Each sale is off by a tiny amount, and the errors pile up:

float_money.adb

```ada
with Ada.Text_IO;       use Ada.Text_IO;
with Ada.Float_Text_IO; use Ada.Float_Text_IO;

procedure Float_Money is
   Dollars : Float   := 0.0;
   Cents   : Natural := 0;
begin
   for Sale in 1 .. 10_000 loop     --  ten thousand sales of 10 cents
      Dollars := Dollars + 0.10;
      Cents   := Cents + 10;
   end loop;
   Put ("As Float dollars: ");
   Put (Dollars, Fore => 1, Aft => 4, Exp => 0);
   New_Line;
   Put_Line ("As whole cents:  " & Cents'Image);
end Float_Money;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata float_money.adb
$ ./float_money
As Float dollars: 999.9029
As whole cents:   100000
```

Ten thousand sales of 10 cents should come to exactly $1000. The Float total is almost ten cents short; the whole-cent total is exact. Count money in whole cents and only turn it into dollars when you print it.

An array of records, like `Bin`, is how you hold a table of data: one record per row.

#### Remember

- `type T is record ... end record;`
- `R.Field` reads a field
- `(R with delta F => V)` copy with changes
- Money in integer cents

#### Common mistakes

- Leaving a field out of an aggregate
- Using Float for money

### Exercises

Exercise 11.1

#### Room calculator

Make a `Rectangle` record with a width and a length. Write `Area` and `Perimeter` functions and use them on a 12 × 14.5 ft room.

Target output

```
$ ./rectangles
Floor area: 174.0 sq ft
Baseboard:  53.0 ft
```

Hint

Both are one-line expression functions taking a Rectangle.

Solution

rectangles.adb

```ada
with Ada.Text_IO;       use Ada.Text_IO;
with Ada.Float_Text_IO; use Ada.Float_Text_IO;

procedure Rectangles is
   type Rectangle is record
      Width, Length : Float;
   end record;

   function Area (R : Rectangle) return Float is (R.Width * R.Length);
   function Perimeter (R : Rectangle) return Float is
     (2.0 * (R.Width + R.Length));

   Room : constant Rectangle := (Width => 12.0, Length => 14.5);
begin
   Put ("Floor area: ");
   Put (Area (Room), Fore => 1, Aft => 1, Exp => 0);
   Put_Line (" sq ft");
   Put ("Baseboard:  ");
   Put (Perimeter (Room), Fore => 1, Aft => 1, Exp => 0);
   Put_Line (" ft");
end Rectangles;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata rectangles.adb
$ ./rectangles
Floor area: 174.0 sq ft
Baseboard:  53.0 ft
```

Exercise 11.2

#### Top of the class

Make a `Student` record with a name and a score, put four students in an array, and print the top student and the class average.

Target output

```
$ ./students
Top student: Grace with 97
Class average: 92
```

Hint

Same pattern as exercise 8.1: remember the index of the best so far.

Solution

students.adb

```ada
with Ada.Text_IO;           use Ada.Text_IO;
with Ada.Strings.Unbounded; use Ada.Strings.Unbounded;

procedure Students is
   type Student is record
      Name  : Unbounded_String;
      Score : Natural;
   end record;

   type Roster is array (Positive range <>) of Student;

   Class : constant Roster :=
     [(To_Unbounded_String ("Ada"), 91),
      (To_Unbounded_String ("Grace"), 97),
      (To_Unbounded_String ("Alan"), 88),
      (To_Unbounded_String ("Barbara"), 94)];

   Best  : Positive := Class'First;
   Total : Natural := 0;
begin
   for I in Class'Range loop
      Total := @ + Class (I).Score;
      if Class (I).Score > Class (Best).Score then
         Best := I;
      end if;
   end loop;
   Put_Line ("Top student: " & To_String (Class (Best).Name)
             & " with" & Class (Best).Score'Image);
   Put_Line ("Class average:" & Natural'Image (Total / Class'Length));
end Students;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata students.adb
$ ./students
Top student: Grace with 97
Class average: 92
```

Exercise 11.3

#### Moving points

Make a `Point` record with X and Y. Use delta aggregates to make moved copies, and show the original is unchanged.

Target output

```
$ ./points
Start: 2, 3
Up:    2, 13
Trip:  7, 2
Start is unchanged: 2, 3
```

Hint

A function can return `(P with delta X => P.X + DX, Y => P.Y + DY)`. Wrap an aggregate in a second pair of brackets when it is the whole body of an expression function.

Solution

points.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Points is
   type Point is record
      X, Y : Integer;
   end record;

   function Moved (P : Point; DX, DY : Integer) return Point is
     ((P with delta X => P.X + DX, Y => P.Y + DY));

   Start : constant Point := (X => 2, Y => 3);
   Up    : constant Point := (Start with delta Y => Start.Y + 10);
   Trip  : constant Point := Moved (Start, 5, -1);
begin
   Put_Line ("Start:" & Start.X'Image & "," & Start.Y'Image);
   Put_Line ("Up:   " & Up.X'Image & "," & Up.Y'Image);
   Put_Line ("Trip: " & Trip.X'Image & "," & Trip.Y'Image);
   Put_Line ("Start is unchanged:" & Start.X'Image & "," & Start.Y'Image);
end Points;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata points.adb
$ ./points
Start: 2, 3
Up:    2, 13
Trip:  7, 2
Start is unchanged: 2, 3
```

# 12

## Packages and specifications

A specification says what a package offers. The body says how it does it. Keeping the two apart is how large Ada programs stay manageable.

### The idea

Think of a wall socket. The faceplate tells you everything you need to use it: the shape of the plug and the voltage you'll get. The wiring behind the wall is someone else's business, and it can be redone without changing the faceplate. In Ada, the faceplate is the *specification* and the wiring is the *body*.

|  | Specification (.ads) | Body (.adb) |
| --- | --- | --- |
| Holds | What's offered: types, constants, and subprogram declarations with their contracts | How it works: each subprogram's statements, plus any helpers |
| Who reads it | Everyone who uses the package | Only the package's author |
| Change it and | Everything that uses the package is recompiled | Only the body is recompiled |

Code that uses a package is called a *client* of it, and clients only ever see the specification. Here's a small package with a client:

temperature.ads

```ada
package Temperature is

   type Celsius    is new Float;
   type Fahrenheit is new Float;

   Boiling : constant Celsius := 100.0;

   function To_F (C : Celsius) return Fahrenheit;
   function To_C (F : Fahrenheit) return Celsius;

end Temperature;
```

temperature.adb

```
package body Temperature is

   function To_F (C : Celsius) return Fahrenheit is
     (Fahrenheit (C * 9.0 / 5.0 + 32.0));

   function To_C (F : Fahrenheit) return Celsius is
     (Celsius ((F - 32.0) * 5.0 / 9.0));

end Temperature;
```

main.adb

```ada
with Ada.Text_IO;       use Ada.Text_IO;
with Ada.Float_Text_IO; use Ada.Float_Text_IO;
with Temperature;       use Temperature;

procedure Main is
   Body_Temp : constant Fahrenheit := 98.6;
begin
   Put ("Water boils at ");
   Put (Float (To_F (Boiling)), Fore => 1, Aft => 1, Exp => 0);
   Put_Line (" F");
   Put ("Body temperature is ");
   Put (Float (To_C (Body_Temp)), Fore => 1, Aft => 1, Exp => 0);
   Put_Line (" C");
end Main;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata main.adb
$ ./main
Water boils at 212.0 F
Body temperature is 37.0 C
```

`temperature.ads` is the specification: two types, a constant and two function declarations, and not a word about how the conversion is done. `temperature.adb` is the body: `package body`, then the two functions in full. `main.adb` is the client: `with Temperature;` gives it the specification, and that's all it gets.

File names follow the package name: `package Temperature` lives in `temperature.ads` and `temperature.adb`. Run `gnatmake` on the main program only; it sees `with Temperature;` and compiles the package for you.

### You've already written specifications

Every subprogram has a specification too: its first line, with its name, parameters and return type, up to the word `is`. In lesson 9, `Square`'s specification is `function Square (X : Integer) return Integer`. End a specification with a semicolon instead of `is`, and you've declared the subprogram without its body, which then comes later or in another file. A package specification is a list of these, gathered in one file, along with the types and constants they use. The two `Put_Line` declarations you read in `a-textio.ads` in lesson 1 are exactly that.

### The spec is enough to compile a client

Because a client only sees the specification, it can be compiled before the body even exists. Here the folder holds only the spec and the main program. `gcc -c` compiles a single file without trying to build a whole program:

Terminal

```
$ ls
main.adb
temperature.ads
$ gcc -c -gnat2022 -gnata main.adb
$ ls
main.adb
main.ali
main.o
temperature.ads
$ gnatmake -q -gnat2022 -gnata main.adb
cannot generate code for file temperature.ads (package spec)
gnatmake: "temperature.ads" compilation error
```

`main.adb` compiled: GNAT checked every call against the specification and produced `main.o`. Only the full build fails, because a specification on its own can't become machine code. That's what lets a team split up the work: one person writes the specification, and others write code against it while the body is still being built.

### The body must keep the spec's promises

GNAT checks the body against the specification. Every subprogram the specification declares needs a body with exactly the same parameters. Leave one out:

temperature.ads

```ada
package Temperature is

   type Celsius    is new Float;
   type Fahrenheit is new Float;

   Boiling : constant Celsius := 100.0;

   function To_F (C : Celsius) return Fahrenheit;
   function To_C (F : Fahrenheit) return Celsius;

end Temperature;
```

temperature.adb

```
package body Temperature is

   function To_F (C : Celsius) return Fahrenheit is
     (Fahrenheit (C * 9.0 / 5.0 + 32.0));

end Temperature;
```

main.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;
with Temperature;  use Temperature;

procedure Main is
begin
   Put_Line ("Boiling:" & To_F (Boiling)'Image);
end Main;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata main.adb
temperature.adb:1:14: error: missing body for "To_C" declared at temperature.ads:9
gnatmake: "temperature.adb" compilation error
```

The message names the missing subprogram and the line of the specification that promised it.

### What the body keeps to itself

Anything declared only in the body is invisible to clients. That's the place for helpers the package needs but nobody else should depend on. This body rounds its answers with a helper, `Round_To_Tenth`, that isn't in the specification, so `main` can't call it:

temperature.ads

```ada
package Temperature is

   type Celsius    is new Float;
   type Fahrenheit is new Float;

   Boiling : constant Celsius := 100.0;

   function To_F (C : Celsius) return Fahrenheit;
   function To_C (F : Fahrenheit) return Celsius;

end Temperature;
```

temperature.adb

```ada
package body Temperature is

   --  Only the body can see this: it isn't in the spec.
   function Round_To_Tenth (X : Float) return Float is
     (Float'Rounding (X * 10.0) / 10.0);

   function To_F (C : Celsius) return Fahrenheit is
     (Fahrenheit (Round_To_Tenth (Float (C) * 9.0 / 5.0 + 32.0)));

   function To_C (F : Fahrenheit) return Celsius is
     (Celsius (Round_To_Tenth ((Float (F) - 32.0) * 5.0 / 9.0)));

end Temperature;
```

main.adb

```
with Ada.Text_IO; use Ada.Text_IO;
with Temperature;  use Temperature;

procedure Main is
begin
   Put_Line (Round_To_Tenth (3.14159)'Image);
end Main;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata main.adb
main.adb:6:14: error: "Round_To_Tenth" is undefined
gnatmake: "main.adb" compilation error
```

As far as the client can tell, `Round_To_Tenth` doesn't exist. The package's author can rename it, change it or delete it without breaking a single client.

### Private types: hiding what's inside a type

A specification can also hide the inside of a type. `type Account is private;` tells clients that the type exists, and nothing more. Its real definition goes in the specification's *private part*, after the word `private`, where the compiler can see it but clients can't use it:

bank_accounts.ads

```ada
package Bank_Accounts is

   type Account is private;

   function Balance (A : Account) return Natural;   --  in cents

   procedure Deposit (A : in out Account; Amount : Positive)
     with Post => Balance (A) = Balance (A)'Old + Amount;

   procedure Withdraw (A : in out Account; Amount : Positive)
     with Pre  => Amount <= Balance (A),
          Post => Balance (A) = Balance (A)'Old - Amount;

private

   type Account is record
      Cents : Natural := 0;
   end record;

end Bank_Accounts;
```

bank_accounts.adb

```ada
package body Bank_Accounts is

   function Balance (A : Account) return Natural is (A.Cents);

   procedure Deposit (A : in out Account; Amount : Positive) is
   begin
      A.Cents := A.Cents + Amount;
   end Deposit;

   procedure Withdraw (A : in out Account; Amount : Positive) is
   begin
      A.Cents := A.Cents - Amount;
   end Withdraw;

end Bank_Accounts;
```

main.adb

```ada
with Ada.Text_IO;   use Ada.Text_IO;
with Bank_Accounts; use Bank_Accounts;

procedure Main is
   Savings : Account;
begin
   Deposit (Savings, 2_500);
   Withdraw (Savings, 400);
   Put_Line ("Balance:" & Balance (Savings)'Image & " cents");
end Main;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata main.adb
$ ./main
Balance: 2100 cents
```

- Everything above `private` is the *visible part*: what clients may use. Here that's the type's name and three operations, with their contracts (lesson 14 covers contracts in depth).
- Below `private`, an Account is a record holding a number of cents (lesson 11). The compiler needs that to know how much memory an Account takes, but clients can't touch the field.
- So the only way to change a balance is through `Deposit` and `Withdraw`, and their contracts are checked every time. No client can slip a negative balance past them.

Here's a client that tries to reach inside anyway. The specification and body are the same as above; only `main.adb` has changed:

main.adb

```ada
with Ada.Text_IO;   use Ada.Text_IO;
with Bank_Accounts; use Bank_Accounts;

procedure Main is
   Savings : Account;
begin
   Deposit (Savings, 2_500);
   Savings.Cents := Savings.Cents - 400;   --  reaching inside
   Put_Line ("Balance:" & Balance (Savings)'Image & " cents");
end Main;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata main.adb
main.adb:8:04: error: invalid prefix in selected component "Savings"
main.adb:8:21: error: invalid prefix in selected component "Savings"
gnatmake: "main.adb" compilation error
```

GNAT refuses. A *selected component* is its name for the dot form, `Savings.Cents`, and from `Main`'s side of the fence an Account has nothing after the dot. Because no client can depend on how an Account is built, the author could later store dollars and cents separately, or keep a history of every transaction, and no client would need a single line changed. They would be recompiled, because the private part is in the specification file.

### Splitting saves rebuilding

Clients depend on the specification, not the body, and `gnatmake` knows it. Build once, leaving off `-q` to watch:

Terminal

```
$ gnatmake -gnat2022 -gnata main.adb
x86_64-linux-gnu-gcc-13 -c -gnat2022 -gnata main.adb
x86_64-linux-gnu-gcc-13 -c -gnat2022 -gnata temperature.adb
x86_64-linux-gnu-gnatbind-13 -x main.ali
x86_64-linux-gnu-gnatlink-13 main.ali
```

Now change only the body: `To_F`'s formula from `C * 9.0 / 5.0` to `C * 1.8`, which gives the same answer. Build again:

Terminal

```
$ gnatmake -gnat2022 -gnata main.adb
x86_64-linux-gnu-gcc-13 -c -gnat2022 -gnata temperature.adb
x86_64-linux-gnu-gnatbind-13 -x main.ali
x86_64-linux-gnu-gnatlink-13 main.ali
```

Only `temperature.adb` was recompiled. `main.adb` was left alone, because nothing it can see had changed. Now add a line to the specification, a new constant `Freezing`, and build again:

Terminal

```
$ gnatmake -gnat2022 -gnata main.adb
x86_64-linux-gnu-gcc-13 -c -gnat2022 -gnata main.adb
x86_64-linux-gnu-gcc-13 -c -gnat2022 -gnata temperature.adb
x86_64-linux-gnu-gnatbind-13 -x main.ali
x86_64-linux-gnu-gnatlink-13 main.ali
$ ./main
Water boils at 212.0 F
Body temperature is 37.0 C
```

This time both were recompiled, because a change to a specification could affect any client. In a program with hundreds of files, keeping specifications stable and making changes inside bodies saves a lot of rebuilding, and a lot of breakage.

### What goes where

| Put it in | What |
| --- | --- |
| The specification | Types clients need (private, if they don't need the inside), constants, subprogram declarations with their contracts, and comments on how to use them |
| The private part | The full definitions of private types |
| The body | Every subprogram's statements, helpers, and anything else clients shouldn't depend on |

One more rule of thumb: if only the body needs another package, put its `with` in the body. Then clients don't end up depending on that package too.

You've been using a package since lesson 1. `Ada.Text_IO`'s specification is the `a-textio.ads` file you looked inside, and its body, `a-textio.adb`, sits next to it in the same folder. Your programs only ever saw the specification.

#### Remember

- Specification `.ads`: what. Body `.adb`: how
- Clients see only the specification
- `type T is private;` hides a type's insides
- Change a body and only it is recompiled
- Build the main file; gnatmake finds the rest

#### Common mistakes

- Declaring a subprogram in the spec but never writing it in the body
- File names that don't match the package name
- Reaching into a private type with a dot
- Putting helpers in the spec that clients don't need

### Exercises

Exercise 12.1

#### Your own toolbox

Move `Is_Prime` from lesson 9 into a package called `Number_Tools`, and add `GCD` (greatest common divisor) and `Digit_Sum`. Use all three from a main program.

Target output

```
$ ./main
Is 97 prime? TRUE
GCD of 84 and 36: 12
Digit sum of 2026: 10
```

Hint

For GCD use Euclid's method: replace (X, Y) with (Y, X mod Y) until Y is 0; X is the answer. For digit sum, add `N mod 10` and divide N by 10 until it's 0.

Solution

number_tools.ads

```ada
package Number_Tools is

   function Is_Prime (N : Integer) return Boolean;

   function GCD (A, B : Positive) return Positive;

   function Digit_Sum (N : Natural) return Natural;

end Number_Tools;
```

number_tools.adb

```ada
package body Number_Tools is

   function Is_Prime (N : Integer) return Boolean is
      D : Integer := 2;
   begin
      if N < 2 then
         return False;
      end if;
      while D * D <= N loop
         if N mod D = 0 then
            return False;
         end if;
         D := @ + 1;
      end loop;
      return True;
   end Is_Prime;

   function GCD (A, B : Positive) return Positive is
      X : Natural := A;
      Y : Natural := B;
      R : Natural;
   begin
      while Y /= 0 loop
         R := X mod Y;
         X := Y;
         Y := R;
      end loop;
      return X;
   end GCD;

   function Digit_Sum (N : Natural) return Natural is
      Rest : Natural := N;
      Sum  : Natural := 0;
   begin
      while Rest > 0 loop
         Sum  := @ + Rest mod 10;
         Rest := @ / 10;
      end loop;
      return Sum;
   end Digit_Sum;

end Number_Tools;
```

main.adb

```ada
with Ada.Text_IO;  use Ada.Text_IO;
with Number_Tools; use Number_Tools;

procedure Main is
begin
   Put_Line ("Is 97 prime? " & Is_Prime (97)'Image);
   Put_Line ("GCD of 84 and 36:" & GCD (84, 36)'Image);
   Put_Line ("Digit sum of 2026:" & Digit_Sum (2026)'Image);
end Main;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata main.adb
$ ./main
Is 97 prime? TRUE
GCD of 84 and 36: 12
Digit sum of 2026: 10
```

Exercise 12.2

#### Body from a specification

You're given a specification and a main program that uses it. Write `tally.adb` so the main program prints the target output. Don't change the specification or the main program.

Target output

```
$ ./main
Visitors: 7
After reset: 0
```

tally.ads

```ada
package Tally is

   type Counter is private;

   function Total (C : Counter) return Natural;

   procedure Add (C : in out Counter; By : Positive := 1)
     with Post => Total (C) = Total (C)'Old + By;

   procedure Reset (C : in out Counter)
     with Post => Total (C) = 0;

private

   type Counter is record
      Count : Natural := 0;
   end record;

end Tally;
```

main.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;
with Tally;       use Tally;

procedure Main is
   Visitors : Counter;
begin
   Add (Visitors);
   Add (Visitors);
   Add (Visitors, By => 5);
   Put_Line ("Visitors:" & Total (Visitors)'Image);
   Reset (Visitors);
   Put_Line ("After reset:" & Total (Visitors)'Image);
end Main;
```

Hint

Copy each declaration from the specification into the body, and replace its semicolon with `is` and the statements. Inside the body a Counter is a record, so `C.Count` is available there, even though clients can't see it.

Solution

tally.ads

```ada
package Tally is

   type Counter is private;

   function Total (C : Counter) return Natural;

   procedure Add (C : in out Counter; By : Positive := 1)
     with Post => Total (C) = Total (C)'Old + By;

   procedure Reset (C : in out Counter)
     with Post => Total (C) = 0;

private

   type Counter is record
      Count : Natural := 0;
   end record;

end Tally;
```

tally.adb

```ada
package body Tally is

   procedure Add (C : in out Counter; By : Positive := 1) is
   begin
      C.Count := C.Count + By;
   end Add;

   procedure Reset (C : in out Counter) is
   begin
      C.Count := 0;
   end Reset;

   function Total (C : Counter) return Natural is (C.Count);

end Tally;
```

main.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;
with Tally;       use Tally;

procedure Main is
   Visitors : Counter;
begin
   Add (Visitors);
   Add (Visitors);
   Add (Visitors, By => 5);
   Put_Line ("Visitors:" & Total (Visitors)'Image);
   Reset (Visitors);
   Put_Line ("After reset:" & Total (Visitors)'Image);
end Main;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata main.adb
$ ./main
Visitors: 7
After reset: 0
```

The contracts in the specification are checked on every call. If `Add` got its sum wrong, its postcondition would say so, and the message would point at the body you wrote.

Exercise 12.3

#### Hands off

This main program won't compile, because it reaches inside a private type. Make it print the target output using only the package's own operations.

Target output

```
$ ./main
Balance: 2100 cents
```

bank_accounts.ads

```ada
package Bank_Accounts is

   type Account is private;

   function Balance (A : Account) return Natural;   --  in cents

   procedure Deposit (A : in out Account; Amount : Positive)
     with Post => Balance (A) = Balance (A)'Old + Amount;

   procedure Withdraw (A : in out Account; Amount : Positive)
     with Pre  => Amount <= Balance (A),
          Post => Balance (A) = Balance (A)'Old - Amount;

private

   type Account is record
      Cents : Natural := 0;
   end record;

end Bank_Accounts;
```

main.adb

```ada
with Ada.Text_IO;   use Ada.Text_IO;
with Bank_Accounts; use Bank_Accounts;

procedure Main is
   Savings : Account;
begin
   Deposit (Savings, 2_500);
   Savings.Cents := Savings.Cents - 400;   --  reaching inside
   Put_Line ("Balance:" & Balance (Savings)'Image & " cents");
end Main;
```

Hint

Which operation in the specification takes money out of an account?

Solution

Terminal

```
$ gnatmake -q -gnat2022 -gnata main.adb
main.adb:8:04: error: invalid prefix in selected component "Savings"
main.adb:8:21: error: invalid prefix in selected component "Savings"
gnatmake: "main.adb" compilation error
```

Replace the line that subtracts from `Savings.Cents` with a call to `Withdraw`. Its precondition and postcondition now guard the change too:

main.adb

```ada
with Ada.Text_IO;   use Ada.Text_IO;
with Bank_Accounts; use Bank_Accounts;

procedure Main is
   Savings : Account;
begin
   Deposit (Savings, 2_500);
   Withdraw (Savings, 400);
   Put_Line ("Balance:" & Balance (Savings)'Image & " cents");
end Main;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata main.adb
$ ./main
Balance: 2100 cents
```

# 13

## When things go wrong

Run-time problems raise exceptions. You can catch them, recover, and raise your own.

safe_input.adb

```ada
with Ada.Text_IO;    use Ada.Text_IO;
with Ada.Exceptions; use Ada.Exceptions;

procedure Safe_Input is

   Too_Many : exception;

   function Read_Number (Prompt : String) return Integer is
   begin
      loop
         Put (Prompt);
         begin
            return Integer'Value (Get_Line);
         exception
            when Constraint_Error =>
               Put_Line ("  That isn't a whole number. Try again.");
         end;
      end loop;
   end Read_Number;

   Boxes : Integer;
begin
   Boxes := Read_Number ("How many boxes? ");
   if Boxes > 1_000 then
      raise Too_Many with "the truck holds 1000 boxes";
   end if;
   Put_Line ("Loading" & Boxes'Image & " boxes.");
exception
   when E : Too_Many =>
      Put_Line ("Refused: " & Exception_Message (E));
end Safe_Input;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata safe_input.adb
$ ./safe_input
How many boxes? twelve
  That isn't a whole number. Try again.
How many boxes? 12
Loading 12 boxes.
$ ./safe_input
How many boxes? 5000
Refused: the truck holds 1000 boxes
```

An *exception* is Ada's signal that something went wrong while the program was running. Ada *raises* one when a rule is broken, like a value out of range, and you can raise your own with `raise`. A *handler* is code that catches an exception and decides what to do instead of stopping the program. Handlers go in an `exception` section at the end of a block.

An `exception` section at the end of a block catches errors raised inside it. Here the inner `begin ... exception ... end;` block catches bad input, prints a message, and the surrounding loop asks again. The handler at the bottom of the program catches the exception the program raised itself.

| Exception | Typical cause |
| --- | --- |
| `Constraint_Error` | a value out of range, a bad `'Value`, divide by zero, an index past the end |
| `End_Error` | reading input after it has ended (Ctrl+D) |
| `Assertion_Error` | a `Pre` or `Post` contract or a predicate failed (lesson 14) |
| your own | `Too_Many : exception;` then `raise Too_Many with "message";` |

`when E : Too_Many =>` names the exception so `Exception_Message (E)` can show its message. Catch only what you can actually handle; an exception nobody catches stops the program and prints where it happened, which is the most useful thing it can do while you're developing.

### Where an exception goes

When an exception is raised, Ada abandons the rest of the current block and looks for a handler there. If there isn't one, it leaves that block too. If the block is inside a subprogram, the exception travels back to whoever called it, and so on outwards. This is called *propagation*. If it travels all the way out of the main program, the program stops with the `raised ...` message you've seen in earlier lessons.

propagate.adb

```ada
with Ada.Text_IO;    use Ada.Text_IO;
with Ada.Exceptions; use Ada.Exceptions;

procedure Propagate is

   Out_Of_Stock : exception;

   procedure Take (Wanted, In_Stock : Natural) is
   begin
      Put_Line ("Taking" & Wanted'Image & "...");
      if Wanted > In_Stock then
         raise Out_Of_Stock with "only" & In_Stock'Image & " left";
      end if;
      Put_Line ("Done.");
   end Take;

begin
   Take (Wanted => 5, In_Stock => 12);
   Take (Wanted => 20, In_Stock => 12);
   Put_Line ("This line is skipped.");
exception
   when E : Out_Of_Stock =>
      Put_Line ("Main program caught it: " & Exception_Message (E));
end Propagate;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata propagate.adb
$ ./propagate
Taking 5...
Done.
Taking 20...
Main program caught it: only 12 left
```

The second call raised `Out_Of_Stock` inside `Take`. `Take` has no handler, so its last line never ran and the exception travelled back to the main program. The main program skipped its next line too, and its handler caught the exception.

#### Remember

- `begin ... exception when X => ... end;`
- `raise X with "why";`
- Wrap a single step in its own block to retry it

#### Common mistakes

- `when others => null;`: it hides every bug
- Catching an exception and carrying on with a variable that never got its value

### Exercises

Exercise 13.1

#### Safe division

Read two numbers and print the first divided by the second. If either isn't a number, or the second is zero, print a clear message instead of crashing.

Target output, with typed input underlined

```
$ ./divide
Numerator: 10
Denominator: 4
Result: 2
$ ./divide
Numerator: 10
Denominator: 0
Can't do that: divide.adb:11 divide by zero
$ ./divide
Numerator: ten
Can't do that: bad input for 'Value: "ten"
```

Hint

Both problems raise Constraint_Error. `Exception_Message` tells you which.

Solution

divide.adb

```ada
with Ada.Text_IO;    use Ada.Text_IO;
with Ada.Exceptions; use Ada.Exceptions;

procedure Divide is
   A, B : Integer;
begin
   Put ("Numerator: ");
   A := Integer'Value (Get_Line);
   Put ("Denominator: ");
   B := Integer'Value (Get_Line);
   Put_Line ("Result:" & Integer'Image (A / B));
exception
   when E : Constraint_Error =>
      Put_Line ("Can't do that: " & Exception_Message (E));
end Divide;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata divide.adb
$ ./divide
Numerator: 10
Denominator: 4
Result: 2
$ ./divide
Numerator: 10
Denominator: 0
Can't do that: divide.adb:11 divide by zero
$ ./divide
Numerator: ten
Can't do that: bad input for 'Value: "ten"
```

Exercise 13.2

#### Average a list

Read numbers one per line until a blank line, skipping anything that isn't a number, then print the average.

Target output, with typed input underlined

```
$ ./average
Enter numbers, one per line. A blank line finishes.
10
20
x
  Skipping "x"
35

Average of 3 numbers: 21.67
```

Hint

Read each line into a constant inside a block inside a `loop`. `exit when Line = "";` and put the handler on that block.

Solution

average.adb

```ada
with Ada.Text_IO;       use Ada.Text_IO;
with Ada.Float_Text_IO; use Ada.Float_Text_IO;

procedure Average is
   Total : Integer := 0;
   Count : Natural := 0;
begin
   Put_Line ("Enter numbers, one per line. A blank line finishes.");
   loop
      declare
         Line : constant String := Get_Line;
      begin
         exit when Line = "";
         Total := @ + Integer'Value (Line);
         Count := @ + 1;
      exception
         when Constraint_Error =>
            Put_Line ("  Skipping """ & Line & """");
      end;
   end loop;

   if Count = 0 then
      Put_Line ("No numbers entered.");
   else
      Put ("Average of" & Count'Image & " numbers: ");
      Put (Float (Total) / Float (Count), Fore => 1, Aft => 2, Exp => 0);
      New_Line;
   end if;
end Average;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata average.adb
$ ./average
Enter numbers, one per line. A blank line finishes.
10
20
x
  Skipping "x"
35

Average of 3 numbers: 21.67
```

# 14

## Contracts: promises Ada checks

A contract writes down what a subprogram needs and what it promises, and Ada checks both halves every time it runs.

### The idea

Say you hire someone to fill your truck's fuel tank. The deal has two halves. You promise to ask for an amount that fits. They promise that afterwards the tank holds what it held before, plus what you asked for. In Ada, a deal like that is a *contract*, written on the subprogram's first lines, where every caller can see it:

| Part | Who must make it true | When it fails, the bug is in |
| --- | --- | --- |
| `Pre` (precondition) | The caller, before the call | The code that made the call |
| `Post` (postcondition) | The subprogram, by the time it returns | The subprogram itself |

That last column is the whole point. When a contract fails, Ada tells you which side broke the deal, and on which line. Each part is just a condition, an expression that's True or False (lesson 5), written after `with`.

### Preconditions: the caller's promise

Here's the fuel-tank deal as a procedure. The second call asks for 30 gallons when only 16 more will fit:

tank_pre.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Tank_Pre is

   Capacity : constant := 26;

   procedure Fill (Tank : in out Natural; Gallons : Positive)
     with Pre  => Tank + Gallons <= Capacity,
          Post => Tank = Tank'Old + Gallons;

   procedure Fill (Tank : in out Natural; Gallons : Positive) is
   begin
      Tank := Tank + Gallons;
   end Fill;

   Diesel : Natural := 0;
begin
   Fill (Diesel, 10);
   Put_Line ("Tank:" & Diesel'Image & " gallons");
   Fill (Diesel, 30);
   Put_Line ("Tank:" & Diesel'Image & " gallons");
end Tank_Pre;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata tank_pre.adb
$ ./tank_pre
Tank: 10 gallons

raised ADA.ASSERTIONS.ASSERTION_ERROR : failed precondition from tank_pre.adb:8
```

- `Pre => Tank + Gallons <= Capacity` means "only call me when the fuel fits".
- The second call broke that promise, so Ada stopped the program *before* `Fill` ran. The tank was never overfilled.
- `ASSERTION_ERROR` is the exception every failed contract raises (lesson 13). `failed precondition` says whose promise broke, and `tank_pre.adb:8` is the line where the precondition is written.
- The fix belongs in the caller: don't ask for more than fits. `Fill` is fine as it is.

### Postconditions: the subprogram's promise

A precondition protects the subprogram from bad calls. A postcondition protects everyone else from a buggy subprogram. This `Fill` has the same contract, but its body has a bug:

tank_post.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Tank_Post is

   Capacity : constant := 26;

   procedure Fill (Tank : in out Natural; Gallons : Positive)
     with Pre  => Tank + Gallons <= Capacity,
          Post => Tank = Tank'Old + Gallons;

   procedure Fill (Tank : in out Natural; Gallons : Positive) is
   begin
      Tank := Tank + Gallons - 1;   --  the bug: one gallon short
   end Fill;

   Diesel : Natural := 0;
begin
   Put_Line ("Tank:" & Diesel'Image & " gallons");
   Fill (Diesel, 10);
   Put_Line ("Tank:" & Diesel'Image & " gallons");
end Tank_Post;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata tank_post.adb
$ ./tank_post
Tank: 0 gallons

raised ADA.ASSERTIONS.ASSERTION_ERROR : failed postcondition from tank_post.adb:9
```

- `Tank'Old` means "`Tank` as it was when the call started". A postcondition runs at the end, when `Tank` already holds the new value, so `'Old` is how it compares before with after. You can put `'Old` on any value in a postcondition; it's most often used on `in out` parameters like this one.
- In a function's postcondition, `'Result` plays the matching part: the value about to be returned, as in lesson 9's `Percent_Of'Result`.
- The call was fine, so the message says `failed postcondition`: the bug is inside `Fill`. Without the contract, the tank would quietly be a gallon short, and nobody would notice until the truck ran dry.

### Without -gnata, nothing is checked

GNAT skips contract checks unless you ask for them with `-gnata`. This is exactly the program from the precondition section, built without it:

Terminal

```
$ gnatmake -q -gnat2022 tank_pre.adb
$ ./tank_pre
Tank: 10 gallons
Tank: 40 gallons
```

The contract is still written in the code, but nothing checks it, so the program happily reports 40 gallons in a 26-gallon tank. That's why every command in this course includes `-gnata`. Real projects keep the checks on while developing and testing. Some switch them off in the finished program for speed, but only once testing, or a proof tool (see the end of this lesson), has shown they can't fail.

### Contracts are for bugs, not for bad input

This is the part people find hardest, so it's worth being precise. A failed contract means the *program* is wrong: someone wrote a call or a body that breaks the deal. A user typing 30 when only 16 gallons fit isn't a bug. It's ordinary life, and the program should expect it. So check input with `if` and tell the user, and only call `Fill` once you know the call keeps its promise:

tank_input.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Tank_Input is

   Capacity : constant := 26;

   procedure Fill (Tank : in out Natural; Gallons : Positive)
     with Pre  => Tank + Gallons <= Capacity,
          Post => Tank = Tank'Old + Gallons;

   procedure Fill (Tank : in out Natural; Gallons : Positive) is
   begin
      Tank := Tank + Gallons;
   end Fill;

   Diesel : Natural := 10;
   Wanted : Integer;
begin
   Put ("Gallons to add: ");
   Wanted := Integer'Value (Get_Line);
   if Wanted < 1 then
      Put_Line ("Add at least 1 gallon.");
   elsif Diesel + Wanted > Capacity then
      Put_Line ("That won't fit: there's room for"
                & Integer'Image (Capacity - Diesel) & " gallons.");
   else
      Fill (Diesel, Wanted);
      Put_Line ("Tank:" & Diesel'Image & " gallons");
   end if;
end Tank_Input;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata tank_input.adb
$ ./tank_input
Gallons to add: 12
Tank: 22 gallons
$ ./tank_input
Gallons to add: 30
That won't fit: there's room for 16 gallons.
```

The precondition never fails here, because the `if` makes sure of it. A good rule of thumb: `if`, `case` and exception handlers deal with things that can legitimately go wrong. Contracts state things that must never go wrong. If a contract fails, fix the code; don't catch `Assertion_Error` and carry on.

### Contracts on types: predicates

A range (lesson 7) can say "0 to 100", but it can't say "even" or "a vowel". A *predicate* is a rule attached to a subtype, checked whenever a value goes into it:

predicates.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Predicates is

   subtype Vowel is Character
     with Static_Predicate => Vowel in 'a' | 'e' | 'i' | 'o' | 'u';

   subtype Even is Integer
     with Dynamic_Predicate => Even mod 2 = 0;

   Pairs : Even := 4;
begin
   Put ("Vowels:");
   for V in Vowel loop
      Put (" " & V);
   end loop;
   New_Line;
   Put_Line ("Is 'y' a vowel? " & Boolean'Image ('y' in Vowel));

   Put_Line ("Pairs:" & Pairs'Image);
   Pairs := Pairs + 3;
   Put_Line ("Pairs:" & Pairs'Image);
end Predicates;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata predicates.adb
$ ./predicates
Vowels: a e i o u
Is 'y' a vowel? FALSE
Pairs: 4

raised ADA.ASSERTIONS.ASSERTION_ERROR : Dynamic_Predicate failed at predicates.adb:21
```

- `Static_Predicate` is for rules made of fixed values, ranges and lists, like the five vowels. Because Ada can work out the values in advance, you can loop over the subtype (`for V in Vowel loop`) and test membership with `in`.
- The predicate says FALSE for `'y'` because it lists exactly five letters. Whether y is a vowel really depends on the word, "gym" against "yes", and a rule about one character can't see the word. Exercise 10.4 tries a better rule, and shows where it still goes wrong.
- `Dynamic_Predicate` takes any condition, like `Even mod 2 = 0`. It's checked every time a value is stored, which is how `Pairs := Pairs + 3` was caught on line 21. You can't loop over a dynamic predicate, because Ada can't list its values in advance.
- Inside the rule, the subtype's own name stands for the value being checked: `Even mod 2 = 0` means "this value mod 2 is 0".

### A contract from the real world

You've already met one. In lesson 1 you looked inside `a-textio.ads` at the two `Put_Line`s. Here is the whole declaration of the one that writes to a file:

Terminal

```
$ sed -n '555,564p' /usr/lib/gcc/x86_64-linux-gnu/13/adainclude/a-textio.ads
   procedure Put_Line
     (File : File_Type;
      Item : String)
   with
     Pre      => Is_Open (File) and then Mode (File) /= In_File,
     Post     =>
       Line_Length (File)'Old = Line_Length (File)
       and Page_Length (File)'Old = Page_Length (File),
     Global   => (In_Out => File_System),
     Annotate => (GNATprove, Always_Return);
```

- `Pre`: you may only call it on a file that is open and isn't open for reading only (`/= In_File`). `and then` matters here: asking a closed file for its mode would itself be an error, so the second check only runs once the first has passed.
- `Post`: writing a line doesn't change the file's line or page length settings. That's `'Old` again, on function calls this time.
- `Global` and `Annotate` are for a proof tool. Ignore them for now.

Those contracts are written for SPARK, a strict subset of Ada with a tool, GNATprove, that can prove a contract holds for every possible input without running the program at all. That's part of why Ada and SPARK are used in places where software must not fail, such as aircraft and railways. You don't need SPARK for this course, but the contracts you learn to write here are the same ones it checks.

#### Remember

- `Pre`: the caller's promise. `Post`: the subprogram's promise
- `X'Old`: X at the start of the call. `F'Result`: the function's answer
- Checked only when compiled with `-gnata`
- Contracts catch bugs; `if` handles bad input
- Predicates put rules on subtypes

#### Common mistakes

- Compiling without `-gnata` and assuming contracts are checked
- Using a precondition to reject user input
- Catching `Assertion_Error` instead of fixing the code
- A postcondition so weak that wrong answers still pass it

### Exercises

Exercise 14.1

#### Bank withdrawal

Write `Withdraw (Balance : in out Natural; Amount : Positive)` with a precondition that there's enough money and a postcondition that the balance went down by exactly the amount. Start with 100, withdraw 40, then try to withdraw 80.

Target output

```
$ ./withdraw_demo
Balance: 60

raised ADA.ASSERTIONS.ASSERTION_ERROR : failed precondition from withdraw_demo.adb:6
```

Hint

The precondition is `Amount <= Balance`. The postcondition compares `Balance` with `Balance'Old`.

Solution

withdraw_demo.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Withdraw_Demo is

   procedure Withdraw (Balance : in out Natural; Amount : Positive)
     with Pre  => Amount <= Balance,
          Post => Balance = Balance'Old - Amount;

   procedure Withdraw (Balance : in out Natural; Amount : Positive) is
   begin
      Balance := Balance - Amount;
   end Withdraw;

   Account : Natural := 100;
begin
   Withdraw (Account, 40);
   Put_Line ("Balance:" & Account'Image);
   Withdraw (Account, 80);
   Put_Line ("Balance:" & Account'Image);
end Withdraw_Demo;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata withdraw_demo.adb
$ ./withdraw_demo
Balance: 60

raised ADA.ASSERTIONS.ASSERTION_ERROR : failed precondition from withdraw_demo.adb:6
```

Without the precondition, `Natural`'s range check would still stop the program, but the message would point at the subtraction inside `Withdraw`. The precondition points at the real mistake: the call that asked for more than there was.

Exercise 14.2

#### Find the bug

This function's postcondition fails. The contract is correct; the body isn't. Read GNAT's message, work out which case the body gets wrong, and fix the body without touching the contract.

Target output

```
$ ./largest_demo
Largest of 3, 9, 4:  9
Largest of 8, 2, 15: 15
```

largest_demo.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Largest_Demo is

   function Largest (A, B, C : Integer) return Integer
     with Post => Largest'Result >= A and then Largest'Result >= B
                  and then Largest'Result >= C
                  and then (Largest'Result = A or else Largest'Result = B
                            or else Largest'Result = C);

   function Largest (A, B, C : Integer) return Integer is
   begin
      if A > B then
         return A;
      elsif B > C then
         return B;
      else
         return C;
      end if;
   end Largest;

begin
   Put_Line ("Largest of 3, 9, 4: " & Largest (3, 9, 4)'Image);
   Put_Line ("Largest of 8, 2, 15:" & Largest (8, 2, 15)'Image);
end Largest_Demo;
```

Hint

Try the failing call by hand: with 8, 2 and 15, which branch runs? The first branch only compares A with B.

Solution

Terminal

```
$ gnatmake -q -gnat2022 -gnata largest_demo.adb
$ ./largest_demo
Largest of 3, 9, 4:  9

raised ADA.ASSERTIONS.ASSERTION_ERROR : failed postcondition from largest_demo.adb:7
```

`A > B` was true for 8 and 2, so the function returned 8 without ever looking at C. The postcondition says the answer must be at least as big as all three, and 8 isn't. Compare A with both others first:

largest_demo.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Largest_Demo is

   function Largest (A, B, C : Integer) return Integer
     with Post => Largest'Result >= A and then Largest'Result >= B
                  and then Largest'Result >= C
                  and then (Largest'Result = A or else Largest'Result = B
                            or else Largest'Result = C);

   function Largest (A, B, C : Integer) return Integer is
   begin
      if A >= B and then A >= C then
         return A;
      elsif B >= C then
         return B;
      else
         return C;
      end if;
   end Largest;

begin
   Put_Line ("Largest of 3, 9, 4: " & Largest (3, 9, 4)'Image);
   Put_Line ("Largest of 8, 2, 15:" & Largest (8, 2, 15)'Image);
end Largest_Demo;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata largest_demo.adb
$ ./largest_demo
Largest of 3, 9, 4:  9
Largest of 8, 2, 15: 15
```

Notice the contract's last part: the answer must also *be* one of A, B or C. Without it, returning `Integer'Last` would pass every time. A contract is only as good as what it rules out.

Exercise 14.3

#### Vowels by predicate

Redo exercise 10.2 using a `Vowel` subtype with a static predicate, and test each character with `in Vowel` instead of a `case`.

Target output, with typed input underlined

```
$ ./vowel_count
Type a sentence: The Analytical Engine weaves algebraical patterns
Vowels: 18
```

Hint

Lower-case each character with `To_Lower` before the `in` test.

Solution

vowel_count.adb

```ada
with Ada.Text_IO;             use Ada.Text_IO;
with Ada.Characters.Handling; use Ada.Characters.Handling;

procedure Vowel_Count is

   subtype Vowel is Character
     with Static_Predicate => Vowel in 'a' | 'e' | 'i' | 'o' | 'u';

   Count : Natural := 0;
begin
   Put ("Type a sentence: ");
   declare
      Line : constant String := Get_Line;
   begin
      for C of Line loop
         if To_Lower (C) in Vowel then
            Count := @ + 1;
         end if;
      end loop;
   end;
   Put_Line ("Vowels:" & Count'Image);
end Vowel_Count;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata vowel_count.adb
$ ./vowel_count
Type a sentence: The Analytical Engine weaves algebraical patterns
Vowels: 18
```

Same sentence, same answer as exercise 10.2: 18. The rule for what counts as a vowel now lives in one place, on the type, where any part of the program can use it.

# 15

## Project: a gradebook

One program that uses nearly everything: input, loops, records, arrays, functions, strings and exceptions.

Build a gradebook that reads lines like `Grace 97`, rejects bad lines with a reason, and prints a report with letter grades, the class average and the top student. Names can contain spaces, so split at the *last* space. Work through it in steps and run the program after each one:

- Read lines in a loop until a blank one, and echo each back.
- Split a line at its last space with `Index (Line, " ", Backward)`.
- Turn the score part into a `subtype Score is Integer range 0 .. 100` with `Score'Value`, so 105 is rejected automatically.
- Store students in an array of records with a count of how many are used.
- Write `Letter` as a case expression.
- Print the report, lining up columns with `Head` and `Put (X, Width => 7)`.
- Catch bad lines with your own `Bad_Line` exception.

Try it yourself before opening the solution in exercise 15.1. The two extra exercises push further.

#### Remember

- Build in small steps; compile after each
- Let types reject bad data for you

#### Common mistakes

- Writing the whole program before compiling once

### Exercises

Exercise 15.1

#### The gradebook

Build the gradebook described above.

Target output, with typed input underlined

```
$ ./gradebook
Enter "name score" lines. A blank line finishes.
> Ada 91
> Grace 97
> Alan 8x
  Skipped: score must be a whole number 0 to 100
> Barbara 105
  Skipped: score must be a whole number 0 to 100
> Edsger Dijkstra 79
> Linus 64
> 

Name              Score  Grade
Ada                  91    A
Grace                97    A
Edsger Dijkstra      79    C
Linus                64    D
Class average: 82.8
Top student:   Grace
```

Hint

Start from the average program in lesson 13: it already reads lines until a blank one and skips bad input.

Solution

gradebook.adb

```ada
with Ada.Text_IO;           use Ada.Text_IO;
with Ada.Integer_Text_IO;   use Ada.Integer_Text_IO;
with Ada.Float_Text_IO;     use Ada.Float_Text_IO;
with Ada.Strings;           use Ada.Strings;
with Ada.Strings.Fixed;     use Ada.Strings.Fixed;
with Ada.Strings.Unbounded; use Ada.Strings.Unbounded;
with Ada.Exceptions;        use Ada.Exceptions;

procedure Gradebook is

   Max_Students : constant := 50;
   subtype Score is Integer range 0 .. 100;

   type Student is record
      Name : Unbounded_String;
      Mark : Score;
   end record;

   Class : array (1 .. Max_Students) of Student;
   Count : Natural := 0;

   Bad_Line   : exception;
   Class_Full : exception;

   function Letter (S : Score) return Character is
     (case S is
         when 90 .. 100 => 'A',
         when 80 .. 89  => 'B',
         when 70 .. 79  => 'C',
         when 60 .. 69  => 'D',
         when others    => 'F');

   --  "Grace 97" -> name "Grace", score 97 (split at the last space)
   procedure Add (Line : String) is
      Space : constant Natural := Index (Line, " ", Backward);
      Mark  : Score;
   begin
      if Space = 0 then
         raise Bad_Line with "expected a name, a space, then a score";
      end if;
      begin
         Mark := Score'Value (Line (Space + 1 .. Line'Last));
      exception
         when Constraint_Error =>
            raise Bad_Line with "score must be a whole number 0 to 100";
      end;
      if Count = Max_Students then
         raise Class_Full;
      end if;
      Count := @ + 1;
      Class (Count) := (To_Unbounded_String (Line (Line'First .. Space - 1)),
                        Mark);
   end Add;

   procedure Report is
      Total : Natural := 0;
      Best  : Positive := 1;
   begin
      New_Line;
      Put_Line ("Name              Score  Grade");
      for I in 1 .. Count loop
         Put (Head (To_String (Class (I).Name), 16));
         Put (Class (I).Mark, Width => 7);
         Put_Line ("    " & Letter (Class (I).Mark));
         Total := @ + Class (I).Mark;
         if Class (I).Mark > Class (Best).Mark then
            Best := I;
         end if;
      end loop;
      Put ("Class average: ");
      Put (Float (Total) / Float (Count), Fore => 1, Aft => 1, Exp => 0);
      New_Line;
      Put_Line ("Top student:   " & To_String (Class (Best).Name));
   end Report;

begin
   Put_Line ("Enter ""name score"" lines. A blank line finishes.");
   loop
      Put ("> ");
      declare
         Line : constant String := Trim (Get_Line, Both);
      begin
         exit when Line = "";
         Add (Line);
      exception
         when E : Bad_Line =>
            Put_Line ("  Skipped: " & Exception_Message (E));
      end;
   end loop;

   if Count = 0 then
      Put_Line ("No students entered.");
   else
      Report;
   end if;
exception
   when Class_Full =>
      Put_Line ("The class is full at" & Max_Students'Image & " students.");
      Report;
end Gradebook;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata gradebook.adb
$ ./gradebook
Enter "name score" lines. A blank line finishes.
> Ada 91
> Grace 97
> Alan 8x
  Skipped: score must be a whole number 0 to 100
> Barbara 105
  Skipped: score must be a whole number 0 to 100
> Edsger Dijkstra 79
> Linus 64
> 

Name              Score  Grade
Ada                  91    A
Grace                97    A
Edsger Dijkstra      79    C
Linus                64    D
Class average: 82.8
Top student:   Grace
```

Exercise 15.2

#### Guessing game

Pick a random number from 1 to 100 and let the player guess, answering Higher or Lower and counting tries. Ignore input that isn't a number.

Target output

```
$ ./guess
I'm thinking of a number from 1 to 100.
Your guess: 50
Lower.
Your guess: 25
Lower.
Your guess: 12
Lower.
Your guess: 6
Lower.
Your guess: 3
Lower.
Your guess: 1
Higher.
Your guess: 2
Got it in 7 tries!
```

The secret number is random, so your guesses and answers will be different.

Hint

`package Random_Secret is new Ada.Numerics.Discrete_Random (Secret_Range);` creates a random generator for your range; `Reset` seeds it and `Random` draws a number. That `is new` line is called a generic instantiation.

Solution

guess.adb

```ada
with Ada.Text_IO; use Ada.Text_IO;
with Ada.Numerics.Discrete_Random;

procedure Guess is
   subtype Secret_Range is Integer range 1 .. 100;
   package Random_Secret is new Ada.Numerics.Discrete_Random (Secret_Range);

   Gen    : Random_Secret.Generator;
   Secret : Secret_Range;
   Tries  : Natural := 0;
   G      : Integer;
begin
   Random_Secret.Reset (Gen);            --  seed from the clock
   Secret := Random_Secret.Random (Gen);
   Put_Line ("I'm thinking of a number from 1 to 100.");
   loop
      Put ("Your guess: ");
      begin
         G := Integer'Value (Get_Line);
         Tries := @ + 1;
         if G < Secret then
            Put_Line ("Higher.");
         elsif G > Secret then
            Put_Line ("Lower.");
         else
            Put_Line ("Got it in" & Tries'Image & " tries!");
            exit;
         end if;
      exception
         when Constraint_Error =>
            Put_Line ("Numbers only, please.");
      end;
   end loop;
end Guess;
```

Terminal

```
$ gnatmake -q -gnat2022 -gnata guess.adb
$ ./guess
I'm thinking of a number from 1 to 100.
Your guess: 50
Lower.
Your guess: 25
Lower.
Your guess: 12
Lower.
Your guess: 6
Lower.
Your guess: 3
Lower.
Your guess: 1
Higher.
Your guess: 2
Got it in 7 tries!
```

The run above was played by a script that halves the range each time. Halving always finds a number from 1 to 100 in seven guesses or fewer, since 2⁷ = 128.

Exercise 15.3

#### Stretch goals

Extend the gradebook: sort the report from highest score to lowest, show how many students got each letter, and print the median.

Hint

For sorting, insertion sort is short: take each student and slide it left past every lower score. For letter counts, `array (Character range 'A' .. 'F') of Natural` works (E is never used).

Notes

No single answer here: if it compiles, the output is right, and you can explain every line, you're done. Good next steps after this course are [learn.adacore.com](https://learn.adacore.com/)'s Introduction to Ada and the [Ada 2022 Reference Manual](http://www.ada-auth.org/standards/22rm/html/RM-TOC.html), which is free to read.

A–Z

## Glossary

Every term this course uses, in plain words, with a link to the lesson that explains it properly.

aggregate

A whole array or record value written out at once, like `[72, 95, 88]`. [Lesson 8](#arrays)

argument

The actual value you give a subprogram when you call it: the 7 in `Square (7)`. [Lesson 9](#subprograms)

array

A row of values of one type, each picked out by its index. [Lesson 8](#arrays)

Assertion_Error

The exception raised when a contract or predicate fails. [Lesson 14](#contracts)

attribute

A built-in fact about a type or value, written after an apostrophe: `X'Image`, `Integer'Last`. [Lesson 2](#variables)

body

How a package or subprogram works: its statements. A package body is its `.adb` file. [Lesson 12](#packages)

Boolean

The type with only two values, `True` and `False`. [Lesson 2](#variables)

branch

The statements under one condition of an `if` or one choice of a `case`. [Lesson 5](#decisions)

call

Running a subprogram by writing its name, with any arguments in brackets. [Lesson 9](#subprograms)

case

A statement that picks a branch by comparing one value against lists of choices. [Lesson 5](#decisions)

Character

The type of a single letter, digit, space or symbol, written in single quotes: `'J'`. [Lesson 2](#variables)

client

Code that uses a package. Clients only see the package's specification. [Lesson 12](#packages)

compiler

The program (GNAT) that checks your source code and turns it into an executable. [Setup](#setup)

component

Ada's word for a field of a record. [Lesson 11](#records)

condition

An expression that works out to True or False, like `Score >= 90`. [Lesson 5](#decisions)

constant

A named value that is set once and can never change. [Lesson 2](#variables)

contract

A `Pre` or `Post` rule on a subprogram, checked on every call when compiled with `-gnata`. [Lesson 14](#contracts)

declaration

A line that creates a name and says what it is, such as `Age : Integer := 34;`. [Lesson 2](#variables)

declare block

A `declare ... begin ... end;` section that creates names in the middle of a program. [Lesson 4](#input)

discrete type

A type whose values can be counted one by one: integer types and enumerations, not Float. [Lesson 8](#arrays)

element

One value inside an array. [Lesson 8](#arrays)

enumeration

A type whose values are names you list yourself, like `(Mon, Tue, Wed)`. [Lesson 7](#types)

exception

Ada's signal that something went wrong while the program was running, like `Constraint_Error`. [Lesson 13](#exceptions)

executable

The program file the compiler produces, which you run with `./name`. [Setup](#setup)

expression

Anything that works out to a value: `2 + 3`, `Age`, `Square (7)`. [Lesson 3](#arithmetic)

field

One named value inside a record, reached with a dot: `Bolt.Qty`. [Lesson 11](#records)

Float

The type for numbers with a fractional part, like `1.82`. [Lesson 2](#variables)

function

A subprogram that calculates and returns a value. [Lesson 9](#subprograms)

handler

Code in an `exception` section that catches an exception and decides what to do. [Lesson 13](#exceptions)

identifier

A name you make up for a variable, constant, procedure or type. [Lesson 2](#variables)

if expression

An `if` that produces a value instead of running statements: `(if A then B else C)`. [Lesson 5](#decisions)

index

The position of an element in an array: `Scores (2)`. [Lesson 8](#arrays)

Integer

The type for whole numbers; about ±2.1 billion on GNAT. [Lesson 2](#variables)

iteration

One trip through the statements of a loop. [Lesson 6](#loops)

keyword

A word with a fixed meaning in Ada, like `begin` or `if`, that can't be used as a name. [Lesson 1](#hello)

literal

A value typed straight into the program: `42`, `1.82`, `'J'`, `"Hi"`. [Lesson 1](#hello)

local variable

A variable declared inside a subprogram, made fresh on each call. [Lesson 9](#subprograms)

mod

A remainder that wraps around like a clock; always between 0 and B − 1 for a positive B. [Lesson 3](#arithmetic)

'Old

In a postcondition, `X'Old` is the value X had when the call started. [Lesson 14](#contracts)

operand

A value an operator works on: in `A + B`, A and B. [Lesson 3](#arithmetic)

operator

A symbol that does a calculation or comparison: `+`, `/`, `<`, `and`. [Lesson 3](#arithmetic)

overloading

Several subprograms sharing a name, with Ada picking one by the arguments you pass. [Lesson 1](#hello)

package

A named collection of types, constants and subprograms, split into a spec and a body. [Lesson 12](#packages)

parameter

A name in a subprogram's first line standing for a value it will be given. [Lesson 9](#subprograms)

postcondition

`Post`: what a subprogram promises is true when it returns. [Lesson 14](#contracts)

precondition

`Pre`: what must be true when a subprogram is called. The caller's promise. [Lesson 14](#contracts)

predicate

A rule attached to a subtype, like "even" or "a vowel", checked when a value goes into it. [Lesson 14](#contracts)

private type

A type whose inside is hidden from clients, who can only use the operations its package offers. [Lesson 12](#packages)

procedure

A named list of statements. Every program starts in a main procedure. [Lesson 1](#hello)

propagation

An exception travelling outwards through blocks and calls until a handler catches it. [Lesson 13](#exceptions)

raise

To signal an exception, either automatically when a rule breaks or with the `raise` statement. [Lesson 13](#exceptions)

range

`A .. B`: every value from A to B, both included. Empty when A is bigger than B. [Lesson 6](#loops)

record

A type that bundles values of different types together as named fields. [Lesson 11](#records)

rem

The remainder left over from whole-number `/`; it takes the sign of the left number. [Lesson 3](#arithmetic)

'Result

In a function's postcondition, the value the function is about to return. [Lesson 14](#contracts)

scope

The part of a program where a name can be used. [Lesson 4](#input)

slice

A piece of an array or String, like `Word (1 .. 4)`. [Lesson 10](#strings)

specification

What a package or subprogram offers: for a package, its `.ads` file; for a subprogram, its first line. [Lesson 12](#packages)

statement

One instruction for the computer to carry out, ending in a semicolon. [Lesson 1](#hello)

String

The type for text: a row of Characters with a fixed length. [Lesson 2](#variables)

string literal

Text in double quotes typed straight into the program. [Lesson 1](#hello)

strongly typed

Every value has exactly one type, and Ada never converts between types unless you ask. [Lesson 2](#variables)

subprogram

A procedure or a function. [Lesson 9](#subprograms)

subtype

An existing type with an extra rule, usually a narrower range. It still mixes with its type. [Lesson 7](#types)

type

A kind of value: which values are allowed and what you can do with them. [Lesson 2](#variables)

Unbounded_String

A string type that can grow and shrink, unlike a plain String. [Lesson 10](#strings)

use clause

`use Ada.Text_IO;`: lets you write a package's names without the package in front. [Lesson 1](#hello)

variable

A named box that holds one value of one type, which can change. [Lesson 2](#variables)

with clause

`with Ada.Text_IO;`: gives your program access to a package. [Lesson 1](#hello)

Programs tested with GNAT 13.3 on Ubuntu 24.04, built with `gnatmake -q -gnat2022 -gnata`. The Windows and Mac install steps follow MSYS2's, Alire's and GetAda's own instructions. Each program's output above was captured from a real terminal session; typed input is underlined.

Going further: [learn.adacore.com](https://learn.adacore.com/) (Introduction to Ada, What's New in Ada 2022), the [Ada 2022 Reference Manual](http://www.ada-auth.org/standards/22rm/html/RM-TOC.html), and [Alire](https://alire.ada.dev) for libraries and projects.

Your ticks are saved in this browser only.

# Resources as good as Nand2Tetris

- [[Ray tracing in one weekend]]
- [[TinyRenderer]]
- [[Crafting interpreters]]
- [[Little X series]]
- [[Charles Petzold's CODE]]
- [[Linux from scratch]] [Site](https://www.linuxfromscratch.org/)
- [[hackattic]]: Free alternative codecrafters.io
- [[Handmade Hero index]]

## Books
- **The Practice of Programming** by Brian W Kernighan and Rob Pike
- **Programming pearls** by Jon Bentley
- **Coders at Work** by Peter Seibel

## AI Research - Good resources for learning from first principles

**Tiny Renderer** is my first pick if you want more of the *Handmade Hero* feeling. **Stanford CS144** is my pick if what you want is the *CodeCrafters* loop: a starter repo, a concrete checkpoint, and tests that tell you when you’ve got it.

### First-principles courses with good feedback loops

| Resource | What you build and how you get feedback |
|---|---|
| [Tiny Renderer](https://haqr.eu/tinyrenderer/) | A 3D software renderer in C++, starting with the ability to set one pixel. Draw a line, fill a triangle, then render a model. **The image is your feedback.** |
| [Stanford CS144: Computer Networking](https://cs144.github.io/) | Start with an in-memory byte stream, then build pieces of TCP/IP in C++. The [starter repo](https://github.com/CS144/minnow) has **local tests for each checkpoint**. |
| [Crafting Interpreters](https://craftinginterpreters.com/) | Build a scripting language twice: a tree-walking interpreter in Java, then a bytecode VM in C. The book is free online; if you’d rather get a spec before seeing the implementation, [CodeCrafters has a track based on it](https://app.codecrafters.io/courses/interpreter/overview). |
| [Writing a C Compiler](https://norasandler.com/book) | Start by compiling a C program that returns a constant, then grow the language one feature at a time. The book is paid, but the **chapter-by-chapter test suite** is public. |
| [MIT Operating System Engineering](https://pdos.csail.mit.edu/6.1810/2026/labs/util.html) | Start by writing Unix utilities, then extend the xv6 teaching kernel. There’s a local grader. More setup and bigger labs than CodeCrafters, but a good follow-on to Nand2Tetris. |
| [HDLBits](https://hdlbits.01xz.net/wiki/Problem_sets) | Small Verilog problems with **instant simulation feedback**. Less of a single big build, more of a “give me one satisfying puzzle right now” option. |
| [Neural Networks: Zero to Hero](https://github.com/karpathy/nn-zero-to-hero) | Build a tiny automatic differentiation engine before moving on to language models. More like a code-along than an autograded course; try just the **micrograd** lesson before committing to the series. |

### How to turn an ambitious project into your own CodeCrafters track

**Make the finish line tiny, not the idea.** “Build a database” can stay exciting. It just shouldn’t be the task you sit down to do today.

Pick a small demo you’d genuinely like to see work, then work backward. Describe each stage as **one new behavior you can check**, not a component you have to finish. For example:

> **Ambition:** Build a database.  
> **First demo:** Set a value, quit, reopen, and get the same value back.

1. **Remember one value in memory.** Pass when `set("color", "blue")` followed by `get("color")` returns `"blue"`.
2. **Use it from a terminal.** Pass when typing `SET color blue` and then `GET color` prints `blue`. Only support single-word values.
3. **Survive one restart.** Pass when `GET color` still prints `blue` in a fresh process. A plain text file is fine. No SQL, indexes, or networking yet.

Write down the **first physical action** for the current stage: “Open `test_store` and write the failing assertion,” not “work on storage.” Only detail the next one or two stages. If a stage feels overwhelming because you don’t know how to do it, make the next challenge a short experiment with one question to answer. If it feels boring, add an optional stretch goal *after* the basic check passes. Before stopping, leave yourself one line saying exactly where to resume.

For learning the skill itself, I’d start with these:

- **[How to ADHD: Breaking Down Projects & Goals](https://www.youtube.com/watch?v=QJsAH6jiTN4)** — a short, directly relevant starting point, especially if you get stuck before you can name the first step.
- **[*Mastering Your Adult ADHD*](https://chadd.org/attention-article/mastering-your-adult-adhd)** — a structured CBT workbook if you want to practice planning and problem-solving rather than just read tips.
- **[Elephant Carpaccio](https://docs.google.com/document/d/1TCuuu-8Mm14oxsOnlk8DqfZAA1cvtYu9WGv67Yj_sSk/pub)** — a software exercise for practicing the other half of this skill: slicing a project so each step produces something runnable.

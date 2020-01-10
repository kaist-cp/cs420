# KAIST CS420: Compiler Design

## Logistics

- Instructor: [Jeehoon Kang](https://cp.kaist.ac.kr/jeehoon.kang)
- Time & Place: Tue & Thu 10:30am-11:45am, Rm. 1220, Bldg. E3-1
- Websites: https://github.com/kaist-cp/cs420, https://gg.kaist.ac.kr/course/3/
- Announcements: in [issue
  tracker](https://github.com/kaist-cp/cs420/issues?q=is%3Aissue+is%3Aopen+label%3Aannouncement)



## Course description

### Context

**Compilers bridge the gap between human and machine.** Human wants to easily express complex
idea. On the other hand, machine understands only a few words (instructions) to be efficiently
implemented in silicon. Compilers transform programs from a form suitable for human to easily
express complex idea, to a form suitable for machine to efficiently execute. Since the gap between
human and machine is fundamentally wide, compilers have been constructed and widely used since the
beginning of the history of computing. Even, the first practical compiler predates the first
practical operating systems (according to Wikipedia)!

**In response to industry shifts, new compilers should be written and rewritten.** First, human wants
to express more and more complex idea, especially in the era of artificial intelligence and big
data. Second, machine changes in response to physics (e.g. the ending of Dennard scaling and Moore's
law) and industrial needs (e.g. Internet of Things and distributed systems). New compilers should be
constructed to close the new gap between changing human and changing machine. For this reason,
industrial needs for (and salary of) compiler engineers have been constantly high.

### Goal

**In this class, we will learn how to construct a compiler by actually building one.** You are going
to benefit from the provided skeleton code of a clean slate educational compiler--dubbed *KECC:
KAIST Educational C Compiler* (think: [KENS](https://an.kaist.ac.kr/kensv3-doc/) for networking or
[Pintos](https://pintos-os.org/), [xv6](https://pdos.csail.mit.edu/6.828/2019/xv6.html) for
operating systems). We are going to discuss parsing only briefly, because the topic is assumed to be
dealt with CS322 (Formal Languages and Automata). We will focus on translation from human-friendly
form to machine-friendly form, and compiler optimizations. Specifically, we will discuss (1) how to
transform a C program to an [intermediate representation
(IR)](https://en.wikipedia.org/wiki/Intermediate_representation); (2) how to perform register
promotion, static single assignment, global value numbering, and register allocation optimizations
on the IR; and (3) how to transform an IR program to an
[RISC-V](https://en.wikipedia.org/wiki/RISC-V) assembly program. KECC will provide a significant
amount of bootstrapping code so that you can focus on the topic of this course.


### Textbook

- Slides (TBA). In the meantime, please see the [slide for a CS320 guest
  lecture](https://docs.google.com/presentation/d/1HqkeVkNLffpafZzb-lU5jVoxVC0HYqLag7T9GNiGCRQ/edit?usp=sharing).
- [The LLVM Compiler Infrastructure](https://github.com/llvm/llvm-project)


### Tools

- KECC: KAIST Educational C Compiler (TBA)

- We will use [Rust](https://www.rust-lang.org/) as the language of implementation, because its
  ownership type system greatly simplifies the development of large-scale system software.


## Prerequisites

- It is **strongly recommended** that students already took courses on:

    + Mathematics (freshman calculus, MAS101 & MAS102): proposition statement and proof
    + Data structures (CS206): linked list, stack, queue
    + Systems programming (CS230): memory layout
    + Programming languages (CS320): lambda calculus, interpreter

  Without a proper understanding of these topics, you will likely struggle in this course.

- Other recommendations which would help you in this course:

    + Basic understanding of computer architecture (CS311)
    + Programming experience in [Rust](https://www.rust-lang.org/)



## Grading & honor code

### Homework (80%)

You will implement translations and optimizations on KECC.

### Midterm exam (20%)

The exam will evaluate your theoretical understanding of compiler theory.  There will not be a final
exam.

### Attendance (?%)

You should submit a token to the [Course Management](https://gg.kaist.ac.kr) website for each
session.  You should submit a token within **12 hours from the beginning of a session**.

### Honor code

Please sign [KAIST School of Computing Honor Code](https://gg.kaist.ac.kr/assignment/4/).



## Communication

- Course-related announcements and information will be posted on the
  [website](https://github.com/kaist-cp/cs420) as well as on the [GitHub issue
  tracker](https://github.com/kaist-cp/cs420/issues).  You are expected to read all
  announcements within 24 hours of their being posted.  It is highly recommended to watch the
  repository so that new announcements will automatically be delivered to you email address.

- Ask your questions via email **only if** they are either confidential or personal.  Otherwise, ask
  administrative questions in [this repository's issue
  tracker](https://github.com/kaist-cp/cs420/issues) and technical questions in [the
  helpdesk](https://github.com/kaist-cp/helpdesk).  Any questions failing to do so
  (e.g. email questions on course materials) will not be answered.

    + I'm requiring you to ask questions online first for two reasons. First, clearly writing a
      question is the first step to reach an answer. Second, you can benefit from questions and
      answers of other students.

- Emails to the instructor or TAs should begin with "CS420:" in the subject line, followed by a
  brief description of the purpose of your email.  The content should at least contain your name and
  student number.  Any emails failing to do so (e.g. emails without student number) will not be
  answered.

# KAIST CS420: Compiler Design (2022 Spring)

## Logistics

- Instructor: [Jeehoon Kang](https://cp.kaist.ac.kr/jeehoon.kang)
- Teaching assistant: [Kyeongmin Cho](https://cp.kaist.ac.kr/kyeongmin.cho) (head), [Minseong Jang](https://cp.kaist.ac.kr/minseong.jang) (main), [Chunmyong Park](https://cp.kaist.ac.kr/chunmyong.park)
- Time & Place: Mon & Wed 1:00pm-2:15pm, Rm. 117, Bldg. N1
- Websites: https://github.com/kaist-cp/cs420, https://gg.kaist.ac.kr/course/10/
- Announcements: in [issue
  tracker](https://github.com/kaist-cp/cs420/issues?q=is%3Aissue+is%3Aopen+label%3Aannouncement)
- Office Hour: Fri 9:00am-12:00pm, [Zoom room](https://zoom.us/j/6537354068) (The passcode is same as the class, announced at KLMS). It is not required, but if you want to come, do so by 9:15am. See [below](https://github.com/kaist-cp/cs420#rules) for office hour policy.



## Course description

### Context

**Compilers bridge the gap between human and machine.** Human wants to easily express complex
idea. On the other hand, machine understands only a few words (instructions) to be efficiently
implemented in silicon. Compilers transform programs from a form suitable for human to easily
express complex idea, to a form suitable for machine to efficiently execute. Since the gap between
human and machine is fundamentally wide, compilers have been constructed and widely used since the
beginning of the history of computing. Even, the first practical compiler predates the first
practical operating systems (according to Wikipedia)!

**In response to industry shifts, new compilers should be written and written again.** First, human
wants to express more and more complex idea, especially in the era of artificial intelligence and
big data. Second, machine changes in response to physics (e.g. the ending of Dennard scaling and
Moore's law) and industrial needs (e.g. Internet of Things and distributed systems). New compilers
should be constructed to close the new gap between changing human and changing machine. For this
reason, industrial needs for (and salary of) compiler engineers have been constantly high.


### Goal

**In this class, we will learn how to construct a compiler by actually building one.** You are going
to benefit from the provided skeleton code of a clean slate educational compiler--dubbed *KECC:
KAIST Educational C Compiler* (think: [KENS](https://an.kaist.ac.kr/kensv3-doc/) for networking or
[Pintos](https://pintos-os.org/), [xv6](https://pdos.csail.mit.edu/6.828/2019/xv6.html) for
operating systems). We are going to discuss parsing only briefly, because the topic is assumed to be
dealt with in CS322: Formal Languages and Automata. (You don't need to know parsing to take this
course, though.) We will focus on translation from human-friendly form to machine-friendly form, and
compiler optimizations. Specifically, we will discuss (1) how to transform a C program to an
[SSA](https://en.wikipedia.org/wiki/Static_single_assignment_form)-based [intermediate
representation (IR)](https://en.wikipedia.org/wiki/Intermediate_representation); (2) how to perform
register promotion, static single assignment, global value numbering, and register allocation
optimizations on the IR; and (3) how to transform an IR program to a
[RISC-V](https://en.wikipedia.org/wiki/RISC-V) assembly program. KECC will provide a significant
amount of skeleton code so that you can focus on the topic of this course.

**We will also briefly discuss the recent trends of compiler construction.** I see two crucial
recent trends: scripting languages and parallelism. (1) Scripting languages like JavaScript and Python,
unlike C, should be compiled (or interpreted) at run-time, and therefore, there is no clear
distinction of compile- and run-time. It is a challenge in that compile time should also be
optimized, but it is also an opportunity in that compile may gather and benefit from run-time
information. (2) It is crucial to exploit the massive parallelism of modern applications like deep
learning and high-performance computing (HPC), because they require so huge computation. Due to the
complexity of workloads, their parallelism should be automatically discovered and exploited by
compilers, which is a big challenge.

**We will also briefly study the theory of compiler.** We will focus on the correctness of
compiler. In general, in what sense a compiler is correct, and how to prove it?  Specifically, how
to prove the correctness of KECC's transformations and optimizations? As it will turn out, this
compiler correctness theory will greatly help you efficiently build your own compiler.


### Resources

- [Slides](https://docs.google.com/presentation/d/1SqtU-Cn60Sd1jkbO0OSsRYKPMIkul0eZoYG9KpMugFE/edit?usp=sharing).
  If you have any suggestions to improve the slide, please leave comments in the slide.

- [Videos for 2020 Spring](https://www.youtube.com/playlist?list=PL5aMzERQ_OZ8RWqn-XiZLXm1IJuaQbXp0).

  + You can turn on the subtitles in the YouTube videos. Auto-transcribed subtitles will be shown.

- [KECC: KAIST Educational C Compiler](https://github.com/kaist-cp/kecc-public)

- [The LLVM Compiler Infrastructure](https://github.com/llvm/llvm-project)


### Tools

Make sure you're capable of using the following development tools:

- [Git](https://git-scm.com/): for downloading KECC and version-controlling your development. If
  you're not familiar with Git, walk through [this
  tutorial](https://www.atlassian.com/git/tutorials).

    + **IMPORTANT**: you should not expose your work to others. In particular, you should not fork
      the [upstream](https://github.com/kaist-cp/kecc-public) and push there. Please the following
      steps:

        * Directly clone the upstream without forking it.

          ```bash
          $ git clone --origin upstream git@github.com:kaist-cp/kecc-public.git
          $ cd kecc-public
          $ git remote -v
          upstream	git@github.com:kaist-cp/kecc-public.git (fetch)
          upstream	git@github.com:kaist-cp/kecc-public.git (push)
          ```

        * To get updates from the upstream, fetch and merge `upstream/main`.

          ```bash
          $ git fetch upstream
          $ git merge upstream/main
          ```

    + If you want to manage your development in a Git server, please create your own private
      repository.

        * You may upgrade your GitHub account to "PRO", which is free of charge.  
          Refer to the [documentation](https://education.github.com/students)

        * Set up your repository as a remote.

          ```bash
          $ git remote add origin git@github.com:<github-id>/kecc-public.git
          $ git remote -v
          origin	 git@github.com:<github-id>/kecc-public.git (fetch)
          origin	 git@github.com:<github-id>/kecc-public.git (push)
          upstream git@github.com:kaist-cp/kecc-public.git (fetch)
          upstream git@github.com:kaist-cp/kecc-public.git (push)
          ```
        * Push to your repository.

          ```bash
          $ git push -u origin main
          ```

- [Rust](https://www.rust-lang.org/): as the language of homework implementation. We chose Rust
  because its ownership type system greatly simplifies the development of large-scale system
  software.

  We recommend you to read [this page](https://cp.kaist.ac.kr/helpdesk#technical-expertise) that describes how to study Rust.

- [Visual Studio Code](https://code.visualstudio.com/) (optional): for developing your homework. If you prefer other editors, you're good to go.

- You can connect to server by `ssh s<student-id>@cp-service.kaist.ac.kr -p13002`, e.g., `ssh s20071163@cp-service.kaist.ac.kr -p13002`.

    + **IMPORTANT: Don't try to hack. Don't try to freeze the server. Please be nice.**

    + Your initial password is `123454321`. IMPORTANT: you should change it ASAP.

    + I require you to register public SSH keys to the server. (In March, we'll expire your password so that you can log in only via SSH keys.)
      See [this tutorial](https://serverpilot.io/docs/how-to-use-ssh-public-key-authentication/) for more information on SSH public key authentication.
      Use `ed25519`.

    + In your client, you may want to set your `~/.ssh/config` as follows for easier SSH access:

      ```
      Host cs420
        Hostname cp-service.kaist.ac.kr
        Port 13002
        User s20071163
      ```

      Then you can connect to the server by `ssh cs420`.

    + Now you can [use it as a VSCode remote server as in the video](https://www.youtube.com/watch?v=TTVuUIhdn_g&list=PL5aMzERQ_OZ8RWqn-XiZLXm1IJuaQbXp0&index=3).

    + [NOTE: We recommend the `rust-analyzer` plugin instead of `rls`](https://github.com/rust-analyzer/rust-analyzer).

    + [NOTE: If permission denied error occurs when trying to install `CodeLLDB Extension` into the 
      remote server](https://github.com/kaist-cp/cs420/issues/5), please follow the steps: 
      1. Download [this file](https://github.com/vadimcn/vscode-lldb/releases/download/v1.5.0/codelldb-x86_64-linux.vsix) at the remote server.
      1. Follow [the instructions](https://code.visualstudio.com/docs/editor/extension-gallery#_install-from-a-vsix) to install it.

    + [NOTE: If you cannot connect to the remote server via VSCode with `fail to create hard link` error message](https://github.com/kaist-cp/cs420/issues/91), please follow the steps:
      1. Close VSCode window and try to connect to the remote server via terminal(or cmd). If you encounter `Connection timed out` error message, try again after a few minutes.
      1. Delete all the files in `~/.vscode-server/bin/`.
      
- [Compiler Explorer](https://godbolt.org/) (optional): for comparing the result of KECC to LLVM IR or RISC-V assembly code.
  See [this video](https://www.youtube.com/watch?v=YuG0poPl8N4) for instructions.
  
  + NOTE: If you want to see LLVM IR code after using a specific optimization pass (e.g., mem2reg, gvn, etc.), 
    please follow the steps:
    1. Uncheck `Directives` and `Comments` options in the filter menu at the top of the compiler window.
    1. Type your C code, and obtain LLVM IR code by compiling it using `-O0 -Xclang -disable-O0-optnone -emit-llvm` flags.
       - See [this link](https://gist.github.com/cmpark0126/459514e8e73cac0c76284b8ec47334a7) for the description of each compilation flag.
    1. Copy LLVM IR code.
    1. Change the target language from C to LLVM IR and compiler from `clang` to `opt`.
    1. Paste LLVM IR code, and optimize it using `-mem2reg`(or `-gvn`) flag.


## Prerequisites

- It is **strongly recommended** that students already took courses on:

    + Mathematics (calculus MAS101, MAS102, or discrete mathematics CS204): proposition statement and proof
    + Data structures (CS206): linked list, stack, queue
    + Systems programming (CS230): memory layout and stack frame
    + Programming languages (CS320): lambda calculus, interpreter

  Without a proper understanding of these topics, you will likely struggle in this course.

- Other recommendations which would help you in this course:

    + Basic understanding of computer architecture (CS311)
    + Programming experience in [Rust](https://www.rust-lang.org/)


## Grading & honor code

### Homework (80%)

You will implement translations and optimizations on KECC. All homework submissions will be
automatically graded online so that you can immediately see your score. If your compiler is correct
and the generated assemblies perform comparably to those generated by `gcc -O1`, you're going to get
A+ (if not A\#) even if you miss the final exam.

Since compiler construction requires nontrivial undertaking, you're encouraged to ask questions on
the homework in the [issue tracker](https://github.com/kaist-cp/cs420/issues) at the early stage of
the semester.

### Final exam (20%, 13th of June, 13:00-15:45)

The exam will evaluate your understanding of compiler theory.  There will not be a midterm exam.

### Attendance (?%)

You should submit a token to the [Course Management](https://gg.kaist.ac.kr/course/10/) website for
each session.  **You should answer to the quiz by the end of the day.**


## Communication

### Registration

- Make sure you can log in the [lab submission website](https://gg.kaist.ac.kr).

    + Reset your password here: https://gg.kaist.ac.kr/accounts/password_reset/
      The email address is your `@kaist.ac.kr` address.

    + The id is your student id (e.g., 20071163).

    + Log in with your student id and the new password.
      If you cannot, please contact the head TA in the chat.

### Rules

- Course-related announcements and information will be posted on the
  [website](https://github.com/kaist-cp/cs420) as well as on the [GitHub issue
  tracker](https://github.com/kaist-cp/cs420/issues).  You are expected to read all
  announcements within 24 hours of their being posted.  It is highly recommended to watch the
  repository so that new announcements will automatically be delivered to you email address.

- Ask questions on course materials and assignments in [this repository's issue tracker](https://github.com/kaist-cp/cs420/issues).
    + Don't send emails to the instructor or TAs for course materials and assignments.
    + Before asking a question, search it in Google and Stack Overflow.
    + Describe your question as detailed as possible. It should include following things:
      * Environment (OS, gcc, g++ version, and any other related program information).
      * Command(s) that you used and the result. Any logs should be formatted in code. Refer to [this](https://guides.github.com/features/mastering-markdown/).
      * Any directory or file changes you've made. If it is solution file, just describe which part of the code is modified.
      * Googling result. Search before asking, and share the keyword used for searching and what you've learned from it.
    + Give a proper title to your issue.
    + Read [this](https://github.com/kaist-cp/cs420#communication) for more instructions.

    + I'm requiring you to ask questions online first for two reasons. First, clearly writing a
      question is the first step to reach an answer. Second, you can benefit from questions and
      answers of other students.

- Ask your questions via email **only if** they are either confidential or personal. Any questions
   failing to do so (e.g. email questions on course materials) will not be answered.

- We are NOT going to discuss *new* questions during the office hour. Before coming to the office
  hour, please check if there is a similar question on the issue tracker. If there isn't, file a new
  issue and start discussion there. The agenda of the office hour will be the issues that are not
  resolved yet.

- Emails to the instructor or the head TA should begin with "CS420:" in the subject line, followed
  by a brief description of the purpose of your email. The content should at least contain your name
  and student number. Any emails failing to do so (e.g. emails without student number) will not be
  answered.

- If we should go online, we'll meet at the [Zoom room](https://kaist.zoom.us/my/jeehoon.kang).
  your Zoom name should be `<your student number> <your name>` (e.g., `20071163 강지훈`).
  Change your name by referring to [this](https://support.zoom.us/hc/en-us/articles/201363203-Customizing-your-profile).

- This course is conducted in English. But you may ask questions in Korean. Then I will translate it to English.

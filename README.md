# KAIST CS420: Compiler Design

## Logistics

- Instructor: [Jeehoon Kang](https://cp.kaist.ac.kr/jeehoon.kang)
- Time: Tue/Thu 9:00-10:30
- Place: TBA
  + Rm. 1101, Bldg. E3-1. **YOUR PHYSICAL ATTENDANCE IS REQUIRED** unless announced otherwise.
  <!-- + [Zoom room](https://kaist.zoom.us/my/jeehoon.kang) (if remote participation is absolutely necessary). The passcode is announced at KLMS. -->
  + [Youtube channel](https://www.youtube.com/playlist?list=PL5aMzERQ_OZ8RWqn-XiZLXm1IJuaQbXp0). Turn on English subtitle at YouTube, if necessary.
- Websites: <https://github.com/kaist-cp/cs420>, <https://gg.kaist.ac.kr/course/15/>
- Announcements: in [issue
  tracker](https://github.com/kaist-cp/cs420/issues?q=is%3Aissue+is%3Aopen+label%3Aannouncement)
  + We assume you read each announcement within 24 hours.
  + We strongly recommend you to watch the repository.
- TA: [Jaewoo Kim](https://cp.kaist.ac.kr/jaewoo.kim/) (Head TA), [Janggun Lee](https://cp.kaist.ac.kr/janggun.lee/)
  + Office Hour: Fri 9:00am-10:00am, Room 4432, E3-1. It is not required, but if you want to come, do so by 9:15am. See [below](https://github.com/kaist-cp/cs420#rules) for office hour policy.


## Course description

### Context

**Compilers bridge the gap between human and machine.** Human wants to easily express complex idea.
On the other hand, machine understands only a few words (instructions) to be efficiently implemented in silicon.
Compilers transform programs from a form suitable for human to easily express complex idea, to a form suitable for machine to efficiently execute.
Since the gap between human and machine is fundamentally wide, compilers have been constructed and widely used since the beginning of the history of computing.
Even, the first practical compiler predates the first practical operating systems (according to Wikipedia)!

**In response to industry shifts, new compilers should be written and written again.**
First, human wants to express more and more complex idea, especially in the era of artificial intelligence and big data.
Second, machine changes in response to physics (e.g. the ending of Dennard scaling and Moore's law) and industrial needs (e.g. Internet of Things and distributed systems).
New compilers should be constructed to close the new gap between changing human and changing machine.
For this reason, industrial needs for (and salary of) compiler engineers have been constantly high.


### Goal

**In this class, we will learn how to construct a compiler by actually building one.**
You are going to benefit from the provided skeleton code of a clean slate educational compiler--dubbed
*KECC: KAIST Educational C Compiler*
(think: [KENS](https://an.kaist.ac.kr/kensv3-doc/) for networking or
[Pintos](https://pintos-os.org/), [xv6](https://pdos.csail.mit.edu/6.828/2019/xv6.html) for operating systems).
We are going to discuss parsing only briefly, because the topic is assumed to be dealt with in CS322: Formal Languages and Automata.
(You don't need to know parsing to take this course, though.)
We will focus on translation from human-friendly form to machine-friendly form, and compiler optimizations.
Specifically, we will discuss
(1) how to transform a C program to an [SSA](https://en.wikipedia.org/wiki/Static_single_assignment_form)-based [intermediate representation (IR)](https://en.wikipedia.org/wiki/Intermediate_representation);
(2) how to perform register promotion, static single assignment, global value numbering, and register allocation optimizations on the IR; and
(3) how to transform an IR program to a [RISC-V](https://en.wikipedia.org/wiki/RISC-V) assembly program.
KECC will provide a significant amount of skeleton code so that you can focus on the topic of this course.

**We will also briefly discuss the recent trends of compiler construction.**
I see two crucial recent trends: scripting languages and parallelism.
(1) Scripting languages like JavaScript and Python, unlike C, should be compiled (or interpreted) at run-time, and therefore, there is no clear
distinction of compile- and run-time.
It is a challenge in that compile time should also be optimized, but it is also an opportunity in that compile may gather and benefit from run-time information.
(2) It is crucial to exploit the massive parallelism of modern applications like deep learning and high-performance computing (HPC), because they require so huge computation.
Due to the complexity of workloads, their parallelism should be automatically discovered and exploited by compilers, which is a big challenge.

**We will also briefly study the theory of compiler.**
We will focus on the correctness of compiler.
In general, in what sense a compiler is correct, and how to prove it?
Specifically, how to prove the correctness of KECC's transformations and optimizations?
As it will turn out, this compiler correctness theory will greatly help you efficiently build your own compiler.


### Textbook

- [Slides](https://docs.google.com/presentation/d/1SqtU-Cn60Sd1jkbO0OSsRYKPMIkul0eZoYG9KpMugFE/edit?usp=sharing).

- [KECC: KAIST Educational C Compiler](https://github.com/kaist-cp/kecc-public)
  + [Code Documentation](https://kaist-cp.github.io/kecc-public/kecc/)

- [The LLVM Compiler Infrastructure](https://github.com/llvm/llvm-project)


### Prerequisites

- It is **strongly recommended** that students already took courses on:

    + Mathematics (MAS101): proposition statement and proof
    + Data structures (CS206): linked list, stack, queue
    + Systems programming (CS230) or Computer Organization (CS311): memory layout, stack and heap, assembly language
    + Programming languages (CS320): lambda calculus, interpreter

  Without a proper understanding of these topics, you will likely struggle in this course.

- Other recommendations which would help you in this course:

    + Basic understanding of computer architecture (CS311)
    + Programming experience in [Rust](https://www.rust-lang.org/) (see also [cs220](https://github.com/kaist-cp/cs220))


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
          Refer to the [documentation](https://education.github.com/students).

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

- You can connect to server by `ssh s<student-id>@rack.fearless.systems -p13001`, e.g., `ssh s20071163@rack.fearless.systems -p13001`.

    + **IMPORTANT: Don't try to hack. Don't try to freeze the server. Please be nice.**

    + Your initial password will be announced at <https://gg.kaist.ac.kr>.

    + I require you to register public SSH keys to the server. (In September, we'll expire your password so that you can log in only via SSH keys.)
      See [this tutorial](https://serverpilot.io/docs/how-to-use-ssh-public-key-authentication/) for more information on SSH public key authentication.
      Use `ed25519`.

    + In your client, you may want to set your `~/.ssh/config` as follows for easier SSH access:

      ```
      Host cs420
        Hostname rack.fearless.systems
        Port 13001
        User s20071163
      ```

      Then you can connect to the server by `ssh cs420`.

    + Now you can [use it as a VSCode remote server as in the video](https://www.youtube.com/watch?v=TTVuUIhdn_g&list=PL5aMzERQ_OZ8RWqn-XiZLXm1IJuaQbXp0&index=3).

    + Install [rustup](https://rustup.rs) in the server to begin working on Rust!

      ```sh
      curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
      ```

    + Install necessary dependencies for KECC

      ```sh
      sudo apt update
      sudo apt install \
        git man-db locales \
        vim neovim emacs \
        zsh bash-completion tmux \
        build-essential gcc clang make cmake python3 csmith libcsmith-dev creduce \
        gcc-riscv64-linux-gnu g++-riscv64-linux-gnu qemu-user-static \
        graphviz curl \
        zip python3-pip

      pip install tqdm
      ```

    + [NOTE: We recommend the `rust-analyzer` plugin instead of `rls`](https://github.com/rust-analyzer/rust-analyzer).

    + [NOTE: If permission denied error occurs when trying to install `CodeLLDB Extension` into the remote server](https://github.com/kaist-cp/cs420/issues/5),
      please follow the steps:
      1. Download [this file](https://github.com/vadimcn/vscode-lldb/releases/download/v1.5.0/codelldb-x86_64-linux.vsix) at the remote server.
      1. Follow [the instructions](https://code.visualstudio.com/docs/editor/extension-gallery#_install-from-a-vsix) to install it.

    + [NOTE: If you cannot connect to the remote server via VSCode with `fail to create hard link` error message](https://github.com/kaist-cp/cs420/issues/91), please follow the steps:
      1. Close VSCode window and try to connect to the remote server via terminal(or cmd). If you encounter `Connection timed out` error message, try again after a few minutes.
      1. Delete all the files in `~/.vscode-server/bin/`.


## Grading & honor code

### Cheating

**IMPORTANT: PAY CLOSE ATTENTION. VERY SERIOUS.**

- Sign the KAIST CS Honor Code for this semester.
  Failure to do so may lead to expulsion from the course.

- We will employ sophisticated tools to detect code plagiarism.
    + Search for "code plagiarism detector" on Google Images to understand how these tools can identify advanced forms of plagiarism.
      Do not attempt plagiarism in any form.

### Homework (60%)

- You will implement translations and optimizations inside KECC.

- All homework submissions will be automatically graded online so that you can immediately see your score.

- Since compiler construction requires nontrivial undertaking, you're encouraged to ask questions on the homework in the [issue tracker](https://github.com/kaist-cp/cs420/issues) at the early stage of
the semester.

- You are **permitted** to use ChatGPT or other LLMs.

### Midterm and Final Exams (40%)

- Dates & Times: April 15th (Tue), June 10th (Tue), 9:00-11:00

- The exams will evaluate your understanding of compiler theory.

- Your physical attendance is required.


### Attendance (?%)

- A quiz must be completed on the [Course Management](https://gg.kaist.ac.kr/course/19) website for each session (if any).
  **Quizzes should be completed by the end of the day.**

- Failing to complete a significant number of quizzes will result in an automatic F.


## Communication

### Registration

- Make sure you can log into the [lab submission website](https://gg.kaist.ac.kr).
    + Use your `kaist-cp-class` account for login.
    + Your ID is your `@kaist.ac.kr` email address.
    + Reset your password here: [https://auth.fearless.systems/if/flow/default-recovery-flow/](https://auth.fearless.systems/if/flow/default-recovery-flow/)
    + Contact the instructor if login issues arise.

### Rules

- Course-related announcements and information will be posted on the [course website](https://github.com/kaist-cp/cs420) and the [GitHub issue tracker](https://github.com/kaist-cp/cs420/issues).
  It is expected that you read all announcements within 24 hours of their posting.
  Watching the repository is highly recommended for automatic email notifications of new announcements.

- Questions about course materials and assignments should be posted in [the course repository's issue tracker](https://github.com/kaist-cp/cs420/issues).
    + Avoid emailing the instructor or TAs regarding course materials and assignments.
    + Search your question using Google and Stack Overflow before posting.
    + Describe your question in detail, including:
        * Environment (OS, gcc, g++ version, and other relevant program information).
        * Used commands and their results, with logs formatted in code.
          See [this guide](https://guides.github.com/features/mastering-markdown/).
        * Any changes made to directories or files.
          For solution files, describe the modified code sections.
        * Your Google search results, including search terms and learned information.
    + Use a clear and descriptive title for your issue.
    + The requirement to ask questions online first is twofold: It ensures clarity in your query and allows everyone to benefit from shared questions and answers.

- Email inquiries should be reserved for confidential or personal matters.
  Questions not adhering to this guideline (e.g., course material queries via email) will not be addressed.


- Office hours will not cover *new* questions.
  Check the issue tracker for similar questions before attending.
  If your question is not listed, post it as a new issue for discussion.
  Office hour discussions will focus on unresolved issues.

- Emails to the instructor or head TA should start with "cs420:" in the subject line, followed by a brief description.
  Include your name and student number in the email.
  Emails lacking this information (e.g., those without a student number) will not receive a response.


- If you join the session remotely from Zoom (https://kaist.zoom.us/my/jeehoon.kang),
  your Zoom name should be `<your student number> <your name>` (e.g., `20071163 강지훈`).
  Change your name by referring to [this](https://support.zoom.us/hc/en-us/articles/201363203-Customizing-your-profile).

- The course is conducted in English.
  However, you may ask questions in Korean, which will be translated into English.

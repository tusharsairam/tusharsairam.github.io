---
title: Common Files in FOSS Repos
date: 2024-10-02 15:47
---
> [!Tip] C/C++ files
> The following files present in a source code repo are relevant for C and/or C++ code
> - `.clang-format` - Automatically generated depending on the IDE used. This is a linter file that formats your code's indentatiom and spaces etc.
> - `.clang-tidy` - Similar to `.clang-format` but it does a deeper inspection and cleaning. It fixes syntax in the code that may be bad practice, or that which may be bug-prone etc.
> - `.clangd` - A language server. Your IDE can use this to provide smart code suggestions

- `.gitignore` - List of directories, files, and filename extensions that should not be tracked by git
- `README.md` - A markdown file to describe what the project is about
- `.eslint.rc` - Configuration file for ==ESLint (static code analyser for JS)==
- `.gitattributes` - ~~Properties/settings for the git repo~~ Assigning attributes to file patterns
	- LF Checkout - LF stands for "Line Feed". You tell git to checkout files with LF (single EOF character, used in Linux systems)
- `.gitallowed` - Excludes the patterns specified in this file from git-secrets matching
	- git-secrets matching prevents sensitive and confidential data from accidentally being committed
- `.gitmodules` - List of all git ==submodules==
- `.gn` - A file used by the GN (Generate-Ninja) meta build system to generate Ninja build files that can be used by Ninja to build the project
- `AUTHORS.md` - List of contributors of the project
- `LICENSE.md` - The license document of the project (GPL-3.0, MIT, BSD, Apache etc.)
- `CODE_OF_CONDUCT.md` - Rules and the code of conduct to abide by when contributing to the project
- `.mailmap` - Used to keep track of contributors' names and e-mail IDs when they have changed. Maps author names and e-mail IDs to canonical real names
- `.rustfmt.toml` - Specifies the Rust style for auto-reformatting. _There is Rust stuff in the project_
- `.vpython3` - Contains patterns for Python wheel dependencies used by the Python scripts in the repo (context: Chromium's repo). It creates a VirtualEnv environment where all the listed dependencies are install. **This way, it helps keep the dev environment the same across all systems**
- `.yapfignore` - Tells ==YAPF== to ignore the specified files from formatting

>[!Tip] YAPF
>YAPF is a formatter for Python scripts

- `codereview.settings` - GitHub's code review settings file
- `WATCHLISTS` - Specifies which developer is interested in watching/tracking which part of the source code repo. Maps code files/directories' changes to the contributor's e-mail for CCing
- `OWNERS` - Who is responsible for primarily maintaining the project? 
- `CPPLINT.cfg` - Config file for linting C++ code
- `DEPS` - Manages the dependencies of a repo
- `DIR_METADATA` - As the name says, metadata for the directory (usually the root/top-level directory)
- `PRESUBMIT*` - Before submitting code, this performs some pre-processing and conducts tests on the code to ensure all checks have passed

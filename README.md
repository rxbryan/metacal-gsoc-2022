# Google Summer of Code 2022
List of project ideas for students applying to the Google Summer of Code program in 2022 (GSoC 2022).

## Timeline/milestones

The program has been announced and [it is coming with major changes](https://opensource.googleblog.com/2021/11/expanding-google-summer-of-code-in-2022.html).

Please always refer to the [official timeline](https://developers.google.com/open-source/gsoc/timeline). 
  
  
## Application Process

#### 0. Get familiar with GSoC

First of all, and if you have not done that yet, read [the student guide](https://google.github.io/gsocguides/student/) which will allow you to understand all this process and how the program works overall. Refer to its left side menu to quick access sections that may interest you the most, although we recommend you to read everything.  
  
#### 1. Discuss the project idea with the mentor(s)

This is a required step unless you have dived in into the existing codebase and understood everything perfectly (very hard) and the idea you prefer is on the list below.

If your idea is not listed, please discuss it with the mentors in the available [contact channels](https://github.com/metacall/gsoc-2021#find-us). We're always open to new ideas and won't hesitate on choosing them if you demonstrate to be a good candidate!  
  
#### 2. Understand that

- You're committing to a project and we may ask you to publicly publish your weekly progress on it.
- It's the second year Metacall is joining the GSoC program and we will ask you to give feedback on our mentorship and management continuously.
- You wholeheartedly agree with the [code of conduct](https://github.com/metacall/core/blob/develop/.github/CODE_OF_CONDUCT.md).
- You must tell us if there's any proposed idea that you don't think would fit the timeline or could be boring (yes, we're asking for feedback).
  
#### 3. Fill out the application form

We recommend you to follow [Google's guide to Writing a Proposal](https://google.github.io/gsocguides/student/writing-a-proposal) as we won't be too harsh on the format and we won't give any template. But hey, we're giving you a starting point!

You can send the proposal link to any readable format you wish: Google Docs, plain text, in markdown... and preferably hosted online, accessible with a common browser **without downloading anything**.

You can also ask for a review anytime to the community or mentor candidates before the student application deadline. It's much easier if you get feedback early than to wait for the last moment.
  

## Project Ideas

You can also propose your own.

### Cross-Platform Builds

**Skills**: C/C++, Bash, Batch, Guix, DevOps

**Expected size of the project**: Medium (175 hours)

**Difficulty rating**: Medium

**Description**:
MetaCall has multiple runtimes embedded on it and one of its objectives is to be as cross platform as possible. Each runtime has its own dependencies which create a huge dependency tree sometimes. This is a big complexity that is difficult to handle. Currently we are using Guix as our build system in Linux and we achieved to compile MetaCall and make it completely self-contained (in Linux for amd64 at the moment of writting). This allows MetaCall to be installed even in a BusyBox and work properly without any other system dependency. Current implementation of the build system consist of 3 repositories:

 - Distributable Linux: https://github.com/metacall/distributable-linux
 - Distributable Windows: https://github.com/metacall/distributable-windows
 - Distributable MacOs: https://github.com/metacall/distributable-macos

The objective of this idea is to improve the cross-platform support for the main supported platforms:
 - Implementing the build script for MacOs from scratch with automated CI, in a similar way to how Windows distributable has been done. Add tests and improve the install script for Linux [by adding support to MacOs](https://github.com/metacall/install/blob/master/install.sh#L213).
 - Improving the language support[[1]](https://github.com/metacall/distributable-windows/issues/14)[[2]](https://github.com/metacall/distributable-windows/issues/13) for Windows with their [respective tests](https://github.com/metacall/distributable-windows/blob/master/test.bat) and [improving the install script](https://github.com/metacall/install/blob/282d193329c6c9bbfca86b5ce4c7db8679f67c88/install.ps1#L158) for Windows.
 - Adding [new architectures](https://www.gnu.org/savannah-checkouts/gnu/autoconf/manual/autoconf-2.70/html_node/Specifying-Target-Triplets.html#Specifying-Target-Triplets) to Linux with their respective tests (for this it is possible to use Docker multi-architecture for supporting those tests) and the install script support for those architectures.

Finally it would be interesting to add a README in [this repository](https://github.com/metacall/distributable) in order to document the three repositories and build systems.

**Expected outcomes**: To implement and complete CI/CD builds for Windows, Linux and MacOS in order to provide cross-platform binary distributions of MetaCall.

**Possible mentors**: Harsh Bardhan Mishra, Gil Arasa Verge, Vicente Eduardo Ferrer Garcia

**Resources**:
 - Guix Build System options: https://guix.gnu.org/manual/en/html_node/Additional-Build-Options.html
 - Docker Multi-Arch support: https://docs.docker.com/desktop/multi-arch/

### Builder

**Skills**: Go, Docker, BuildKit, Sandboxing, Kubernetes

**Expected size of the project**: Large (350 hours)

**Difficulty rating**: Medium

**Description**:
Currently MetaCall is offered as Docker image on [Docker Hub](https://hub.docker.com/r/metacall/core). It includes 4 tags (`deps`, `dev`, `runtime` and `cli`) with only one architecture (`amd64`). Right now all the languages are packaged at once into the image, producing a big sized image, specially on the `dev` tag. The idea of this project is to implement a CLI with a separated API which provides a way to generate compact Docker images with MetaCall runtime. Docker does not allow to selectively choose from multiple layers merging them into one with efficient caching, we could do this by means of templating the Dockerfile itself but using the Buildkit API will make the solution much more robust and with the benefit of all the features from Buildkit like caching.

Another main requirement for this project is that it must be run under rootless and daemonless, inside a Dockerized environment (i.e this must be able to be run inside Docker without permissions and without a Docker daemon, so it can be run in a Kubernetes cluster, pushing and pulling the resulting images from a private registry).

This project has to be efficient and sandboxed, focused on FaaS development and producing compact images in terms of size and dependencies, so the bandwidth and attack surface is reduced.

**Expected outcomes**: A command line interface and library that is able to selectively compose docker images and run inside Docker/Kubernetes for MetaCall with efficient caching and compact size.

**Possible mentors**:  Fernando Vaño Garcia, Vicente Eduardo Ferrer Garcia, Gil Arasa Verge

**Resources**:
 - MetaCall Builder repository: https://github.com/metacall/builder
 - MetaCall Core Docker build system: https://github.com/metacall/core/blob/develop/docker-compose.sh | https://github.com/metacall/core/blob/develop/docker-compose.yml | https://github.com/metacall/core/tree/develop/tools
 - Buildkit examples: https://github.com/moby/buildkit/tree/master/examples

### Deploy CLI

**Skills**: TypeScript

**Expected size of the project**: Medium (175 hours)

**Difficulty rating**: Easy

**Description**:
This project will implement a CLI for deploying projects into MetaCall FaaS. This CLI should be able to be integrated with the MetaCall CLI. The objective of this project is to provide an interactive and also automated way to deploy projects into MetaCall FaaS. It has already some of the functionalities provided, and some tests implemented. The CLI should be tested through automated testing and all the requirements listed in the [TODO](https://github.com/metacall/deploy/blob/master/TODO.md) must be achieved.

For better deployment, the Deploy CLI should be integrable with MetaCall CLI, providing a self contained distributable with all the compiled code which can be launched or invoked from an external CLI.

**Expected outcomes**: A command line interface that is able to deploy projects and manage deployments in MetaCall FaaS.

**Possible mentors**: Thomas Rory Gummerson, Jose Antonio Dominguez, Alexandre Gimenez Fernandez, Vicente Eduardo Ferrer Garcia

**Resources**:
 - MetaCall Deploy repository: https://github.com/metacall/deploy
 - MetaCall FaaS: https://dashboard.metacall.io
 - Video Deploying a hundred functions into MetaCall FaaS using the Dashobard: https://www.youtube.com/watch?v=2RAqTmQAWEc
 - MetaCall Protocol: https://github.com/metacall/protocol
 - MetaCall FaaS TypeScript reimplementation: https://github.com/metacall/faas

### FaaS TypeScript Reimplementation

**Skills**: TypeScript

**Expected size of the project**: Large (350 hours)

**Difficulty rating**: Medium

**Description**:
This project offers a reimplementation of [MetaCall FaaS](https://dashboard.metacall.io) but with a simpler and less performant implementation. The objective of this FaaS reimplementation is to provide a simple and portable FaaS that can be run from the CLI in order to locally test the functions and complete projects that can be deployed into MetaCall FaaS. This is a very important part of the project because it is needed in order to fullfill the developer workflow when developing distributed polyglot applications.

It should mimick the [MetaCall FaaS REST API](https://github.com/metacall/deploy/blob/master/src/test/integration.protocol.spec.ts) but without need of authentication and with only the required capabilities for development. This repository will share parts with MetaCall FaaS through [MetaCall Protocol](https://github.com/metacall/protocol) so code can be reused between the repositories.

For better deployment, the MetaCall FaaS should be integrable with MetaCall CLI, providing a self contained distributable with all the compiled code which can be launched or invoked from an external CLI via API.

**Expected outcomes**: An embeddable library that can be used for locally testing MetaCall projects as if they were hosted on the FaaS.

**Possible mentors**: Thomas Rory Gummerson, Jose Antonio Dominguez, Alexandre Gimenez Fernandez, Vicente Eduardo Ferrer Garcia

**Resources**:
 - MetaCall FaaS TypeScript reimplementation repository: https://github.com/metacall/faas
 - MetaCall FaaS: https://dashboard.metacall.io
 - Video Deploying a hundred functions into MetaCall FaaS using the Dashboard: https://www.youtube.com/watch?v=2RAqTmQAWEc
 - MetaCall Protocol: https://github.com/metacall/protocol
 - MetaCall Deploy: https://github.com/metacall/deploy

### MetaCall CLI Bootstrapping / Refactor

**Skills**: C/C++, Python or NodeJS

**Expected size of the project**: Large (350 hours)

**Difficulty rating**: Medium

**Description**:
Recently MetaCall has provided support for C language. This means that now we can allow MetaCall to compile itself, or provide core functionality by just using plugins. Thanks to this unlocked feature, we can rewrite the CLI in C and other languages, simplifiying the whole design and making it much more simplified, abstracted and with the possibility of very simple extension in multiple languages. The main objective of this project is to separate the current commands (`load`, `inspect`, `call`...) into separated single function files, where each function/file is mapped to one command. It is also important to provide a good CLI and REPL interface. Right now MetaCall CLI has support for two modes, the CLI and REPL mode. The CLI works as follows:
  - `metacall index.js`
  - `metacall asd.py --some-extra-flag`
  - `metacall pip install flask`

It can run multiple scripts and it can use package managers from the command line. We should unify this interface into a way that can be pluggeable but also consistent, so it can be standardized. For example, in the future we want to add support for [`libseccmp` capabilities](https://github.com/metacall/gsoc-2021#cli-security-through-seccomp-sandboxing), and this should be able to be indicated from the flags in the CLI, and it should be implemented as a plugin too.

The REPL mode is sightly different, it has a list of commands that can be executed and listed when asking for help. Those commands include loading a file, calling to a function, inspecting loaded functions. This REPL must be able to be extended, either adding extra commands or launching sub-REPLs or CLIs that are already implemented in separated repositories. The objective of this is to provide a unified tool that can be easily extended with existing projects easily. An example of those is the [Polyglot REPL](https://github.com/metacall/polyglot-repl) and the [Deploy CLI](https://github.com/metacall/deploy).

All the parsing of the CLI commands and the REPL commands can be mostly fully reimplemented in another language like Python or NodeJS (preferably Python because packages are easier to deploy in Guix, otherwise the NPM package must be provided in a self-contained compiled form). So most of the hard work can be done in a high level language and the CLI can be completely bootstrapped from scratch.

For trying out how the CLI or REPL works right now, you can download it here: https://github.com/metacall/install

The student must send a proper design in order to be accepted before starting the refactor, it can be discussed with the Staff on the [different chats we are available](https://github.com/metacall/gsoc-2022/blob/main/README.md#find-us).

**Expected outcomes**: A refactor of MetaCall CLI with a bootstrappable design with all the current functionality (and improvements) provided as means of CLI plugins.

**Possible mentors**:  Fernando Vaño Garcia, Vicente Eduardo Ferrer Garcia, Gil Arasa Verge

**Resources**:

 - MetaCall CLI Issue: https://github.com/metacall/core/issues/84
 - NodeJS REPL: https://nodejs.org/api/repl.html
 - NodeJS Readline: https://nodejs.org/api/readline.html
 - Python Prompt Toolkit: https://github.com/prompt-toolkit/python-prompt-toolkit
 - MetaCall CLI Source: https://github.com/metacall/core/tree/develop/source/cli/metacallcli
 - MetaCall CLI Idea GSoC 2021: https://github.com/metacall/gsoc-2021#cli-plugin-support

### Rust Actix + Inline TypeScript React Server Side Rendering (tsx)

**Skills**: Rust, TypeScript, React

**Expected size of the project**: Medium (175 hours)

**Difficulty rating**: Medium

**Description**:
Recently MetaCall has provided support for [inlining other languages into Rust](https://github.com/metacall/core/blob/adcc50496d53797011b87f42131cb857d0009ffb/source/ports/rs_port/tests/inline_test.rs#L13) though its macro system. This allows adding languages like Python or TypeScript into Rust easily. The main idea of this project is to create a Proof of Concept of an Actix server that easily embeds Server Side Rendering with TypeScript. This should be like a small framework which uses MetaCall and allows writing endpoint handlers where you can embed TypeScript directly with simplicity. In order to achieve this, the Rust Port will need to be extended, adding extra functionality required.

The Proof of Concept can contain also benchmarks, in order to compare it to other server side rendering solutions or in order to be the baseline for future optimizations in MetaCall TypeScript support. Adding documentation and examples is needed too, so it can be reused in the future by other users and the functionality and utility of the framework is shown.

**Expected outcomes**: A small framework based on Actix implementing inline server side rendering with React (TypeScript).

**Possible mentors**: Jose Antonio Dominguez, Alexandre Gimenez Fernandez, Vicente Eduardo Ferrer Garcia, Gil Arasa Verge

**Resources**:

 - MetaCall Rust Port Crate: https://docs.rs/metacall/latest/metacall/
 - MetaCall Rust Port Source: https://github.com/metacall/core/tree/develop/source/ports/rs_port
 - MetaCall FaaS SSR Example: https://github.com/metacall/basic-react-SSR-example

### Rust Loader Support

**Skills**: Rust, Rust Compiler API

**Expected size of the project**: Large (350 hours)

**Difficulty rating**: Hard

**Description**:
Recently MetaCall [has stared support for Rust](https://github.com/metacall/core/tree/develop/source/loaders/rs_loader), allowing to embed Rust code into other languages like NodeJS, Python, etc. The current state of the loader allows to load and compile Rust code, obtaining the list of functions and types. The work to be done is to implement FFI calls, some options have been investigated already, like [`abi_stable`](https://crates.io/crates/abi_stable) crate, although the API is quite complex, and it has limitations like async support (although [there are alternatives](https://crates.io/crates/async_ffi)). Another workaround in order to implement FFI is to use the Compiler API to automatically generate stubs on the fly for each function, as we use the same compiler version to build the code and to call it, may be there is no need to make the ABI stable.

Once a call has been done, another parts can be implemented, like `metacall_load_from_memory` which is basically an eval, or `metacall_load_from_package` which should be able to load precompiled packages. We may rely on `rlib` format, specially the `dylib` format which is the dynamic library version of `rlib` with [all the metadata information](https://rustc-dev-guide.rust-lang.org/backend/libs-and-metadata.html) required for the introspection in the `.rustc` section.

**Expected outcomes**: Complete Rust language support for MetaCall (including FFI and introspection information through Rust Compiler API).

**Possible mentors**: Vicente Eduardo Ferrer Garcia, Gil Arasa Verge, Fernando Vaño Garcia

**Resources**:

 - MetaCall Rust Loader Source: https://github.com/metacall/core/tree/develop/source/loaders/rs_loader
 - Rust ABI Stable Source: https://github.com/rodrimati1992/abi_stable_crates
 - Rust Async FFI Source: https://github.com/oxalica/async-ffi
 - Rust FFI Guide: https://michael-f-bryan.github.io/rust-ffi-guide/overview.html
 - Rust Compiler API: https://rustc-dev-guide.rust-lang.org/
 - Rust Live Reload: https://fasterthanli.me/articles/so-you-want-to-live-reload-rust

### Run MetaCall in Browser

**Skills**: WebAssembly, CMake, C/C++

**Expected size of the project**: Large (350 hours)

**Difficulty rating**: Hard

**Description**:
Nowadays WebAssembly is gaining a lot of traction, allowing more functionality each time and supporting as a compilation target in a wide variety of languages. One of the first attempts to be able to virtualize a Linux into the browser was JSLinux from Fabrice Bellard. This solution was implemented originally using `asm.js` and existing technology before the standardization of WebAssembly. The project has been refactored through the time but there are other alternatives nowadays which can be more powerful and production ready. A newer alternative to JSLinux is CheerpX, a modern compiler forked from LLVM for C/C++ to WebAssembly.

The objective of this idea is to create a Proof of Concept in order to run MetaCall in browser, other technologies different from the previously mentioned can be also considered, there is no restrictions. We should be able to create a very minimal ISO image that can be virtualized inside a browser which can execute MetaCall or at least a way to execute MetaCall in browser with few languages, so we can demonstrate the interoperability. The project should allow to create a base to run polyglot applications easily in the browser.

**Expected outcomes**: A working version of MetaCall running in browser (in WebAssembly), with the ability to interop between different languages.

**Possible mentors**: Thomas Rory Gummerson, Vicente Eduardo Ferrer Garcia, Gil Arasa Verge, Jose Antonio Dominguez, Alexandre Gimenez Fernandez

**Resources**:

 - JSLinux from Fabrice Bellard: https://bellard.org/jslinux/
 - CheerpX Compiler: https://github.com/leaningtech/cheerp-compiler
 - CheerpX WebVM Article: https://leaningtech.com/webvm-server-less-x86-virtual-machines-in-the-browser/
 - CheerpX WebVM Demo: https://webvm.io

### Polyglot Debugger

**Skills**: C/C++, Debuggers, VSCode (JavaScript/TypeScript)

**Expected size of the project**: Large (350 hours)

**Difficulty rating**: Hard

**Description**:
The objective of this project is to implement support in MetaCall for debugging, mainly in Visual Studio Code IDE, but it can be done with any other IDE that supports the debugging protocols from NodeJS, Python, etc. This project may imply to modify MetaCall Core in order to enable debugging, or it may be done in a completely new project that adds debugging capabilities to MetaCall CLI or Core. This project is highly experimental at the moment of writing, but there has been already some investigation respect to it. The easy part is to launch the runtime with debugging protocol enabled, for example: `node --inspect script.js`. But attaching to running processes is more complex. For the latter one there is a [requirement in MetaCall Core](https://github.com/metacall/core/issues/231) which makes it more difficult to implement.

The final result of this project should be to be able to debug a polyglot application, adding breakpoints and jumping between languages inside Visual Studio Code (or any other similar IDE). It may require to implement a derived launch plugin for unifiying other launcher plugins.

**Expected outcomes**: A Visual Studio Code extension capable of debug a polyglot application with MetaCall.

**Possible mentors**: Thomas Rory Gummerson, Vicente Eduardo Ferrer Garcia, Gil Arasa Verge, Jose Antonio Dominguez, Alexandre Gimenez Fernandez

**Resources**:

 - NodeJS Debugging: https://nodejs.org/en/docs/guides/debugging-getting-started/
 - Python Debugging in VSCode: https://code.visualstudio.com/docs/python/debugging
 - GraalVM Polyglot Live Extension: https://marketplace.visualstudio.com/items?itemName=hpi-swa.polyglot-live-programming
 - VSCode Managing Polyglot Environments: https://dev.to/gokayokyay/managing-vscode-profiles-for-the-polyglot-developers-5d1j

### Migrate existing CI/CD pipelines to GitHub

**Skills**: GitHub Actions, YAML, BASH, CMake

**Expected size of the project**: Medium (175 hours)

**Difficulty rating**: Easy

**Description**:
Currently, the Continuous Integration and Continuous Deployment pipelines are hosted on GitLab and uses their in-built GitLab CI. We use particular scripts to manage our automated testing and deployment through GitLab CI. The objective of this project is to migrate our CI/CD pipelines and deployment to GitHub Actions and GitHub Container Registry. GitHub Actions is free for public projects and does not limit our CI/CD minutes per month, hence providing a credible way of testing and deploying. Through GitHub Container Registry, we would like to centralize all our Docker images under our GitHub organization, while escaping the rate limiting issues around Docker Hub.

The project would involve writing workflows using GitHub Actions to automate this process and allow the project to be built on every push and pull request event. We would also like to implement remote caching for faster builds, CI/CD matrix for testing on various architectures, and improving the developer experience for contributors. Finally it would be interesting to document the whole CI/CD strategy and workflow, for rest of the community to adopt this work as a best practise.

**Expected outcomes**: A standardization of all MetaCall pipelines implemented on different platforms than GitHub.

**Possible mentors**: Harsh Bardhan Mishra, Gil Arasa Verge

**Resources**:

- GitHub Actions: https://github.com/features/actions
- GitHub Container Registry: https://ghcr.io/
- Ccache: https://ccache.dev/
- MetaCall on GitLab: https://gitlab.com/metacall/core

## Find Us

The three chats are bridged by Matrix (messages sent from one can be seen from all).


Telegram:
<a href="https://t.me/joinchat/BMSVbBatp0Vi4s5l4VgUgg" alt="Telegram"><img src="https://img.shields.io/static/v1?label=metacall&message=join&color=blue&logo=telegram&style=flat" /></a>

Discord: 
  <a href="https://discord.gg/upwP4mwJWa" alt="Discord"><img src="https://img.shields.io/discord/781987805974757426?label=discord&style=flat" /></a>

Matrix:
  <a href="https://matrix.to/#/#metacall:matrix.org" alt="Matrix"><img src="https://img.shields.io/matrix/metacall:matrix.org?label=matrix&style=flat" /></a>

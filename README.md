# proj270-基于 WASI 的 Linux 系统下安全可验证的 WASM 运行时

## 基于 WASI 的 Linux 系统下安全可验证的 WASM 运行时

### 项目描述

WebAssembly（Wasm）是一种二进制代码格式，融合了跨平台、高性能和安全的特性。虽然 Wasm 最初是为了提高 Web 性能而提出的，但其应用场景不仅局限于 Web 浏览器，它在各种应用场景中展现出其强大的潜力。它为开发者提供了使用 C、C++、Rust 等多种编程语言编写高效代码的能力，并通过将其编译为 WebAssembly 模块，实现跨平台运行。此外，WebAssembly 在受控的沙盒环境中执行，为应用提供了安全的运行环境。

Wasm Runtime（WebAssembly Runtime）是用于执行 WebAssembly 模块的运行时环境，它负责加载、解析和执行 Wasm 的二进制模块。Wasm Runtime 的关键功能包括加载，解析，执行，内存管理，导入和导出。一些常见的 Wasm Runtimes 有 Javascript V8、Wasmtime、WasmEdge 等，它们可以在不同的主机环境中独立运行 Wasm 模块。

WASI（WebAssembly System Interface）是 Wasm 的系统接口，提供 Wasm 模块与底层操作系统进行交互的标准化接口。WASI 提升了 Wasm 在不同操作系统和环境中的可移植性，它提供一种通用、标准化的接口，使得相同的 Wasm 模块能够在各种实现了 Wasi 的运行时上运行而无需修改。

Wasm 技术的其中一个关键应用方向在于使应用在各种主机系统上都能够安全执行而不对其他资源产生安全威胁，本题目需要通过沙箱等技术，构建一个高效、可移植、可验证安全性的 Wasm 运行时，其与主机系统交互时不仅能维护 Wasm 的内存安全，而且能通过 WASI 维护主机操作系统的资源的访问隔离。

### 预期目标

- 使用 Rust 语言实现一个的 Wasm runtime，能够在保持 Wasm 的强隔离安全属性的同时仍然允许这些程序通过 WASI 与外部世界交互；
- 该 Wasm runtime 能够通过主机调用和系统调用的安全性模糊测试；
- 在例如 LMbench、SQLite、SPEC CPU 等测试下，该 Wasm runtime 的 hostcall 执行性能能够优于 Wasmtime 等当下流行的 runtime 并保证其可验证的安全性；

### 特征

- 建模实际场景中试图破坏 Wasm runtime 沙箱和其他沙箱的恶意沙箱代码的威胁模型;
- 使用 Rust 语言设计 Wasm runtime 系统的自动验证器，能够动态地检查所有主机调用的参数是否符合内存安全、文件系统隔离和网络隔离这三项安全策略的要求;
- 实现一部分的 WASI 系统接口用于保证该 Wasm runtime 与主机系统交互时的内存安全、文件系统隔离和网络隔离。
- 详细地建模分析在该 Wasm runtime 下，操作系统内任何系统调用对安全性的影响；
- 实现完善的使用文档并开源
- 支持在Linux和Mac OS系统下的运行使用

### 已有参考资料

[1] Haas, Andreas, et al. "Bringing the web up to speed with WebAssembly." Proceedings of the 38th ACM SIGPLAN Conference on Programming Language Design and Implementation. 2017.  

[2] WebAssembly Specification: [WebAssembly Specification &#8212; WebAssembly 2.0 (Draft 2024-01-17)](https://webassembly.github.io/spec/core/)  

[3] WebAssembly system interface (WASI): [https://wasi.dev](https://wasi.dev)  

[4] Wasmtime: [https://wasmtime.dev](https://wasmtime.dev)  

[5] Fuzz testing: https://en.wikipedia.org/wiki/Fuzzing  

[6]W. Huang, “Add more operand stack overflow checks for fastinterp,” [GitHub - bytecodealliance/wasm-micro-runtime: WebAssembly Micro Runtime (WAMR)](https://github.com/bytecodealliance/wasm-micro-runtime/) commit/d6e781af281601e6b93601ebfcd6d2fd675960da, 2022.  

[7] T. Ridge, D. Sheets, T. Tuerk, A. Giugliano, A. Madhavapeddy, and
P. Sewell, “SibylFS: formal specification and oracle-based testing
for POSIX and real-world file systems,” in SOSP, 2015.  

[8] R. J. Connor, T. McDaniel, J. M. Smith, and M. Schuchard, “PKU
pitfalls: Attacks on PKU-based memory isolation systems,” in
USENIX Sec, 2020.  
### 赛题分类

2.4 运行时支撑大类 --> 2.4.99 未归类运行时支撑

### 参赛要求

- 以小组为单位参赛，最多三人一个小组，且小组成员是来自同一所高校的本科生（2024 春季学期或之后本科毕业的大一~大四的学生）或研究生；
- 如学生参加了多个项目，参赛学生选择一个自己参加的项目参与评奖；
- 参赛团队需要遵守比赛规则和技术要求，并且按照规定的时间节点提交设计方案、报告和演示材料。

### 难度

中等

### License

GPL-3.0 License

### 所属赛道

2024全国大学生操作系统毕设的“OS功能挑战”赛道

### 项目导师

- 姓名：李博睿
- 单位：东南大学
- GitHub ID: https://github.com/liborui
- Email：libr@seu.edu.cn

### 测试方法（仅列举部分）

比较对象：

1. Wasmer
2. Wasmtime 
3. WAMR

Benchmark：

1. SQLite
2. LMbench
3. SPEC2006 CPU

横向比较不同Runtime下的Benchmark结果

### 安全性验证（仅列举部分）

- 内存安全检查
  - 内存读写权限检查  
  - 内存访问长度检查

- 文件系统安全检查
  - 多文件访问检查
  - 文件访问模式检查

- 网络系统安全检查
  - 网络地址隔离检查
  - 网络协议安全检查

# 仓库指南

## 项目结构与模块组织

此仓库包含三个相互独立的 eduPIC 实现：`C/`、`Cpp/` 和 `Rust/`。Rust 版本使用 `Rust/src/main.rs`，其元数据位于 `Rust/Cargo.toml`。C 风格和 C++ 移植版都是单文件程序，分别位于 `C/eduPIC.cc` 和 `Cpp/eduPIC.cpp`。参考产物位于 `C/ReferenceCase_RawData/`，而 `C/eduPIC_manual.pdf` 记录了原始工作流程。请从各自的目录中运行每个实现，因为仿真会将 `picdata.bin`、`conv.dat`、`info.txt` 和 `*.dat` 输出写入当前工作目录。

## 构建、测试与开发命令

- `cd Rust && cargo run --release -- 0` 启动一次全新的 Rust 仿真。
- `cd Rust && cargo run --release -- 100 m` 继续运行 100 个周期，并写出测量输出。
- `g++ -O2 -std=c++11 Cpp/eduPIC.cpp -o Cpp/eduPIC` 构建 C++ 实现。
- `g++ -O2 -std=c++11 C/eduPIC.cc -o C/eduPIC` 构建 C 风格实现；尽管文件夹名如此，`.cc` 仍然需要使用 C++ 编译器。

Rust 命令假定存在一个完整的工具链以及系统链接器。此仓库没有提供仓库级的构建脚本或自动化测试目标。

## 编码风格与命名约定

请匹配现有的文件内风格，而不是激进地重新格式化。代码块内使用 4 个空格缩进，在已有对齐的常量声明处保持对齐，并保留文件顶部的横幅注释。遵循现有命名模式：如 `N_G` 和 `PRESSURE` 这类全大写常量，Rust 中使用 `lower_snake_case` 函数，以及 `C/` 和 `Cpp/` 中既有的过程式命名。如果你修改 Rust 代码，在可用时运行 `cargo fmt`；C/C++ 的格式请手动保持一致。

## 测试指南

仓库中没有提交单元测试套件。验证更改时，请运行一次简短的仿真，检查全新运行（`0`）和续跑两种情况，并在行为发生变化时，将生成的输出（如 `density.dat`、`eepf.dat`、`ifed.dat` 和 XT 分布）与 `C/ReferenceCase_RawData/` 进行比较。确认 `picdata.bin` 可以被正常写入并重新加载。

## 提交与拉取请求指南

此源码快照不包含 `.git` 目录，因此本地提交历史不可用。请使用简短的祈使式提交标题并明确范围，例如 `Rust: fix particle reload boundary`。保持每次提交只聚焦于一个实现，除非同一个数值改动需要在多个移植版本中同步。拉取请求应描述数值上的影响，列出运行过的命令，注明重新生成的输出文件，并在参考结果发生变化时附上图表或表格。

## 输出与数据清洁

不要提交生成的 `*.dat`、`*.bin` 或 `info.txt` 文件，除非你是有意更新参考数据。在开始新的基线运行前，先删除旧输出，以免续跑仿真混合数据集。
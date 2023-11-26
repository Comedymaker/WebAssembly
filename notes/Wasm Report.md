# Wasm Report

### What is WASM

+ WebAssembly本身是基于栈式虚拟机的二进制指令集，它可以作为各个高级语言的编译目标
+ 运行效率远高于JS
+ 大部分高级语言都可以编译为Wasm模块
+ 通过WASI (WebAssembly System Interface)可以直接运行在操作系统上，因此在嵌入式、IOT、云、边缘计算等领域都有应用场景

about WASM: https://dl.acm.org/doi/pdf/10.1145/3062341.3062363

### Wasm Specification

###### Modules

![img](E:\WebAssembly\notes\img\v2-91645e03d95eceebd23ceca5e9ff4b1b_720w.png)

<img src="img\image-20231118120417983.png" alt="image-20231118120417983" style="zoom:150%;" />

##### 代码结构

1. **模块（Module）**：
   - Wasm代码的基本单元，包含了执行所需的所有信息。
   - 模块由类型（types）、函数（functions）、表格（tables）、内存（memories）、全局变量（globals）和导出（exports）等组件组成。
2. **类型（Types）**：
   - 包含了函数签名，定义了函数的输入和输出类型。
3. **函数（Functions）**：
   - 包含了Wasm代码的实际逻辑。
   - 每个函数都有一个索引，以便在模块内部或从外部调用。
4. **表格（Tables）**：
   - 用于存储函数引用或其他数据。
   - 可以用于实现类似于函数指针表的功能。
5. **内存（Memories）**：
   - 用于存储数据，如整数、浮点数等。
   - 内存可以由Wasm模块访问和操作。
6. **全局变量（Globals）**：
   - 可以在模块中定义的全局变量。
7. **导出（Exports）**：
   - 定义了模块向外部暴露的功能和数据。

##### 运行流程

1. Decoding（译码）
2. Validation（验证）
3. Execution（执行）

##### 运行特性

1. **虚拟机（Virtual Machine）**：
   
   - Wasm代码在运行时由虚拟机执行。虚拟机负责将Wasm二进制代码转换为目标平台的机器码，并执行它。
   - WebAssembly仅是一种格式及其正式语义的规范，在实践中可以通过多种方式运行，包括在浏览器中、在独立运行时中以及通过解释器。在所有方法中，代码都以源代码的形式开始，然后编译为WebAssembly，并通过上述方法之一进行分发和运行。在浏览器中运行是最初预期的WebAssembly运行方式，如图2所示。WebAssembly在JavaScript引擎内部运行，该引擎将WebAssembly即时编译为本机代码并执行。目前，WebAssembly需要JavaScript的粘合层，用于加载WebAssembly模块，将模块编译为机器代码，实例化其导入，然后调用一些导出函数。
   - WebAssembly本身没有主函数的概念。相反，所有全局变量在实例化时初始化，然后WebAssembly充当一个可以从JavaScript调用的函数库。还有一些独立的WebAssembly运行时，可以在浏览器之外运行独立的WebAssembly模块。这些通常是即时编译器，可以在没有JavaScript粘合层的情况下本地运行WebAssembly。最后，WebAssembly也可以使用解释器运行，这带来了解释器通常的权衡，即更高的可移植性但性能较低。非浏览器解决方案，包括独立运行时和解释器，也可能支持称为WebAssembly System Interface（WASI）的标准扩展提案，该接口定义了WebAssembly跨不同平台执行系统调用的标准接口，允许独立的WebAssembly可执行文件。
   
2. **栈机（Stack machine）**：
   
   - WebAssembly的计算模型基于栈机。栈机（Stack Machine）是一种计算模型，它使用栈（stack）作为主要的内部数据结构。在栈机中，所有的操作都是通过对栈进行操作来完成的。
   - 简单指令对数据执行基本操作，它们从操作数栈中弹出参数，并将结果推回栈中。
   
   ![image-20231126114619678](img\image-20231126114619678.png)
   
3. **线性内存（Linear Memory）**：
   - Wasm使用线性内存模型，允许连续的、固定大小的内存区域。
   - 线性内存是一个连续的、可变的原始字节数组。这样的内存创建时有一个初始大小，但可以动态增长。程序可以在任意字节地址（包括未对齐的地址）上加载和存储线性内存中的值。整数加载和存储可以指定一个比相应值类型大小更小的存储大小。如果访问超出当前内存大小的范围，将触发陷阱。
   
4. **安全性（Security）**：
   - Wasm在运行时提供安全性和隔离性，确保Wasm模块不会对宿主环境造成损害。

### WASM与沙箱

##### 传统安全性策略

基于所有权与组的访问控制（基于多用户）

最大威胁：自己运行的代码有风险

##### 沙箱

代码不能直接与操作系统交互，宿主机（可能是浏览器，也可能是 wasm 运行时）将函数放入代码可以使用的沙箱中，可以逐一限制每个程序可以做什么

个人理解：前者是基于不同用户给予不同权限来保持安全性，后者则是基于功能设置权限，即对于某个完成特定功能的沙箱，它就只能使用指定的、限制的安全系统调用，这部分是由宿主机决定的

TODO: 延伸了解一下容器

##### vs 容器

容器：将应用及其依赖项打包到一个称为容器的单独单元中，以便能够在不同的计算环境中运行

沙箱：一种安全机制，为执行中的程序提供隔离环境

### WASI & POSIX

##### POSIX

POSIX: 可移植操作系统接口（Portable Operating System Interface of UNIX ）

POSIX本身其实就是一套接口规范，规定了每个API需要实现的功能，具体由对应的操作系统来实现

about POSIX: https://zhuanlan.zhihu.com/p/392588996

##### WASI

WASI: WebAssembly 系统接口（WebAssembly System Interface）

WASI和WASM Runtime是相互配合的，WASM Runtime在执行过程中需要使用WASI提供的系统调用和接口来与底层系统进行交互

WASI和POSIX: 像POSIX这样的系统接口标准是针对特定的一类操作系统（UNIX）的，而WASI是一种为WebAssembly设计的通用系统接口标准，可在不同的计算环境中运行

about WASI: https://zhuanlan.zhihu.com/p/61423010

### WebGPU

https://developer.chrome.com/blog/webgpu-cross-platform/

https://github.com/juj/wasm_webgpu

TODO

### WASM for ML

https://github.com/WebAssembly/wasi-nn

TODO

##### ChatGPT

> 1. **性能和效率**：
>    - Wasm的设计目标之一是提供高性能的执行环境。这使得它成为在浏览器中运行机器学习模型的理想选择，尤其是对于需要高性能的任务，例如图像识别、语音识别等。
> 2. **跨平台性**：
>    - Wasm是可移植的二进制代码格式，可以在不同平台上运行，这种特性使得可以在各种设备和系统上使用相同的机器学习模型，而无需重新编写代码。
> 3. **边缘计算**：
>    - 机器学习模型在边缘设备上的执行对于许多应用场景非常重要。Wasm可以帮助将这些模型部署到边缘设备，因其轻量级和高效的特性，可以在资源受限的设备上运行机器学习模型。
> 4. **模型部署和分享**：
>    - Wasm可以使机器学习模型更易于部署和分享。通过将模型编译为Wasm格式，可以将模型作为一个单独的二进制文件进行分发，而不必依赖于特定的机器学习框架或平台。
> 5. **Web平台上的应用**：
>    - 在Web平台上，Wasm可以让机器学习模型在浏览器中运行，而无需依赖于插件或特定的扩展。这为开发者提供了在Web应用中利用机器学习功能的可能性。
> 6. **库和工具支持**：
>    - 一些机器学习框架和工具已经开始支持将模型编译为Wasm格式，这为开发者提供了更多选择，并使得在不同环境中部署机器学习模型更加方便。

### WASM for Edge Computing —— First Part

based on:

https://www.infoworld.com/article/3703052/how-webassembly-will-transform-edge-computing.html#:~:text=The%20WebAssembly%20Component%20Model%20supercharges,suit%20more%20varied%20use%20cases.

##### ChatGPT

> WebAssembly（Wasm）是一种低级编程语言，可以在支持的平台上高效地运行，它的设计目标之一是提供高性能的执行环境。Edge Computing（边缘计算）则是一种计算模式，强调在离数据产生或消费的地方进行数据处理和计算，以减少延迟并提高效率。
>
> WebAssembly可以与Edge Computing相结合，以提供更高效的边缘计算方案。Wasm的轻量级和高性能特性使其成为在边缘设备上执行计算密集型任务的理想选择。它可以使边缘设备（如传感器、智能手机、物联网设备等）能够运行复杂的应用程序和算法，而无需依赖于传统的高级编程语言或特定硬件。
>
> 通过WebAssembly，开发人员可以将代码编译成一种可移植的格式，然后在各种设备上运行，这使得在边缘计算环境中部署和执行代码变得更加容易和高效。边缘计算场景中，Wasm的使用可以提高设备的计算性能、节省带宽并减少数据传输，从而更有效地处理数据和执行任务。
>
> 因此，WebAssembly和Edge Computing可以结合使用，以提供更高效、灵活和响应速度更快的边缘计算解决方案。

##### 抽象层级的发展

![webassembly edge computing 02](img\Epochs of technology)

##### Wasm相比于K8S（轻量级）

作为下一个重要的技术抽象层，Wasm致力于解决嵌入到每个应用程序中的日常依赖性的共同复杂性。它解决了横跨多个云和边缘进行分布的应用程序运行的成本问题，以满足严格的性能和可靠性要求。

Wasm的微小体积和安全沙盒意味着它可以在任何地方安全执行。冷启动时间在5到50微秒的范围内，Wasm有效地解决了冷启动问题。它既与Kubernetes兼容，又不依赖于它。其微小的大小意味着它可以比容器扩展到更高的密度，在许多情况下，甚至可以按需执行，每次调用都能保持高效率。但Wasm模块相对于微型K8s容器化应用程序究竟小了多少呢？

一个经过优化的Wasm模块通常大小在20KB到30KB左右。与Kubernetes容器（通常是几百MB）相比，我们想要分发的Wasm计算单元要小几个数量级。它们较小的尺寸减少了加载时间，增加了可移植性，并意味着我们甚至可以在更接近用户的地方运行它们。

##### Wasm与跨平台

跨边缘运行的应用程序经常面临设备多样性所带来的挑战。以向边缘设备传输视频为例。在这些设备上，有成千上万种独特的操作系统、硬件和版本组合，需要为您的应用程序提供高性能的扩展。如今，团队通过为每个部署域构建不同版本的应用程序来解决这个问题——一个用于Windows，一个用于Linux，一个用于Mac，针对x86架构。通用组件有时可以跨越边界，但它们往往受到微妙的差异和漏洞的困扰。这令人筋疲力尽。

WebAssembly组件模型允许我们将应用程序分解为基于合同的、可在运行时热插拔的组件，平台可以根据需要在运行时满足。与在构建时将代码静态编译到每个二进制文件中不同，开发人员、架构师或平台工程师可以选择与该合同匹配的任何组件。这使得在不修改任何代码的情况下，可以简单地将应用程序迁移到许多云、边缘、服务或部署环境中。

这意味着像wasmCloud这样的平台可以在运行时加载最新和最新版本的组件，使开发人员免受昂贵的逐个应用程序的维护之苦。此外，不同的组件可以根据上下文、当前操作条件、隐私、安全性或任何其他因素的组合在不同的时间和位置进行连接——而无需修改您的原始应用程序。开发人员摆脱了样板式的繁琐工作，有更多时间专注于功能开发。企业可以在数百甚至数千个应用程序上实现这些节省。

### WASM for Edge Computing —— Second Part

based on:

WebAssembly for Edge Computing:  Potential and Challenges

##### 关于迁移性

已有解决方案：

1. 虚拟机（Virtual Machine）
2. 容器（container）
3. Java —— 缺乏语言独立性、太过庞大
4. JavaScript —— 缺乏语言独立性、性能过差

![image-20231126112845842](img\Comparison of code execution environments)

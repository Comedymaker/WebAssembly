# WebAssembly (Wasm)

WebAssembly is a new type of code that can be run in modern web browsers — it is a low-level assembly-like language with a compact binary format that runs with near-native performance and provides languages such as C/C++, C# and Rust with a compilation target so that they can run on the web. It is also designed to run alongside JavaScript, allowing both to work together.

##### what is WebAssembly

https://www.zhihu.com/question/304577684

##### The whole process

![image-20231020105005296](img\image-20231020105005296.png)

![image-20231020105343372](img\image-20231020105343372.png)



##### WebAssembly System Interface (Wasi)

+ WASI stands for WebAssembly System Interface. It's an API designed by the [Wasmtime](https://github.com/bytecodealliance/wasmtime) project that provides access to several operating-system-like features, including files and filesystems, Berkeley sockets, clocks, and random numbers, that we'll be proposing for standardization.
+ a brief intro: https://wasmbyexample.dev/examples/wasi-introduction/wasi-introduction.all.en-us.html

##### WasmEdge

[WasmEdge](https://github.com/WasmEdge/WasmEdge) is a lightweight, high-performance, and extensible WebAssembly runtime(*a stage of the programming lifecycle*)

some refs:

+ https://m.elecfans.com/article/1972391.html
+ [why docker and wasm work together](https://www.docker.com/blog/why-containers-and-webassembly-work-well-together/)

main users:

https://wasmedge.org/docs/contribute/users/

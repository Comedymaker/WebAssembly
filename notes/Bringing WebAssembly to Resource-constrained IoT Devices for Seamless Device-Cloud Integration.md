# Bringing WebAssembly to Resource-constrained IoT Devices for Seamless Device-Cloud Integration

### Background

##### WebAssembly

+ definition: WebAssembly is a bytecode format designed to run on a wide range of platforms. It also serves as a common compilation target of various high-level languages (e.g., Rust, C++) and exhibits better runtime performance than other CLRs.
+ advantage: WebAssembly seems to be a promising technology to achieve seamless device-cloud integration on resource-constrained devices by its portability and efficiency.

##### 3 approaches to run the bytecode

+ **Interpreter**: repeatedly reads one instruction of the bytecode and translates it to machine code which is easy to implement and requires few resources
+ **JIT**: compile the code while running
+ **AOT**: compile the code before running it

### Problem

##### Main problem

resource-constrained devices, which are commonly deployed in the wild, have difficulty participating in this device-cloud integration because they can hardly run WebAssembly efficiently.

##### 3 challenges for supporting device-cloud integration with WebAssembly on resource-constrained devices

+ To  support the device-cloud integrated application and execute it efficiently on resource-constrained devices —— Need a dedicated runtime to translate bytecode into native instruction
+ To guarantee the sandboxed execution of WebAsssembly
+ To optimize the energy consumption of the on-device execution

##### Solutions for upper 3 challenges

+  WAIT advocates a loading agent to regularly communicate with the cloud server and load the WebAssembly module if available. **A lightweight AOT compiler** and **a series of memory optimizations** lie inside the agent to correctly and efficiently execute WebAssembly on resource-constrained devices.
+ WAIT introduces memory **safety** and **control-flow integrity** checks. To minimize the overhead during execution, WAIT moves most of the checks to the AOT compilation stage and carefully selects the instructions being checked.
+ WAIT provides **IoT-related APIs** for peripheral-accessing and duty-cycling to facilitate complete IoT programming. Moreover, WAIT adopts the **bulk instruction writing** and the **I/O direct accessing** approach to reduce the energy consumption of both compile- and run-time

### WAIT

##### System workflow

![image-20231030102321705](img\image-20231030102321705.png)


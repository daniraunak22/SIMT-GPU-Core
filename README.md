# GPU Streaming Multiprocessor SIMT RTL Diagram

This repository contains the RTL diagrams for a custom GPU Streaming Multiprocessor (SM) that I designed using a warp-based SIMT (Single Instruction, Multiple Threads) model. The diagrams capture how a modern GPU core handles multiple warps, per-thread divergence, and memory banking, all at the register-transfer level. This project is fully diagrammed—I did not write SystemVerilog—but it shows the full data flow and control logic across the GPU pipeline.

The GPU is organized around a 5-stage pipeline: Fetch, Decode, Execute, Memory, and Writeback. Warp IDs and per-thread active masks are passed through every stage to make sure that divergent threads execute correctly. I also included a branch unit, a warp state table, and a SIMT stack to handle threads that take different paths on a branch and then reconverge properly.

To manage multiple warps, I designed a round-robin warp scheduler. Each warp has its own state, including program counter and active mask, and the scheduler issues instructions only for warps that are ready. This ensures that multiple warps can share the pipeline efficiently while keeping execution correct.

Memory operations are represented with a banked scratchpad memory model. I used four banks in this design, showing how loads and stores map to banks, how bank conflicts are detected, and how threads are replayed if conflicts occur. The diagrams also show how pipeline stalling can prevent data hazards, making memory accesses safe and predictable.

Overall, this project demonstrates a full GPU SM at the RTL level. The diagrams show how the pipeline, warp scheduler, SIMT stack, and memory all work together to handle multi-warp execution, branching, and shared memory access. It’s intended as a clear visualization of GPU architecture concepts, SIMT control flow, and banked memory management.

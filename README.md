## Custom GPU Streaming Multiprocessor (SIMT) RTL Diagrams

This repository contains RTL diagrams for a custom GPU Streaming Multiprocessor (SM) that I designed using a warp-based SIMT (Single Instruction, Multiple Threads) model. The diagrams illustrate how multiple warps are executed in a GPU-like pipeline, how per-thread branching and reconvergence is handled, and how a banked scratchpad memory system is managed. All designs are diagrammed at the RTL level—I did not write SystemVerilog—but the diagrams fully capture data flow, control signals, and memory interactions.

Below is a step-by-step walkthrough of how the project was developed, with each step corresponding to an uploaded image.

# Step 1 — Top-Level Overview

Image: 1_top_level_overview.png

The project started with a simple top-level overview of the GPU SM. At this stage, I created a high-level block diagram with major functional units: the 5-stage pipeline, warp scheduler, and memory subsystem. No internal signals were drawn yet—this diagram served as a conceptual starting point to organize the design.

Step 2 — 5-Stage Pipeline

Image: 2_pipeline.png

Next, I designed the core 5-stage pipeline: Fetch, Decode, Execute, Memory, and Writeback. In this diagram, I illustrated how instructions flow through each stage, including how data moves between registers, ALUs, and memory interfaces. This stage also highlighted how scalar and vector operations are differentiated and how per-thread lanes could be tracked with active masks.

Step 3 — Warp State, SIMT Stack, and Branch Unit

Image: 3_warp_state_simt_branch.png

After the pipeline, I focused on control flow for warps. I diagrammed the Warp State Table, SIMT Stack, and Branch Unit, showing how warps track program counters and active threads. The SIMT stack handles divergence and reconvergence when threads in a warp take different paths through branches. This diagram clarifies how individual threads within a warp execute conditionally and how the branch unit updates the pipeline with the correct active masks.

Step 4 — Warp Scheduler Integration

Image: 4_warp_scheduler.png

With the pipeline and control-flow units complete, I integrated the Warp Scheduler. This diagram shows how the scheduler selects the next ready warp using a round-robin scheme and how it interacts with the Warp State Table and SIMT Stack. The scheduler ensures only valid, non-stalled warps are issued to the pipeline and that warp IDs and active masks propagate correctly through the stages.

Step 5 — Load/Store Unit (LSU)

Image: 5_lsu.png

Next, I designed the Load/Store Unit (LSU) and banked scratchpad memory system. I included four memory banks to show banked access, bank conflict detection, thread replay, and pipeline stalling. The diagram details how each memory request is mapped to a bank, how conflicts are identified, and how stalled threads are managed to maintain correct execution timing.

Step 6 — Final Top-Level Integration

Image: 6_full_top_level.png

Finally, I combined all previous components into a complete top-level diagram with signals connecting every module. This diagram illustrates the full GPU SM, showing the pipeline, warp scheduler, SIMT stack, branch unit, warp state, and LSU all working together. Signals like warp IDs, active masks, PC values, and memory addresses are fully integrated, providing a clear end-to-end view of warp execution, memory operations, and control flow.

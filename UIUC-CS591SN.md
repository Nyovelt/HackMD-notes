# CS 591 Sys-Net
## Lecture2: Some interesting paper from ATC/OSDI 2022 - Xudong Sun
### Automatic Reliability Testing For Cluster Management Controllers
Fuzz test
### DuoAI
Ivy vs CoQ
- What is the advantages of Ivy compared to other verification language like CoQ? Because I have heard of CoQ but Ivy is new to me. 

### RESIN: Memory Leak
#### Previous Tech
- static 
- dynamic

人工智障

Fair idea - 首先内存如果慢慢上涨，那多半有问题。其次，在其他机器上运行没问题，那一定是你的问题。

- 误报率有点高

### Debugging the OmniTable Way
Idea: replay
Insight: Lazy materialization
>SteamDrill introduces lazy materialization as a solution.
Rather than materializing an OmniTable during execution,
SteamDrill uses deterministic record and replay [10] to cap-
ture a log of non-deterministic inputs to the execution. The
system uses the log to generate OmniTable state on-demand
by instrumenting and re-executing the original execution as
necessary to resolve debugging queries. Delaying OmniTable
materialization allows SteamDrill to filter OmniTable data
before extracting state instead of afterwards.

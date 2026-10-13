---
tags:
  - RECH
  - readings
data: 2026-09-29
---
# The Anatomy of a Misprediction: Tracking Terrestrial and Orbital Radiation-Induced Errors in Systolic Arrays
Source: [[✅ Tracking Terrestrial and Orbital Radiation-Induced Errors in Systolic Arrays_2026_Rennes_UniTN_UBC.pdf]]

## Summary
This paper presents a cross-layer analysis of how radiation-induced errors affect AI accelerators based on **systolic arrays** (like Google's TPU). The researchers combined beam experiments (physical radiation), RTL-level fault injection (hardware simulation), and software-level fault injection to track how a single bit-flip in hardware propagates into a "misprediction" in an AI model.

**Key Findings:**
- **Radiation Impact:** The Failure-in-Time (FIT) rate in orbit (LEO) is estimated to be at least 5 orders of magnitude higher than on the ground. Additionally, protons (the main radiation source in orbit) are on average **11× more likely** to induce a failure than neutrons (the main source on Earth).
- **Error Pattern Invariance:** The manifestation of error patterns (Single, Line, Box) is an intrinsic property of the systolic array hardware architecture, independent of the impinging particle type or the software convolution parameters.
- **Error Patterns:** Faults are categorized into three patterns: **Single** (single PE), **Line** (entire row/column), and **Box** (rectangular area).
- **Criticality:** "Box" and "Line" errors are far more likely to lead to critical system failures (SDCs) than single-bit flips, even though they are less frequent.
- **Hardware Vulnerability:** **Input buffers** are the most critical hardware component, accounting for nearly 50% of the vulnerable area and being the sole source of Box-pattern errors. The **MAC datapath** and the **Propagate register** are also high-risk resources.
- **Mitigation:** Applying ECC to input buffers can eliminate the most damaging Box-pattern errors, but this only covers about 46% of the overall Architectural Vulnerability Factor (AVF), leaving Line and Single error patterns unprotected.

## Conclusions of the Tests
The experimental results lead to several critical conclusions regarding AI accelerator reliability:
- **Model Robustness:** The quantization process (scaling, rounding, and clamping to the 8-bit signed range) and subsequent convolution operations provide some inherent resilience to single-element errors, as small perturbations are often absorbed before reaching the final output.
- **Spatial Vulnerability:** While single-bit errors are common, they are rarely critical. In contrast, **Line** and **Box** errors (which affect multiple PEs) bypass the model's internal robustness and have a critical SDC probability (PVF) that can reach up to 19%, making them the dominant source of mispredictions.
- **Resource Bottlenecks:** The **Propagate register** and **Input buffers** were identified as high-risk areas. Specifically, a fault in the Propagate register can steer weights to the wrong PEs, causing widespread errors.
- **Ineffectiveness of Simple ECC:** Applying ECC to input buffers eliminates Box patterns, but covers only ~46% of AVF. Full protection requires hardware-level redundancy or software-level fault-tolerant algorithms (like Algorithm-Based Fault Tolerance), especially for orbital deployments.
- **Vulnerability Distribution:** PVF is **model-dependent**. For some models (e.g., DeepLabv3), later layers are more vulnerable, meaning errors near the output are more likely to cause mispredictions. However, in architectures with skip connections (e.g., U-Net), early layers can be more critical because skip connections mask faults in later layers.

## Vocabulary
- **Systolic Array:** A network of processing elements (PEs) that rhythmically pass data to their neighbors, ideal for the matrix multiplications used in AI.
- **General Matrix Multiply**: GEMM
- **Weight Stationary dataflow**: in AI a dataflow where the AI model weight are stored in register internal to the computation unit, while the inputs change.
- **SEU (Single Event Upset)**: kind of error that manifest as logical bit flip.
- **RTL (Register Transfer Level):** A high-level description of the digital logic of a chip used for simulation.
- **SDC (Silent Data Corruption):** An error where the system provides an incorrect output without alerting the user or crashing.
- **CS (Cross-Section):** A measure of how sensitive a device is to radiation (probability of an error per particle flux).
- **HVF (Hardware Vulnerability Factor):** The probability that a fault in a specific hardware resource will actually lead to an observable error.
- **PVF (Program Vulnerability Factor):** The probability that a hardware error will propagate through the software/model to cause an incorrect final output.
- **AVF (Architectural Vulnerability Factor)**: defined as AVF = HVF × PVF, it gives the overall probability that a fault propagates from the low-level hardware through the application to produce an incorrect output.
- **FIT (Failure-in-Time):** The number of failures expected in $10^9$ device-hours of operation.
- **PE (Processing Element):** The basic computational unit in a systolic array.

## Conclusion:
The paper demonstrates that relying on simple ECC is insufficient for space-grade AI because the most critical failures stem from spatial, multi-bit error patterns (Line and Box faults) rather than isolated bit-flips. True reliability for orbital AI requires a co-design approach where hardware redundancy is paired with algorithm-level fault tolerance.

---

# Artificial Neural Networks for Space and Safety-Critical Applications: Reliability Issues and Potential Solutions
Source: [[✅Artificial_Neural_Networks_for_Space_and_Safety-Critical_Applications_Reliability_Issues_and_Potential_Solutions.pdf]]

## Summary
This paper provides a comprehensive review of the reliability challenges and evaluation methodologies for Artificial Neural Networks (ANNs) deployed in safety-critical and space environments. It emphasizes that while ANNs have inherent resilience to some faults, the complexity of modern AI hardware (GPUs, FPGAs, Edge devices) introduces significant vulnerabilities to radiation-induced transients.

**Key Points:**
- **Hardware Diversity:** The paper compares different acceleration architectures:
    - **GPUs:** High parallelism but vulnerable in functional units (ALUs/FPUs), register files, and the memory hierarchy (caches, shared memory).
    - **FPGAs:** Flexible but particularly vulnerable in **Configuration Memory (CRAM)**, where a single bit-flip can alter the circuit's behavior entirely. Additionally, hardware implementation of functions like `sigmoid` or `tanh` introduces precision-related vulnerabilities.
    - **Edge devices (e.g., Coral TPU):** Optimized for efficiency but have limited resources for redundancy and error protection, making them susceptible to SDCs.
- **Evaluation Hierarchy:** It defines a 5-level "reliability evaluation ladder" from the most realistic but expensive to the most efficient but abstract:
    1. **Beam Experiments:** Real radiation on real hardware (most realistic).
    2. **Circuit/Gate-Level Simulation:** Gate-level netlist simulation, detailed but computationally expensive.
    3. **RTL Fault Injection:** Balanced approach using hardware descriptions.
    4. **Microarchitectural Fault Injection:** Targets specific hardware structures.
    5. **Software/Application-Level Injection:** Fastest and most abstract, but may miss low-level hardware interactions.
- **The "Silent" Danger:** The primary concern is **Silent Data Corruption (SDC)**, where the AI provides a wrong answer without any system warning or crash. Importantly, the paper distinguishes between **tolerable SDCs** (e.g., a slightly shifted bounding box that doesn't affect system behavior) and **critical SDCs** (e.g., misdetection or false positives that can lead to safety failures).

## Vocabulary
- **SEU (Single Event Upset):** A transient fault where a particle strike flips a single bit in a memory cell or register.
- **TID (Total Ionizing Dose):** The cumulative radiation dose absorbed by a semiconductor over time, leading to permanent degradation.
- **SEL (Single Event Latch-up):** A destructive radiation effect that creates a short circuit, potentially destroying the device.
- **COTS (Commercial Off-The-Shelf):** Standard industrial components not specifically hardened for space, which are often used in AI due to their high performance.
- **AI-Specific Hardening Solutions:** Important mitigation strategies beyond basic redundancy include **Algorithm-Based Fault Tolerance (ABFT)**, **Smart Pooling** (filtering out-of-bounds values during max pooling), and **Fault-Aware Training** (injecting transient faults during the training phase).

## Conclusion:
The review highlights that there is no "one-size-fits-all" solution for AI reliability in space. While ANNs are naturally more robust than traditional software, the shift towards high-density COTS hardware increases the risk of SDCs. The path forward requires moving from simple fault injection to a structured "reliability ladder" that combines physical beam testing with detailed RTL simulations to ensure safety-critical performance.

---

# The Memory Wall: Past, Present, and Future of DRAM
Source: [[✅The Memory Wall_ Past, Present, and Future of DRAM.pdf]]

## Summary
This article explores the "Memory Wall"—the growing gap between the rapid increase in compute performance (FLOPs) and the stagnating scaling of DRAM (Dynamic Random Access Memory). While logic transistors have continued to shrink and speed up, DRAM density and bandwidth growth have slowed significantly, creating a severe bottleneck for AI accelerators.

**Key Points:**
- **The Scaling Crisis:** DRAM density scaling has slowed from its historical pace to roughly 2x every decade. This lag makes memory the primary limiting factor for large-scale AI models.
- **HBM (High Bandwidth Memory):** To fight the wall, the industry moved to HBM, which stacks DRAM dies vertically. While it provides massive bandwidth, it is extremely expensive and complex to manufacture, accounting for a huge portion of GPU costs (e.g., H100/B200). The base die of HBM is also moving to logic (FinFET) processes with custom interfaces.
- **Basic Architecture:** DRAM relies on a 1T1C (1 Transistor, 1 Capacitor) cell. The capacitor stores the charge (the bit), but since capacitors leak, the memory must be "refreshed" constantly, consuming power and creating latency.
- **Short-Term Roadmap:** Before true 3D DRAM, the industry is relying on **4F² cell layouts** and **Vertical Channel Transistors (VCT)** for continued scaling.
- **The Future:** To overcome the wall, the industry is looking toward **3D DRAM** (which shifts the primary manufacturing bottleneck from lithography to vertical deposition and high-aspect-ratio etching) and **CIM (Compute-In-Memory)** to reduce data movement.

## Vocabulary
- **Memory Wall:** The phenomenon where the processor's speed increases much faster than the memory's ability to provide data, leaving the CPU/GPU idling.
- **1T1C:** The fundamental DRAM cell structure consisting of one access transistor and one storage capacitor.
- **Refresh Cycle:** The process of reading and rewriting the charge in a DRAM cell to prevent data loss due to leakage.
- **Sense Amplifier:** A circuit that detects the tiny voltage change on a bitline when a cell is accessed and amplifies it to a full logic 0 or 1.
- **HBM (High Bandwidth Memory):** A high-performance RAM interface that uses TSVs (Through-Silicon Vias) to stack memory dies, placing them closer to the processor.
- **CIM (Compute-In-Memory):** An architectural approach where computations are performed directly within the memory array, eliminating the need to move data across the memory bus.

## Conclusion:
The "Memory Wall" is no longer just a technical limitation but an economic one, as the cost and complexity of HBM scale aggressively. 3D DRAM represents the lowest-risk long-term option for scaling, shifting the core manufacturing challenge from lithography to vertical deposition and etching. Concurrently, the industry must develop CIM (Compute-In-Memory) to complement 3D DRAM by eliminating the energy and latency costs of data movement.

---

# How We’ll Reach a 1 Trillion Transistor GPU
Source: [[✅How We’ll Reach a 1 Trillion Transistor GPU - IEEE Spectrum.pdf]]

## Summary
This article discusses the roadmap for scaling GPU transistor counts to the trillion-device mark to meet the relentless demands of Generative AI. Since single-chip designs have hit the "reticle limit" (the maximum size a lithography tool can print), the industry is shifting from monolithic dies to **Integrated Chiplets** and **3D System Integration**.

**Key Points:**
- **The End of 2D Scaling:** The transition to chiplets marks the definitive end of 50 years of traditional 2D Moore's Law and Dennard Scaling.
- **Breaking the Reticle Limit:** Instead of trying to make one giant chip, manufacturers are using **CoWoS (Chip-on-Wafer-on-Substrate)** and **SoIC (System-on-Integrated-Chips)** to interconnect multiple smaller chiplets on a silicon interposer. Stacking massive compute and memory in 3D creates critical challenges in moving data (Memory Wall) and managing massive heat (1 kW+ power draws), necessitating these advanced packaging techniques.
- **3D Stacking:** The transition from 2D/2.5D to true 3D integration allows stacking compute dies and memory (like HBM) vertically, drastically increasing interconnection density and reducing energy costs.
- **Energy-Efficient Performance (EEP):** The focus has shifted to EEP (Throughput/Watt $\times$ Throughput), which is currently tripling every two years through EUV lithography, new materials, and system-technology co-optimization (STCO).
- **Silicon Photonics:** To avoid communication bottlenecks between GPUs in a cluster, optical interconnects are being integrated to allow hundreds of servers to act as a single, unified GPU.

## Vocabulary
- **Reticle Limit:** The maximum physical area (typically $\sim 800 \text{mm}^2$) that can be exposed by a lithography machine in a single step.
- **Chiplet:** A small, functional piece of a larger semiconductor design that can be manufactured independently and integrated into a larger system.
- **Interposer:** A silicon layer that sits between the chiplets and the substrate, providing high-density wiring to connect the dies.
- **CoWoS (Chip-on-Wafer-on-Substrate):** TSMC's advanced packaging technology for 2.5D integration of chips and HBM.
- **SoIC (System-on-Integrated-Chips):** A 3D integration technology that uses hybrid bonding (copper-to-copper) for much denser vertical connections than solder bumps.
- **TSV (Through-Silicon Via):** Vertical electrical connections that pass completely through a silicon wafer.
- **EUV (Extreme Ultraviolet) Lithography:** Advanced lithography using shorter wavelengths to print smaller, denser transistors.

## Conclusion:
The era of "just shrink the transistor" is over. Reaching a trillion transistors requires a paradigm shift toward **System-Technology Co-Optimization (STCO)**, where the chip is no longer a single device but a vertically integrated system of chiplets and optical interconnects, treating the entire server rack as a single compute unit.

---

# Thinking Inside the Box: Injecting Realistic Radiation Faults in ML Accelerators
Source:[[✅Thinking_Inside_the_Box_Injecting_Realistic_Radiation_Faults_in_ML_Accelerators.pdf]]

## Summary
This paper addresses the gap between simplified software-based fault injection (which often uses unrealistic single-bit flips) and expensive physical beam experiments. The authors propose **TPU-FI**, a software-level framework that uses data from actual accelerated beam experiments to inject "realistic" radiation faults—specifically "box-style" faults (rectangular regions of affected elements)—into ML models (tested on Google Coral Edge TPUs). 

**Key Findings:**
- **Realism:** TPU-FI can estimate the Failure-in-Time (FIT) rate of ML models within 1.7$\times$ of beam experiment results, making it a viable, low-cost alternative for reliability assessment.
- **Fault Geometry:** Radiation often affects multiple elements in a "box" rather than isolated bits. Traditional FI tools that only use single-bit flips can under-report misclassification rates by up to 4$\times$.
- **Model Sensitivity:** Both fully connected and convolutional layers are highly sensitive to these faults, whereas transpose convolution layers (used for upsampling) are highly robust.
- **Input Dependence:** Fault propagation is highly dependent on the input data; some inputs activate specific convolutional feature maps with large values that make the model more vulnerable to faults. Additionally, inputs with higher prediction confidence are much more robust to critical SDCs.
- **Quantization Effects:** Because the TPU quantizes accumulator results to `uint8`, resulting bit-flips are not uniform, typically affecting the least significant bit 59% of the time.

## Vocabulary
- **TPU-FI:** A software-based fault injection framework that injects realistic, data-driven radiation faults (e.g., box-style) into ML models.
- **Box-style Faults:** Faults that affect a rectangular region of elements in a tensor, reflecting how real particles strike hardware.
- **FIT (Failure-in-Time):** The rate of failures expected in $10^9$ device-hours.
- **SDC (Silent Data Corruption):** Any deviation in the model's output. The paper distinguishes between *tolerable SDCs* (final classification is unaffected) and *critical SDCs* (final classification changed).
- **Beam Experiment:** Testing hardware by exposing it to high-energy particle beams to simulate space radiation.

## Conclusion:
Traditional fault injection tools are often too optimistic because they ignore the spatial nature of radiation strikes. By integrating real-world beam data into a software framework (TPU-FI), developers can identify the most vulnerable "critical layers" of a model and apply targeted hardening (like selective duplication or clipping) to improve reliability without the prohibitive cost of constant physical testing.

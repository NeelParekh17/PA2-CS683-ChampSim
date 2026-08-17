# ChampSim Simulator Guide

ChampSim is a trace-based simulator for microarchitecture studies.

## Prerequisites & GCC 7 Setup (Ubuntu)

If running on newer Ubuntu systems, GCC 7 may be required for legacy compiler toolchains:

```bash
sudo apt update
sudo add-apt-repository ppa:ubuntu-toolchain-r/test
# Update sources list if needed
sudo apt-get install gcc-7 g++-7
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-7 0
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-7 0

# Set alternative interactively
sudo update-alternatives --config g++
sudo update-alternatives --config gcc
```

## Compilation

Build configurations specify the L1D prefetcher, L2C prefetcher, LLC prefetcher, branch predictor, and replacement policies:

### Non-Inclusive Baseline
```bash
./build_champsim.sh [L2_PREFETCHER]
# Example:
./build_champsim.sh no
./build_champsim.sh ip_stride
```

### Exclusive Hierarchy
```bash
./build_champsim_exclusive.sh [L2_PREFETCHER]
# Example:
./build_champsim_exclusive.sh no
```

## Running Simulations

Single-core simulation command:
```bash
./bin/champsim-exclusive-no \
    -warmup_instructions 25000000 \
    -simulation_instructions 25000000 \
    -traces <TRACE_DIR>/<TRACE_FILE>.champsimtrace.xz
```

### Key Parameters:
- `-warmup_instructions`: Number of instructions executed to warm up cache and predictor state (e.g., 25 million).
- `-simulation_instructions`: Number of detailed simulation instructions for statistics collection (e.g., 25 million).
- `-traces`: Path to `.champsimtrace.xz` trace file.

## Performance Evaluation Metrics
- **IPC (Instructions Per Cycle)**: Core throughput metric.
- **MPKI (Misses Per Kilo-Instruction)**: L1D, L1I, L2C, and LLC miss frequency per 1000 retired instructions.
- **Cache Hits / Misses / Evictions**: Detailed breakdown in stdout report at simulation completion.

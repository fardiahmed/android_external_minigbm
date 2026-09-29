# A16_RISCV/external/minigbm: android16-riscv

Changes made for the Android 16 (AOSP, riscv64) bring-up of the BananaPi BPI-F3 (SpacemiT K1) and the BananaPi BPI-SM10 (SpacemiT K3), on branch `android16-riscv`.

gbm_mesa backend fixes for the SpacemiT PowerVR GPU.

## Changes

- **gbm_mesa: fall back to the system dma-buf heap for dumb buffers**: GPUs outside separate_dc_gpu_list (SpacemiT K1/K3 PowerVR) leave gbm_node_fd on the render node, which has no dumb-buffer ioctls (ENOSYS). Linear YUV buffers for the VPU, camera and CPU only need a linear dma-buf, so allocate them from /dev/dma_heap/system instead.
- **gbm_mesa: give every BO a unique pseudo GEM handle**: The drv layer refcounts BOs and matches cached mappings by bo->handle. gbm_mesa BOs all used 0, so destroying one only freed it when no other BO was alive (leaking dup()ed plane fds and dma-buf memory) and a lock could reuse another BO's mapping.
- **gbm_mesa: import Mesa from vendor/spacemit/hardware/mesa**: The Mesa prebuilts moved from device/spacemit/k1 to their own repository, shared by the K1 and K3 boards.

## Build

```
./build.sh k1   # BPI-F3 (K1): kernel, bootloaders and Android images
./build.sh k3   # BPI-SM10 (K3), not booted yet
```

`./build.sh --help` lists the options (`--skip-kernel`, `--kernel-only`, ...).

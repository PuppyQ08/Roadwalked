#Background
Kernel is memory-bound and the theoretical occupancy is 25% limited by the resigter usage. Register/threads is 255.

#Plan
1. Apply restrict to global mem pointers.
   (without restrict, compiler assume that pointer might alias, pointing to overlapping memory, so everytime load or store from a pointer, compiler aasume others might have changed.
   Then it keeps alive values in register longer, instead of reloading from memory later.
2. Noinline the big helper function.
   (Register used by inline function won't be released after the function call).
3. Set index variable by volatile. Need to be tested. See [this](https://forums.developer.nvidia.com/t/tricks-to-fight-register-pressure-or-how-i-got-down-from-29-to-15-registers/16678)
4. launch_bounds the kernel. First number is maxThreadsPerBlock, so compiler can allocate more register to each thread. Because compiler assume we launch with 1024 threads.

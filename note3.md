## 静态 Pipelining

Hazards 分为 Structural，Data(RAW, WAR, WAW), Control, 解决Hazrds的方式有两种：

1. Dynamic 的（Tomasulo)。
2. Static 的 (通过 Compiler)。

编译器可以使用代码移动（code movement) 。通过代码移动可以缓解甚至消除 Hazards。

#### Unrolling

Unrolled Loop: 往往不知道循环的上界，如果是 $n$, 假设 Unroll the loop 去用 $k$ 份循环体的拷贝。

1. 先执行 $n\% k$ 次初始的循环体。
2. 再执行 unrolled body, 外层有一个 $n/k$ 次的循环。
3. 足够大的 $n$ 大多数时间都在运行 unrolled body。

##### Compiler Perspectives on Code Movements

编译器是关心的是程序中的依赖，而 HW hazard 和 pipelining 有关。对于寄存器是好判断的（确定名字即可），但内存是那一判断的（不同位置的寄存器值不同，对应的内存也不一样）。

#### Software Pipelining

重新排列循环使得每次 iteration 是由不同原循环不同的 iteration 中的指令组成。

##### Symbolic Loop Unrolling

扩大了结果到被使用间的距离，比 Unrolling 代码更短。

##### Loop-carried dependence

```cpp
for(i = 0; i < 100; ++i) {
    A[i + 1] = A[i] + C[i]; /// S1
    B[i + 1] = B[i] + A[i + 1]; /// S2
}
```

S2 用了 S1 的结果，没有办法并行。

但不是有 Loop-carried dependence 就没法并行。

#### VILW

每个指令包含多个操作。可以帮助处理 Loop Unrolling。
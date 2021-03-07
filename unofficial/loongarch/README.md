# 目前推测的 LoongArch 指令 (v20210311)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [注意事项](#%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9)
- [记法与约定](#%E8%AE%B0%E6%B3%95%E4%B8%8E%E7%BA%A6%E5%AE%9A)
- [基本特性](#%E5%9F%BA%E6%9C%AC%E7%89%B9%E6%80%A7)
- [寄存器规范](#%E5%AF%84%E5%AD%98%E5%99%A8%E8%A7%84%E8%8C%83)
  - [整数寄存器](#%E6%95%B4%E6%95%B0%E5%AF%84%E5%AD%98%E5%99%A8)
  - [浮点寄存器](#%E6%B5%AE%E7%82%B9%E5%AF%84%E5%AD%98%E5%99%A8)
- [指令列表](#%E6%8C%87%E4%BB%A4%E5%88%97%E8%A1%A8)
  - [`sext.h` 符号扩展 16 位 -> 原生宽度](#sexth-%E7%AC%A6%E5%8F%B7%E6%89%A9%E5%B1%95-16-%E4%BD%8D---%E5%8E%9F%E7%94%9F%E5%AE%BD%E5%BA%A6)
  - [`sext.b` 符号扩展 8 位 -> 原生宽度](#sextb-%E7%AC%A6%E5%8F%B7%E6%89%A9%E5%B1%95-8-%E4%BD%8D---%E5%8E%9F%E7%94%9F%E5%AE%BD%E5%BA%A6)
  - [`addw` 32 位加（三寄存器）](#addw-32-%E4%BD%8D%E5%8A%A0%E4%B8%89%E5%AF%84%E5%AD%98%E5%99%A8)
  - [`add` 原生宽度加（三寄存器）](#add-%E5%8E%9F%E7%94%9F%E5%AE%BD%E5%BA%A6%E5%8A%A0%E4%B8%89%E5%AF%84%E5%AD%98%E5%99%A8)
  - [`subw` 32 位有符号减（三寄存器）](#subw-32-%E4%BD%8D%E6%9C%89%E7%AC%A6%E5%8F%B7%E5%87%8F%E4%B8%89%E5%AF%84%E5%AD%98%E5%99%A8)
  - [`sub` 原生宽度有符号减（三寄存器）](#sub-%E5%8E%9F%E7%94%9F%E5%AE%BD%E5%BA%A6%E6%9C%89%E7%AC%A6%E5%8F%B7%E5%87%8F%E4%B8%89%E5%AF%84%E5%AD%98%E5%99%A8)
  - [`selnez` 非零则选择](#selnez-%E9%9D%9E%E9%9B%B6%E5%88%99%E9%80%89%E6%8B%A9)
  - [`seleqz` 为零则选择](#seleqz-%E4%B8%BA%E9%9B%B6%E5%88%99%E9%80%89%E6%8B%A9)
  - [`nor` 按位或非（三寄存器）](#nor-%E6%8C%89%E4%BD%8D%E6%88%96%E9%9D%9E%E4%B8%89%E5%AF%84%E5%AD%98%E5%99%A8)
  - [`and` 按位与（三寄存器）](#and-%E6%8C%89%E4%BD%8D%E4%B8%8E%E4%B8%89%E5%AF%84%E5%AD%98%E5%99%A8)
  - [`or` 按位或（三寄存器）](#or-%E6%8C%89%E4%BD%8D%E6%88%96%E4%B8%89%E5%AF%84%E5%AD%98%E5%99%A8)
  - [`xor` 按位异或（三寄存器）](#xor-%E6%8C%89%E4%BD%8D%E5%BC%82%E6%88%96%E4%B8%89%E5%AF%84%E5%AD%98%E5%99%A8)
  - [`sll` 逻辑左移（三寄存器）](#sll-%E9%80%BB%E8%BE%91%E5%B7%A6%E7%A7%BB%E4%B8%89%E5%AF%84%E5%AD%98%E5%99%A8)
  - [`sbs` 如位域有置位则置位（Set if Bits Set）](#sbs-%E5%A6%82%E4%BD%8D%E5%9F%9F%E6%9C%89%E7%BD%AE%E4%BD%8D%E5%88%99%E7%BD%AE%E4%BD%8Dset-if-bits-set)
  - [`srl` 逻辑右移（三寄存器）](#srl-%E9%80%BB%E8%BE%91%E5%8F%B3%E7%A7%BB%E4%B8%89%E5%AF%84%E5%AD%98%E5%99%A8)
  - [`mul` 原生宽度乘（三寄存器）](#mul-%E5%8E%9F%E7%94%9F%E5%AE%BD%E5%BA%A6%E4%B9%98%E4%B8%89%E5%AF%84%E5%AD%98%E5%99%A8)
  - [`syscall` 系统调用](#syscall-%E7%B3%BB%E7%BB%9F%E8%B0%83%E7%94%A8)
  - [`ofs.w` 记录偏移量（4 字节长）（Offset by Words）](#ofsw-%E8%AE%B0%E5%BD%95%E5%81%8F%E7%A7%BB%E9%87%8F4-%E5%AD%97%E8%8A%82%E9%95%BFoffset-by-words)
  - [`slliw` 32 位逻辑左移（立即数）](#slliw-32-%E4%BD%8D%E9%80%BB%E8%BE%91%E5%B7%A6%E7%A7%BB%E7%AB%8B%E5%8D%B3%E6%95%B0)
  - [`slli` 原生宽度逻辑左移（立即数）](#slli-%E5%8E%9F%E7%94%9F%E5%AE%BD%E5%BA%A6%E9%80%BB%E8%BE%91%E5%B7%A6%E7%A7%BB%E7%AB%8B%E5%8D%B3%E6%95%B0)
  - [`srliw` 32 位逻辑右移（立即数）](#srliw-32-%E4%BD%8D%E9%80%BB%E8%BE%91%E5%8F%B3%E7%A7%BB%E7%AB%8B%E5%8D%B3%E6%95%B0)
  - [`srli` 原生宽度逻辑右移（立即数）](#srli-%E5%8E%9F%E7%94%9F%E5%AE%BD%E5%BA%A6%E9%80%BB%E8%BE%91%E5%8F%B3%E7%A7%BB%E7%AB%8B%E5%8D%B3%E6%95%B0)
  - [`sraiw` 32 位算术右移（立即数）](#sraiw-32-%E4%BD%8D%E7%AE%97%E6%9C%AF%E5%8F%B3%E7%A7%BB%E7%AB%8B%E5%8D%B3%E6%95%B0)
  - [`srai` 原生宽度算术右移（立即数）](#srai-%E5%8E%9F%E7%94%9F%E5%AE%BD%E5%BA%A6%E7%AE%97%E6%9C%AF%E5%8F%B3%E7%A7%BB%E7%AB%8B%E5%8D%B3%E6%95%B0)
  - [`roriw` 32 位循环右移（立即数）](#roriw-32-%E4%BD%8D%E5%BE%AA%E7%8E%AF%E5%8F%B3%E7%A7%BB%E7%AB%8B%E5%8D%B3%E6%95%B0)
  - [`rori` 原生宽度循环右移（立即数）](#rori-%E5%8E%9F%E7%94%9F%E5%AE%BD%E5%BA%A6%E5%BE%AA%E7%8E%AF%E5%8F%B3%E7%A7%BB%E7%AB%8B%E5%8D%B3%E6%95%B0)
  - [`ext.w` 32 位位域提取](#extw-32-%E4%BD%8D%E4%BD%8D%E5%9F%9F%E6%8F%90%E5%8F%96)
  - [`mask` 位域掩码](#mask-%E4%BD%8D%E5%9F%9F%E6%8E%A9%E7%A0%81)
  - [`fadd.w` 浮点加（单精度）](#faddw-%E6%B5%AE%E7%82%B9%E5%8A%A0%E5%8D%95%E7%B2%BE%E5%BA%A6)
  - [`fadd.d` 浮点加（双精度）](#faddd-%E6%B5%AE%E7%82%B9%E5%8A%A0%E5%8F%8C%E7%B2%BE%E5%BA%A6)
  - [`fsub.w` 浮点减（单精度）](#fsubw-%E6%B5%AE%E7%82%B9%E5%87%8F%E5%8D%95%E7%B2%BE%E5%BA%A6)
  - [`fsub.d` 浮点减（双精度）](#fsubd-%E6%B5%AE%E7%82%B9%E5%87%8F%E5%8F%8C%E7%B2%BE%E5%BA%A6)
  - [`fmul.w` 浮点乘（单精度）](#fmulw-%E6%B5%AE%E7%82%B9%E4%B9%98%E5%8D%95%E7%B2%BE%E5%BA%A6)
  - [`fmul.d` 浮点乘（双精度）](#fmuld-%E6%B5%AE%E7%82%B9%E4%B9%98%E5%8F%8C%E7%B2%BE%E5%BA%A6)
  - [`fdiv.w` 浮点除（单精度）](#fdivw-%E6%B5%AE%E7%82%B9%E9%99%A4%E5%8D%95%E7%B2%BE%E5%BA%A6)
  - [`fdiv.d` 浮点除（双精度）](#fdivd-%E6%B5%AE%E7%82%B9%E9%99%A4%E5%8F%8C%E7%B2%BE%E5%BA%A6)
  - [`slti` 有符号小于立即数则置位](#slti-%E6%9C%89%E7%AC%A6%E5%8F%B7%E5%B0%8F%E4%BA%8E%E7%AB%8B%E5%8D%B3%E6%95%B0%E5%88%99%E7%BD%AE%E4%BD%8D)
  - [`sltiu` 无符号小于立即数则置位](#sltiu-%E6%97%A0%E7%AC%A6%E5%8F%B7%E5%B0%8F%E4%BA%8E%E7%AB%8B%E5%8D%B3%E6%95%B0%E5%88%99%E7%BD%AE%E4%BD%8D)
  - [`addiw` 32 位加（立即数）](#addiw-32-%E4%BD%8D%E5%8A%A0%E7%AB%8B%E5%8D%B3%E6%95%B0)
  - [`addi` 原生宽度加（立即数）](#addi-%E5%8E%9F%E7%94%9F%E5%AE%BD%E5%BA%A6%E5%8A%A0%E7%AB%8B%E5%8D%B3%E6%95%B0)
  - [`ati` 最高位立即数加（Add Top Immediate）](#ati-%E6%9C%80%E9%AB%98%E4%BD%8D%E7%AB%8B%E5%8D%B3%E6%95%B0%E5%8A%A0add-top-immediate)
  - [`andi` 按位与（立即数）](#andi-%E6%8C%89%E4%BD%8D%E4%B8%8E%E7%AB%8B%E5%8D%B3%E6%95%B0)
  - [`ori` 按位或（立即数）](#ori-%E6%8C%89%E4%BD%8D%E6%88%96%E7%AB%8B%E5%8D%B3%E6%95%B0)
  - [`xori` 按位异或（立即数）](#xori-%E6%8C%89%E4%BD%8D%E5%BC%82%E6%88%96%E7%AB%8B%E5%8D%B3%E6%95%B0)
  - [`lui` 装载高位立即数](#lui-%E8%A3%85%E8%BD%BD%E9%AB%98%E4%BD%8D%E7%AB%8B%E5%8D%B3%E6%95%B0)
  - [`ahi` 更高位立即数加（Add Higher Immediate）](#ahi-%E6%9B%B4%E9%AB%98%E4%BD%8D%E7%AB%8B%E5%8D%B3%E6%95%B0%E5%8A%A0add-higher-immediate)
  - [`auipc` PC-relative 高位立即数加](#auipc-pc-relative-%E9%AB%98%E4%BD%8D%E7%AB%8B%E5%8D%B3%E6%95%B0%E5%8A%A0)
  - [`lw.2` 另一个 32 位读](#lw2-%E5%8F%A6%E4%B8%80%E4%B8%AA-32-%E4%BD%8D%E8%AF%BB)
  - [`sw.2` 另一个 32 位写](#sw2-%E5%8F%A6%E4%B8%80%E4%B8%AA-32-%E4%BD%8D%E5%86%99)
  - [`ld.2` 另一个 64 位读](#ld2-%E5%8F%A6%E4%B8%80%E4%B8%AA-64-%E4%BD%8D%E8%AF%BB)
  - [`sd.2` 另一个 64 位写](#sd2-%E5%8F%A6%E4%B8%80%E4%B8%AA-64-%E4%BD%8D%E5%86%99)
  - [`lb` 符号扩展 8 位读](#lb-%E7%AC%A6%E5%8F%B7%E6%89%A9%E5%B1%95-8-%E4%BD%8D%E8%AF%BB)
  - [`lh` 符号扩展 16 位读](#lh-%E7%AC%A6%E5%8F%B7%E6%89%A9%E5%B1%95-16-%E4%BD%8D%E8%AF%BB)
  - [`lw` 符号扩展 32 位读](#lw-%E7%AC%A6%E5%8F%B7%E6%89%A9%E5%B1%95-32-%E4%BD%8D%E8%AF%BB)
  - [`ld` 64 位读](#ld-64-%E4%BD%8D%E8%AF%BB)
  - [`sb` 8 位写](#sb-8-%E4%BD%8D%E5%86%99)
  - [`sh` 16 位写](#sh-16-%E4%BD%8D%E5%86%99)
  - [`sw` 32 位写](#sw-32-%E4%BD%8D%E5%86%99)
  - [`sd` 64 位写](#sd-64-%E4%BD%8D%E5%86%99)
  - [`lbu` 零扩展 8 位读](#lbu-%E9%9B%B6%E6%89%A9%E5%B1%95-8-%E4%BD%8D%E8%AF%BB)
  - [`lhu` 零扩展 16 位读](#lhu-%E9%9B%B6%E6%89%A9%E5%B1%95-16-%E4%BD%8D%E8%AF%BB)
  - [`flw` 浮点 32 位读](#flw-%E6%B5%AE%E7%82%B9-32-%E4%BD%8D%E8%AF%BB)
  - [`fsw` 浮点 32 位写](#fsw-%E6%B5%AE%E7%82%B9-32-%E4%BD%8D%E5%86%99)
  - [`fld` 浮点 64 位读](#fld-%E6%B5%AE%E7%82%B9-64-%E4%BD%8D%E8%AF%BB)
  - [`fsd` 浮点 64 位写](#fsd-%E6%B5%AE%E7%82%B9-64-%E4%BD%8D%E5%86%99)
  - [`beqz` 为零则跳](#beqz-%E4%B8%BA%E9%9B%B6%E5%88%99%E8%B7%B3)
  - [`bnez` 非零则跳](#bnez-%E9%9D%9E%E9%9B%B6%E5%88%99%E8%B7%B3)
  - [`jalr` 跳并链接（寄存器）](#jalr-%E8%B7%B3%E5%B9%B6%E9%93%BE%E6%8E%A5%E5%AF%84%E5%AD%98%E5%99%A8)
  - [`j` 跳](#j-%E8%B7%B3)
  - [`jal` 跳并链接（立即数）](#jal-%E8%B7%B3%E5%B9%B6%E9%93%BE%E6%8E%A5%E7%AB%8B%E5%8D%B3%E6%95%B0)
  - [`beq` 相等则跳](#beq-%E7%9B%B8%E7%AD%89%E5%88%99%E8%B7%B3)
  - [`bne` 不等则跳](#bne-%E4%B8%8D%E7%AD%89%E5%88%99%E8%B7%B3)
  - [`bgt` 有符号大于则跳](#bgt-%E6%9C%89%E7%AC%A6%E5%8F%B7%E5%A4%A7%E4%BA%8E%E5%88%99%E8%B7%B3)
  - [`ble` 有符号小于等于则跳](#ble-%E6%9C%89%E7%AC%A6%E5%8F%B7%E5%B0%8F%E4%BA%8E%E7%AD%89%E4%BA%8E%E5%88%99%E8%B7%B3)
  - [`bgtu` 无符号大于则跳](#bgtu-%E6%97%A0%E7%AC%A6%E5%8F%B7%E5%A4%A7%E4%BA%8E%E5%88%99%E8%B7%B3)
  - [`bleu` 无符号小于等于则跳](#bleu-%E6%97%A0%E7%AC%A6%E5%8F%B7%E5%B0%8F%E4%BA%8E%E7%AD%89%E4%BA%8E%E5%88%99%E8%B7%B3)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## 注意事项

* 此文档系基于一定量的 LoongArch 二进制代码逆向工程的结果，并非来自龙芯官方。一切以龙芯或将发布的完整指令集文档为准。
* 此文档使用的助记符和语法借鉴了 RISC-V 和 MIPS 的汇编语言。部分新颖的指令助记符为生造，此时会附上简短的英文全称。

## 记法与约定

* 位的编号从 0 开始，0 为最低位（LSB）。
* `xxx[HI:LO]` 表示 `xxx` 的从低位 `LO`（含）到高位 `HI`（含）的位域。
* `simm` 表示该立即数域应被视作有符号数，如被用于更宽的操作，高位应作符号扩展。
* `uimm` 表示该立即数域应被视作无符号数，如被用于更宽的操作，高位应作零扩展。
* “原生宽度”指整数寄存器的宽度。
* 在 C 伪代码中，原生宽度的有符号、无符号数用 `intptr_t` 或 `uintptr_t` 类型表示。
* `PC` 是程序计数器，其值为当前执行的指令最低字节（LSB）所位于的虚拟地址。
* **UNPREDICTABLE** 含义与 MIPS 指令集手册中一致。

## 基本特性

* 有 32 个整数寄存器，32 个浮点寄存器。不清楚 FPU 是否必然存在。
* 目前应该仅定义了小端序（Little-endian）的 ABI 与硬件。不清楚是否存在大端序（Big-endian）硬件。
* 目前到手的二进制采用原生宽度均为 64 位。不清楚是否存在原生宽度为 32 位的 ABI 与硬件（但已推入上游的 triples 显然包含了类似 MIPS o32 与 n32 的 ABI）。三种 ABI 应该分别叫 64、32、x32。
* 推测 LoongArch 的指针宽度（虚拟地址空间大小）与原生宽度相同（除 x32 ABI 外）。
* 所有跳转指令均没有延迟槽，目前已知的跳转指令都是 PC-relative 的。
* 所有位运算、逻辑运算如未明确说明，均操作整个原生宽度。

关于 FPU：

* FPU 如果存在，按照一切常识，都应当遵循 IEEE-754 2008 规范。
* 浮点寄存器至少有 32 位，不清楚是否存在阉割版的 FPU（例如不支持双精度），也不清楚是否比 64 位还宽（复用为向量寄存器）。

本文档中假定浮点寄存器最宽为 64 位，但在叙述中不排除其更宽的可能性。

## 寄存器规范

### 整数寄存器

|编号|别名|保存方|备注|
|----|----|------|----|
|0|zero|-|读取时固定为 0，写入为空操作（但不影响可能产生的副作用）|
|1|ra|Callee|返回地址|
|2|tp|-|非常可能但不确定，线程指针；用户态不应修改|
|3|sp|Callee|栈指针|
|4-11|a0-a7|Caller|入参/临时量|
|12-20|t0-t8|Caller|临时量|
|21|gp|-|不确定，可能为全局指针；用户态不应修改|
|22|s9/fp|Callee|保证不被过程调用覆盖；可用作帧指针|
|23-31|s0-s8|Callee|保证不被过程调用覆盖|

注：

* 返回值寄存器复用 `a0-a1`，用于返回最多两倍原生宽度的数据。

### 浮点寄存器

|编号|别名|保存方|备注|
|----|----|------|----|
|0-23|f0-f23|Caller|具体用途分类不确定|
|24-31|fs0-fs7|Callee|保证不被过程调用覆盖|

## 指令列表

### `sext.h` 符号扩展 16 位 -> 原生宽度

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="7">二级选择码</th>
<th colspan="5">三级选择码</th>
<th colspan="5">源寄存器</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">000000</td>
<td align="center" colspan="4">0000</td>
<td align="center" colspan="7">0000000</td>
<td align="center" colspan="5">10110</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`sext.h rd, rj`

行为：将 rj 的低 16 位符号扩展到原生宽度，存入 rd

### `sext.b` 符号扩展 8 位 -> 原生宽度

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="7">二级选择码</th>
<th colspan="5">三级选择码</th>
<th colspan="5">源寄存器</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">000000</td>
<td align="center" colspan="4">0000</td>
<td align="center" colspan="7">0000000</td>
<td align="center" colspan="5">10111</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`sext.b rd, rj`

行为：将 rj 的低 8 位符号扩展到原生宽度，存入 rd

### `addw` 32 位加（三寄存器）

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="7">二级选择码</th>
<th colspan="5">源寄存器 2</th>
<th colspan="5">源寄存器 1</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">000000</td>
<td align="center" colspan="4">0000</td>
<td align="center" colspan="7">0100000</td>
<td align="center" colspan="5">rk</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`addw rd, rj, rk`

行为：`rd = ((int32_t) rj) + ((int32_t) rk)` 结果符号扩展

注：2 的补码表示中，有符号与无符号加法行为相同。

### `add` 原生宽度加（三寄存器）

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="7">二级选择码</th>
<th colspan="5">源寄存器 2</th>
<th colspan="5">源寄存器 1</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">000000</td>
<td align="center" colspan="4">0000</td>
<td align="center" colspan="7">0100001</td>
<td align="center" colspan="5">rk</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`add rd, rj, rk`

行为：`rd = rj + rk`

注：2 的补码表示中，有符号与无符号加法行为相同。

### `subw` 32 位有符号减（三寄存器）

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="7">二级选择码</th>
<th colspan="5">源寄存器 2</th>
<th colspan="5">源寄存器 1</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">000000</td>
<td align="center" colspan="4">0000</td>
<td align="center" colspan="7">0100010</td>
<td align="center" colspan="5">rk</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`subw rd, rj, rk`

行为：`rd = ((int32_t) rj) - ((int32_t) rk)` 结果符号扩展

### `sub` 原生宽度有符号减（三寄存器）

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="7">二级选择码</th>
<th colspan="5">源寄存器 2</th>
<th colspan="5">源寄存器 1</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">000000</td>
<td align="center" colspan="4">0000</td>
<td align="center" colspan="7">0100011</td>
<td align="center" colspan="5">rk</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`sub rd, rj, rk`

行为：`rd = ((intptr_t) rj) - ((intptr_t) rk)`

### `selnez` 非零则选择

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="7">二级选择码</th>
<th colspan="5">源寄存器 2</th>
<th colspan="5">源寄存器 1</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">000000</td>
<td align="center" colspan="4">0000</td>
<td align="center" colspan="7">0100110</td>
<td align="center" colspan="5">rk</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`selnez rd, rj, rk`

行为：`rd = rk ? rj : 0`

注：

可配合 `seleqz` 与 `or` 实现三目运算符的语义。欲计算 `R = C ? T : F` 可采用以下写法（会 clobber T 与 F）：

```plain
selnez T, T, C  // T = C ? T : 0
seleqz F, F, C  // F = C ? 0 : F
or     R, T, F  // T 与 F 有且仅有一个为全零
```

### `seleqz` 为零则选择

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="7">二级选择码</th>
<th colspan="5">源寄存器 2</th>
<th colspan="5">源寄存器 1</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">000000</td>
<td align="center" colspan="4">0000</td>
<td align="center" colspan="7">0100111</td>
<td align="center" colspan="5">rk</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`seleqz rd, rj, rk`

行为：`rd = rk ? 0 : rj`

注：

可配合 `selnez` 与 `or` 实现三目运算符的语义。欲计算 `R = C ? T : F` 可采用以下写法（会 clobber T 与 F）：

```plain
selnez T, T, C  // T = C ? T : 0
seleqz F, F, C  // F = C ? 0 : F
or     R, T, F  // T 与 F 有且仅有一个为全零
```

### `nor` 按位或非（三寄存器）

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="7">二级选择码</th>
<th colspan="5">源寄存器 2</th>
<th colspan="5">源寄存器 1</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">000000</td>
<td align="center" colspan="4">0000</td>
<td align="center" colspan="7">0101000</td>
<td align="center" colspan="5">rk</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`nor rd, rj, rk`

行为：`rd = rj | (~rk)`

注：

* 习惯上可用 `rj = zero` 起到逻辑非的作用，此时可写作 `not rd, rk`。
* 目前分析过的少量该指令对应的操作均为逻辑非，因此不能排除该指令实际上为逻辑异或非（xnor）的可能性，尽管该可能性非常渺茫。

### `and` 按位与（三寄存器）

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="7">二级选择码</th>
<th colspan="5">源寄存器 2</th>
<th colspan="5">源寄存器 1</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">000000</td>
<td align="center" colspan="4">0000</td>
<td align="center" colspan="7">0101001</td>
<td align="center" colspan="5">rk</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`and rd, rj, rk`

行为：`rd = rj & rk`

### `or` 按位或（三寄存器）

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="7">二级选择码</th>
<th colspan="5">源寄存器 2</th>
<th colspan="5">源寄存器 1</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">000000</td>
<td align="center" colspan="4">0000</td>
<td align="center" colspan="7">0101010</td>
<td align="center" colspan="5">rk</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`or rd, rj, rk`

行为：`rd = rj | rk`

注：习惯上，可用 rj = zero 起到寄存器间移动的作用，此时可写作伪指令 `mv rd, rk`。

### `xor` 按位异或（三寄存器）

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="7">二级选择码</th>
<th colspan="5">源寄存器 2</th>
<th colspan="5">源寄存器 1</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">000000</td>
<td align="center" colspan="4">0000</td>
<td align="center" colspan="7">0101011</td>
<td align="center" colspan="5">rk</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`xor rd, rj, rk`

行为：`rd = rj ^ rk`

### `sll` 逻辑左移（三寄存器）

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="7">二级选择码</th>
<th colspan="5">源寄存器 2</th>
<th colspan="5">源寄存器 1</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">000000</td>
<td align="center" colspan="4">0000</td>
<td align="center" colspan="7">0101110</td>
<td align="center" colspan="5">rk</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`sll rd, rj, rk`

行为：`rd = rj << rk`

注意：`rk < 0` 或 `rk >= 原生宽度` 时的行为未知。

### `sbs` 如位域有置位则置位（Set if Bits Set）

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="7">二级选择码</th>
<th colspan="5">源寄存器 2</th>
<th colspan="5">源寄存器 1</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">000000</td>
<td align="center" colspan="4">0000</td>
<td align="center" colspan="7">0110000</td>
<td align="center" colspan="5">rk</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`sbs rd, rj, rk`

行为：`rd = (rj & rk) != 0 ? 1 : 0`

### `srl` 逻辑右移（三寄存器）

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="7">二级选择码</th>
<th colspan="5">源寄存器 2</th>
<th colspan="5">源寄存器 1</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">000000</td>
<td align="center" colspan="4">0000</td>
<td align="center" colspan="7">0110010</td>
<td align="center" colspan="5">rk</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`srl rd, rj, rk`

行为：将 rj 逻辑右移 rk 位，存入 rd

注意：`rk < 0` 或 `rk >= 原生宽度` 时的行为未知。

### `mul` 原生宽度乘（三寄存器）

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="7">二级选择码</th>
<th colspan="5">源寄存器 2</th>
<th colspan="5">源寄存器 1</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">000000</td>
<td align="center" colspan="4">0000</td>
<td align="center" colspan="7">0111011</td>
<td align="center" colspan="5">rk</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`mul rd, rj, rk`

行为：rj 与 rk 相乘，低原生宽度位存入 rd

注：2 的补码表示中，有符号与无符号乘法行为相同。

### `syscall` 系统调用

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="7">二级选择码</th>
<th colspan="5">???</th>
<th colspan="5">???</th>
<th colspan="5">???</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">000000</td>
<td align="center" colspan="4">0000</td>
<td align="center" colspan="7">1010110</td>
<td align="center" colspan="5">00000</td>
<td align="center" colspan="5">00000</td>
<td align="center" colspan="5">00000</td>
</tr>
</tbody>
</table>

语法：`syscall`

行为：陷入内核态，执行系统调用后返回到下一条指令继续执行

注：

* 目前到手的 Linux 二进制分析结果显示，系统调用号置于 `a7`，参数按顺序置于 `a0-a6`，返回值置于 `a0`，其他寄存器值显然不变。

### `ofs.w` 记录偏移量（4 字节长）（Offset by Words）

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="7">二级选择码</th>
<th colspan="5">源寄存器 2</th>
<th colspan="5">源寄存器 1</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">000000</td>
<td align="center" colspan="4">0000</td>
<td align="center" colspan="7">1011001</td>
<td align="center" colspan="5">rk</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`ofs.w rd, rj, rk`

行为：`rd = 4 * rj + rk`

### `slliw` 32 位逻辑左移（立即数）

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="6">二级选择码</th>
<th colspan="6">立即数</th>
<th colspan="5">源寄存器</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">000000</td>
<td align="center" colspan="4">0001</td>
<td align="center" colspan="6">000000</td>
<td align="center" colspan="1">1</td>
<td align="center" colspan="5">uimm5</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`slliw rd, rj, uimm5`

行为：rj 低 32 位逻辑左移 uimm5 位，存入 rd 低 32 位，rd 其余位置零（如有）

注：习惯上，可用 `slliw rd, rj, 0` 起到零扩展 32 位到原生宽度的作用。

### `slli` 原生宽度逻辑左移（立即数）

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="6">二级选择码</th>
<th colspan="6">立即数</th>
<th colspan="5">源寄存器</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">000000</td>
<td align="center" colspan="4">0001</td>
<td align="center" colspan="6">000001</td>
<td align="center" colspan="6">uimm6</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`slli rd, rj, uimm6`

行为：`rd = rj << uimm6`

### `srliw` 32 位逻辑右移（立即数）

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="6">二级选择码</th>
<th colspan="6">立即数</th>
<th colspan="5">源寄存器</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">000000</td>
<td align="center" colspan="4">0001</td>
<td align="center" colspan="6">000100</td>
<td align="center" colspan="1">1</td>
<td align="center" colspan="5">uimm5</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`srliw rd, rj, uimm5`

行为：rj 低 32 位逻辑右移 uimm5 位，存入 rd 低 32 位，rd 其余位置零（如有）

### `srli` 原生宽度逻辑右移（立即数）

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="6">二级选择码</th>
<th colspan="6">立即数</th>
<th colspan="5">源寄存器</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">000000</td>
<td align="center" colspan="4">0001</td>
<td align="center" colspan="6">000101</td>
<td align="center" colspan="6">uimm6</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`srli rd, rj, uimm6`

行为：`rd = ((uintptr_t) rj) >> uimm6`

### `sraiw` 32 位算术右移（立即数）

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="6">二级选择码</th>
<th colspan="6">立即数</th>
<th colspan="5">源寄存器</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">000000</td>
<td align="center" colspan="4">0001</td>
<td align="center" colspan="6">001000</td>
<td align="center" colspan="1">1</td>
<td align="center" colspan="5">uimm5</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`sraiw rd, rj, uimm5`

行为：rj 低 32 位算术右移 uimm5 位，存入 rd 低 32 位，rd 其余位置为符号扩展（如有）

### `srai` 原生宽度算术右移（立即数）

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="6">二级选择码</th>
<th colspan="6">立即数</th>
<th colspan="5">源寄存器</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">000000</td>
<td align="center" colspan="4">0001</td>
<td align="center" colspan="6">001001</td>
<td align="center" colspan="6">uimm6</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`srai rd, rj, uimm6`

行为：`rd = rj >> uimm6`

### `roriw` 32 位循环右移（立即数）

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="6">二级选择码</th>
<th colspan="6">立即数</th>
<th colspan="5">源寄存器</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">000000</td>
<td align="center" colspan="4">0001</td>
<td align="center" colspan="6">001100</td>
<td align="center" colspan="1">1</td>
<td align="center" colspan="5">uimm5</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`roriw rd, rj, uimm5`

行为：rj 低 32 位循环右移 uimm5 位，存入 rd 低 32 位，rd 其余位置零（如有）

### `rori` 原生宽度循环右移（立即数）

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="6">二级选择码</th>
<th colspan="6">立即数</th>
<th colspan="5">源寄存器</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">000000</td>
<td align="center" colspan="4">0001</td>
<td align="center" colspan="6">001101</td>
<td align="center" colspan="6">uimm6</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`rori rd, rj, uimm6`

行为：rj 循环右移 uimm6 位，存入 rd

### `ext.w` 32 位位域提取

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="6">立即数 2</th>
<th colspan="6">立即数 1</th>
<th colspan="5">源寄存器</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">000000</td>
<td align="center" colspan="4">0001</td>
<td align="center" colspan="1">1</td>
<td align="center" colspan="5">uimm5b</td>
<td align="center" colspan="1">1</td>
<td align="center" colspan="5">uimm5a</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`ext.w rd, rj, uimm5a, uimm5b`

行为：提取 rj 低 32 位从第 uimm5a 位（含）到第 uimm5b 位（含）的位域内容，存入 rd。

即 `rd = (((uint32_t) rj) >> uimm5a) & ((1 << (uimm5b + 1) - 1)` 高位零扩展

注：

* `uimm5a > uimm5b` 时的行为未知，按照常识，应该为 **UNPREDICTABLE**。

### `mask` 位域掩码

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="6">立即数 2</th>
<th colspan="6">立即数 1</th>
<th colspan="5">源寄存器</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">000000</td>
<td align="center" colspan="4">0011</td>
<td align="center" colspan="6">uimm6b</td>
<td align="center" colspan="6">uimm6a</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`mask rd, rj, uimm6a, uimm6b`

行为：只保留 rj 从第 uimm6a 位（含）到第 uimm6b 位（含）的位域内容，其余位置零，存入 rd。

即 `rd = rj & ((1 << (uimm6b + 1)) - (1 << uimm6a))`

注：

* `uimm6a > uimm6b` 时的行为未知，按照常识，应该为 **UNPREDICTABLE**。

### `fadd.w` 浮点加（单精度）

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="7">二级选择码</th>
<th colspan="5">源寄存器 2</th>
<th colspan="5">源寄存器 1</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">000000</td>
<td align="center" colspan="4">0100</td>
<td align="center" colspan="7">0000001</td>
<td align="center" colspan="5">fk</td>
<td align="center" colspan="5">fj</td>
<td align="center" colspan="5">fd</td>
</tr>
</tbody>
</table>

语法：`fadd.w fd, fj, fk`

行为：`fd[31:0] = fj[31:0] + fk[31:0]` 高位 **UNPREDICTABLE**

### `fadd.d` 浮点加（双精度）

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="7">二级选择码</th>
<th colspan="5">源寄存器 2</th>
<th colspan="5">源寄存器 1</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">000000</td>
<td align="center" colspan="4">0100</td>
<td align="center" colspan="7">0000010</td>
<td align="center" colspan="5">fk</td>
<td align="center" colspan="5">fj</td>
<td align="center" colspan="5">fd</td>
</tr>
</tbody>
</table>

语法：`fadd.d fd, fj, fk`

行为：`fd[63:0] = fj[63:0] + fk[63:0]` 高位 **UNPREDICTABLE**

### `fsub.w` 浮点减（单精度）

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="7">二级选择码</th>
<th colspan="5">源寄存器 2</th>
<th colspan="5">源寄存器 1</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">000000</td>
<td align="center" colspan="4">0100</td>
<td align="center" colspan="7">0000101</td>
<td align="center" colspan="5">fk</td>
<td align="center" colspan="5">fj</td>
<td align="center" colspan="5">fd</td>
</tr>
</tbody>
</table>

语法：`fsub.w fd, fj, fk`

行为：`fd[31:0] = fj[31:0] - fk[31:0]` 高位 **UNPREDICTABLE**

### `fsub.d` 浮点减（双精度）

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="7">二级选择码</th>
<th colspan="5">源寄存器 2</th>
<th colspan="5">源寄存器 1</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">000000</td>
<td align="center" colspan="4">0100</td>
<td align="center" colspan="7">0000110</td>
<td align="center" colspan="5">fk</td>
<td align="center" colspan="5">fj</td>
<td align="center" colspan="5">fd</td>
</tr>
</tbody>
</table>

语法：`fsub.d fd, fj, fk`

行为：`fd[63:0] = fj[63:0] - fk[63:0]` 高位 **UNPREDICTABLE**

### `fmul.w` 浮点乘（单精度）

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="7">二级选择码</th>
<th colspan="5">源寄存器 2</th>
<th colspan="5">源寄存器 1</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">000000</td>
<td align="center" colspan="4">0100</td>
<td align="center" colspan="7">0001001</td>
<td align="center" colspan="5">fk</td>
<td align="center" colspan="5">fj</td>
<td align="center" colspan="5">fd</td>
</tr>
</tbody>
</table>

语法：`fmul.w fd, fj, fk`

行为：`fd[31:0] = fj[31:0] * fk[31:0]` 高位 **UNPREDICTABLE**

### `fmul.d` 浮点乘（双精度）

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="7">二级选择码</th>
<th colspan="5">源寄存器 2</th>
<th colspan="5">源寄存器 1</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">000000</td>
<td align="center" colspan="4">0100</td>
<td align="center" colspan="7">0001010</td>
<td align="center" colspan="5">fk</td>
<td align="center" colspan="5">fj</td>
<td align="center" colspan="5">fd</td>
</tr>
</tbody>
</table>

语法：`fmul.d fd, fj, fk`

行为：`fd[63:0] = fj[63:0] * fk[63:0]` 高位 **UNPREDICTABLE**

### `fdiv.w` 浮点除（单精度）

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="7">二级选择码</th>
<th colspan="5">源寄存器 2</th>
<th colspan="5">源寄存器 1</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">000000</td>
<td align="center" colspan="4">0100</td>
<td align="center" colspan="7">0001101</td>
<td align="center" colspan="5">fk</td>
<td align="center" colspan="5">fj</td>
<td align="center" colspan="5">fd</td>
</tr>
</tbody>
</table>

语法：`fdiv.w fd, fj, fk`

行为：`fd[31:0] = fj[31:0] / fk[31:0]` 高位 **UNPREDICTABLE**

### `fdiv.d` 浮点除（双精度）

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="7">二级选择码</th>
<th colspan="5">源寄存器 2</th>
<th colspan="5">源寄存器 1</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">000000</td>
<td align="center" colspan="4">0100</td>
<td align="center" colspan="7">0001110</td>
<td align="center" colspan="5">fk</td>
<td align="center" colspan="5">fj</td>
<td align="center" colspan="5">fd</td>
</tr>
</tbody>
</table>

语法：`fdiv.d fd, fj, fk`

行为：`fd[63:0] = fj[63:0] / fk[63:0]` 高位 **UNPREDICTABLE**

### `slti` 有符号小于立即数则置位

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="12">立即数</th>
<th colspan="5">源寄存器</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">000000</td>
<td align="center" colspan="4">1000</td>
<td align="center" colspan="12">simm12</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`slti rd, rj, simm12`

行为：`rd = ((intptr_t) rj) < simm12 ? 1 : 0`

### `sltiu` 无符号小于立即数则置位

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="12">立即数</th>
<th colspan="5">源寄存器</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">000000</td>
<td align="center" colspan="4">1001</td>
<td align="center" colspan="12">uimm12</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`sltiu rd, rj, uimm12`

行为：`rd = ((uintptr_t) rj) < uimm12 ? 1 : 0`

### `addiw` 32 位加（立即数）

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="12">立即数</th>
<th colspan="5">源寄存器</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">000000</td>
<td align="center" colspan="4">1010</td>
<td align="center" colspan="12">simm12</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`addiw rd, rj, simm12`

行为：`rd = ((int32_t) rj) + simm12` 结果符号扩展

注：

* 习惯上，用 `rj = zero` 的该指令表达装载小立即数的语义，此时可写作伪指令 `li rd, simm12`。

### `addi` 原生宽度加（立即数）

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="12">立即数</th>
<th colspan="5">源寄存器</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">000000</td>
<td align="center" colspan="4">1011</td>
<td align="center" colspan="12">simm12</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`addi rd, rj, simm12`

行为：`rd = rj + simm12`

### `ati` 最高位立即数加（Add Top Immediate）

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="12">立即数</th>
<th colspan="5">源寄存器</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">000000</td>
<td align="center" colspan="4">1100</td>
<td align="center" colspan="12">uimm12</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`ati rd, rj, uimm12`

行为：`rd = rj + (uimm12 << 52)`

注：

* 本指令与 MIPS R6 的 `dati` 指令类似，区别在于立即数域的宽度和装载的位域。
* 本指令可按需与 `lui ahi ori` 配合，以构造任意的 64 位立即数。

### `andi` 按位与（立即数）

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="12">立即数</th>
<th colspan="5">源寄存器</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">000000</td>
<td align="center" colspan="4">1101</td>
<td align="center" colspan="12">uimm12</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`andi rd, rj, uimm12`

行为：`rd = rj & uimm12`

注：

* 习惯上，使用 `andi zero, zero, 0x0` 表示空操作，此时可写作 `nop`。

### `ori` 按位或（立即数）

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="12">立即数</th>
<th colspan="5">源寄存器</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">000000</td>
<td align="center" colspan="4">1110</td>
<td align="center" colspan="12">uimm12</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`ori rd, rj, uimm12`

行为：`rd = rj | uimm12`

注：

* 本指令可按需与 `lui ahi ati` 配合，以构造任意的 64 位立即数。

### `xori` 按位异或（立即数）

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="12">立即数</th>
<th colspan="5">源寄存器</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">000000</td>
<td align="center" colspan="4">1111</td>
<td align="center" colspan="12">uimm12</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`xori rd, rj, uimm12`

行为：`rd = rj ^ uimm12`

### `lui` 装载高位立即数

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="1">选择码</th>
<th colspan="20">立即数</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">000101</td>
<td align="center" colspan="1">0</td>
<td align="center" colspan="20">uimm20</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`lui rd, uimm20`

行为：`rd = uimm20 << 12` 高位零扩展

注：

* 本指令与 MIPS、RISC-V 的同名指令类似，区别在于立即数域的宽度和装载的位域。
* 本指令可按需与 `ahi ati ori` 配合，以构造任意的 64 位立即数。

### `ahi` 更高位立即数加（Add Higher Immediate）

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="1">选择码</th>
<th colspan="20">立即数</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">000101</td>
<td align="center" colspan="1">1</td>
<td align="center" colspan="20">uimm20</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`ahi rd, uimm20`

行为：`rd += uimm20 << 32`

注：

* 本指令与 MIPS R6 的 `dahi` 指令类似，区别在于立即数域的宽度和装载的位域。
* 本指令可按需与 `lui ati ori` 配合，以构造任意的 64 位立即数。

### `auipc` PC-relative 高位立即数加

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="1">选择码</th>
<th colspan="20">立即数</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">000111</td>
<td align="center" colspan="1">0</td>
<td align="center" colspan="20">uimm20</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`auipc rd, uimm20`

行为：`rd = PC + uimm20 << 12`

### `lw.2` 另一个 32 位读

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="2">选择码</th>
<th colspan="14">立即数</th>
<th colspan="5">基址寄存器</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">001001</td>
<td align="center" colspan="2">00</td>
<td align="center" colspan="14">simm14</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`lw.2 rd, simm14(rj)`

行为：**不确定**，大致为 `rd = *((int32_t *)(rj + (simm14 << 2)))` 高位可能为符号扩展

注：

* 偏移量为立即数左移 2 位，因此实质上可表示的偏移量为 16 位宽的 4 的倍数。
* 与 `lw` 的其他区别未知。

### `sw.2` 另一个 32 位写

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="2">选择码</th>
<th colspan="14">立即数</th>
<th colspan="5">基址寄存器</th>
<th colspan="5">源寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">001001</td>
<td align="center" colspan="2">01</td>
<td align="center" colspan="14">simm14</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`sw.2 rd, simm14(rj)`

行为：**不确定**，大致为 `*((int32_t *)(rj + (simm14 << 2))) = rd`

注：

* 偏移量为立即数左移 2 位，因此实质上可表示的偏移量为 16 位宽的 4 的倍数。
* 与 `sw` 的其他区别未知。

### `ld.2` 另一个 64 位读

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="2">选择码</th>
<th colspan="14">立即数</th>
<th colspan="5">基址寄存器</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">001001</td>
<td align="center" colspan="2">10</td>
<td align="center" colspan="14">simm14</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`ld.2 rd, simm14(rj)`

行为：**不确定**，大致为 `rd = *((int64_t *)(rj + (simm14 << 2)))` 高位可能为符号扩展

注：

* 偏移量为立即数左移 2 位，因此实质上可表示的偏移量为 16 位宽的 4 的倍数。
* 与 `ld` 的其他区别未知。

### `sd.2` 另一个 64 位写

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="2">选择码</th>
<th colspan="14">立即数</th>
<th colspan="5">基址寄存器</th>
<th colspan="5">源寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">001001</td>
<td align="center" colspan="2">11</td>
<td align="center" colspan="14">simm14</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`sd.2 rd, simm14(rj)`

行为：**不确定**，大致为 `*((int64_t *)(rj + (simm14 << 2))) = rd`

注：

* 偏移量为立即数左移 2 位，因此实质上可表示的偏移量为 16 位宽的 4 的倍数。
* 与 `sd` 的其他区别未知。

### `lb` 符号扩展 8 位读

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="12">立即数</th>
<th colspan="5">基址寄存器</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">001010</td>
<td align="center" colspan="4">0000</td>
<td align="center" colspan="12">simm12</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`lb rd, simm12(rj)`

行为：`rd = *((int8_t *)(rj + simm12))`（高位符号扩展）

### `lh` 符号扩展 16 位读

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="12">立即数</th>
<th colspan="5">基址寄存器</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">001010</td>
<td align="center" colspan="4">0001</td>
<td align="center" colspan="12">simm12</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`lh rd, simm12(rj)`

行为：`rd = *((int16_t *)(rj + simm12))`（高位符号扩展）

### `lw` 符号扩展 32 位读

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="12">立即数</th>
<th colspan="5">基址寄存器</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">001010</td>
<td align="center" colspan="4">0010</td>
<td align="center" colspan="12">simm12</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`lw rd, simm12(rj)`

行为：`rd = *((int32_t *)(rj + simm12))`（高位符号扩展（如原生宽度大于 32 位））

### `ld` 64 位读

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="12">立即数</th>
<th colspan="5">基址寄存器</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">001010</td>
<td align="center" colspan="4">0011</td>
<td align="center" colspan="12">simm12</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`ld rd, simm12(rj)`

行为：`rd = *((int64_t *)(rj + simm12))`

### `sb` 8 位写

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="12">立即数</th>
<th colspan="5">基址寄存器</th>
<th colspan="5">源寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">001010</td>
<td align="center" colspan="4">0100</td>
<td align="center" colspan="12">simm12</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`sb rd, simm12(rj)`

行为：`*((int8_t *)(rj + simm12)) = (int8_t)rd`

### `sh` 16 位写

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="12">立即数</th>
<th colspan="5">基址寄存器</th>
<th colspan="5">源寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">001010</td>
<td align="center" colspan="4">0101</td>
<td align="center" colspan="12">simm12</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`sh rd, simm12(rj)`

行为：`*((int16_t *)(rj + simm12)) = (int16_t)rd`

### `sw` 32 位写

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="12">立即数</th>
<th colspan="5">基址寄存器</th>
<th colspan="5">源寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">001010</td>
<td align="center" colspan="4">0110</td>
<td align="center" colspan="12">simm12</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`sw rd, simm12(rj)`

行为：`*((int32_t *)(rj + simm12)) = (int32_t)rd`

### `sd` 64 位写

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="12">立即数</th>
<th colspan="5">基址寄存器</th>
<th colspan="5">源寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">001010</td>
<td align="center" colspan="4">0111</td>
<td align="center" colspan="12">simm12</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`sd rd, simm12(rj)`

行为：`*((int64_t *)(rj + simm12)) = (int64_t)rd`

### `lbu` 零扩展 8 位读

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="12">立即数</th>
<th colspan="5">基址寄存器</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">001010</td>
<td align="center" colspan="4">1000</td>
<td align="center" colspan="12">simm12</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`lbu rd, simm12(rj)`

行为：`rd = *((uint8_t *)(rj + simm12))`（高位零扩展）

### `lhu` 零扩展 16 位读

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="12">立即数</th>
<th colspan="5">基址寄存器</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">001010</td>
<td align="center" colspan="4">1001</td>
<td align="center" colspan="12">simm12</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`lhu rd, simm12(rj)`

行为：`rd = *((uint16_t *)(rj + simm12))`（高位零扩展）

### `flw` 浮点 32 位读

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="12">立即数</th>
<th colspan="5">基址寄存器</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">001010</td>
<td align="center" colspan="4">1100</td>
<td align="center" colspan="12">simm12</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">fd</td>
</tr>
</tbody>
</table>

语法：`flw fd, simm12(rj)`

行为：`fd[31:0] = *((int32_t *)(rj + simm12))` 高位 **UNPREDICTABLE**

### `fsw` 浮点 32 位写

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="12">立即数</th>
<th colspan="5">基址寄存器</th>
<th colspan="5">源寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">001010</td>
<td align="center" colspan="4">1101</td>
<td align="center" colspan="12">simm12</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">fd</td>
</tr>
</tbody>
</table>

语法：`fsw fd, simm12(rj)`

行为：`*((int32_t *)(rj + simm12)) = fd[31:0]`

### `fld` 浮点 64 位读

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="12">立即数</th>
<th colspan="5">基址寄存器</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">001010</td>
<td align="center" colspan="4">1110</td>
<td align="center" colspan="12">simm12</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">fd</td>
</tr>
</tbody>
</table>

语法：`fld fd, simm12(rj)`

行为：`fd[63:0] = *((int64_t *)(rj + simm12))` 高位 **UNPREDICTABLE**

### `fsd` 浮点 64 位写

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="4">选择码</th>
<th colspan="12">立即数</th>
<th colspan="5">基址寄存器</th>
<th colspan="5">源寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">001010</td>
<td align="center" colspan="4">1111</td>
<td align="center" colspan="12">simm12</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">fd</td>
</tr>
</tbody>
</table>

语法：`fsd fd, simm12(rj)`

行为：`*((int64_t *)(rj + simm12)) = fd[63:0]`

### `beqz` 为零则跳

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="16">立即数低位</th>
<th colspan="5">源寄存器</th>
<th colspan="5">立即数高位</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">010000</td>
<td align="center" colspan="16">simm21[15:0]</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">simm21[20:16]</td>
</tr>
</tbody>
</table>

语法：`beqz rj, label`

行为：`if (rj == 0) PC += simm21 * 4`

### `bnez` 非零则跳

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="16">立即数低位</th>
<th colspan="5">源寄存器</th>
<th colspan="5">立即数高位</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">010001</td>
<td align="center" colspan="16">simm21[15:0]</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">simm21[20:16]</td>
</tr>
</tbody>
</table>

语法：`bnez rj, LABEL`

行为：`if (rj != 0) PC += simm21 * 4`

### `jalr` 跳并链接（寄存器）

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="16">立即数?</th>
<th colspan="5">源寄存器</th>
<th colspan="5">目的寄存器</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">010011</td>
<td align="center" colspan="16">0000000000000000</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`jalr rd, rj`

行为：`rd = PC + 4; PC = rj`

注：

* 按照跳转指令的编码规律，所有其他跳转指令都带一个偏移量，那么该指令的未知位域也应该是偏移量（这样一来就与 RISC-V 的 `jalr` 用法完全相同），但目前观测到的该指令的该域均为全 0，因此不能排除其实际上为保留位的可能性。
* 如 `rd = ra` 则为常规的过程调用，汇编上可简化为 `jalr rj`。
* 如 `rd = zero` 则不会记录返回地址，此时汇编上可简化为 `jr rj`。
* `jalr zero, ra` 汇编上可简化为 `ret`。

### `j` 跳

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="16">立即数低位</th>
<th colspan="1">?</th>
<th colspan="9">立即数高位</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">010100</td>
<td align="center" colspan="16">simm25[15:0]</td>
<td align="center" colspan="1">?</td>
<td align="center" colspan="9">simm25[24:16]</td>
</tr>
</tbody>
</table>

语法：`j LABEL`

行为：`PC += simm25 * 4`

注：

* 目前观测到的该指令标 ? 的位均保持与立即数最高位相同（即符号扩展）。因此尚不能确定该指令的立即数究竟为 25 位（与老胡 PPT 保持一致）还是 26 位。

### `jal` 跳并链接（立即数）

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="16">立即数低位</th>
<th colspan="1">?</th>
<th colspan="9">立即数高位</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">010101</td>
<td align="center" colspan="16">simm25[15:0]</td>
<td align="center" colspan="1">?</td>
<td align="center" colspan="9">simm25[24:16]</td>
</tr>
</tbody>
</table>

语法：`jal LABEL`

行为：`R1 = PC + 4; PC += simm25 * 4`

注：

* 隐式指定的链接寄存器为 r1（ra）。
* 目前观测到的该指令标 ? 的位均保持与立即数最高位相同（即符号扩展）。因此尚不能确定该指令的立即数究竟为 25 位（与老胡 PPT 保持一致）还是 26 位。

### `beq` 相等则跳

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="16">立即数</th>
<th colspan="5">源寄存器 2</th>
<th colspan="5">源寄存器 1</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">010110</td>
<td align="center" colspan="16">simm16</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`beq rd, rj, LABEL`

行为：`if (rd == rj) PC += simm16 * 4`

### `bne` 不等则跳

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="16">立即数</th>
<th colspan="5">源寄存器 2</th>
<th colspan="5">源寄存器 1</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">010111</td>
<td align="center" colspan="16">simm16</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`bne rd, rj, LABEL`

行为：`if (rd != rj) PC += simm16 * 4`

### `bgt` 有符号大于则跳

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="16">立即数</th>
<th colspan="5">源寄存器 2</th>
<th colspan="5">源寄存器 1</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">011000</td>
<td align="center" colspan="16">simm16</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`bgt rd, rj, LABEL`

行为：`if ((intptr_t)rd > (intptr_t)rj) PC += simm16 * 4`

### `ble` 有符号小于等于则跳

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="16">立即数</th>
<th colspan="5">源寄存器 2</th>
<th colspan="5">源寄存器 1</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">011001</td>
<td align="center" colspan="16">simm16</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`ble rd, rj, LABEL`

行为：`if ((intptr_t)rd <= (intptr_t)rj) PC += simm16 * 4`

### `bgtu` 无符号大于则跳

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="16">立即数</th>
<th colspan="5">源寄存器 2</th>
<th colspan="5">源寄存器 1</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">011010</td>
<td align="center" colspan="16">simm16</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`bgtu rd, rj, LABEL`

行为：`if ((uintptr_t)rd > (uintptr_t)rj) PC += simm16 * 4`

### `bleu` 无符号小于等于则跳

<table>
<thead>
<tr>
<th>31</th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th></th>
<th></th><th></th><th></th><th></th><th></th><th></th><th></th><th>0</th>
</tr>
<tr>
<th colspan="6">操作码</th>
<th colspan="16">立即数</th>
<th colspan="5">源寄存器 2</th>
<th colspan="5">源寄存器 1</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="6">011011</td>
<td align="center" colspan="16">simm16</td>
<td align="center" colspan="5">rj</td>
<td align="center" colspan="5">rd</td>
</tr>
</tbody>
</table>

语法：`bleu rd, rj, LABEL`

行为：`if ((uintptr_t)rd <= (uintptr_t)rj) PC += simm16 * 4`

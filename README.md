# Asynchronous-FIFO
FIFO
which stands for "First In, First Out," is a method used to manage and organize items or processes where the first item added is the first one to be removed or processed. It's a concept commonly applied in various fields like inventory management, data structures, and computing.

In order to understand FIFO design, one needs to understand how the FIFO pointers work. The write pointer always points to the next word to be written; therefore, on reset, both pointers are set to zero, which also happens to be the next FIFO word location to be written. On a FIFO-write operation, the memory location that is pointed to by the write pointer is written, and then the write pointer is incremented to point to the next location to be written.

Gray Code Counter
A Gray code counter is a type of counter that uses Gray code, also known as reflected binary code, to represent numbers. Gray code is a binary numeral system where two successive values differ in only one bit. This property makes Gray code particularly useful in situations where minimizing errors during transitions between values is critical, such as in rotary encoders and digital communication systems.

Gray code counter - Style #1
Dual n-bit Gray code counter

A dual n-bit Gray code counter is a Gray code counter that generates both an n-bit Gray code sequence (described in section 3.2) and an (n-1)-bit Gray code sequence

![image](https://github.com/user-attachments/assets/6688728d-1ead-40c2-9965-f51a211ceb8f)
Gray code counter - Style #2
this code actually employs two sets of registers to eliminate the need to translate Gray pointer values to binary values. The second set of registers (the binary registers) can also be used to address the FIFO memory directly without the need to translate memory addresses into Gray codes. The n-bit Gray-code pointer is still required to synchronize the pointers into the opposite clock domains, but the n-1-bit binary pointers can be used to address memory directly.

![image](https://github.com/user-attachments/assets/7acfd051-b16d-433d-9287-fc0499ad03ca)

Design and Architecture
![image](https://github.com/user-attachments/assets/02d8b454-f614-4b47-b734-819aa461d144)
Fig: FIFO1 partitioning with synchronized pointer comparison

To facilitate static timing analysis of the style #1 FIFO design, the design has been partitioned into the following six Verilog modules with the following functionality and clock domains:

fifo1.v - (see Example 2 in section 6.1) - this is the top-level wrapper-module that includes all clock domains. The top module is only used as a wrapper to instantiate all of the other FIFO modules used in the design.

fifomem.v - (see Example 3 in section 6.2) - this is the FIFO memory buffer that is accessed by both the write and read clock domains. This buffer is most likely an instantiated, synchronous dual-port RAM. Other memory styles can be adapted to function as the FIFO buffer

sync_r2w.v - (see Example 4 in section 6.3) - this is a synchronizer module that is used to synchronize the read pointer into the write-clock domain. The synchronized read pointer will be used by the wptr_full module to generate the FIFO full condition.

sync_w2r.v - (see Example 5 in section 6.4) - this is a synchronizer module that is used to synchronize the write pointer into the read-clock domain. The synchronized write pointer will be used by the rptr_empty module to generate the FIFO empty condition.

rptr_empty.v - (see Example 6 in section 6.5) - this module is completely synchronous to the read-clock domain and contains the FIFO read pointer and empty-flag logic.

wptr_full.v - (see Example 7 in section 6.6) - this module is completely synchronous to the write-clock domain and contains the FIFO write pointer and full-flag logic.

Handling full & empty conditions
The empty flag will be generated in the read-clock domain. The read pointer catches up to the write pointer (including the pointer MSBs). Pointers that are one bit larger than needed to address the FIFO memory buffer are used. In order to efficiently register the rempty output, the synchronized write pointer is actually compared against the rgraynext (the next Gray code that will be registered into the rptr).

assign rempty_val = (rgraynext == rq2_wptr); always @(posedge rclk or negedge rrst_n) if (!rrst_n) rempty <= 1'b1; else rempty <= rempty_val;

Generating full
The full flag will be generated in the write-clock domain. The write pointer catches up to the read pointer (except for different pointer MSBs). The read pointer be synchronized into the write clock domain before doing pointer comparison. Three conditions that are all necessary for the FIFO to be full:

(1) The wptr and the synchronized rptr MSB's are not equal (because the wptr must have wrapped one more time than the rptr).

(2) The wptr and the synchronized rptr 2nd MSB's are not equal

(3) All other wptr and synchronized rptr bits must be equal

assign wfull_val = (wgraynext=={~wq2_rptr[ADDRSIZE:ADDRSIZE-1], wq2_rptr[ADDRSIZE-2:0]}); always @(posedge wclk or negedge wrst_n) if (!wrst_n) wfull <= 1'b0; else wfull <= wfull_val;

Waveform
![image](https://github.com/user-attachments/assets/2d36dce6-bb6c-4c33-8f2e-a75996f897e1)


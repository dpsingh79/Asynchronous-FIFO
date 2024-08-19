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

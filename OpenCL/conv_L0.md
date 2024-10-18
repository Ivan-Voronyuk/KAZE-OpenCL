```cpp
inline half8 conv_L0(const half8 L0[5][90], const uchar8* const rot, half8 buff[90], ushort c) {
    /*
     * top-to-down:
     *
     * rot.s7
     * rot.s6
     * rot.s5 - middle
     * rot.s4
     * rot.s3 - lowest <=> newest
     *
     */
    buff[c] = (L0[rot->s3][c] + L0[rot->s7][c]) * kernel_5x5.s2 +
              (L0[rot->s4][c] + L0[rot->s6][c]) * kernel_5x5.s1 + L0[rot->s5][c] * kernel_5x5.s0;
    barrier(CLK_LOCAL_MEM_FENCE);
    return (get_L2(buff, c) + get_R2(buff, c)) * kernel_5x5.s2 +
           (get_L1(buff, c) + get_R1(buff, c)) * kernel_5x5.s1 + buff[c] * kernel_5x5.s0;
}
```

#barrier
#L0
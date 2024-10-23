```cpp
inline void iterate_pre_L0(half8 L0[5][90], const uchar8 L0_u8[][90], half8 L_[][90],
                           half8 buff[90], half8 G[][90], uchar8* const rot, const ushort c,
                           const half k23) {
    /*
     * top-to-down:
     *
     * rot.s7
     * rot.s6
     * rot.s5 - middle
     * rot.s4
     * rot.s3 - lowest <=> newest
     *
     * rot.s2
     * rot.s1 - current elem
     * rot.s0 - newest (forward)
     *
     */
    /*
    +---+---+---+---|-:-+
    |   |   |   | L+| L+|
    +---+---+---+-3-|-:-+
    |   |   |   |LU2|LU2|
    +---+---+---+ 3 |-:-+
    |   |   |   |LU |LU |
    +---+---+---+ 3 |-:-+
    |   |   |   |~D |~D |
    +---+---+---+-3-|-:-+---+
    |   |   | G | G | G | G |
    +---+-0-+-1-+-2-|-3-+---+---+
    |   | - | L_| L_| L_| L_| L_|
    +---+---+-0-+-1-|-2-+-3-+---+---+---+
    | - | - | - | - | L0| L1| L2| L3| L4|
    +---+---+---+---|-0-+-1-+-2-+-3-+---+
    ' |   |   |   |   |   :   :
    ' s2  s1  s0  :   :   :   :
    ' s7  s6  s5  s4  s3  :   :
    '     |   |   |   |   |   :
    '     s2  s1  s0  :   :   :
    '     s7  s6  s5  s4  s3  :
    '         |   |   |   |   |
    '         s2  s1  s0  :   :
    '         s7  s6  s5  s4  s3
    */
    L0[rot->s7][c] = L0[rot->s6][c] = L0[rot->s5][c] = L0[rot->s4][c] = L0[rot->s3][c] =
        convert_half8(L0_u8[0][c]) / 2;
    L_[rot->s1][c] = L_[rot->s0][c] = conv_L0(L0, rot, buff, c);

    rotate_forward(rot);
    L0[rot->s3][c] = convert_half8(L0_u8[1][c]) / 2;
    L_[rot->s0][c] = conv_L0(L0, rot, buff, c);
    G[rot->s0][c] =
        g2_3(pown(dx(L_[rot->s1], c), 2) + pown((L_[rot->s2][c] - L_[rot->s0][c]) / 2, 2), k23);

    rotate_forward(rot);
    L0[rot->s3][c] = convert_half8(L0_u8[1][c]) / 2;
    L_[rot->s0][c] = conv_L0(L0, rot, buff, c);
    G[rot->s0][c] =
        g2_3(pown(dx(L_[rot->s1], c), 2) + pown((L_[rot->s2][c] - L_[rot->s0][c]) / 2, 2), k23);
};
```
#barrier
#convert_half8
#L0 #L_
#G
#rotate_forward 
[[conv_L0]]
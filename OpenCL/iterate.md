```cpp
inline void iterate_pre(half8 L0[5][90], const uchar8 L0_u8[][90], half8 L_[][90], half8 buff[90],
                        half8 G[][90], half8 Up[ITER], half8 Dn[ITER], half8 Lt[ITER],
                        half8 Rt[ITER], half8 g[ITER], half8 LP[ITER][3][90], uchar8* const rot,
                        const ushort c, const half k23, constexpr half tau[ITER]) {
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
    +---+---+---+---|-:-+
    |   |   |   |LU2|LU2|
    +---+---+---+---|-:-+
    |   |   |   |LU |LU |
    +---+---+---+---|-:-+
    |   |   |   |~D |~D |
    +---+---+---+---|-:-+---+
    |   |   | G | G | G | G |
    +---+---+---+---|---+-#-+---+
    |   | - | L_| L_| L_| L_| L_|
    +---+---+---+---|---+---+-#-+---+---+
    | - | - | - | - | L0| L1| L2| L3| L4|
    +---+---+---+---|---+---+-#-+---+---+
    '                 |   |   |   |   |
    '                 s2  s1  s0  :   :
    '                 s7  s6  s5  s4  s3
    */
    for (ushort pos = 0; pos < ITER; ++pos) {  // acctual L+ pos is "pos-1"

        __private half8 L0_s8 = L0[rot->s7][c];

        rotate_forward(rot);
        L0[rot->s3][c] = convert_half8(L0_u8[1][c]) / 2;
        barrier(CLK_LOCAL_MEM_FENCE);
        L_[rot->s0][c] = conv_L0(L0, rot, buff, c);
        barrier(CLK_LOCAL_MEM_FENCE);
        G[rot->s0][c] =
            g2_3(pown(dx(L_[rot->s1], c), 2) + pown((L_[rot->s2][c] - L_[rot->s0][c]) / 2, 2), k23);
        barrier(CLK_LOCAL_MEM_FENCE);
        const half8 D2 = 4 * G[rot->s1][c] + (get_L_to0(G[rot->s1], c) + get_R_to0(G[rot->s1], c) +
                                              G[rot->s0][c] + G[rot->s2][c]);
        const half8 dt = G[rot->s1][c] / D2;
        Up[pos] = G[rot->s2][c] / D2 + dt;
        Dn[pos] = G[rot->s0][c] / D2 + dt;
        Lt[pos] = get_L_to0(G[rot->s1], c) / D2 + dt;
        Rt[pos] = get_R_to0(G[rot->s1], c) / D2 + dt;
        g[pos] = 2 * L0[rot->s7][c] / D2;
        LP[0][rot->s0][c] =
            L0[rot->s7][c] * (1 - tau[0]) - tau[0] * (L0_s8 * Up[pos] + L0_s8[rot->s6] * Dn[pos] +
                                                      get_L_to0(L0[rot->s7], c) * Lt[pos] +
                                                      get_R_to0(L0[rot->s7], c) * Rt[pos] - g[pos]);
        barrier(CLK_LOCAL_MEM_FENCE);

        for (ushort layer = 1; layer < pos - 1; ++layer) {
            LP[layer][rot->s0][c] =
                LP[pos][layer - 1][c] * (1 - tau[0]) -
                tau[0] *
                    (LP[layer - 1][rot->s2][c] * Up[pos] + LP[layer - 1][rot->s0][c] * Dn[pos] +
                     get_L_to0(LP[layer - 1][rot->s1], c) * Lt[pos] +
                     get_R_to0(LP[layer - 1][rot->s1], c) * Rt[pos] - g[pos]);
        }
    }
}


```
#barrier 
#convert_half8
#L0 
#L_ 
#G 
#Up
#Dn
#Lt
#Rt
#g
#LP
#rotate_forward
[[conv_L0]]
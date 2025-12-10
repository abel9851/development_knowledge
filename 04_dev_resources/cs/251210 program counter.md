```verilog

// This file is part of www.nand2tetris.org
// and the book "The Elements of Computing Systems"
// by Nisan and Schocken, MIT Press.
// File name: projects/3/a/PC.hdl
/**
 * A 16-bit counter.
 * if      reset(t): out(t+1) = 0
 * else if load(t):  out(t+1) = in(t)
 * else if inc(t):   out(t+1) = out(t) + 1
 * else              out(t+1) = out(t) // 저장한다는 뜻 3개 sel bit 중 
                                    // 하나라도 1이라면 register의
                                    // load bit는 1이 된다.
 */
 
 /**
CHIP PC {
    IN in[16], reset, load, inc;
    OUT out[16];
    
    PARTS:
    Mux16(a=fbOut, b=false, sel=reset, out=reset16MuxOut);
    Mux16(a=fbOut, b=in, sel=load, out=load16MuxOut);
    Inc16(in=fbOut, out=inc16Out);
    Mux16(a=fbOut, b=inc16Out, sel=inc, out=incMux16Out);
    
    Mux16(a=incMux16Out, b=load16MuxOut, sel=load, out=middleSelectedOut);
    Mux16(a=middleSelectedOut, b=reset16MuxOut, sel=reset, out=finalSelectedOut);
    
    Or(a=reset, b=inc, out=or1Out);
    Or(a=or1Out, b=load, out=or2Out);
    Register(in=finalSelectedOut, load=or2Out, out=out, out=fbOut);
}

*/

CHIP PC {
    IN in[16], reset, load, inc;
    OUT out[16];

    PARTS:
    // 1. Inc 처리 (가장 낮은 우선순위)
    Inc16(in=fbOut, out=pcPlusOne);
    Mux16(a=fbOut, b=pcPlusOne, sel=inc, out=w1);

    // 2. Load 처리 (Inc보다 우선)
    // load가 1이면 w1(Inc 결과)을 무시하고 in을 선택
    Mux16(a=w1, b=in, sel=load, out=w2);

    // 3. Reset 처리 (가장 높은 우선순위)
    // reset이 1이면 w2(Load나 Inc 결과)를 무시하고 0을 선택
    Mux16(a=w2, b=false, sel=reset, out=w3);

    // 4. 레지스터 저장
    // 아무 신호가 없으면(0,0,0) w1에서 fbOut이 선택되어 계속 뺑뺑이 돈다.
    // 따라서 load 조건을 따질 필요 없이 항상 업데이트(true)하면 됨.
    Register(in=w3, load=true, out=out, out=fbOut);
}
// 왜 카운터가 필요하지? 그리고 왜 이름이 PC이지?
// PC라는 것은 program counter를 의미하고, 이는 5장의 컴퓨터 아키텍처에서 사용될 것이다.
// = 은 여기서는 저장한다는 뜻이다.
// 포인트는 4개
// elseif라는 것은 우선순위가 적용된다는 것
// 그 우선순위를 고르기 위해서는 여러개의 Mux의 순서가 지켜져야한다는 것
// else의 out(t+1) = out(t)는 선택비트가 모두 false일 경우, 이전주기의 출력이
// 다음 출력에도 나온다는 뜻이니, 이는 곧 저장기능을 구현해야한다는 것.
// fbOut은 처음 입력받는 3개의 Mux에 너무나도 필수적이라는 것
// 증분기는 소프트웨어와는 달리, 무조건 전기신호를 양쪽다 받으니 미리 처리한다.
// 소프트웨어적으로는 비효율적으로 보이지만 하드웨어적으로는 지극히 정상.
// 직렬우선순위가 중요하다!
```


#Nand2Tetris #기초 

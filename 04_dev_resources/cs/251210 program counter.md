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


[[Drawing-2025-12-10 program counter 개선 1.13.45.excalidraw]]
- 여태까지와 마찬가지로 오직 생각으로만 설계를 하고 시뮬레이션해봤다.
- 이상하게 느낀 부분이 있다.
	- 증분기를 사용해서 미리 +1하는 부분이 계속 마음에 걸렸다. 소프트웨어에서는 if분기를 사용해서 true라면 계산하고, false라면 계산안하는, 효율적인 처리를 하지 않는가? 하지만 아무리 생각해봐도 inc bit가 True일 때에 +1을 하고 False일 때 계산을 안하도록 하는 방식이 떠오르지 않았다. False일 때에는 feedback loop로 받은 output이 선택되도록 하고, True일 때에는 사전에 증분기로 +1한 값이 선택되도록 하는게 최선이었다.
		- 확인해보니까 하드웨어는 공간에 배치된 부품에 전기가 들어오면 무조건 그 전기가 흐르는 구조라는 거다. 소프트웨어는 그렇지 않다. 위에서 아래로 실행되면서 CPU라는 관리자가 하나하나 코드를 확인한다. 흐르는 물이 어느 방향으로 갈지, 오른쪽(if true)으로 갈지, 왼쪽으로 갈지(if false)를 선택할 수 있다. 시간이 흘러감에 따라 위에서 아래로 물이 흐르며, 방향을 바꿀 수 있고 그렇게 물이 흐르지 않는 파이프가 생길 수 있다.
		- 하지만 하드웨어는 다르다. 하드웨어는 부품이 납땜이 되어서 전기가 흐르는 구조다. if조건이 안맞는다고 해서 부품이 사라질수는 없다. 전기는 무조건 흘러서, 따뜻해진 물을 흘려보낼지(if true), 아무런 처리를 하지 않은 물을 흘려보낼지, 밸브(inc bit)로 선택하는거다.(Y자 파이프)
		- 하드웨어는 병렬적이다.
		- 나는 소프트웨어 패러다임(당연하다고 믿고 깔고 들어가는 전제 조건들)에 갇혀있던거다. 소프트웨어적인 생각으로 하드웨어를 구현하려했으니, 이질감을 느낀거였다.
- program counter를 만들면서 제일 어려웠던 부분은 selection bit들의 동작구현이었다.
	- reset, load, inc bit들의 조건에 따라 값이 변하고 최종적으로 하나의 값이 register의 입력으로 들어가도록 해야하는데, 상기한 3개의 selection bit들의 on/off에 따라 변한 값들을 어떻게 최종적으로 하나의 입력값으로 정리해야하는지 감이 안잡혔기 때문이다.
		- 초기에는 입력에 필요한 값들을 모두 미리 준비한 뒤, 멀티플렉서를 사용해서 선택하는 방식을 사용했는데 모두 같은 레벨에서 준비할 필요가 없었다. 멀티플렉서를 체이닝하면서 가장 우선순위가 높은 처리를 가장 뒤에 배치하고 우선순위가 가장 낮은 처리를 제일 앞에 두면 되는 거였다.
- 착각한 게 있었다. reset, load, inc bit들이 1,0,0 / 0,1,0 / 0,0,1 식으로 움직인다고 가정한거다.
	- 소프트웨어개발할 때에는 별 의문없이 else if를 사용했는데 이번에 program counter를 구현하면서 모든 selection bit가 1이라면 if에 정의된 것이 가장 우선순위가 높고, 그 다음의 else if가 2번째, 그 다음의 else if가 3번째, 모든 조건이 false라면 마지막으로 적용되는게 else 즉, 우선순위가 작용한다는 것을 몸소 깨달았다.
- 메모리에 저장한다는 것은, 데이터(전기)가 계속 흘러서 out(t) -> in(t) -> out(t+1)으로 돌고 돈다는 것을 알았다.
- register의 load bit를 구현할 때에도 고민했다. 3개의 selection bit가 있고 이 bit들 중에 하나라도 true라면 load bit는 true로 해야하고 그렇지 않다면 false로 해서, false일 경우 else out(t+1) = out(t)를 만족할수 있겠지, 라고 생각했다. 그래서 Or게이트 2개를 사용해서 load bit의 동작을 구현했지만, 사실 이보다 더 간단한게, 이번 program counter의 reigster의 load bit는 계속 true를 유지하는, true로 하드코딩하면 된다는 거였다.
	- 왜냐하면 **상태 유지(No Change)** 조건까지 포함한 모든 경우의 수가 Mux 체인을 통해 처리되기 때문이다. 제어 신호가 모두 0일 때도 **기존 출력값이 다시 입력(Feedback)되도록 설계**되었으므로, 레지스터는 조건 분기 없이 매 클럭마다 값을 덮어써도 안전하다.
	- 값의 유지를 '쓰기 방지(Load=False)'가 아닌 **'자기 자신을 다시 쓰기(Feedback Loop)'**로 구현했기 때문이다. 따라서 레지스터의 제어 로직을 복잡하게 할 필요 없이, 데이터 경로(Data Path)에서 선택된 값을 무조건 받아들이기만 하면 된다.
	- 즉, 입력에 output(t)가 있고 이는 else 사양을 충족한다. 

#Nand2Tetris #기초 

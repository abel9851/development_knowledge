
```java

// Square 전체를 보면 응집력이 낮다.
// calculateArea와 calcuatePerimeter는 사각형의 크기를 계산하므로 밀접한 관련이 있는 것으로 간주한다.
// 응집력이 높다.
// 마찬가지로 draw와 rotate는 render를 하므로 응집력이 높다.
// 하지만 총 4개를 묶은 Square 클래스 전체로 보자면 크기와 관련있는
// 위의 2개의 메소드와 rendering과 관련이 있는 아래의 2개의 메소드는 관련이 거의 없으므로
// 응집력이 낮다.
// 응집력이 낮다는 것은, 단일 책임 원칙에 부합하지 않다는 것이므로
// 클래스를 분리하는 것이 좋다.
// 단일 책임 원칙이란 소프트웨어의 부품(클래스, 모듈, 함수 등)은 하나의 책임을 가져야한다는 것이다.
// 하나의 책임은, 응집력이 높을수록 부합하다.
// 응집력이란 여러 부분의 소프트웨어 부품들의 관련된 정도를 의미한다.
// 난 여태까지 내용물을 보기 보다는 클래스의 이름과 부합한지를 확인했는데
// 내용물에 해당하는 메소드들의 응집력(cohesion)을 먼저 고려했어야 했다.
// 이름은 그 다음 바꿔도 되는 것이었다.
// 앞으로는 리뷰를 하거나 코드를 작성할 때 응집력을 고려해서, 단일 책임의 원칙을 지키며
// 작업을 해야겠다.
// 좋은 인사이트를 얻었다.
// 단일 책임 원칙이란, 한가지 역할을 하는 것이 아닌 한가지 책임을 지는 것으로 기준을 잡아야한다.


public class Square {
    int side = 5;

    public int calculateArea() {
        return side * side;
    }

    public int calculatePerimeter() {
        return side * 4;
    }

    public void draw() {
        if (highResolutionMonitor) {
            // Render a high resolution image of a square
        } else {
            // Render a normal image of a square
        }
    }

    public void rotate (int degree) {
        // Rotate the image of the square clockwise to
        // the required degree and re-render
    }
}

// 클래스를 2개로 분리하여, 응집력을 높이고 단일책임의 원칙에 맞게 수정했다.
// 첫번째 클래스인 Square의 책임은 정사각형과 관련된 측정을 처리하는 것이고
// 두번째 클래스인 SquareUI의 책임은 정사각형의 rendering을 처리하는 것이다.
public class Square {
    int side = 5;

    public int calculateArea() {
        return side * side;
    }

    public int calculatePerimeter() {
        return side * 4;
    }
}


public class SquareUI {
    public void draw() {
        if (highREsolutionMonitor) {
            // Render a high resolution image of a square
        } else {
            // Render a normal image of a square
        }
    }

    public void rotate(int degree) {
        // Rotate the image of the square clockwise to
        // the required degree and re-render
    }

}

```
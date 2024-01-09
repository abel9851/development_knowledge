
```java

// 단일 책임 원칙에서는 코드를 명시했을 때 눈에 보이는 것으로 원칙을 지키고 있는지 판단한다는 것을 전제로 한다.
// 아래의 수정전의 Student와 수정 후의 Student는 save() 메소드를 분리하고 안하고의 차이가 있다.
// 처리하는 로직은 같지만 겉으로 보이는 것은 다르다.
// 수정 전의 Student 클래스는 save()에 MySQL 데이터베이스의 low level의 세부사항에 대해 코드를 작성했다.
// 나중에 이 MySQL이 아닌, mongoDB, postgreSQL등 다른 데이터베이스로 변경할 경우,
// Student 내부에서 코드를 수정해야한다.
// 즉, 데이터베이스의 low level 코드와 Student는 긴밀한 결합(coupling)을 하고 있다.
// 소프트웨어 세계에서는 긴밀한 결합은 선호하지 않는다. 결합을 완전히 제거할 순 없지만, 최대한 느슨하게 결합을 유지하는게 좋다.
// 수정 후의 Student를 보면 겉으로 명시된 것만 보면, 데이터베이스의 로직처리를 하고 있지 않다.
// 따로 분리한 repository 클래스에서는 데이터베이스의 로직을 담당하고 있다.
// 그리고 수정후의 Student에서는 그 repository 클래스를 참조하고 호출하고 있을 뿐이다.
// 이렇게 하면 데이터베이스의 변경이 필요할 때 Student 클래스는 수정하지 않고
// repository 코드만 변경하면 된다는 이점이 있다.(이렇게 느슨한 결합관계가 아닌, 수정전의 긴밀한 결합관계의 코드가 수백개 있다면 수정하기 위해 코드를 찾는데 시간이 걸리 뿐더러
// 한눈에 코드를 봐서는 이 클래스가 어떤 역할을 하는지 하나로 정의내릴 수 없게 된다.)
// 그리고 이렇게 느슨한 결합을 유지하면, Student는 학생의 profile data를 처리한다는 단일 책임을, repository는 데이터베이스 로직 처리를 한다는 단일 책임을 갖는다.
// 즉, 결합이 느슨할수록 단일 책임의 원칙에 부합하고, 이는 코드 관리면에서도 유용하며, 한눈에 소프트웨어 컴포넌트(부품)이 어떤 역할을 하는지 인식하기 편하다.
// 결합이란, 여러 소프트웨어 부품들이 상호의존하는 수준을 의미한다.

public class Student {
    private String studentId;
    private Date studentDOB;
    private String address;

    public void save() {
        // Serialize object into a string representation
        String objectStr = MyUtils.serializeIntoAString(this);
        Connection connection = null;
        Statement stmt = null;
        try {
        Class.forName("com.mysql.jdbc.Driver");
        connection =
        DriverManger.getConnection("jdbc:mysql://localhost:3306/MyDB". "root", "password");
        stmt = connection.createStatement();
        stmt.execute("INSERT INTO STUDENT VALUES (" + objectStr + ")");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

// 수정 후
public class Student {
    private String studentId;
    private Date studentDOB;
    private String address;

    public void save() {
        new StudentRepository().save(this);
    }
}


public String getStudentId() {
    return studentId;
}

public void setStudentId(String studentId) {
    this.studentID = studentId;
}

public class StudentRepository {
    public void save(Student student) {
        // Serialize object into a string representation
        String objectStr = MyUtils.serializeIntoAString(student);
        Connection connection = null;
        Statement stmt = null;
        try {
        Class.forName("com.mysql.jdbc.Driver");
        connection =
        DriverManger.getConnection("jdbc:mysql://localhost:3306/MyDB". "root", "password");
        stmt = connection.createStatement();
        stmt.execute("INSERT INTO STUDENT VALUES (" + objectStr + ")");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

```
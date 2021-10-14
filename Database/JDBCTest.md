# JDBC
#Database/JDBCTest

---
```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.Scanner;

public class JDBCTest {

	public static void main(String[] args) {
		int input;
		ResultSet rset;
		Scanner sc = new Scanner(System.in);
		PreparedStatement query;
		
		try {
			Class.forName("oracle.jdbc.driver.OracleDriver");
		} catch(ClassNotFoundException e){
			System.err.println("ClassNotFoundException : " + e.getMessage());
		}

		try {
			Connection conn = DriverManager.getConnection("jdbc:oracle:thin:@//localhost:1521/xepdb1", "scott", "tiger");
			
			/*
			 * 학번을 입력받아 그 학생의 모든 속성을 출력 
			 */
			System.out.print("학번을 입력하세요:");
			input = sc.nextInt();
			
			query = conn.prepareStatement("select * from student where sid = ?");
			query.setInt(1, input);
			rset = query.executeQuery();
			while(rset.next()) {
				System.out.println("Sid : " + rset.getInt("sid") +
						", Sname : " + rset.getString("sname") +
						", Deptno : " + rset.getInt("deptno") +
						", Advisor : " + rset.getInt("advisor") +
						", Gen : " + rset.getString("gen") +
						", Addr : " + rset.getString("addr") +
						", Birthdate : " + rset.getString("birthdate") + 
						", Grade : " + rset.getDouble("grade") + "\n");
			}
			
			/*
			 * 학과 번호를 입력받아 그 학과에 재학중인 학생의 성적을 0.01 더하기
			 */
			System.out.print("학과번호를 입력하세요:");
			input = sc.nextInt();
			System.out.println("성적 +0.01 update \n");
			
			query = conn.prepareStatement("update student set grade = grade + 0.01 where deptno = ?");
			query.setInt(1, input);
			query.executeUpdate();
					
			/*
			 * (학과번호, 학번)의 오름차순으로 모든 학생의 학과번호, 학번, 이름, 성적 출력
			 */
			query = conn.prepareStatement("select deptno, sid, sname, grade from student order by deptno, sid");
			rset = query.executeQuery();
			while(rset.next()) {
				System.out.println("Deptno : " + rset.getInt("deptno") +
						", Sid : " + rset.getInt("sid") +
						", Sname : " + rset.getString("sname") +
						", Grade : " + rset.getDouble("grade"));
			}
		} catch (SQLException e) {
			System.err.println("SQLException : " + e);
		}
	}

}

```
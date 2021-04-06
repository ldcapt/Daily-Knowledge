# [Day 3] - [MySQL vs JDBC](https://freetuts.net/ket-noi-mysql-bang-java-jdbc-driver-2790.html)

# Danh sách từ khóa


## Kết nối MySQL bằng Java JDBC Connection

Đầu tiên, hãy import 3 class quan trọng nhất vào project của bạn, đó là `SQLException`, `DriverManager`, và `Connection` từ **package** `java.sql.*` 

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
```

Tiếp theo sử dụng phương thức `getConnection()` từ  class `DriverManager` để khởi tạo **Connection Object**

Có 3 tham số cần truyền vào phương thức này gồm:
- **URL** - Đường link đến database. Đối với MySQL thì có dạng như sau: `jdbc:mysql://localhost:3306/database_name`, tức là 
  + chúng ta đang kết nối với máy chủ local host, 
  + Port 3306, 
  + Tên của cơ sở dữ liệu là `database_name`
- **user** - Tên đăng nhập để kết nối vào database
- **password** - Mật khẩu của user

```java
Connection conn = null;

try {
  // DB parameter
  String url = "jdbc:mysql://localhost:3306/mysqljdbc";
  String user = "root";
  String password = "secret";

  // Create a connection to the database
  conn = DriverManager.getConnection(url, user, password);
  // more processing here
  // ...
} catch (SQLException e) {
  System.out.println(e.getMessage());
} finally {
  try {
    if (conn != null) {
      conn.close();
    }
  } catch (SQLException e) {
    System.out.println(e.getMessage());
  }
}
```

Nếu quá trình đăng nhập không thành công bởi một số lý do như: sai mật khẩu, tên đăng nhập, cổng kết nối không tồn tại, database không tồn tại, ... sẽ phát sinh lỗi **SQLException**. Vì vậy nên đặt khối kết nối bên trong vùng `try ... catch` để đảm bảo an toàn.

Ngoài ra khi sử dụng xong cần ngắt kết nối ngay để tránh bị tấn công vào dữ liệu. Vùng đóng kết nối nằm bên trong khối `finally`

<hr>

Tuy nhiên trong thực tế không nên sử dụng cách này bởi nó sẽ gặp một số vấn đề như: Khi đổi database hoặc thông số kết ối thì phải sửa khá nhiều vị trí, đồng thời không được bảo mật khi mã nguồn có quá nhiều nơi kết nối. Để tránh điều đó, có một giải pháp đó là tạo một file riêng chứa những thông số kết nối này. Ví dụ: 
```
# MySQL DB parameters
user=root
password=secret
url=jdbc:mysql://localhost:3306/mysqljdbc
```

Đoạn code được viết lại như sau:

```java
Connection conn = null;
 
try(FileInputStream f = new FileInputStream("db.properties")) {
    // load the properties file
    Properties pros = new Properties();
    pros.load(f);
 
    // assign db parameters
    String url       = pros.getProperty("url");
    String user      = pros.getProperty("user");
    String password  = pros.getProperty("password");
    // create a connection to the database
    conn = DriverManager.getConnection(url, user, password);
} catch(IOException e) {
   System.out.println(e.getMessage());
} finally {
    try{
        if(conn != null)
            conn.close();
    }catch(SQLException ex){
        System.out.println(ex.getMessage());
    }
     
}
```

Với mõi lần tương tác đến MySQL thì bạn sẽ tạo ra một két nối mới, điều này khiến chương trình tốn quá nhiều bộ nhớ để lưu trữ nhưng đối tượng connection đó.

Vì vậy ta nên tạo ra một class riêng và nơi nào muốn sử dụng thì chỉ cần gọi đến là được.

```java
import java.io.FileInputStream;
import java.io.IOException;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.util.Properties;
 
/**
 *
 * @author mysqltutorial.org
 */
public class MySQLJDBCUtil {
 
    /**
     * Get database connection
     *
     * @return a Connection object
     * @throws SQLException
     */
    public static Connection getConnection() throws SQLException {
        Connection conn = null;
 
        try (FileInputStream f = new FileInputStream("db.properties")) {
 
            // load the properties file
            Properties pros = new Properties();
            pros.load(f);
 
            // assign db parameters
            String url = pros.getProperty("url");
            String user = pros.getProperty("user");
            String password = pros.getProperty("password");
             
            // create a connection to the database
            conn = DriverManager.getConnection(url, user, password);
        } catch (IOException e) {
            System.out.println(e.getMessage());
        }
        return conn;
    }
}
```

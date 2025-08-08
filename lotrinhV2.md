# Phần 1: Nhập môn NoSQL và MongoDB (Sử dụng MongoDB Compass & VSCode)

Phần này tập trung vào việc xây dựng nền tảng kiến thức vững chắc về MongoDB.

## 1.1. Giới thiệu về NoSQL và MongoDB

### NoSQL là gì?

NoSQL (thường được diễn giải là "Not Only SQL") là một thuật ngữ chung cho các cơ sở dữ liệu không tuân theo mô hình quan hệ (bảng và mối quan hệ) của SQL. Chúng được thiết kế để giải quyết các vấn đề mà cơ sở dữ liệu quan hệ truyền thống xử lý kém hiệu quả.

**Sự khác biệt SQL vs. NoSQL:**

- **Cấu trúc dữ liệu (Schema):**
  - **SQL:** Schema cố định (Schema-on-write). Bạn phải định nghĩa cấu trúc bảng (cột, kiểu dữ liệu) trước khi thêm dữ liệu.
  - **NoSQL:** Schema linh hoạt (Schema-on-read). Dữ liệu có thể được lưu trữ mà không cần một cấu trúc định sẵn. Mỗi bản ghi có thể có các trường khác nhau.
- **Khả năng mở rộng (Scalability):**
  - **SQL:** Mở rộng theo chiều dọc (Vertical Scaling - tăng cường sức mạnh cho một máy chủ duy nhất như CPU, RAM).
  - **NoSQL:** Mở rộng theo chiều ngang (Horizontal Scaling - thêm nhiều máy chủ hơn vào hệ thống). Điều này giúp xử lý lượng dữ liệu khổng lồ (Big Data) và lưu lượng truy cập cao hiệu quả hơn.

**Khi nào nên sử dụng NoSQL?**

- Khi bạn có dữ liệu lớn, phi cấu trúc hoặc bán cấu trúc.
- Khi yêu cầu về hiệu suất đọc/ghi rất cao.
- Khi ứng dụng cần sự linh hoạt và phát triển nhanh, cấu trúc dữ liệu thay đổi thường xuyên.
- Khi cần khả năng mở rộng quy mô dễ dàng.

**Các loại hình NoSQL:**

- **Document Databases (Cơ sở dữ liệu tài liệu):** Lưu trữ dữ liệu dưới dạng các tài liệu (document), thường là JSON hoặc BSON (Binary JSON). Rất linh hoạt. **MongoDB là loại này.**
- **Key-Value Stores:** Lưu trữ dữ liệu dưới dạng cặp khóa-giá trị (key-value). Rất đơn giản và nhanh cho việc truy xuất dữ liệu theo khóa. (Ví dụ: Redis, DynamoDB).
- **Column-family Stores:** Lưu trữ dữ liệu theo cột thay vì theo hàng. Hiệu quả cho các truy vấn tổng hợp trên một tập hợp con của các cột. (Ví dụ: Cassandra, HBase).
- **Graph Databases (Cơ sở dữ liệu đồ thị):** Được thiết kế để lưu trữ và điều hướng các mối quan hệ phức tạp giữa các thực thể. (Ví dụ: Neo4j, Amazon Neptune).

### MongoDB là gì?

**MongoDB** là một hệ quản trị cơ sở dữ liệu **document** hàng đầu, thuộc họ NoSQL. Nó lưu trữ dữ liệu trong các tài liệu giống JSON, giúp việc lưu trữ và truy vấn dữ liệu trở nên trực quan và mạnh mẽ.

**Tại sao MongoDB phổ biến?**

- **Schema linh hoạt:** Dễ dàng thay đổi cấu trúc dữ liệu.
- **Truy vấn mạnh mẽ:** Hỗ trợ các truy vấn phong phú, sắp xếp, và tổng hợp dữ liệu.
- **Khả năng mở rộng cao:** Dễ dàng mở rộng theo chiều ngang.
- **Hiệu suất cao:** Hoạt động tốt với các tập dữ liệu lớn.

**Kiến trúc của MongoDB:**

- **Database:** Một nơi chứa các collection. Tương đương với một database trong SQL.
- **Collection:** Một nhóm các document. Tương đương với một **bảng** (table) trong SQL. Tuy nhiên, nó không yêu cầu các document phải có cùng cấu trúc.
- **Document:** Một bản ghi dữ liệu, được lưu dưới định dạng **BSON** (một dạng nhị phân của JSON). Tương đương với một **hàng** (row) trong SQL.

### Ví dụ so sánh lưu trữ "Người dùng"

**Cách lưu trữ trong SQL (Cần 3 bảng):**

```sql
-- users (id, username, password)
-- user_profiles (user_id, first_name, last_name, birthdate)
-- user_addresses (id, user_id, street, city, country)
-- => Để lấy đủ thông tin, bạn cần thực hiện JOIN qua 3 bảng.
```

**Cách lưu trữ trong MongoDB (Chỉ cần 1 document):**

```json
{
  "_id": "ObjectId('...')",
  "username": "nguyenvana",
  "password": "hashed_password_goes_here",
  "profile": {
    "first_name": "An",
    "last_name": "Nguyễn Văn",
    "birthdate": "1995-10-20"
  },
  "addresses": [
    { "type": "home", "street": "123 Đường ABC", "city": "Hà Nội" },
    { "type": "work", "street": "456 Đường XYZ", "city": "Hồ Chí Minh" }
  ]
}
```

=> Tất cả thông tin liên quan được gom lại trong một tài liệu duy nhất, giúp truy vấn nhanh hơn và dễ hiểu hơn.

---

## 1.2. Cài đặt và Thiết lập Môi trường

- **Cài đặt MongoDB Community Server:**
  - Truy cập trang chủ của MongoDB: `MongoDB Community Server Download`
  - Chọn phiên bản, hệ điều hành của bạn và tải về gói cài đặt (.msi cho Windows, .tgz cho macOS/Linux).
  - Làm theo hướng dẫn cài đặt trên trang chủ.
- **Cài đặt và kết nối MongoDB Compass:**
  - Tải về MongoDB Compass từ cùng trang download ở trên.
  - Sau khi cài đặt, mở Compass và kết nối tới chuỗi `mongodb://localhost:27017`.
- **Cài đặt Extension "MongoDB for VS Code":**
  - Mở VS Code, vào tab Extensions (Ctrl+Shift+X).
  - Tìm kiếm `MongoDB for VS Code` và nhấn **Install**.
  - Kết nối tới `mongodb://localhost:27017` từ biểu tượng chiếc lá MongoDB.

---

## 1.3. Các khái niệm cốt lõi (CRUD Operations)

Mở **Mongo Shell** trong VS Code hoặc Terminal.

```javascript
// Chuyển sang database QuanLyThuVien
use QuanLyThuVien
```

### Create (Thêm mới)

**Thêm một document (insertOne):**

```javascript
db.Sach.insertOne({
  tieu_de: "Lão Hạc",
  tac_gia: "Nam Cao",
  nam_xuat_ban: 1943,
  the_loai: ["Truyện ngắn", "Văn học hiện thực"],
  so_luong: 15,
});
```

**Thêm nhiều document (insertMany):**

```javascript
db.Sach.insertMany([
  {
    tieu_de: "Số Đỏ",
    tac_gia: "Vũ Trọng Phụng",
    nam_xuat_ban: 1936,
    the_loai: ["Tiểu thuyết", "Trào phúng"],
    so_luong: 10,
  },
  {
    tieu_de: "Dế Mèn Phiêu Lưu Ký",
    tac_gia: "Tô Hoài",
    nam_xuat_ban: 1941,
    the_loai: ["Truyện thiếu nhi"],
    so_luong: 50,
  },
  {
    tieu_de: "Chí Phèo",
    tac_gia: "Nam Cao",
    nam_xuat_ban: 1941,
    the_loai: ["Truyện ngắn", "Văn học hiện thực"],
    so_luong: 0,
  },
]);
```

### Read (Đọc/Truy vấn)

**Tìm tất cả:**

```javascript
db.Sach.find({});
```

**Tìm với điều kiện (`{ field: value }`):**

```javascript
db.Sach.find({ tac_gia: "Nam Cao" });
```

**Tìm với toán tử so sánh (`$gt`, `$lt`, `$gte`, `$lte`):**

```javascript
db.Sach.find({ nam_xuat_ban: { $lt: 1942 } });
```

### Update (Cập nhật)

**Cập nhật một document (updateOne) với toán tử `$set`:**

```javascript
db.Sach.updateOne({ tieu_de: "Lão Hạc" }, { $set: { so_luong: 20 } });
```

**Cập nhật nhiều document (updateMany) với toán tử `$addToSet`:**

```javascript
db.Sach.updateMany(
  { tac_gia: "Nam Cao" },
  { $addToSet: { the_loai: "Kinh điển" } }
);
```

### Delete (Xóa)

**Xóa một document (deleteOne):**

```javascript
db.Sach.deleteOne({ so_luong: 0 }); // Xóa "Chí Phèo"
```

**Xóa nhiều document (deleteMany):**

```javascript
db.Sach.deleteMany({ nam_xuat_ban: { $lt: 1940 } }); // Xóa "Số Đỏ"
```

---

# Phần 2: Kỹ thuật Nâng cao trong MongoDB

## 2.1. Truy vấn Nâng cao

**Toán tử logic (`$or`, `$and`):**

```javascript
db.Sach.find({
  $or: [{ tac_gia: "Nam Cao" }, { tac_gia: "Vũ Trọng Phụng" }],
});
```

**Truy vấn trên mảng (`$size`, `$all`):**

```javascript
// Tìm sách có đúng 2 thể loại
db.Sach.find({ the_loai: { $size: 2 } });

// Tìm sách chứa cả 2 thể loại
db.Sach.find({ the_loai: { $all: ["Truyện ngắn", "Văn học hiện thực"] } });
```

**Sử dụng Projection (Chiếu):**

```javascript
// Chỉ hiển thị tiêu đề, tác giả và ẩn _id
db.Sach.find({}, { tieu_de: 1, tac_gia: 1, _id: 0 });
```

## 2.2. Indexing (Đánh chỉ mục)

**Index là gì?** Giống như mục lục sách, index giúp tăng tốc độ truy vấn bằng cách tránh quét toàn bộ collection (COLLSCAN) và đi thẳng tới dữ liệu (IXSCAN).

**Tạo Index:**

```javascript
// Index đơn
db.Sach.createIndex({ tac_gia: 1 }); // 1: tăng dần, -1: giảm dần

// Index kết hợp
db.Sach.createIndex({ the_loai: 1, nam_xuat_ban: -1 });
```

**Kiểm tra Index:**

```javascript
db.Sach.find({ tac_gia: "Nam Cao" }).explain("executionStats");
```

_Nhìn vào `executionStages.stage`: `IXSCAN` là tốt, `COLLSCAN` là tệ._

## 2.3. Aggregation Framework

Đây là một "đường ống" xử lý dữ liệu qua các giai đoạn (`$group`, `$sort`, `$limit`, `$unwind`, ...).

**Yêu cầu 1: Thống kê số sách của mỗi tác giả.**

```javascript
db.Sach.aggregate([
  { $group: { _id: "$tac_gia", so_luong_sach: { $sum: 1 } } },
]);
```

**Yêu cầu 2: Tìm thể loại sách phổ biến nhất.**

```javascript
db.Sach.aggregate([
  { $unwind: "$the_loai" },
  { $group: { _id: "$the_loai", so_luong: { $sum: 1 } } },
  { $sort: { so_luong: -1 } },
  { $limit: 1 },
]);
```

**Yêu cầu 3: Tính tổng số sách trong kho theo năm xuất bản.**

```javascript
db.Sach.aggregate([
  { $group: { _id: "$nam_xuat_ban", tong_so_luong: { $sum: "$so_luong" } } },
  { $sort: { _id: 1 } },
]);
```

---

# Phần 3: Ứng dụng MongoDB với Java Web Servlet

## 3.1. Thiết lập dự án (Maven)

Thêm các dependency vào `pom.xml`:

```xml
<dependencies>
    <!-- MongoDB Driver -->
    <dependency>
        <groupId>org.mongodb</groupId>
        <artifactId>mongodb-driver-sync</artifactId>
        <version>4.11.1</version>
    </dependency>
    <!-- Servlet API -->
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <version>4.0.1</version>
        <scope>provided</scope>
    </dependency>
    <!-- JSTL -->
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>jstl</artifactId>
        <version>1.2</version>
    </dependency>
</dependencies>
```

## 3.2. Lớp kết nối (Singleton Pattern)

`MongoConnection.java`

```java
import com.mongodb.client.MongoClient;
import com.mongodb.client.MongoClients;
import com.mongodb.client.MongoDatabase;

public class MongoConnection {
    private static final String CONNECTION_STRING = "mongodb://localhost:27017";
    private static final String DATABASE_NAME = "QuanLyThuVien";
    private static MongoClient mongoClient = null;

    private MongoConnection() {}

    public static MongoDatabase getDatabase() {
        if (mongoClient == null) {
            mongoClient = MongoClients.create(CONNECTION_STRING);
        }
        return mongoClient.getDatabase(DATABASE_NAME);
    }
}
```

## 3.3. Lớp DAO (Data Access Object)

`SachDAOImpl.java`

```java
import com.mongodb.client.MongoCollection;
import com.mongodb.client.MongoDatabase;
import com.mongodb.client.model.Filters;
import org.bson.Document;
import java.util.ArrayList;
import java.util.List;

public class SachDAOImpl {
    private final MongoCollection<Document> sachCollection;

    public SachDAOImpl() {
        MongoDatabase database = MongoConnection.getDatabase();
        this.sachCollection = database.getCollection("Sach");
    }

    public List<Document> layTatCaSach() {
        return sachCollection.find().into(new ArrayList<>());
    }

    public void themSach(Document sach) {
        sachCollection.insertOne(sach);
    }
    //... các phương thức khác
}
```

## 3.4. Servlets và Trang JSP

**`DanhSachSachServlet.java`**

```java
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet("/danh-sach")
public class DanhSachSachServlet extends HttpServlet {
    private SachDAOImpl sachDAO = new SachDAOImpl();

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        req.setAttribute("danhSachSach", sachDAO.layTatCaSach());
        req.getRequestDispatcher("/WEB-INF/views/danh-sach.jsp").forward(req, resp);
    }
}
```

**`danh-sach.jsp`**

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ taglib uri="[http://java.sun.com/jsp/jstl/core](http://java.sun.com/jsp/jstl/core)" prefix="c" %>
<html>
<head>
    <title>Danh sách Sách</title>
</head>
<body>
    <h1>Danh sách Sách trong Thư viện</h1>
    <table border="1">
        <thead>
            <tr>
                <th>Tiêu đề</th>
                <th>Tác giả</th>
                <th>Số lượng</th>
            </tr>
        </thead>
        <tbody>
            <c:forEach var="sach" items="${danhSachSach}">
                <tr>
                    <td><c:out value="${sach.getString('tieu_de')}" /></td>
                    <td><c:out value="${sach.getString('tac_gia')}" /></td>
                    <td><c:out value="${sach.getInteger('so_luong')}" /></td>
                </tr>
            </c:forEach>
        </tbody>
    </table>
</body>
</html>
```

## 3.5. Sơ đồ luồng ứng dụng

Người dùng 
➡️ Trình duyệt 
➡️ Request 
➡️ Server (Tomcat) 
➡️ Servlet 
➡️ DAO 
➡️ MongoDB Driver 
➡️ MongoDB Server 
➡️ Response 
➡️ JSP 
➡️ HTML 
➡️ Trình duyệt.

# Hướng dẫn MongoDB và Java Web Servlet

## Phần 1: Nhập môn NoSQL và MongoDB (Sử dụng MongoDB Compass & VSCode)

Phần này tập trung vào việc xây dựng nền tảng kiến thức vững chắc về MongoDB.

### 1.1. Giới thiệu về NoSQL và MongoDB

#### NoSQL là gì?

NoSQL (thường được diễn giải là "Not Only SQL") là một thuật ngữ chung cho các cơ sở dữ liệu không tuân theo mô hình quan hệ (bảng và mối quan hệ) của SQL. Chúng được thiết kế để giải quyết các vấn đề mà cơ sở dữ liệu quan hệ truyền thống xử lý kém hiệu quả.

#### Sự khác biệt SQL vs. NoSQL:

**Cấu trúc dữ liệu (Schema):**
- **SQL**: Schema cố định (Schema-on-write). Bạn phải định nghĩa cấu trúc bảng (cột, kiểu dữ liệu) trước khi thêm dữ liệu.
- **NoSQL**: Schema linh hoạt (Schema-on-read). Dữ liệu có thể được lưu trữ mà không cần một cấu trúc định sẵn. Mỗi bản ghi có thể có các trường khác nhau.

**Khả năng mở rộng (Scalability):**
- **SQL**: Mở rộng theo chiều dọc (Vertical Scaling - tăng cường sức mạnh cho một máy chủ duy nhất như CPU, RAM).
- **NoSQL**: Mở rộng theo chiều ngang (Horizontal Scaling - thêm nhiều máy chủ hơn vào hệ thống). Điều này giúp xử lý lượng dữ liệu khổng lồ (Big Data) và lưu lượng truy cập cao hiệu quả hơn.

#### Khi nào nên sử dụng NoSQL?

- Khi bạn có dữ liệu lớn, phi cấu trúc hoặc bán cấu trúc.
- Khi yêu cầu về hiệu suất đọc/ghi rất cao.
- Khi ứng dụng cần sự linh hoạt và phát triển nhanh, cấu trúc dữ liệu thay đổi thường xuyên.
- Khi cần khả năng mở rộng quy mô dễ dàng.

#### Các loại hình NoSQL:

1. **Document Databases (Cơ sở dữ liệu tài liệu)**: Lưu trữ dữ liệu dưới dạng các tài liệu (document), thường là JSON hoặc BSON (Binary JSON). Rất linh hoạt. MongoDB là loại này.
2. **Key-Value Stores**: Lưu trữ dữ liệu dưới dạng cặp khóa-giá trị (key-value). Rất đơn giản và nhanh cho việc truy xuất dữ liệu theo khóa. (Ví dụ: Redis, DynamoDB).
3. **Column-family Stores**: Lưu trữ dữ liệu theo cột thay vì theo hàng. Hiệu quả cho các truy vấn tổng hợp trên một tập hợp con của các cột. (Ví dụ: Cassandra, HBase).
4. **Graph Databases (Cơ sở dữ liệu đồ thị)**: Được thiết kế để lưu trữ và điều hướng các mối quan hệ phức tạp giữa các thực thể. (Ví dụ: Neo4j, Amazon Neptune).

#### MongoDB là gì?

MongoDB là một hệ quản trị cơ sở dữ liệu document hàng đầu, thuộc họ NoSQL. Nó lưu trữ dữ liệu trong các tài liệu giống JSON, giúp việc lưu trữ và truy vấn dữ liệu trở nên trực quan và mạnh mẽ.

**Tại sao MongoDB phổ biến?**
- **Schema linh hoạt**: Dễ dàng thay đổi cấu trúc dữ liệu.
- **Truy vấn mạnh mẽ**: Hỗ trợ các truy vấn phong phú, sắp xếp, và tổng hợp dữ liệu.
- **Khả năng mở rộng cao**: Dễ dàng mở rộng theo chiều ngang.
- **Hiệu suất cao**: Hoạt động tốt với các tập dữ liệu lớn.

**Kiến trúc của MongoDB:**
- **Database**: Một nơi chứa các collection. Tương đương với một database trong SQL.
- **Collection**: Một nhóm các document. Tương đương với một bảng (table) trong SQL. Tuy nhiên, nó không yêu cầu các document phải có cùng cấu trúc.
- **Document**: Một bản ghi dữ liệu, được lưu dưới định dạng BSON (một dạng nhị phân của JSON). Tương đương với một hàng (row) trong SQL.

#### Ví dụ so sánh lưu trữ "Người dùng":

**Cách lưu trữ trong SQL (Cần 3 bảng):**
- users (id, username, password)
- user_profiles (user_id, first_name, last_name, birthdate)
- user_addresses (id, user_id, street, city, country)

=> Để lấy đủ thông tin, bạn cần thực hiện JOIN qua 3 bảng.

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

### 1.2. Cài đặt và Thiết lập Môi trường

#### Cài đặt MongoDB Community Server:
1. Truy cập trang chủ của MongoDB: MongoDB Community Server Download
2. Chọn phiên bản, hệ điều hành của bạn và tải về gói cài đặt (.msi cho Windows, .tgz cho macOS/Linux).
3. Làm theo hướng dẫn cài đặt trên trang chủ. Đây là nguồn tài liệu chính xác và cập nhật nhất.

#### Cài đặt và kết nối MongoDB Compass:
1. Tải về MongoDB Compass từ cùng trang download ở trên.
2. Sau khi cài đặt, mở Compass lên. Nếu bạn cài MongoDB Server trên máy cục bộ với cấu hình mặc định, Compass sẽ tự động đề xuất chuỗi kết nối: `mongodb://localhost:27017`.
3. Chỉ cần nhấn Connect là bạn đã kết nối thành công.

#### Cài đặt Extension "MongoDB for VS Code":
1. Mở VS Code.
2. Vào tab Extensions (Ctrl+Shift+X).
3. Tìm kiếm "MongoDB for VS Code" và nhấn Install.
4. Sau khi cài đặt, bạn sẽ thấy biểu tượng chiếc lá MongoDB ở thanh bên. Nhấn vào đó và kết nối tới `mongodb://localhost:27017`.
5. Bây giờ bạn có thể nhấp chuột phải vào server và chọn Launch Mongo Shell để bắt đầu gõ lệnh.

### 1.3. Các khái niệm cốt lõi (CRUD Operations)

Mở Mongo Shell trong VS Code hoặc Terminal để thực hiện các lệnh sau.
Đầu tiên, chuyển sang database QuanLyThuVien. Nếu database này chưa tồn tại, MongoDB sẽ tự động tạo nó khi bạn thêm dữ liệu vào.

```javascript
use QuanLyThuVien
```

#### Create (Thêm mới)

**Thêm một cuốn sách (insertOne):**

```javascript
db.Sach.insertOne({
  "tieu_de": "Lão Hạc",
  "tac_gia": "Nam Cao",
  "nam_xuat_ban": 1943,
  "the_loai": ["Truyện ngắn", "Văn học hiện thực"],
  "so_luong": 15,
  "thong_tin_nha_xuat_ban": {
    "ten": "NXB Văn học",
    "dia_chi": "Hà Nội"
  }
})
```

**Thêm nhiều cuốn sách cùng lúc (insertMany):**

```javascript
db.Sach.insertMany([
  {
    "tieu_de": "Số Đỏ",
    "tac_gia": "Vũ Trọng Phụng",
    "nam_xuat_ban": 1936,
    "the_loai": ["Tiểu thuyết", "Trào phúng"],
    "so_luong": 10
  },
  {
    "tieu_de": "Dế Mèn Phiêu Lưu Ký",
    "tac_gia": "Tô Hoài",
    "nam_xuat_ban": 1941,
    "the_loai": ["Truyện thiếu nhi"],
    "so_luong": 50,
    "thong_tin_nha_xuat_ban": {
      "ten": "NXB Kim Đồng",
      "dia_chi": "Hà Nội"
    }
  },
  {
    "tieu_de": "Chí Phèo",
    "tac_gia": "Nam Cao",
    "nam_xuat_ban": 1941,
    "the_loai": ["Truyện ngắn", "Văn học hiện thực"],
    "so_luong": 0
  }
])
```

#### Read (Đọc/Truy vấn)

**Tìm tất cả các cuốn sách (find({})):**

```javascript
db.Sach.find({})
```

**Tìm sách có tên tác giả là "Nam Cao":**

```javascript
db.Sach.find({ tac_gia: "Nam Cao" })
```

**Tìm sách được xuất bản sau năm 2000:**

Giải thích các toán tử so sánh:
- `$gt`: Greater Than (> - Lớn hơn)
- `$gte`: Greater Than or Equal (>= - Lớn hơn hoặc bằng)
- `$lt`: Less Than (< - Nhỏ hơn)
- `$lte`: Less Than or Equal (<= - Nhỏ hơn hoặc bằng)
- `$ne`: Not Equal (!= - Không bằng)
- `$in`: Nằm trong một mảng các giá trị

```javascript
db.Sach.find({ nam_xuat_ban: { $gt: 2000 } }) // Ví dụ này sẽ không trả về kết quả nào với dữ liệu mẫu
// Thử với điều kiện khác: Tìm sách xuất bản trước năm 1942
db.Sach.find({ nam_xuat_ban: { $lt: 1942 } })
```

**Tìm sách thuộc thể loại "Tiểu thuyết":**

```javascript
db.Sach.find({ the_loai: "Tiểu thuyết" })
```

#### Update (Cập nhật)

Giải thích các toán tử cập nhật phổ biến:
- `$set`: Thiết lập giá trị mới cho một trường. Nếu trường chưa tồn tại, nó sẽ được tạo ra.
- `$inc`: Tăng/giảm giá trị của một trường số (increment).
- `$push`: Thêm một phần tử vào một trường mảng.
- `$addToSet`: Thêm một phần tử vào một trường mảng nếu nó chưa tồn tại.
- `$pop`: Xóa phần tử đầu tiên (-1) hoặc cuối cùng (1) của một mảng.
- `$unset`: Xóa một trường khỏi document.

**Cập nhật số lượng của cuốn sách "Lão Hạc" lên 20:**

```javascript
db.Sach.updateOne(
  { tieu_de: "Lão Hạc" },       // Điều kiện tìm kiếm document
  { $set: { so_luong: 20 } }    // Hành động cập nhật
)
```

**Thêm một thể loại mới "Kinh điển" cho tất cả các sách của "Nam Cao":**

```javascript
db.Sach.updateMany(
  { tac_gia: "Nam Cao" },
  { $addToSet: { the_loai: "Kinh điển" } }
)
```

#### Delete (Xóa)

**Xóa một cuốn sách có số lượng bằng 0 (deleteOne):**

```javascript
db.Sach.deleteOne({ so_luong: 0 })
```

Lệnh này sẽ xóa cuốn "Chí Phèo" khỏi collection.

**Xóa tất cả các sách xuất bản trước năm 1940 (deleteMany):**

```javascript
db.Sach.deleteMany({ nam_xuat_ban: { $lt: 1940 } })
```

Lệnh này sẽ xóa cuốn "Số Đỏ".

## Phần 2: Kỹ thuật Nâng cao trong MongoDB

### 2.1. Truy vấn Nâng cao

#### Toán tử logic ($or, $and):

**Tìm sách của "Nam Cao" HOẶC "Vũ Trọng Phụng":**

```javascript
db.Sach.find({
  $or: [
    { tac_gia: "Nam Cao" },
    { tac_gia: "Vũ Trọng Phụng" }
  ]
})
```

**Tìm sách thuộc thể loại "Văn học hiện thực" VÀ có số lượng lớn hơn 10:**

```javascript
// $and là mặc định, nhưng có thể viết tường minh như sau:
db.Sach.find({
  $and: [
    { the_loai: "Văn học hiện thực" },
    { so_luong: { $gt: 10 } }
  ]
})
// Hoặc viết ngắn gọn
db.Sach.find({ the_loai: "Văn học hiện thực", so_luong: { $gt: 10 } })
```

#### Truy vấn trên mảng (Array):

**Tìm sách có đúng 2 thể loại ($size):**

```javascript
db.Sach.find({ the_loai: { $size: 2 } })
```

**Tìm sách chứa cả thể loại "Truyện ngắn" và "Văn học hiện thực" ($all):**

```javascript
db.Sach.find({ the_loai: { $all: ["Truyện ngắn", "Văn học hiện thực"] } })
```

#### Truy vấn trên document lồng nhau (Embedded Document):

**Tìm sách của nhà xuất bản có tên là "NXB Kim Đồng":**

```javascript
db.Sach.find({ "thong_tin_nha_xuat_ban.ten": "NXB Kim Đồng" })
```

#### Sử dụng Projection (Chiếu):

**Chỉ hiển thị tiêu đề và tác giả, đồng thời ẩn trường _id mặc định:**

```javascript
db.Sach.find(
  {}, // Điều kiện rỗng, tìm tất cả
  { tieu_de: 1, tac_gia: 1, _id: 0 } // 1: hiển thị, 0: ẩn
)
```

### 2.2. Indexing (Đánh chỉ mục)

**Index là gì?** Index trong MongoDB hoạt động giống như mục lục của một cuốn sách. Thay vì phải lật từng trang (quét toàn bộ collection - COLLSCAN) để tìm thông tin, cơ sở dữ liệu có thể nhìn vào mục lục (index) để đi thẳng đến vị trí chứa dữ liệu (quét index - IXSCAN), giúp tăng tốc độ truy vấn lên đáng kể, đặc biệt với các collection lớn.

#### Tạo Index:

**Tạo chỉ mục đơn (single-field index) trên trường tac_gia:**

```javascript
db.Sach.createIndex({ tac_gia: 1 }) // 1 là tăng dần, -1 là giảm dần
```

**Tạo chỉ mục kết hợp (compound index) trên the_loai và nam_xuat_ban:**

```javascript
db.Sach.createIndex({ the_loai: 1, nam_xuat_ban: -1 })
```

#### Sử dụng explain("executionStats"):

Để xem kế hoạch thực thi của một truy vấn, hãy thêm `.explain()` vào cuối.

```javascript
db.Sach.find({ tac_gia: "Nam Cao" }).explain("executionStats")
```

**Phân tích kết quả:** Hãy nhìn vào `executionStats.executionStages.stage`.
- Nếu giá trị là `COLLSCAN`: MongoDB đã phải quét toàn bộ collection.
- Nếu giá trị là `IXSCAN`: MongoDB đã sử dụng index, truy vấn sẽ nhanh hơn nhiều.

### 2.3. Aggregation Framework

Đây là một "đường ống" xử lý dữ liệu, nơi dữ liệu đi qua từng giai đoạn (stage) và được biến đổi.

**Yêu cầu 1: Thống kê số sách của mỗi tác giả.**

```javascript
db.Sach.aggregate([
  // Giai đoạn 1: Nhóm các document theo trường "tac_gia"
  {
    $group: {
      _id: "$tac_gia", // Nhóm theo tác giả
      so_luong_sach: { $sum: 1 } // Đếm mỗi document trong nhóm là 1
    }
  }
])
```

**Yêu cầu 2: Tìm thể loại sách phổ biến nhất.**

```javascript
db.Sach.aggregate([
  // Giai đoạn 1: "Trải phẳng" mảng the_loai, mỗi phần tử trong mảng sẽ tạo ra một document mới
  { $unwind: "$the_loai" },
  // Giai đoạn 2: Nhóm theo tên thể loại đã trải phẳng
  {
    $group: {
      _id: "$the_loai",
      so_luong: { $sum: 1 }
    }
  },
  // Giai đoạn 3: Sắp xếp các nhóm theo số lượng giảm dần
  { $sort: { so_luong: -1 } },
  // Giai đoạn 4: Giới hạn chỉ lấy kết quả đầu tiên (phổ biến nhất)
  { $limit: 1 }
])
```

**Yêu cầu 3: Tính tổng số sách trong thư viện theo từng năm xuất bản.**

```javascript
db.Sach.aggregate([
  // Giai đoạn 1: Nhóm theo năm xuất bản
  {
    $group: {
      _id: "$nam_xuat_ban",
      tong_so_luong: { $sum: "$so_luong" } // Tính tổng giá trị của trường so_luong trong mỗi nhóm
    }
  },
  // Giai đoạn 2: Sắp xếp theo năm xuất bản tăng dần
  { $sort: { _id: 1 } }
])
```

## Phần 3: Ứng dụng MongoDB với Java Web Servlet (Sử dụng IntelliJ)

### 3.1. Thiết lập dự án

1. Tạo một dự án Maven trong IntelliJ (File > New > Project... > Maven).
2. Mở tệp `pom.xml` và thêm các thư viện cần thiết: MongoDB Driver, Servlet API và JSTL.

```xml
<dependencies>
    <dependency>
        <groupId>org.mongodb</groupId>
        <artifactId>mongodb-driver-sync</artifactId>
        <version>4.11.1</version>
    </dependency>

    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <version>4.0.1</version>
        <scope>provided</scope>
    </dependency>

    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>jstl</artifactId>
        <version>1.2</version>
    </dependency>
</dependencies>
```

### 3.2. Lớp kết nối (Singleton Pattern)

Tạo một lớp để quản lý kết nối, đảm bảo ứng dụng chỉ có một instance MongoClient.

**MongoConnection.java**

```java
import com.mongodb.client.MongoClient;
import com.mongodb.client.MongoClients;
import com.mongodb.client.MongoDatabase;

public class MongoConnection {
    private static final String CONNECTION_STRING = "mongodb://localhost:27017";
    private static final String DATABASE_NAME = "QuanLyThuVien";

    private static MongoClient mongoClient = null;
    private static MongoDatabase database = null;

    private MongoConnection() {} // Private constructor

    public static MongoDatabase getDatabase() {
        if (mongoClient == null) {
            mongoClient = MongoClients.create(CONNECTION_STRING);
            database = mongoClient.getDatabase(DATABASE_NAME);
        }
        return database;
    }
}
```

### 3.3. Lớp DAO (Data Access Object)

Lớp này chịu trách nhiệm giao tiếp với database.

**SachDAO.java (Interface)**

```java
import org.bson.Document;
import java.util.List;

public interface SachDAO {
    void themSach(Document sach);
    Document timSachTheoTieuDe(String tieuDe);
    List<Document> layTatCaSach();
    void capNhatSach(String tieuDe, Document sachMoi);
    void xoaSach(String tieuDe);
}
```

**SachDAOImpl.java (Implementation)**

```java
import com.mongodb.client.MongoCollection;
import com.mongodb.client.MongoDatabase;
import com.mongodb.client.model.Filters;
import com.mongodb.client.model.Updates;
import org.bson.Document;

import java.util.ArrayList;
import java.util.List;

public class SachDAOImpl implements SachDAO {
    private final MongoCollection<Document> sachCollection;

    public SachDAOImpl() {
        MongoDatabase database = MongoConnection.getDatabase();
        this.sachCollection = database.getCollection("Sach");
    }

    @Override
    public void themSach(Document sach) {
        sachCollection.insertOne(sach);
    }

    @Override
    public Document timSachTheoTieuDe(String tieuDe) {
        return sachCollection.find(Filters.eq("tieu_de", tieuDe)).first();
    }

    @Override
    public List<Document> layTatCaSach() {
        return sachCollection.find().into(new ArrayList<>());
    }

    @Override
    public void capNhatSach(String tieuDe, Document sachMoi) {
        // Ví dụ: cập nhật số lượng
        int soLuongMoi = sachMoi.getInteger("so_luong");
        sachCollection.updateOne(Filters.eq("tieu_de", tieuDe), Updates.set("so_luong", soLuongMoi));
    }

    @Override
    public void xoaSach(String tieuDe) {
        sachCollection.deleteOne(Filters.eq("tieu_de", tieuDe));
    }
}
```

### 3.4. Servlets và Trang JSP

**DanhSachSachServlet.java**

```java
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet("/danh-sach")
public class DanhSachSachServlet extends HttpServlet {
    private SachDAO sachDAO = new SachDAOImpl();

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        req.setAttribute("danhSachSach", sachDAO.layTatCaSach());
        req.getRequestDispatcher("/WEB-INF/views/danh-sach.jsp").forward(req, resp);
    }
}
```

**ThemSachServlet.java**

```java
import org.bson.Document;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet("/them-sach")
public class ThemSachServlet extends HttpServlet {
    private SachDAO sachDAO = new SachDAOImpl();

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String tieuDe = req.getParameter("tieu_de");
        String tacGia = req.getParameter("tac_gia");
        int namXuatBan = Integer.parseInt(req.getParameter("nam_xuat_ban"));
        int soLuong = Integer.parseInt(req.getParameter("so_luong"));
        
        Document sachMoi = new Document("tieu_de", tieuDe)
                .append("tac_gia", tacGia)
                .append("nam_xuat_ban", namXuatBan)
                .append("so_luong", soLuong);

        sachDAO.themSach(sachMoi);
        resp.sendRedirect(req.getContextPath() + "/danh-sach");
    }
}
```

**danh-sach.jsp (Đặt trong src/main/webapp/WEB-INF/views/)**

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
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
                <th>Năm xuất bản</th>
                <th>Số lượng</th>
            </tr>
        </thead>
        <tbody>
            <c:forEach var="sach" items="${danhSachSach}">
                <tr>
                    <td><c:out value="${sach.getString('tieu_de')}"/></td>
                    <td><c:out value="${sach.getString('tac_gia')}"/></td>
                    <td><c:out value="${sach.getInteger('nam_xuat_ban')}"/></td>
                    <td><c:out value="${sach.getInteger('so_luong')}"/></td>
                </tr>
            </c:forEach>
        </tbody>
    </table>
</body>
</html>
```

### 3.5. Sơ đồ luồng ứng dụng

Luồng xử lý của một request sẽ như sau:

```
Người dùng (Trình duyệt) 
➡️ Gửi HTTP Request 
➡️ Server (Tomcat/Jetty) 
➡️ Servlet (Controller) 
➡️ Gọi phương thức của DAO (Data Access) 
➡️ DAO sử dụng MongoDB Driver 
➡️ Tương tác với MongoDB Server 
➡️ Dữ liệu được trả về 
➡️ Servlet gửi dữ liệu tới JSP 
➡️ JSP render thành HTML 
➡️ Trả về HTTP Response cho Trình duyệt
```

---

**Chúc bạn học tập hiệu quả với lộ trình chi tiết này!**
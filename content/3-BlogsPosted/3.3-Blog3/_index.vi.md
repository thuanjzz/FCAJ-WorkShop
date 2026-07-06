---
title: "Blog 3"
date: 2026-07-05
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# Từ ChromaDB sang pgvector: Đơn giản hóa Stack AI

### 1. Giới thiệu

Trong những phiên bản đầu tiên của dự án Video Semantic Search, chúng tôi đã sử dụng **ChromaDB** như một cơ sở dữ liệu vector (vector database) độc lập. Mặc dù ChromaDB rất tuyệt vời để làm prototype, nhưng việc duy trì một dịch vụ database riêng biệt chỉ để lưu trữ vector đã tăng thêm độ phức tạp không đáng có cho hạ tầng, deployment và quy trình backup.

Ở Tuần 7, chúng tôi đã đưa ra quyết định kiến trúc: migrate sang **pgvector** — một extension của PostgreSQL cho phép lưu trữ và truy vấn vector trực tiếp bên trong cơ sở dữ liệu quan hệ chính của dự án. Bài viết này chia sẻ về lý do, quy trình migrate và cách tối ưu truy vấn.

---

### 2. Tại sao chọn pgvector? (Đánh giá Trade-offs)

| Tiêu chí | ChromaDB (Độc lập) | pgvector (Extension của PostgreSQL) |
|---|---|---|
| **Hạ tầng** | Cần chạy service riêng (tốn port, RAM) | Chạy ngay trong instance RDS PostgreSQL hiện tại |
| **Tính nhất quán dữ liệu** | Khó duy trì ACID giữa SQL metadata và vector DB | Đảm bảo ACID, cho phép JOIN dữ liệu quan hệ trong 1 câu SQL |
| **Backup & Restore** | Phải backup 2 hệ thống độc lập | Chỉ cần một file `pg_dump` cho cả metadata lẫn vector |
| **Độ phức tạp** | Sử dụng API client riêng của ChromaDB | Truy vấn bằng SQL quen thuộc |

---

### 3. Thiết lập pgvector

#### Bước 1: Kích hoạt Extension
Sau khi kết nối vào PostgreSQL database, chạy lệnh:
```sql
CREATE EXTENSION IF NOT EXISTS vector;
```

#### Bước 2: Định nghĩa Schema bảng
Trong hệ thống, mỗi phân cảnh video (scene) có một đoạn mô tả chi tiết bằng chữ (caption). Đoạn mô tả này được convert sang vector 1024 chiều (dùng Bedrock Titan Embeddings v2). Ta lưu trữ nó trong bảng `scenes`:

```sql
CREATE TABLE scenes (
    id SERIAL PRIMARY KEY,
    asset_id INTEGER REFERENCES assets(id) ON DELETE CASCADE,
    start_time DOUBLE PRECISION,
    end_time DOUBLE PRECISION,
    caption TEXT,
    embedding vector(1024) -- Lưu trữ vector 1024 chiều
);
```

---

### 4. Tìm kiếm ngữ nghĩa bằng Cosine Similarity

Với pgvector, việc tìm kiếm phân cảnh phù hợp với truy vấn của người dùng trở nên rất đơn giản. Ta sinh vector embedding của câu truy vấn (`[0.123, -0.456, ...]`), sau đó chạy một câu lệnh SQL:

```sql
SELECT id, start_time, end_time, caption, 
       1 - (embedding <=> '[0.123, -0.456, ...]') AS similarity
FROM scenes
WHERE asset_id = 4
ORDER BY embedding <=> '[0.123, -0.456, ...]'
LIMIT 5;
```

- `<=>` là toán tử tính khoảng cách cosine (cosine distance) trong pgvector.
- `1 - (embedding <=> query)` chuyển khoảng cách thành **cosine similarity** (độ tương đồng, giá trị gần 1 nghĩa là càng giống).
- Bằng cách thêm điều kiện `WHERE asset_id = 4`, ta có thể lọc nhanh theo video asset cụ thể trước khi tính toán vector, nhanh và sạch hơn nhiều so với viết metadata filter trong ChromaDB.

---

### 5. Tối ưu hiệu năng bằng HNSW Index

Khi database tăng trưởng đến hàng trăm ngàn phân cảnh, việc quét tuần tự (sequential scan) sẽ bị chậm. Ta tối ưu hóa bằng cách tạo index **HNSW (Hierarchical Navigable Small World)**:

```sql
CREATE INDEX ON scenes 
USING hnsw (embedding vector_cosine_ops) 
WITH (m = 16, ef_construction = 64);
```

Index này xây dựng một đồ thị nhiều lớp để tìm kiếm lân cận gần đúng (ANN), giảm độ trễ truy vấn từ mili-giây xuống micro-giây.

---

### 6. Kết luận

Chuyển từ ChromaDB sang pgvector giúp đơn giản hóa đáng kể stack công nghệ của dự án. Chúng tôi giảm được số lượng container Docker chạy ở local, đồng nhất hóa chiến lược backup và tận dụng được sức mạnh của câu lệnh JOIN quan hệ truyền thống. Với các dự án đã chạy sẵn PostgreSQL, pgvector là sự lựa chọn hiển nhiên.
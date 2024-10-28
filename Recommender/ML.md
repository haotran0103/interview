
### **2. Các Kỹ Thuật Đã Sử Dụng**
Dự án kết hợp cả **content-based recommendation** và **collaborative filtering** để tạo ra một hệ thống gợi ý **hybrid**:
   - **Content-based Recommendation**: Dựa trên thông tin về phim (như thể loại và thẻ tag), sử dụng `NearestNeighbors` để tìm các phim tương tự.
   - **Collaborative Filtering (SVD)**: Dùng phương pháp Matrix Factorization với mô hình SVD từ thư viện `Surprise` để khai thác mối quan hệ giữa người dùng và phim, dựa trên lịch sử rating của người dùng.

### **3. Quy Trình Triển Khai**
   - **Dữ liệu**: Sử dụng dữ liệu từ bộ dữ liệu phim, bao gồm các thông tin về phim và đánh giá của người dùng. Dữ liệu được chia thành `trainset` và `testset`.
   - **Mô Hình Content-based**: Huấn luyện mô hình `NearestNeighbors` dựa trên ma trận TF-IDF từ nội dung của phim.
   - **Mô Hình Collaborative Filtering (SVD)**: Huấn luyện mô hình SVD trên ma trận người dùng-phim từ dữ liệu huấn luyện.
   - **Hybrid Model**: Kết hợp cả hai điểm gợi ý từ content-based và collaborative filtering với một trọng số `alpha`, tạo ra điểm số tổng hợp (hybrid score).
   - **Đánh Giá**: Đánh giá hệ thống trên `testset` bằng cách tính toán **RMSE** và **Hit Rate (HR)**.

### **4. Kết Quả Đạt Được**
   - **RMSE**: Đo lường độ sai lệch trung bình giữa giá trị dự đoán và giá trị thực của các đánh giá.
   - **Hit Rate**: Dự án đạt mức **Hit Rate ~70%**, nghĩa là 70% người dùng nhận được ít nhất một gợi ý phim mà họ thích, cho thấy khả năng đáp ứng của hệ thống trong việc cung cấp gợi ý phù hợp.

---

### **5. Các Câu Hỏi Tiềm Năng và Cách Trả Lời**

#### **1. Tại sao chọn mô hình hybrid?**
   - **Trả lời**: Mỗi phương pháp có ưu và nhược điểm riêng. Content-based rất hữu ích khi không có nhiều thông tin về người dùng nhưng có nhiều đặc trưng về phim. Ngược lại, collaborative filtering tận dụng lịch sử đánh giá của người dùng để tìm ra sở thích chung. Mô hình hybrid giúp kết hợp điểm mạnh của cả hai để tạo ra gợi ý tốt hơn.

#### **2. Làm thế nào để cải thiện độ chính xác của Hit Rate?**
   - **Trả lời**: Có thể cải thiện bằng cách điều chỉnh **trọng số alpha** để tối ưu hóa điểm số hybrid, tăng cường các thuộc tính cho content-based (như sử dụng embeddings), hoặc tối ưu hóa tham số cho collaborative filtering. Ngoài ra, việc thử nghiệm các mô hình khác như NCF (Neural Collaborative Filtering) cũng có thể giúp tăng độ chính xác.

#### **3. Ý nghĩa của threshold trong Hit Rate?**
   - **Trả lời**: Threshold xác định mức đánh giá mà hệ thống coi là "thích" hoặc "không thích". Ví dụ, với threshold là 4.0, hệ thống coi các đánh giá từ 4.0 trở lên là phim mà người dùng thích. Điều này giúp đánh giá Hit Rate từ góc độ phản hồi tích cực của người dùng.

#### **4. Tại sao chỉ dùng RMSE và Hit Rate? Có thêm chỉ số nào khác không?**
   - **Trả lời**: RMSE và Hit Rate là các chỉ số phổ biến và dễ hiểu cho các hệ thống gợi ý. Tuy nhiên, nếu cần, có thể sử dụng thêm các chỉ số như **Precision**, **Recall**, và **NDCG** để đánh giá thêm về độ chính xác và khả năng xếp hạng của hệ thống.

#### **5. Những thách thức khi làm việc với dữ liệu lớn là gì?**
   - **Trả lời**: Với dữ liệu lớn như 100k bộ phim, tính toán độ tương đồng có thể gặp khó khăn về bộ nhớ. Sử dụng ma trận thưa hoặc các thuật toán gần đúng như Approximate Nearest Neighbors (ANN) là giải pháp. Ngoài ra, việc tối ưu hóa dữ liệu đầu vào và giảm kích thước đặc trưng cũng giúp cải thiện hiệu suất.

#### **6. Làm thế nào để tránh quá tải bộ nhớ với 100k bộ phim?**
   - **Trả lời**: Áp dụng các phương pháp giảm kích thước như PCA hoặc Truncated SVD để giảm số chiều dữ liệu, đồng thời tận dụng mô hình sparse matrix trong content-based và các phương pháp gần đúng để tiết kiệm bộ nhớ.
Khi sử dụng **Approximate Nearest Neighbors (ANN)** với `NearestNeighbors` để tìm các phim tương tự trong content-based recommendation, phương pháp này giúp tiết kiệm RAM và tăng tốc độ xử lý so với việc sử dụng các hàm `sin` và `cos` trực tiếp. Dưới đây là lý do:

### 1. **Cơ chế Lưu Trữ và Xử Lý Dữ Liệu Hiệu Quả của ANN**
   - **Sparse Matrix**: Thay vì lưu ma trận đặc trưng (ví dụ, ma trận TF-IDF hoặc embeddings phim) dưới dạng đầy đủ (dense matrix), các thư viện ANN như `FAISS`, `Annoy`, hay thậm chí là `sklearn.NearestNeighbors` đều hỗ trợ lưu ma trận thưa (sparse matrix). Điều này giúp giảm yêu cầu bộ nhớ vì chỉ lưu những phần thực sự quan trọng.
   - **Giảm Kích Thước Tính Toán**: Các kỹ thuật ANN không cần tính toàn bộ độ tương đồng cho từng cặp phim, mà chỉ tìm các điểm gần đúng (approximate) trong không gian. Thay vì tính toán `cosine similarity` chính xác giữa từng điểm, ANN giảm số lượng phép tính cần thiết, giúp tiết kiệm RAM và thời gian.

### 2. **Tránh Tính Toán Toàn Bộ Cặp Phim với Hàm `sin` và `cos`**
   - Các hàm lượng giác như `sin` và `cos` thường được dùng để tính toán tương đồng cosine. Khi làm việc với một tập dữ liệu lớn (như 100k bộ phim), tính toàn bộ cosine similarity cho từng cặp phim yêu cầu lượng lớn RAM và thời gian. ANN, thay vì sử dụng các hàm lượng giác cho mọi cặp, dùng các thuật toán xấp xỉ để tìm nhanh các điểm gần nhau mà không phải tính chi tiết từng cặp một.

### 3. **Kỹ Thuật Chỉ Số Hóa (Indexing) Của ANN**
   - ANN sử dụng cấu trúc chỉ số hóa như cây KD, LSH (Locality-Sensitive Hashing), hay các kỹ thuật phân mảnh không gian khác để lưu trữ các điểm dữ liệu. Các cấu trúc này tối ưu hóa truy vấn, giúp mô hình nhanh chóng tìm các điểm tương tự mà không cần duyệt toàn bộ không gian, điều mà hàm `cos` không thực hiện được.

### 4. **Tối Ưu RAM Thông Qua Approximate Search**
   - ANN thực hiện tìm kiếm gần đúng (approximate search), tức là tìm các điểm có độ chính xác tương đối, thay vì cần độ chính xác tuyệt đối cho mỗi cặp điểm. Điều này giảm số phép tính toán và bộ nhớ cần thiết, giúp tối ưu hiệu suất tổng thể mà vẫn đạt kết quả tương tự như khi tính toán cosine một cách trực tiếp.

Do đó, **ANN** tối ưu hơn bằng cách giảm lượng tính toán cần thiết và lưu trữ dữ liệu theo các phương pháp giúp tiết kiệm RAM và thời gian hơn so với tính toán cosine similarity với `sin` và `cos`.
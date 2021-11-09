# ML-Echocardiogram-Analysis
# 1.Bài toán phân lớp

- Đặt vấn đề : Bài toán phân loại ảnh là bài toán mà mục tiêu chính đó là phân loại một hình ảnh đầu vào (**input**) thành một nhãn (**label**) đầu ra (**output**). Với bài toán phân loại ảnh dữ liệu siêu âm tim (**Echocardiogram Analysis**) :
    - Đầu vào (**input**) là ảnh chứa mặt cắt của quả tim
    - Đầu ra (**output**) các ảnh của mặt cắt được phân loại đúng với nhãn (**label**)
- Mục tiêu :
    - Phân loại chính xác các **input** về các **label**
    - Sử dụng các mô hình của học sâu để train và test quá trình phân loại , đánh giá độ hiệu quả của bài toán

# 2.Tập dữ liệu (Database)

- Ảnh **RGB** mô tả mặt cắt siêu âm tim (khi dùng đầu dò siêu âm được đặt ở các vị trí khác nhau thì sẽ cho các loại mặt cắt khác nhau) . Ảnh trong tập dữ liệu thuộc **3 classes : 2C, 3C, 4C tương ứng với ảnh 2,3,4 buồng** . Ảnh được tách từ video thành các frame lưu dưới định dạng (id video)_(id frame).jpg với các kích thước khác nhau. Ví dụ 195_1.jpg là video số 195, frame số 1.
- Tệp dữ liệu DATA_CHAMBER_2021 được chia thành 2 folder train và test :
    - Train có tổng 6717 ảnh, gồm 2C(2377 ảnh), 3C(2309 ảnh), 4C(2031 ảnh)
    - Test có tổng 1607 ảnh, gồm 2C(409 ảnh), 3C(367 ảnh), 4C(831 ảnh)
- Dữ liệu đầu vào cho mạng CNN :
    - Định dạng : Tensor
    - Kích cỡ : resize ảnh về 244x244x3

# 3.Mạng CNN (convolution layer - conv) cho bài toán phân lớp

- CNN là bao gồm
    - Tổ hợp các khối gồm lớp conv và lớp pool nối tiếp nhau để tính toán đặc trưng ảnh, các đặc trưng về sau càng có tính khái quát cao.
    - Các lớp FC (fully connected) dùng để biến đổi đặc trưng và đầu ra
    

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e4152761-c840-42b7-b4cc-7d04c7ace668/Untitled.png)

- Các mạng CNN cụ thể :
- VGG16 và VGG19 :
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cddf1274-7da0-42fa-870d-fbfff1f989ba/Untitled.png)
    
    **VGG16 :** 
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/31cc6b73-a9bc-4619-9a1d-f3a0bc4b7046/Untitled.png)
    
    Đầu vào là ảnh RGB với kích thước 224x224
    
    Cấu trúc của VGG16 bao gồm 16 layer :
    
    - 13 layer Conv (2 layer conv-conv,3 layer conv-conv-conv) đều có kernel 3×3, sau mỗi layer conv là max pooling downsize xuống 0.5
    - 3 layer fully connection.
    
    **VGG19** : tương tự như VGG16 nhưng có thêm 3 layer convolution ở 3 layer conv cuối 
    
- **Inception v3**
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0028f284-4243-4347-962a-7a863a1ab50e/Untitled.png)
    
    - Sử dụng module Inception với nhiều kích thước bộ lọc
    - Sử dụng conv2d $1\times 1$ để giảm số kênh ảnh (nút thắt - bottleneck) trước khi thực hiện tích chập $3\times 3, 5\times 5$
    - Sử dụng lớp rút gọn toàn cục (global average pooling) thay thế cho FC
    - Sử dụng hàm lỗi bổ trợ (auxiliary loss) tại các khối mạngl

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/516cd4f0-dc5d-4180-a4a2-c4a8254da249/Untitled.png)

- **Resnet (Mạng phần dư)**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/15e711f2-4e85-4dcc-ae40-5a9d19177b22/Untitled.png)

- Giải quyết **Vanishing Gradients** trong quá trình backward propagation
- 
- Cho phép tạo ra mạng nơ-ron với độ sâu lớn (ResNet50, ResNet101, ResNet152)
- Sử dụng kỹ thuật nút thắt (bottleneck)
- Sử dụng lớp chuẩn hoá loạt (batch normalization)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/46a4fb99-e089-4146-9854-2e610b53b720/Untitled.png)

# 4.Kết quả thu được

- VGG16: 0.97
- VGG19: 0.948
- Resnet50: 0.974
- Inception v3: 1
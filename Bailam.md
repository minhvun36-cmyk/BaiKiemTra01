Câu 1: Phân biệt Value Types và Reference Types trong C#

Value Types (kiểu giá trị):

- Biến lưu trực tiếp giá trị của dữ liệu.
- Khi gán một biến Value Type cho biến khác, giá trị được sao chép sang biến mới.
- Hai biến sau khi gán là độc lập với nhau.
- Thường liên quan đến vùng nhớ Stack đối với biến cục bộ.

Reference Types (kiểu tham chiếu):

- Biến lưu tham chiếu đến đối tượng được cấp phát trong vùng nhớ Heap.
- Khi gán một biến Reference Type cho biến khác, tham chiếu được sao chép.
- Hai biến có thể cùng tham chiếu đến một đối tượng trên Heap.

Kết luận:

  Value Type lưu giá trị trực tiếp và khi gán sẽ sao chép giá trị. Reference Type lưu tham chiếu đến đối tượng và khi gán sẽ sao chép tham chiếu. Về mặt quản lý bộ nhớ, đối tượng Reference Type thường được cấp phát trên Heap, còn biến cục bộ có thể giữ tham chiếu trên Stack.

Câu 2: Init-only Properties init khác gì set thông thường?
-  set: cho phép thuộc tính được gán và thay đổi giá trị sau khi đối tượng đã được khởi tạo.
-  init: chỉ cho phép thuộc tính được thiết lập trong quá trình khởi tạo đối tượng.
-  Sau khi quá trình khởi tạo hoàn tất, thuộc tính có init không thể được thay đổi thông qua việc gán thông thường.
-  init giúp đảm bảo những dữ liệu cần cố định sau khi tạo đối tượng không bị thay đổi ngoài ý muốn.

Trường hợp sử dụng:

  Thường sử dụng init đối với các thuộc tính có giá trị cần được xác định khi tạo đối tượng và không nên thay đổi trong suốt vòng đời của đối tượng, chẳng hạn như mã định danh hoặc thông tin cấu hình ban đầu.

Câu 3: Phân biệt virtual và override
-  virtual được khai báo ở lớp cha.
-  Nó cho phép phương thức của lớp cha có thể được lớp con ghi đè.
-  override được sử dụng ở lớp con.
-  Nó dùng để ghi đè cách thực hiện của phương thức virtual hoặc phương thức đã được override từ lớp cha.
-  Cặp virtual và override được sử dụng để thực hiện tính đa hình (Polymorphism) trong C#.

Kết luận:

  Virtual là cơ chế cho phép ghi đè ở lớp cha, còn override là từ khóa dùng để thực hiện việc ghi đè ở lớp con.

Câu 4: Tại sao thành phần static không thể truy xuất thông qua Object Instance?
-  Thành phần static thuộc về lớp (Class) chứ không thuộc về từng đối tượng (Object Instance).
-  Thành phần static được dùng chung cho toàn bộ lớp và không phụ thuộc vào việc có bao nhiêu đối tượng được tạo ra.
-  Vì vậy, việc tạo một đối tượng bằng new không tạo ra một bản sao riêng của thành phần static cho đối tượng đó.
-  Thành phần static được truy xuất thông qua tên lớp thay vì thông qua đối tượng.

Kết luận:

  Static thuộc về Class, không thuộc về Object Instance, nên phải truy xuất thông qua tên lớp.

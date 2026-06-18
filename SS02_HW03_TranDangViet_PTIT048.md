# BÀI 3: Đọc hiểu & Dò lỗi qua Prompt (Phát hiện Ảo giác AI - Hallucination)

## Giải thích nguyên lý sinh lỗi ảo giác của AI

Hiện tượng Hallucination xảy ra khi AI tạo ra thông tin có vẻ hợp lý nhưng thực tế không chính xác hoặc không tồn tại. Trong trường hợp này, prompt đã định hướng AI sử dụng phương thức `StringUtils.hasSpecialCharacters()`. Vì tên phương thức nghe có vẻ hợp lý và phù hợp với bài toán xử lý chuỗi, AI có thể suy đoán rằng phương thức này tồn tại trong thư viện Apache Commons Lang và sinh ra mã nguồn sử dụng nó.

Tuy nhiên, trên thực tế thư viện Apache Commons Lang không hề cung cấp phương thức `hasSpecialCharacters()`. Nếu lập trình viên sao chép đoạn mã này mà không kiểm tra tài liệu chính thức, chương trình sẽ bị lỗi biên dịch. Đây là một ví dụ điển hình của hiện tượng Hallucination trong phát triển phần mềm.

## Prompt tối ưu mới

```text id="hallucination-safe-prompt"
Hãy viết một hàm Java kiểm tra xem một chuỗi có chứa ký tự đặc biệt hay không. Chỉ sử dụng các API chính thức của Java Standard Library hoặc các thư viện phổ biến có tài liệu chính thức rõ ràng. Không được tự tạo hoặc sử dụng các phương thức chưa được kiểm chứng. Sau khi viết mã nguồn, hãy giải thích ngắn gọn API được sử dụng và chỉ ra cơ sở tài liệu hoặc tên lớp/phương thức chính thức để chứng minh rằng chúng thực sự tồn tại.
```

## Mã nguồn Java do AI sinh ra

```java id="safe-java-code"
public class StringValidator {

    public static boolean hasSpecialCharacters(String text) {

        if (text == null || text.isEmpty()) {
            return false;
        }

        return !text.matches("[a-zA-Z0-9 ]*");
    }

    public static void main(String[] args) {
        System.out.println(hasSpecialCharacters("Hello123"));
        System.out.println(hasSpecialCharacters("Hello@123"));
    }
}
```

## Giải thích API được sử dụng

Đoạn mã sử dụng phương thức `matches()` của lớp `String` trong thư viện chuẩn Java (`java.lang.String`). Đây là API chính thức của Java và được tài liệu Oracle cung cấp đầy đủ.

Biểu thức chính quy `[a-zA-Z0-9 ]*` dùng để kiểm tra chuỗi chỉ chứa chữ cái, chữ số và khoảng trắng. Nếu chuỗi chứa ký tự khác tập này thì hàm trả về `true`, nghĩa là có chứa ký tự đặc biệt.

Việc sử dụng API chuẩn của Java giúp hạn chế nguy cơ Hallucination và đảm bảo mã nguồn có thể biên dịch, vận hành đúng trong thực tế.

```

```

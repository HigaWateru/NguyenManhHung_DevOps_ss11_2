# Bài 2: Biên dịch đóng gói tệp ứng dụng .jar với Gradle và lưu trữ Artifacts

## 1. Thông tin bài tập
- **Khóa học:** DevOps
- **Buổi học:** Session 11 - CI/CD với GitHub Actions
- **Bài thực hành:** Bài 2 (Biên dịch, đóng gói `.jar` và lưu trữ Artifacts)
- **Thư mục bài tập:** `ss11_2`

---

## 2. Kịch bản GitHub Actions (`.github/workflows/ci.yml`)

```yaml
name: CI Java Gradle Build and Artifacts

on:
  push:
    branches: [ main, master ]
  pull_request:
    branches: [ main, master ]
  workflow_dispatch:

jobs:
  test:
    name: Build, Test & Package JAR
    runs-on: ubuntu-latest

    steps:
      - name: Checkout mã nguồn
        uses: actions/checkout@v4

      - name: Thiết lập môi trường JDK 17
        uses: actions/setup-java@v4
        with:
          distribution: 'temurin'
          java-version: '17'
          cache: 'gradle'

      - name: Cấp quyền thực thi cho Gradle Wrapper
        run: chmod +x gradlew

      - name: Chạy Unit Test với Gradle
        run: ./gradlew test

      - name: Đóng gói tệp JAR với Gradle
        run: ./gradlew bootJar

      - name: Lưu trữ thành phẩm JAR Artifact
        uses: actions/upload-artifact@v4
        with:
          name: app-jar
          path: build/libs/*.jar
```

---

## 3. Kết quả đạt được
1. **Biên dịch & đóng gói thành công:** Lệnh `./gradlew bootJar` đã tạo ra file `.jar` hoàn chỉnh chứa ứng dụng Spring Boot và các dependencies nhúng sẵn trong thư mục `build/libs/`.
2. **Lưu trữ Artifacts tự động:** Action `actions/upload-artifact@v4` tự động nén và tải file `.jar` lên máy chủ GitHub Actions dưới dạng artifact `app-jar`.
3. **Kiểm tra Summary:** Sau khi pipeline chạy thành công, mục Artifacts trên tab Actions xuất hiện tệp `app-jar` sẵn sàng để tải về máy và kiểm tra thực thi.

---

## 4. Hình ảnh minh chứng nộp bài

> Hướng dẫn chi tiết cách chụp ảnh và vị trí chụp xem tại tệp: [HUONG_DAN_THUC_HANH_VA_CHUP_ANH.md](HUONG_DAN_THUC_HANH_VA_CHUP_ANH.md).

### Ảnh màn hình Summary tab Actions hiển thị artifact `app-jar`:
![Artifact Summary](screenshot_ex2_artifact_summary.png)

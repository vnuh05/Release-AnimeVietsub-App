# AnimeVietSub App — Bản phát hành

Kho này chứa **bản cài đặt (APK)** và **cấu hình từ xa** của ứng dụng Android xem AnimeVietSub (không chính thức). Mã nguồn ứng dụng được giữ riêng; kho này chỉ dùng để phát hành và cập nhật.

> Ứng dụng không liên kết với AnimeVietSub. Ứng dụng không chứa và không lưu trữ phim: mọi nội dung được tải trực tiếp từ trang AnimeVietSub và thuộc về chủ sở hữu bản quyền.

## Cài đặt

1. Vào [**Releases**](../../releases/latest) và tải file `AnimeVietSub-<phiên bản>.apk`.
2. Mở file APK. Nếu Android hỏi, cho phép trình duyệt / trình quản lý file **"Cài ứng dụng không rõ nguồn"**.
3. Yêu cầu: Android 7.0 trở lên. Chạy tốt trên điện thoại, máy tính bảng và máy gập (giao diện tự chuyển sang bố cục màn hình lớn).

**Dùng ngoài Việt Nam:** lần đầu mở app sẽ hiện màn **Xác minh truy cập** của AnimeVietSub. Bạn chỉ cần bấm ô *"Xác minh bạn là con người"* (Cloudflare) **một lần**. Bước câu hỏi "Xác minh người dùng Việt Nam" phía sau được app tự điền.

## Cập nhật

- App **tự kiểm tra bản mới** khi mở (tối đa 12 giờ/lần) và hỏi trước khi tải. Muốn kiểm tra ngay: **Tài khoản → Cài đặt → Kiểm tra cập nhật**.
- Bản cập nhật tải từ mục Releases của kho này. App kiểm tra **kích thước** và **mã SHA-256** của file trước khi mở trình cài đặt. Android chỉ cho cài đè khi bản mới **ký cùng khoá** với bản đang cài.
- Android không cho cài ngầm: bạn bấm **Cài đặt** một lần. Lần đầu, Android sẽ hỏi cho phép AnimeVietSub cài ứng dụng.
- Cập nhật giữ nguyên dữ liệu (đăng nhập, lịch sử, tủ phim, phim đã tải).

### Tự kiểm tra file APK

Chứng chỉ ký của mọi bản phát hành (SHA-256):

```text
F1:84:E6:F1:F5:29:2F:DA:14:BE:FF:04:8F:4B:46:34:B6:CE:60:D9:D3:43:A2:13:1D:FB:1A:9D:03:EC:B5:54
```

```bash
apksigner verify --print-certs AnimeVietSub-1.1.0.apk   # "certificate SHA-256 digest" phải khớp dòng trên
sha256sum AnimeVietSub-1.1.0.apk                        # phải khớp "sha256" trong version.json
```

## Các file trong kho

| File | Vai trò |
| --- | --- |
| `version.json` | Bản phát hành mới nhất — app đọc để biết có bản mới. |
| `config.json` | Cấu hình từ xa — sửa lỗi do trang AnimeVietSub đổi giao diện **mà không cần phát hành APK mới**. |
| `LICENSE` | Giấy phép MIT cho nội dung kho này. |
| `THIRD_PARTY_NOTICES.md`, `licenses/` | Giấy phép các thư viện mã nguồn mở đóng gói trong APK. |

App đọc hai file JSON qua `https://raw.githubusercontent.com/vnuh05/Release-AnimeVietsub-App/main/<file>` (GitHub có thể lưu đệm tới khoảng 5 phút). App **không gửi** cookie, tài khoản hay thông tin cá nhân khi đọc các file này.

### `version.json`

```json
{
  "versionCode": 2,
  "versionName": "1.1.0",
  "apkUrl": "https://github.com/vnuh05/Release-AnimeVietsub-App/releases/download/v1.1.0/AnimeVietSub-1.1.0.apk",
  "sha256": "<64 ký tự hex>",
  "sizeBytes": 3284612,
  "minSdk": 24,
  "changelog": "Nội dung thay đổi…"
}
```

- `apkUrl` phải là `https` trên GitHub. Link khác bị app từ chối.
- Chỉ cập nhật `version.json` **sau khi** đã tải APK lên Release. Nếu làm ngược lại, app sẽ báo lỗi tải.

### `config.json`

Mọi trường đều tuỳ chọn, trừ `schema`. Trường vắng hoặc sai thì app dùng giá trị đóng sẵn. File hỏng hoặc sai `schema` thì app giữ cấu hình cũ.

```json
{
  "schema": 1,
  "quizAnswers": [ { "keyword": "nhà giáo", "answer": "20/11" } ],
  "selectors": { "home.slider": ".MovieListTop .TPostMv" },
  "adblockRules": [ "||quangcao.example^" ],
  "notice": { "id": "2026-10-thong-bao", "title": "Thông báo", "message": "Nội dung…", "url": "https://…" },
  "minVersionCode": 2
}
```

| Trường | Ý nghĩa | Ràng buộc |
| --- | --- | --- |
| `quizAnswers` | Đáp án trang "Xác minh người dùng Việt Nam": `keyword` là biểu thức tìm trong câu hỏi (không phân biệt hoa thường, `\|` = "hoặc"), `answer` là đáp án điền vào. | Tối đa 20 mục. `keyword` chỉ gồm chữ, số, khoảng trắng và `\| ( ) . ? * + -`. `answer` chỉ gồm chữ, số, khoảng trắng và `/ . -`, dài tối đa 40. Mục sai bị bỏ qua. |
| `selectors` | Ghi đè CSS selector dùng để đọc trang (bảng khoá bên dưới). | Khoá dạng `nhóm.tên`, giá trị một dòng, tối đa 500 ký tự, tối đa 100 khoá. |
| `adblockRules` | Luật chặn quảng cáo bổ sung, cú pháp kiểu EasyList (`\|\|domain^`). | Tối đa 5000 dòng, mỗi dòng tối đa 300 ký tự. |
| `notice` | Thông báo hiện **một lần** cho mỗi `id`. | `message` bắt buộc. `url` (nếu có) phải là `https`. |
| `minVersionCode` | Bản cũ hơn mức này bị yêu cầu cập nhật (ẩn nút "Để sau"). | Số nguyên ≥ 0. |

Các khoá selector ghi đè được:

| Nhóm | Khoá |
| --- | --- |
| Thẻ phim trong danh sách | `listCard.root`, `listCard.link`, `listCard.title`, `listCard.image`, `listCard.imageAttrPreferred`, `listCard.imageAttrFallback`, `listCard.episodeBadge`, `listCard.status` |
| Trang chủ | `home.slider`, `home.section`, `home.sectionTitle`, `home.menuGenreLinks` |
| Trang chi tiết | `detail.title`, `detail.altTitle`, `detail.poster`, `detail.description`, `detail.infoRow`, `detail.genreLinks`, `detail.scorePercent`, `detail.watchButton`, `detail.relatedSeasonLinks`, `detail.webWatchedEpisodes`, `detail.episodeServerBlock`, `detail.episodeServerName`, `detail.episodeList` |
| Bình luận | `comment.item`, `comment.author`, `comment.content`, `comment.time`, `comment.avatar` |

App tải lại `config.json` mỗi lần mở. Muốn áp ngay: **Cài đặt → Tải lại cấu hình**.

## Giấy phép

- Nội dung kho này (APK, file cấu hình, tài liệu) phát hành theo [Giấy phép MIT](LICENSE) © 2026 Nhims.
- Các thư viện bên thứ ba trong APK giữ nguyên giấy phép gốc (Apache 2.0, MIT): xem [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md).
- Tên "AnimeVietSub", nội dung phim và hình ảnh trên trang thuộc về chủ sở hữu tương ứng, không thuộc phạm vi giấy phép trên.

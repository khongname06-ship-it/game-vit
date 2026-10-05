# 🦆 Đua vịt trắc nghiệm

Game trắc nghiệm có đường đua vịt: ai trả lời đúng và nhanh thì vịt bơi trước. Vị trí các vịt cập nhật trực tiếp cho mọi người chơi.

## Cấu trúc
- `index.html`: giao diện và logic game
- `questions.js`: câu hỏi và thời gian mỗi câu
- `config.js`: cấu hình Firebase và tên phòng

## Chạy thử (chế độ DEMO)
Game dùng ES module nên không mở trực tiếp bằng file được. Chạy server tĩnh:

    npx serve .        # hoặc: python3 -m http.server

Khi chưa cấu hình Firebase, game chạy DEMO với vịt máy.

## Bật bảng xếp hạng nhiều người (Firebase, miễn phí)
1. Vào https://console.firebase.google.com, tạo project mới.
2. Vào **Build → Realtime Database → Create Database**, chọn khu vực gần bạn (ví dụ Singapore).
3. Tab **Rules**, dán nội dung sau rồi Publish:

        {
          "rules": {
            "rooms": {
              "$room": {
                "players": {
                  ".read": true,
                  "$pid": {
                    ".write": true,
                    ".validate": "newData.hasChildren(['name','score','at']) && newData.child('name').isString() && newData.child('name').val().length <= 16 && newData.child('score').isNumber() && newData.child('score').val() <= 5000"
                  }
                }
              }
            }
          }
        }

4. **Project settings → Your apps → Web (</>)**, đăng ký app và copy `firebaseConfig` vào `config.js` (nhớ có `databaseURL`).
5. Đổi `ROOM` trong `config.js` cho mỗi buổi thuyết trình. Muốn giới hạn điểm tối đa, chỉnh số `5000` bằng `số câu × 150`.

Lưu ý: luật trên cho phép ai có link đều ghi được, phù hợp game vui trong buổi thuyết trình. Không dùng cho thi cử nghiêm túc.

## Đưa lên GitHub Pages
1. Tạo repo, đẩy toàn bộ file lên nhánh `main`.
2. **Settings → Pages → Deploy from a branch → main / (root)**.
3. Link game: `https://<tên-github>.github.io/<tên-repo>/`

## Màn hình trình chiếu
Mở `.../index.html?host=1` để chiếu đường đua cỡ lớn, không hiện phần trả lời. Gửi link thường cho người nghe.

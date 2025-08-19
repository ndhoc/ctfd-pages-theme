# ctfd-pages-theme

- Repo gốc: https://github.com/frankli0324/ctfd-pages-theme

- English guide: [README_EN.md](./README_EN.md).

## Mô tả

- Dùng để chia các challenges thành các ngăn được đặt menu bên trái, thay thế cho cách hiển thị các challenges ở cùng 1 trang như cách hiển thị ở theme core-beta.

- Áp dụng khi có rất nhiều challenges.

- Ảnh demo:
  
  <img width="1659" height="867" alt="image" src="https://github.com/user-attachments/assets/d49550d2-ef9d-4b61-8aa5-c6c7048d154b" />


## Cách cài đặt

- Yêu cầu đã cài đặt CTFd trước đó.

```sh
git clone https://github.com/CTFd/CTFd # Nếu đã cài đặt thì bỏ qua
git clone https://github.com/ndhoc/ctfd-pages-theme CTFd/themes/pages
```

- Truy cập vào `CTFd/CTFd/api/v1/challenges.py`.

- Thêm import sau ở đầu file:

```py
from CTFd.cache import cache
```

- Sau đó thêm:

```py
@challenges_namespace.route("/categories")
class ChallengeCategories(Resource):
    @challenges_namespace.doc(description="Endpoint to get Challenge categories in bulk")
    @cache.memoize(timeout=60)
    def get(self):
        chal_q = (Challenges.query.with_entities(Challenges.category).group_by(Challenges.category))
        if not is_admin() or request.args.get("view") != "admin":
            chal_q = chal_q.filter(and_(Challenges.state != "hidden", Challenges.state != "locked"))
        return {"success": True, "data": [i.category for i in chal_q]}
```


- Kiểm tra `/api/v1/challenges/categories` đã trả về kết quả chưa.

```json
{"success": true, "data": ["pwn", "web"]}
```

- Video hướng dẫn (Author: Frank0Li0)

[![Video Demo](https://img.youtube.com/vi/1Wqgjok4i88/maxresdefault.jpg)](https://www.youtube.com/watch?v=1Wqgjok4i88)

## Cách sử dụng

1. Vào `Admin Panel` > `Themes`
2. Chọn `pages` theme
3. Enjoy!


---
title: Tài liệu kỹ thuật CyStack API

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - python

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Giới thiệu

Dưới đây là tài liệu hướng dẫn sử dụng CyStack API. Bạn có thể sử dụng các API của chúng tôi để thực hiện các chức năng quản lý trên CyStack Platform. Bằng cách này, bạn có thể tích hợp CyStack Platform vào hệ thống của bạn hoặc tự phát triển một Scanner trên hạ tầng sẵn có của chúng tôi. 

Các chức năng mà bạn được phép/không được phép sử dụng thông qua CyStack APIClient

Chức năng | Truy cập qua API
-- | -- 
Tạo target | Không
Liệt kê và xem thông tin target | Có
Tạo và xem thông tin scan | Có
Xem thông tin lỗ hổng | Có
Xem thông tin và cấu hình Monitoring | Có
Quản lý thông tin người dùng | Không
Quản lý danh sách nhận tin (notifications) | Không
Quản lý API Keys | Không

Chúng tôi có các đoạn code mẫu được viết bằng Shell và Python. Bạn có thể tham khảo ở cột bên phải, ứng với các chức năng được cung cấp. Việc truy cập API bằng các ngôn ngữ lập trình khác hoàn toàn tương tự.

<aside class="notice">
Lưu ý: Các API không thuộc phạm vi tài liệu này được dành cho những mục đích khác của CyStack Team. Các developer <b>không nên sử dụng</b> chúng nếu không được phép.
</aside>

# Xác thực

CyStack Platform sử dụng hình thức xác thực thông qua API key (Bearer Token) cho tất cả các request từ Client gửi đến API Server. Bạn có thể tạo mới một API key từ trang [Quản lý API key](https://cystack.io/apikeys).

Sau khi đã có được API key, bạn phải thêm vào header của tất cả các request key/value sau:

`Authorization: Bearer cystackapiexample`

<aside class="notice">
Bạn phải thay <code>cystackapiexample</code> bằng API key thật của bạn.
</aside>

# Target

## Liệt kê danh sách target

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

## Delete a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.delete(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.delete(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -X DELETE
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.delete(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted" : ":("
}
```

This endpoint deletes a specific kitten.

### HTTP Request

`DELETE http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to delete


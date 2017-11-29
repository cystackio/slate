---
title: Tài liệu kỹ thuật CyStack API

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - python

toc_footers:
  - <a href='https://cystack.io/apikeys'>Tạo API Key</a>
  - <a href='/'>English</a>

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

```python
import requests
import json

ROOT_URL = 'https://api.cystack.io/v1'
API_KEY = 'cystackapiexample'
AUTHENTICATION_HEADER = {'Authorization': 'Bearer %s' % API_KEY}


def list_target():
    endpoint = "%s/targets" % ROOT_URL
    r = requests.get(endpoint, headers=AUTHENTICATION_HEADER)
    return json.loads(r.text)

print list_target()	
```

```shell
curl "https://api.cystack.io/v1/targets"
  -H "Authorization: Bearer cystackapiexample"
```

> Nếu thành công, kết quả nhận được sẽ là JSON như sau:

```json
{  
   "count":1,
   "next":null,
   "previous":null,
   "results":[  
      {  
         "verification":0,
         "href":"/v1/targets/599fc347-3e46-4163-8a1d-444bd1928a85",
         "id":"599fc347-3e46-4163-8a1d-444bd1928a85",
         "address":"https://cystack.net"
      }
   ]
}
```

Endpoint này liệt kê tất cả các target hiện có của user (ứng với API key)

### HTTP Request

`GET https://api.cystack.io/v1/targets`

### Các tham số theo URL

Tham số | Mô tả
--------- | -----------
page | Mặc định, kết quả trả về sẽ được phân trang với 10 kết quả/trang. Tham số page nhận vào một số tự nhiên đại diện cho trang cần lấy
q | Từ khóa dùng để lọc target
ordering | Giá trị được ưu tiên sắp xếp. Giá trị tham số hợp lệ `?ordering=target`, `?ordering=verification`, `?ordering=target,verification`. Nếu muốn sắp xếp theo chiều ngược lại, thêm dấu `-` vào trước giá trị cần sắp xếp chẳng hạn `?ordering=-target`

### Ý nghĩa kết quả trả về

Key | Mô tả
--------- | -----------
count | Số lượng kết quả lấy được
next | URL trang kết quả tiếp theo, nếu trang tiếp theo không có dữ liệu thì trả về `null`
previous | URL trang kết quả phía trước, nếu trang trước không có dữ liệu thì trả về `null`
results | Một mảng các đối tượng target, trong đó các thành phần được mô tả dưới đây
verification | Tình trạng xác thực chủ sở hữu, là 0/1 đại diện cho `đã xác thực chủ sở hữu` / `chưa xác thực chủ sở hữu`
href | Đường dẫn đến endpoint xem thông tin chi tiết target. Địa chỉ tuyệt đối = `https://api.cystack.io` + `đường dẫn`
id | ID của target
address | Địa chỉ truy cập target


<aside class="notice">
Lưu ý — Request phải gửi kèm API Key
</aside>

# Scan

## Khái niệm
Một scan tương ứng với một lần quét lỗ hổng, trên một target xác định

## Liệt kê scan

```python
import requests
import json

ROOT_URL = 'https://api.cystack.io/v1'
API_KEY = 'cystackapiexample'
AUTHENTICATION_HEADER = {'Authorization': 'Bearer %s' % API_KEY}


def list_scan():
    endpoint = "%s/scans" % ROOT_URL
    r = requests.get(endpoint, headers=AUTHENTICATION_HEADER)
    return json.loads(r.text)
```

```shell
curl "https://api.cystack.io/v1/scans"
  -H "Authorization: Bearer cystackapiexample"
```

> Nếu thành công, kết quả nhận được sẽ là JSON như sau:

```json
{  
   "count":1,
   "next":null,
   "previous":null,
   "results":[  
      {  
         "status":"Stopped",
         "profile":"Full Audit",
         "vulnerabilities":0,
         "start_time":"13:58:01 20/11/2017",
         "href":"/v1/scans/07c13583-a14d-43b7-814d-a8f6db64253d",
         "target":{  
            "ip":"42.112.22.134",
            "os":"Unknown",
            "id":"03c0e5ad-767e-4c86-8adb-85509965fd54",
            "address":"https://cystack.net"
         }
      }
   ]
}
```

Endpoint này liệt kê tất cả các scan hiện có của user (ứng với API key)

### HTTP Request

`GET https://api.cystack.io/v1/scans`

### Các tham số theo URL

Tham số | Mô tả
--------- | -----------
page | Mặc định, kết quả trả về sẽ được phân trang với 10 kết quả/trang. Tham số page nhận vào một số tự nhiên đại diện cho trang cần lấy
q | Từ khóa dùng để lọc target
ordering | Giá trị được ưu tiên sắp xếp. Giá trị tham số hợp lệ `?ordering=target`, `?ordering=start_time`, `?ordering=vulnerabilities`, `?ordering=target,start_time,vulnerabilities`. Nếu muốn sắp xếp theo chiều ngược lại, thêm dấu `-` vào trước giá trị cần sắp xếp chẳng hạn `?ordering=-target`
status | Lọc theo trạng thái của scan, các giá trị hợp lệ bao gồm `Stopped`, `Running`, `Stopping`, `Queued`

### Ý nghĩa kết quả trả về

Key | Mô tả
--------- | -----------
count | Số lượng kết quả lấy được
next | URL trang kết quả tiếp theo, nếu trang tiếp theo không có dữ liệu thì trả về `null`
previous | URL trang kết quả phía trước, nếu trang trước không có dữ liệu thì trả về `null`
results | Một mảng các đối tượng scan, trong đó các thành phần được mô tả dưới đây
profile | Tên profile scan được sử dụng trong scan này
vulnerabilities | Số lượng lỗ hổng tìm thấy
start_time | Thời gian bắt đầu scan
href | Đường dẫn đến endpoint xem thông tin chi tiết scan. Địa chỉ tuyệt đối = `https://api.cystack.io` + `đường dẫn`
target | Một dictionary chứa các thông tin của target
ip | IP của target
os | Hệ điều hành mà target đang sử dụng, trong trường hợp không xác định được thì trả về `Unknown`
id | ID của target
address | Địa chỉ của target

## Xem chi tiết một scan

```python
import requests
import json

ROOT_URL = 'https://api.cystack.io/v1'
API_KEY = 'cystackapiexample'
AUTHENTICATION_HEADER = {'Authorization': 'Bearer %s' % API_KEY}


def detail_scan(scan_id):
    endpoint = "%s/scans/%s" % (ROOT_URL, scan_id)
    r = requests.get(endpoint, headers=AUTHENTICATION_HEADER)
    return json.loads(r.text)
    
print detail_scan("9ba42a5f-f31d-4f18-9812-c7f18eca09e8")
```

```shell
curl "https://api.cystack.io/v1/scans/9ba42a5f-f31d-4f18-9812-c7f18eca09e8"
  -H "Authorization: Bearer cystackapiexample"
```

> Nếu thành công, kết quả nhận được sẽ là JSON như sau:

```json
{
  "status": "Stopped",
  "profile": "Full Audit",
  "description": "None",
  "noti_href": [
    "/v1/notifications/29"
  ],
  "start_time": 1511957637,
  "stop_time": 1511958637,
  "vuln_href": [
    "/v1/vulnerabilities/4b2dad22-3fc7-4719-a67c-2bcd0317b632",
    "/v1/vulnerabilities/5c067b1e-ffc6-41fc-9678-352cd391cc3d",
  ],
  "target": {
    "ip": "42.112.22.134",
    "os": "Unknown",
    "id": "395a2f59-ad32-4e1c-b9b0-4063a55cf4e2",
    "address": "https://cystack.net"
  }
}
```

Endpoint này trả về thông tin chi tiết của 1 scan

### HTTP Request

`GET https://api.cystack.io/v1/scans/<scan_id>`

### Ý nghĩa kết quả trả về

Key | Mô tả
--------- | -----------
status | Trạng thái của Scan
profile | Tên profile scan được sử dụng trong scan này
description | Mô tả về scan
start_time | Thời gian bắt đầu scan
start_time | Thời gian kết thúc scan
noti_href | Một danh sách tham chiếu đến những người được notify
vuln_href | Một danh sách tham chiếu đến các lỗ hổng tìm thấy
target | Một dictionary chứa các thông tin của target
ip | IP của target
os | Hệ điều hành mà target đang sử dụng, trong trường hợp không xác định được thì trả về `Unknown`
id | ID của target
address | Địa chỉ của target

# Vulnerability

## Khái niệm
Một Vulnerability tương ứng với một lỗ hổng tìm được sau khi scan

## Liệt kê lỗ hổng

```python
import requests
import json

ROOT_URL = 'https://api.cystack.io/v1'
API_KEY = 'cystackapiexample'
AUTHENTICATION_HEADER = {'Authorization': 'Bearer %s' % API_KEY}


def list_vulnerability():
    endpoint = "%s/vulnerabilities" % ROOT_URL
    print endpoint
    r = requests.get(endpoint, headers=AUTHENTICATION_HEADER)
    return json.loads(r.text)


print list_vulnerability()
```

```shell
curl "https://api.cystack.io/v1/vulnerabilities"
  -H "Authorization: Bearer cystackapiexample"
```

> Nếu thành công, kết quả nhận được sẽ là JSON như sau:

```json
{
  "count": 2,
  "next": "https://cystack.io/v1/vulnerabilities?page=2",
  "previous": null,
  "results": [
    {
      "false_positive": false,
      "severity": "Medium",
      "name": "Click-Jacking vulnerability",
      "href": "/v1/vulnerabilities/060d674c-fb48-4182-83db-85e0323b8fd1",
      "id": "060d674c-fb48-4182-83db-85e0323b8fd1",
      "target": "https://cystack.net"
    }
  ]
}
```

Endpoint này liệt kê tất cả các lỗ hổng hiện có của user (ứng với API key)

### HTTP Request

`GET https://api.cystack.io/v1/vulnerabilities`

### Các tham số theo URL

Tham số | Mô tả
--------- | -----------
page | Mặc định, kết quả trả về sẽ được phân trang với 10 kết quả/trang. Tham số page nhận vào một số tự nhiên đại diện cho trang cần lấy
q | Từ khóa dùng để lọc vulnerability và target
ordering | Giá trị được ưu tiên sắp xếp. Giá trị tham số hợp lệ `?ordering=severity`, `?ordering=target`, `ordering=false_positive`, `ordering=name`, `?ordering=severity,false_positive,name,target`. Nếu muốn sắp xếp theo chiều ngược lại, thêm dấu `-` vào trước giá trị cần sắp xếp chẳng hạn `?ordering=-target`
sev | Lọc theo mức độ nguy hiểm của lỗi, các giá trị hợp lệ bao gồm `High`, `Medium`, `Low`, `Information`

### Ý nghĩa kết quả trả về

Key | Mô tả
--------- | -----------
count | Số lượng kết quả lấy được
next | URL trang kết quả tiếp theo, nếu trang tiếp theo không có dữ liệu thì trả về `null`
previous | URL trang kết quả phía trước, nếu trang trước không có dữ liệu thì trả về `null`
results | Một mảng các đối tượng vulnerabilities, trong đó các thành phần được mô tả dưới đây
false_positive | Lỗi có phải false positive hay không
severity | Mức độ nguy hiểm của lỗi
name | Tên của lỗi
href | Đường dẫn đến endpoint xem thông tin chi tiết vulnerability
id | ID của vulnerability
target | Địa chỉ của target

## Xem chi tiết lỗ hổng

```python
import requests
import json

ROOT_URL = 'https://api.cystack.io/v1'
API_KEY = 'cystackapiexample'
AUTHENTICATION_HEADER = {'Authorization': 'Bearer %s' % API_KEY}


def detail_vulnerability(vuln_id):
    endpoint = "%s/vulnerabilities/%s" % (ROOT_URL, vuln_id)
    r = requests.get(endpoint, headers=AUTHENTICATION_HEADER)
    return json.loads(r.text)


print detail_vulnerability('060d674c-fb48-4182-83db-85e0323b8fd1')
```

```shell
curl "https://api.cystack.io/v1/vulnerabilities/060d674c-fb48-4182-83db-85e0323b8fd1"
  -H "Authorization: Bearer cystackapiexample"
```

> Nếu thành công, kết quả nhận được sẽ là JSON như sau:

```json
{
  "traffic_hrefs": [
    "/v1/vulnerabilities/060d674c-fb48-4182-83db-85e0323b8fd1/traffics/5a1b8babcca1e02c146c4afa"
  ],
  "description": "The whole target has no protection (X-Frame-Options header) against Click-Jacking attacksClickjacking (User Interface redress attack, UI redress attack, UI redressing) is a malicious technique of tricking a Web user into clicking on something different from what the user perceives they are clicking on, thus potentially revealing confidential information or taking control of their computer while clicking on seemingly innocuous web pages.\n\nThe server didn't return an `X-Frame-Options` header which means that this website could be at risk of a clickjacking attack. The `X-Frame-Options` HTTP response header can be used to indicate whether or not a browser should be allowed to render a page inside a frame or iframe. Sites can use this to avoid clickjacking attacks, by ensuring that their content is not embedded into other sites.",
  "scan": {
    "target": {
      "id": "395a2f59-ad32-4e1c-b9b0-4063a55cf4e2",
      "address": "https://cystack.net"
    },
    "id": "06b804ed-2775-4e35-90d2-bd5a047762c3"
  },
  "severity": "Medium",
  "solution": "Configure your web server to include an X-Frame-Options header.",
  "plugin_name": "click_jacking",
  "false_positive": false,
  "references": [
    {
      "url": "http://tools.ietf.org/html/rfc7034",
      "title": "RFC-7034"
    },
    {
      "url": "https://developer.mozilla.org/en-US/docs/Web/HTTP/X-Frame-Options",
      "title": "Mozilla developer network"
    },
    {
      "url": "https://www.owasp.org/index.php/Clickjacking",
      "title": "OWASP Clickjacking document"
    },
    {
      "url": "http://vulndb.github.io/vuln/53",
      "title": "Vulndb-53"
    }
  ],
  "urls": [
    "None"
  ],
  "var": "",
  "attributes": {},
  "id": "060d674c-fb48-4182-83db-85e0323b8fd1",
  "name": "Click-Jacking vulnerability"
}
```

Endpoint này trả về thông tin chi tiết của 1 vulnerability

### HTTP Request

`GET https://api.cystack.io/v1/vulnerabilities/<vuln_id>`

### Ý nghĩa kết quả trả về

Key | Mô tả
--------- | -----------
traffic_hrefs | Danh sách href các traffic của vulnerability
description | Mô tả về lỗi
severity | Mức độ nguy hiểm của vulnerability
solution | Cách xử lý vulnerability
plugin_name | Tên plugin phát hiện ra lỗi
false_positive | Lỗi có phải false positive hay không
scan | Thông tin về đối tượng scan, trong đó bao gồm scan_id và target
references | Danh sách các tài liệu tham chiếu đến lỗi
urls | Danh sách các URL bị lỗi
var | Tham số chứa lỗi
attributes | Các thuộc tính của lỗi (nâng cao)
id | ID của lỗi
name | Tên của lỗi

## Cập nhật lỗ hổng

```python
import requests
import json

ROOT_URL = 'https://api.cystack.io/v1'
API_KEY = 'cystackapiexample'
AUTHENTICATION_HEADER = {'Authorization': 'Bearer %s' % API_KEY}


def update_vulnerability(vuln_id):
    endpoint = "%s/vulnerabilities/%s" % (ROOT_URL, vuln_id)
    data = {"false_positive": True}
    r = requests.put(endpoint, headers=AUTHENTICATION_HEADER, json=data)
    return json.loads(r.text)


print update_vulnerability('060d674c-fb48-4182-83db-85e0323b8fd1')
```

```shell
curl -i -s -k  -X 'PUT' "https://api.cystack.io/v1/vulnerabilities/060d674c-fb48-4182-83db-85e0323b8fd1" \
    -H "Authorization: Bearer cystackapiexample" \
    --data-binary $'{\"false_positive\":true}' 
```

> Nếu thành công, kết quả nhận được sẽ là JSON như sau:

```json
{
  "traffic_hrefs": [
    "/v1/vulnerabilities/060d674c-fb48-4182-83db-85e0323b8fd1/traffics/5a1b8babcca1e02c146c4afa"
  ],
  "description": "The whole target has no protection (X-Frame-Options header) against Click-Jacking attacksClickjacking (User Interface redress attack, UI redress attack, UI redressing) is a malicious technique of tricking a Web user into clicking on something different from what the user perceives they are clicking on, thus potentially revealing confidential information or taking control of their computer while clicking on seemingly innocuous web pages.\n\nThe server didn't return an `X-Frame-Options` header which means that this website could be at risk of a clickjacking attack. The `X-Frame-Options` HTTP response header can be used to indicate whether or not a browser should be allowed to render a page inside a frame or iframe. Sites can use this to avoid clickjacking attacks, by ensuring that their content is not embedded into other sites.",
  "scan": {
    "target": {
      "id": "395a2f59-ad32-4e1c-b9b0-4063a55cf4e2",
      "address": "https://cystack.net"
    },
    "id": "06b804ed-2775-4e35-90d2-bd5a047762c3"
  },
  "severity": "Medium",
  "solution": "Configure your web server to include an X-Frame-Options header.",
  "plugin_name": "click_jacking",
  "false_positive": true,
  "references": [
    {
      "url": "http://tools.ietf.org/html/rfc7034",
      "title": "RFC-7034"
    },
    {
      "url": "https://developer.mozilla.org/en-US/docs/Web/HTTP/X-Frame-Options",
      "title": "Mozilla developer network"
    },
    {
      "url": "https://www.owasp.org/index.php/Clickjacking",
      "title": "OWASP Clickjacking document"
    },
    {
      "url": "http://vulndb.github.io/vuln/53",
      "title": "Vulndb-53"
    }
  ],
  "urls": [
    "None"
  ],
  "var": "",
  "attributes": {},
  "id": "060d674c-fb48-4182-83db-85e0323b8fd1",
  "name": "Click-Jacking vulnerability"
}
```

Endpoint này giúp cập nhật trạng thái `false_positive` của một lỗi

### HTTP Request

`PUT https://api.cystack.io/v1/vulnerabilities/<vuln_id>`

### Các tham số trong json body

Tham số | Mô tả
--------- | -----------
false_positive | Cho biết lỗi này có bị false positive hay không. Các giá trị hợp lệ là `true` và `false`

### Ý nghĩa kết quả trả về

Key | Mô tả
--------- | -----------
traffic_hrefs | Danh sách href các traffic của vulnerability
description | Mô tả về lỗi
severity | Mức độ nguy hiểm của vulnerability
solution | Cách xử lý vulnerability
plugin_name | Tên plugin phát hiện ra lỗi
false_positive | Lỗi có phải false positive hay không
scan | Thông tin về đối tượng scan, trong đó bao gồm scan_id và target
references | Danh sách các tài liệu tham chiếu đến lỗi
urls | Danh sách các URL bị lỗi
var | Tham số chứa lỗi
attributes | Các thuộc tính của lỗi (nâng cao)
id | ID của lỗi
name | Tên của lỗi

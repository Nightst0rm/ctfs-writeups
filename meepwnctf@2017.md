|\\/|/-\T|-|
====
Ta có:    
![](https://latex.codecogs.com/png.latex?%5Clarge%20hack%20%3D%20pad%20*%20%5Csum_%7Bi%3D1%7D%5E%7Bn%3D14%7D%28a_%7Bi%7D.a_%7Bi&plus;1%7D..a_%7Bn%7D%29%20%5CRightarrow%20pad%7Chack)      
Giả sử `md5(flag)[0] != 0x00`, mình sẽ tìm tất cả pad có-thể-có bằng cách lấy tích các factor của hack và lấy các kết quả có độ lớn 16 
bytes (không mất tính tổng quát, nếu không tìm ra được flag, sẽ giảm dần số bytes) sau đó sử dụng đệ quy để brutce force các ký
tự phù hợp trong charset:
```python
import itertools
import string
from hashlib import md5
from Crypto.Util.number import *

alphabet = string.ascii_letters + " !?_@" + string.digits
HACK = 64364485357060434848865708402537097493512746702748009007197338675
factors = [3 ,3 ,5 ,5 , 7, 107, 487, 607 ,28429 ,29287 ,420577267963 ,3680317203978923 ,1002528655290265069]
pads = []

for i in xrange(len(factors)):
	for comb in itertools.combinations(factors, i):
		p = 1
		for n in comb:
			p *= n
		if 117 < len(bin(p)) - 2 < 129:
			pads.append(p)
			
for pad in pads:
	flag = []
	def r_check(n, flag):
		if len(flag) == 14:
			_flag = ''.join(flag)
			if bytes_to_long(md5(_flag).digest()) == pad:
				print "Flag is: MeePwnCTF{{{}}}".format(_flag)
		else:
			for c in alphabet:
				if n % ord(c) == 0:
					r_check((n / ord(c)) - 1, [c] + flag)
	r_check(HACK/pad, flag)
```
Flag is: MeePwnCTF{d0y0ul1keM@TH?}

4lph4||g4mm4
====
Reference:   
[http://gynvael.coldwind.pl/?id=615](http://gynvael.coldwind.pl/?id=615)    
[http://gynvael.coldwind.pl/?id=602](http://gynvael.coldwind.pl/?id=602)   
Chắc chắn mình sẽ không thể giải thích hơn những gì tác giả nói :sure:   
Solution:   
Mở file `repy.py` và thay thế hàm `_`:
```python
def _(s):
	__ = s[::-1].decode("zlib")
	if __ == "hashlib":
		return "hl"
	return __
```   
Tạo thêm 1 file `hl.py` cùng thư mục:   
```python
import hashlib

class md5(object):
	def __init__(self, s):
		print s
		exit()
```     
Chạy crackme và nhận flag: MeePwnCTF{FunnywithPython}    

Old School
===
[Link](https://nightst0rm.net/2017/07/writeupoldschool-meepwnctf-2017/)   

Br0kenMySQL
===
[Link](https://nightst0rm.net/2017/07/writeup-flag-shop-br0kenmysql-v3-meepwnctf/)

Flag Shop
===
[Link](https://nightst0rm.net/2017/07/writeup-flag-shop-br0kenmysql-v3-meepwnctf/)

LonelyBoy
===
[Link](https://nightst0rm.net/2017/07/writeup-lonelyboy-meepwnctf/)

Whoareyou
===
[Link](https://github.com/khtq/xitif/blob/master/whoareyou%40meepwnctf2017.md)

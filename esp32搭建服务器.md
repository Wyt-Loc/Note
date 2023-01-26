# 主要函数

```python
#导入库
import network
```

```python
#设置参数
ap = network.WLAN(network.AP_IF) #类型
ap.active(True)
ap.config(essid=ssid, password=password) #名称，密码
```

```python
#打印 IP 信息
print(ap.ifconfig())
```

```python
#访问 地址  直接浏览器粘贴复制 进行访问
http://192.168.4.1
```

## 整体代码

```python
# Complete project details at https://RandomNerdTutorials.com


try:
  import usocket as socket
except:
  import socket


import network


import esp
esp.osdebug(None)


import gc
gc.collect()


ssid = 'MicroPython-AP'
password = '123456789'


ap = network.WLAN(network.AP_IF)
ap.active(True)
ap.config(essid=ssid, password=password)


while ap.active() == False:
  pass


print('Connection successful')
print(ap.ifconfig())


#测试网页
def web_page():
  html = """<html><head>
		<script type="text/javascript" charset="utf-8" async=""
			src="https://cdn.jsdelivr.net/npm/live2d-widget@3.1.4/lib/L2Dwidget.0.min.js">
		</script>
		<script type="text/javascript"
			src="https://cdn.jsdelivr.net/npm/live2d-widget@3.1.4/lib/L2Dwidget.min.js?_=1557308476616"> </script>
		<script type="text/javascript">
			L2Dwidget.init();
		</script>
	</head>
	<body>
		<script>
			! function() {
				function n(n, e, t) {
					return n.getAttribute(e) || t
				}

				function e(n) {
					return document.getElementsByTagName(n)
				}

				function t() {
					var t = e("script"),
						o = t.length,
						i = t[o - 1];
					return {
						l: o,
						z: n(i, "zIndex", -1),
						o: n(i, "opacity", .5),
						c: n(i, "color", "0,0,0"),
						n: n(i, "count", 99)
					}
				}

				function o() {
					a = m.width = window.innerWidth || document.documentElement.clientWidth || document.body.clientWidth, c = m
						.height = window.innerHeight || document.documentElement.clientHeight || document.body.clientHeight
				}

				function i() {
					r.clearRect(0, 0, a, c);
					var n, e, t, o, m, l;
					s.forEach(function(i, x) {
						for (i.x += i.xa, i.y += i.ya, i.xa *= i.x > a || i.x < 0 ? -1 : 1, i.ya *= i.y > c || i.y < 0 ?
							-1 : 1, r.fillRect(i.x - .5, i.y - .5, 1, 1), e = x + 1; e < u.length; e++) n = u[e],
							null !== n.x && null !== n.y && (o = i.x - n.x, m = i.y - n.y, l = o * o + m * m, l < n
								.max && (n === y && l >= n.max / 2 && (i.x -= .03 * o, i.y -= .03 * m), t = (n.max - l) /
									n.max, r.beginPath(), r.lineWidth = t / 2, r.strokeStyle = "rgba(" + d.c + "," + (t +
										.2) + ")", r.moveTo(i.x, i.y), r.lineTo(n.x, n.y), r.stroke()))
					}), x(i)
				}

				var a, c, u, m = document.createElement("canvas"),
					d = t(),
					l = "c_n" + d.l,
					r = m.getContext("2d"),
					x = window.requestAnimationFrame || window.webkitRequestAnimationFrame || window.mozRequestAnimationFrame ||
					window.oRequestAnimationFrame || window.msRequestAnimationFrame || function(n) {
						window.setTimeout(n, 1e3 / 45)
					},
					w = Math.random,
					y = {
						x: null,
						y: null,
						max: 2e4
					};
				m.id = l, m.style.cssText = "position:fixed;top:0;left:0;z-index:" + d.z + ";opacity:" + d.o, e("body")[0]
					.appendChild(m), o(), window.onresize = o, window.onmousemove = function(n) {
						n = n || window.event, y.x = n.clientX, y.y = n.clientY
					}, window.onmouseout = function() {
						y.x = null, y.y = null
					};
				for (var s = [], f = 0; d.n > f; f++) {
					var h = w() * a,
						g = w() * c,
						v = 2 * w() - 1,
						p = 2 * w() - 1;
					s.push({
						x: h,
						y: g,
						xa: v,
						ya: p,
						max: 6e3
					})
				}
				u = s.concat([y]), setTimeout(function() {
					i()
				}, 100)
			}();
		</script>
	</body></html>"""
  
  return html

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.bind(('', 80))
s.listen(5)


while True:
  conn, addr = s.accept()
  print('Got a connection from %s' % str(addr))
  request = conn.recv(1024)
  print('Content = %s' % str(request))
  response = web_page()
  conn.send(response)
  conn.close()

```


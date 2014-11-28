域名record type 怎么设置
========================

url redirect 显性转发

url frame 隐性转发

A  正常的解析记录，即把域名指向一个IP，然后在这个IP的站点上邦定域名即可（IPV4)

CNAME 别名记录

TXT Record 文本记录，一般很少用

url redirect(301) 301错误转向

NS Record  DNS记录

AAAA 作用和A一样，指向IP是IPV6（第二代IP)

如果你是用来绑定站点的，一般是用A记录。TTL是解级记录缓存更新时间，值越小，更新频率越快，即代表，如果你对记录有做修改，TTL值越小，生效时间越快。

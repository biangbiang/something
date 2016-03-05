解决Python爬取HTTPS网页时的错误
===============================

因为想做一个爬虫定时领取淘宝的淘金币，无奈在使用requests获取页面内容时，收到了错误提示：

    /usr/local/lib/python2.7/dist-packages/requests/packages/urllib3/connectionpool.py:791: InsecureRequestWarning: Unverified HTTPS request is being made. Adding certificate verification is strongly advised. See: https://urllib3.readthedocs.org/en/latest/security.html
      InsecureRequestWarning)
    /usr/local/lib/python2.7/dist-packages/requests/packages/urllib3/connectionpool.py:791: InsecureRequestWarning: Unverified HTTPS request is being made. Adding certificate verification is strongly advised. See: https://urllib3.readthedocs.org/en/latest/security.html
      InsecureRequestWarning)

根据Google到的结果，解决方案如下：

    1.在使用requests前加入：requests.packages.urllib3.disable_warnings()
    2.为requests添加verify=False参数，比如：r = requests.get('https://blog.bbzhh.com',verify=False)

如下是 urllib3 文档的说明

    InsecurePlatformWarning

    New in version 1.11.
    Certain Python platforms (specifically, versions of Python earlier than 2.7.9) have restrictions in their ssl module that limit the configuration that urllib3 can apply. In particular, this can cause HTTPS requests that would succeed on more featureful platforms to fail, and can cause certain security features to be unavailable.
    If you encounter this warning, it is strongly recommended you upgrade to a newer Python version, or that you use pyOpenSSL as described in theOpenSSL / PyOpenSSL section.
    For info about disabling warnings, see Disabling Warnings.

---

2016年03月02日更新：

先附加一个官方的链接：

    https://urllib3.readthedocs.org/en/latest/security.html#snimissingwarning

之所以更新是因为，我在virutalenv的环境下，使用pip安装第三方包时，也遇到了类似的提示：

    /root/env/python27/debug/local/lib/python2.7/site-packages/pip/_vendor/requests/packages/urllib3/util/ssl_.py:315: SNIMissingWarning: An HTTPS request has been made, but the SNI (Subject Name Indication) extension to TLS is not available on this platform. This may cause the server to present an incorrect TLS certificate, which can cause validation failures. For more information, see https://urllib3.readthedocs.org/en/latest/security.html#snimissingwarning.
      SNIMissingWarning
    /root/env/python27/debug/local/lib/python2.7/site-packages/pip/_vendor/requests/packages/urllib3/util/ssl_.py:120: InsecurePlatformWarning: A true SSLContext object is not available. This prevents urllib3 from configuring SSL appropriately and may cause certain SSL connections to fail. For more information, see https://urllib3.readthedocs.org/en/latest/security.html#insecureplatformwarning.
      InsecurePlatformWarning

然而这种时候我是不可能去修改代码的，那么解决方案就在官方链接中的最后一部分：

Disabling Warnings

Making unverified HTTPS requests is strongly discouraged. ˙ ͜ʟ˙

But if you understand the ramifications and still want to do it...

Within the code

If you know what you’re doing and would like to disable all urllib3 warnings, you can use `disable_warnings()`:

    import urllib3
    urllib3.disable_warnings()

Alternatively, if you are using Python’s logging module, you can capture the warnings to your own log:

    logging.captureWarnings(True)

Capturing the warnings to your own log is much preferred over simply disabling the warnings.

Without modifying code

If you are using a program that uses urllib3 and don’t want to change the code, you can suppress warnings by setting the PYTHONWARNINGS environment variable in Python 2.7+ or by using the -W flag with the Python interpreter (see docs), such as:

    PYTHONWARNINGS="ignore:Unverified HTTPS request" ./do-insecure-request.py

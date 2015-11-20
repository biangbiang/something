解决pip无法使用http的源
======================

pip升级到最新版本之后(7.1吧)使用http协议的pip源之后会提示:

    The repository located at pypi.douban.com is not a trusted or secure host and is being ignored. If this repository is available via HTTPS it is recommended to use HTTPS instead, otherwise you may silence this warning and allow it anyways with '--trusted-host pypi.douban.com'.
    Could not find a version that satisfies the requirement psycopg2==2.6.1 (from -r doc/requirements.txt (line 6)) (from versions: )
    No matching distribution found for psycopg2==2.6.1 (from -r doc/requirements.txt (line 6))

解决方法:修改pip.conf

    [global]
    index-url = http://pypi.douban.com/simple
    [install]
    trusted-host = pypi.douban.com

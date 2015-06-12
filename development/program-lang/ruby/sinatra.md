微型Web框架(Ruby) Sinatra
=========================

Sinatra是一个基于Ruby语言的DSL（ 领域专属语言），可以轻松、快速的创建web应用。

    # myapp.rb
    require 'sinatra'

    get '/' do
      'Hello world!'
    end

安装gem，然后运行：

    gem install sinatra
    ruby myapp.rb

---
layout:     post
title:      "Brew, Gem, Rvm and Bundler"
subtitle:   ""
date:       2016-04-20 22:25:43 +0800
author:     "Liyf"
header-img: "img/160420.jpg"
catalog:    true
tags: 
    - ruby
---

> 转载自：[http://alvinhu.com/blog/2013/10/25/brew-gem-rvm-and-bundler/?utm_source=tuicool&utm_medium=referral](http://alvinhu.com/blog/2013/10/25/brew-gem-rvm-and-bundler/?utm_source=tuicool&utm_medium=referral)

<p>brew, gem, rvm, bundler分别为<a href="http://brew.sh">Homebrew</a>, <a href="http://rubygems.org">RubyGems</a>, <a href="http://rvm.io">Ruby Version Manager</a>, <a href="http://bundler.io">Bundler</a>的简称（执行命令）。</p>

<p>Homebrew为Mac OS X提供软件包的管理。Homebrew将软件包分装到单独的目录，然后symlink到<code>/usr/local</code>中。Homebrew不会把文件安装到预置目录之外，所以可以将Homebrew安装到任何位置。它完全基于git和ruby。使用gem来安装gems，用brew来搞定他们的依赖包。</p>

<p>brew的安装：</p>

<table><tbody><tr><td class="code"><pre><code class=""><span class="line">$ ruby -e "$(curl -fsSL https://raw.github.com/mxcl/homebrew/go)"</span></code></pre></td></tr></tbody></table>


<p>RubyGems是一个包管理框架，提供了ruby社区gem的托管服务，用于方便地下载、安装和使用ruby软件包。ruby软件包被称为gem，包含了ruby应用或库。安装RubyGems，需要先下载安装包，然后解压开后运行：</p>

<table><tbody><tr><td class="code"><pre><code class=""><span class="line">$ ruby setup.rb</span></code></pre></td></tr></tbody></table>


<p>brew和gem不同，brew用于操作系统层面上软件包的安装，而gem只是管理ruby软件。</p>

<p>Ruby Version Manager是一个命令行工具，可以方便地安装、管理不同的ruby版本，还可以为每个ruby版本创建不同的gem集合（gemsets），从而使不同的ruby应用可以独立使用自己的gem集合。</p>

<p>rvm的安装：</p>

<table><tbody><tr><td class="code"><pre><code class=""><span class="line">$ \curl -L https://get.rvm.io | bash -s stable</span></code></pre></td></tr></tbody></table>


<p>Bundler为ruby应用维持一个一致性的环境。它会跟踪应用代码和应用所需要的gem，这样应用总能包含它需要的gem（和版本）。</p>

<p>bundler的安装：</p>

<table><tbody><tr><td class="code"><pre><code class=""><span class="line">$ gem install bundler</span></code></pre></td></tr></tbody></table>


<p>安装顺序：先安装rvm，然后选择安装一个ruby版本，就可以提供一个完整的ruby运行环境。之后可以安装brew（brew基于ruby）和gem，分别管理操作系统和ruby的软件包。有了gem之后再安装bundler，因为bundler本身也是一个gem，直接通过gem安装即可。</p>
</div>
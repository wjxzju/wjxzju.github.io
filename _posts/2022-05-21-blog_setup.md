---
title: Hello blog
tags: 随笔

---

# 导语

2022年5月21日： 使用新的blog主题啦，换新！

新的博客主题是 [jekyll-TeXt-theme](https://github.com/kitian616/jekyll-TeXt-theme)，非常感谢开发者的贡献！

# Jekyll 安装

## Ruby2.6

```bash
sudo apt-get install ruby2.6
ruby -v
gem -v
```

## Jekyll & bundler

```bash
vim ~/.zshrc

# paste below codes
# Install Ruby Gems to ~/gems
export GEM_HOME="$HOME/gems"
export PATH="$HOME/gems/bin:$PATH"
# save & close

# install jekyll and bundler
gem install jekyll
gem install bundler
```

# Deploy Blog

```bash
git clone git@github.com:wjxzju/wjxzju.github.io.git
cd wjxzju.github.io
bundle install

bundle exec jekyll
```

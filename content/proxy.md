---
date: 2024-05-01T11:46:47.4747+08:00
last-modified: 2024-05-01T12:02:54.5454+08:00
tags:
  - hacks
---

# Brew

## change mirrors

### brew.git

```bash
# brew.git
cd "$(brew --repo)"
git remote set-url origin https://mirrors.ustc.edu.cn/brew.git

# revert changes
cd "$(brew --repo)"
git remote set-url origin https://github.com/Homebrew/brew.git
```

### homebrew-core.git

```bash
# homebrew-core.git
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git

# revert changes
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://github.com/Homebrew/homebrew-core.git
```

### homebrew-bottles

Add this line to `~/.zshrc` file

```bash
export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles
```

then

```bash
source ~/.zshrc
```

## reference

- https://zhangtianchen.com/archives/brew-su-du-tai-man-zen-me-ban

## proxy functions

Add these functions to `~/.zshrc` file:

```bash
function proxy_off() {
  unset http_proxy
  unset https_proxy
  echo "proxy turned off"
}

function proxy_on() {
  export no_proxy="localhost,127.0.0.1"
  export http_proxy="socks5://127.0.0.1:7891"
  export https_proxy=$http_proxy
  echo "proxy turned on"
}

function proxy_status() {
  echo $https_proxy
  curl cip.cc
}
```

Change `7891` to the port number of system proxy.
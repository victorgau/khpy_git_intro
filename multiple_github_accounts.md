# 在同一台電腦上使用多個Github帳戶

## 設定流程

參考資料：https://docs.github.com/en/authentication/connecting-to-github-with-ssh

## 產生金鑰(key)

需要先產生一對Public/Private金鑰，他們可以用來對傳輸的資料做加密，防止別人看到傳輸的內容。

簡單的說，公開金鑰(Public Key)加密的東西，只有私密金鑰(Private Key)解的開。

所以我們一般會把公開金鑰貼出來給大家，跟大家說如果你們要傳秘密訊息給我，請透過公開金鑰，因為私密金鑰只有我有，所以你傳過來的訊息只有我看的到。

要產生金鑰，請使用下面指令：
```bash
$ ssh-keygen -t ed25519 -C "your_email@example.com"
```

如果是舊版的ssh，請使用下面指令：
```bash
$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

## 設定金鑰

需從GitHub的設定頁上面，填入上面產生的Public Key。

將public key拷貝進clipboard，再貼到GitHub裡面去。

```bash
# windows上面可以這樣做
$ clip < ~/.ssh/id_ed25519.pub
```

```bash
# mac上面可以這樣做
$ pbcopy < ~/.ssh/id_ed25519.pub
```

## 更改電腦上的設定

在同一台電腦上要使用多個GitHub帳戶，需要在`~/.ssh/config`加入不同的HOST設定。

```bash
Host host1
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_email01
Host host2
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_email02
```

git會根據HOST來使用不同的credentials。

## 測試連線

```bash
$ ssh -T git@host1
```

## git clone 的時候使用host來識別需要的credential

```bash
$ git clone git@host1:/path/to/repo.git
```
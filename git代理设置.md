这个报错信息通常是由于网络连接问题引起的，最常见原因包括：

- **代理设置**问题，导致 TLS 握手失败  
- **网络环境**（如公司内网、防火墙、VPN 断开等）  
- **系统或 Git**自带的 SSL/TLS 库（如 GnuTLS、OpenSSL）兼容性或证书问题  

如果你确认自己需要通过代理上网（或者怀疑网络要求需要代理才能正常访问 GitHub），可以尝试给 Git 设置代理，方法如下。若你的网络环境不需要代理，反而是其他 TLS/证书问题导致的，需要从证书配置或网络连通性角度进一步排查。

---

## 1. Git 代理设置方法

### 1.1 临时配置（只针对当前终端 / 会话）

如果你只想在当前终端会话里设置一次性的代理，执行完后关闭终端就失效，可以直接在命令行导出环境变量：

```bash
export http_proxy="http://proxy.example.com:8080"
export https_proxy="http://proxy.example.com:8080"
```

这样，在此终端中执行 `git pull` 时，会使用上述代理。关闭终端后，这些环境变量就会失效。

> 如果你的代理使用用户名密码，可以这样写：
> 
> ```bash
> export http_proxy="http://username:password@proxy.example.com:8080"
> export https_proxy="http://username:password@proxy.example.com:8080"
> ```

### 1.2 针对 Git 的本地或全局配置

如果你想让 Git 在任何地方使用代理，可以通过 Git 的配置来进行设置。全局配置（`--global`）在所有项目中生效；也可以只针对当前仓库（`--local`）进行配置。常用命令如下：

```bash
# 全局配置 HTTP 和 HTTPS 代理
git config --global http.proxy http://proxy.example.com:8080
git config --global https.proxy http://proxy.example.com:8080

# 如果要取消全局代理，使用 --unset
git config --global --unset http.proxy
git config --global --unset https.proxy
```

若你只想在当前仓库中设置（作用域比全局更小），可以在项目文件夹中执行：

```bash
cd /path/to/your/repo
git config --local http.proxy http://proxy.example.com:8080
git config --local https.proxy http://proxy.example.com:8080
```

### 1.3 手动编辑 .gitconfig

无论是全局还是本地，Git 的配置信息最终都写在 `.gitconfig` 文件里，比如：

- 全局：位于 `~/.gitconfig`
- 本地仓库：位于 `.git/config`

你也可以用文本编辑器直接写入类似：

```ini
[http]
    proxy = http://proxy.example.com:8080
[https]
    proxy = http://proxy.example.com:8080
```

然后保存即可。

---

## 2. 如果并不需要代理（排查 TLS/证书等问题）

有时 `gnutls_handshake() failed: The TLS connection was non-properly terminated` 并不一定是代理导致。你可以先确认自己网络是否正常访问 GitHub：

```bash
ping github.com
curl -I https://github.com
```

如果能正常返回，说明网络没有完全被封锁；如果有防火墙或内网策略，你需要与网络管理员核实。

此外，如果你的系统版本较老或者 GnuTLS 版本不兼容，可能也会导致 TLS 握手报错。可尝试以下思路：

1. **更新系统证书和 GnuTLS**  
   在 Linux 系统中，可以尝试：
   
   ```bash
   sudo apt-get update
   sudo apt-get install --only-upgrade gnutls-bin ca-certificates
   ```
   
   也可尝试安装/升级其他相关包以保证系统中有最新的 CA 证书。

2. **改用 SSH 协议**  
   若总是因为 TLS/HTTPS 出现问题，可以改为使用 SSH 的方式进行拉取和推送。操作思路大致是：
   
   ```bash
   git remote remove origin
   git remote add origin git@github.com:lianyant/Algorithm-Learning.git
   ```
   
   然后在 GitHub 上添加你的 SSH 公钥，就无需走 HTTPS/TLS，而且在一些内网环境下可能更稳定。

3. **尝试使用 OpenSSL 取代 GnuTLS**  
   一些自定义编译的 Git 默认可能用的是 GnuTLS，也有人会在编译时选择 OpenSSL。可以查看你的 Git 版本和编译配置，或直接安装官方提供的 Git 版本，看是否改进握手问题。

---

### 小结

- 确定你是否**真的需要代理**：如果是在学校/公司网络，需要代理才能访问外网，那么正确设置 Git 代理即可；若网络本身可以直连 GitHub，则无需添加代理。
- 若网络无需代理却依然出现握手失败报错，可能是**证书或 GnuTLS 问题**，需要升级系统证书或改用 SSH 协议。
- 通过 `git config` 命令或编辑 `.gitconfig`，可设置和取消 Git 的 http/https 代理。

希望这些方法能帮助你解决 `gnutls_handshake() failed` 的错误，让你顺利完成 `git pull`、`git push` 等操作。若问题依旧，可以结合网络调试信息（如 `curl -v https://github.com`）或系统日志进一步排查。

# EZ Subdomain - 强大的子域名枚举工具

## 概述

EZ Subdomain 是一个高效且多功能的子域名枚举工具，适合初学者和高级用户。它包含无状态枚举、有状态枚举、基于被动 API 的子域名收集以及搜索引擎枚举。您可以轻松地同时运行多个模块，以快速发现目标域名的子域名。

EZ Subdomain 具有智能多级子域名枚举、优化内存使用、提高枚举速度以及结果去重等功能，旨在提供更优的子域名枚举体验。

## 功能特点

- **无状态枚举**：通过无状态多线程和可配置设置**快速**进行基于 DNS 的子域名枚举。
- **有状态枚举**：基于全连接的DNS子域名枚举。
- **被动 API 收集**：使用各种外部 API 被动收集子域名（例如 Fofa、Quake、Hunter 等），集成许多免费API，即使没有配置收费API，也能使用。
- **搜索引擎收集**：使用搜索引擎发现子域名。
- **智能多级枚举**：根据枚举结果自动决定多级域名的枚举策略。
- **性能优化**：**高效的内存使用**、优化的速度以及结果去重功能。
- **高可信性**：枚举前进行泛解析判断，减少无用结果。
- **一键下探多级子域名**：支持一键下探多级子域名。

## 本次更新

- 优化 api 被动模块。
- 增加扫描结束标识和运行时间提示（subdomainScan elapsed: time）。
- 增加 msec 社区证书校验。

## 注意事项

- windows 环境若使用无状态枚举模块，请先下载 [npcap](https://npcap.com/)，安装后再使用无状态枚举模块。
- linux 环境若使用无状态枚举模块，请先安装 [libpcap-dev]（apt-get install libpcap-dev），安装后再使用无状态枚举模块。
    - linux 使用无状态枚举模块，需要 sudo 执行 或者使用 setcap 为可执行文件赋予 CAP_NET_RAW 和 CAP_NET_ADMIN 权限。
    - `sudo setcap cap_net_raw,cap_net_admin=eip ./ez_subdomain`
- Linux/Mac 下的枚举速度会比 Windows 更快，更好的体验请使用 Linux/Mac。
- 无状态枚举模块和有状态枚举模块无法同时开启，默认开启无状态枚举模块。无状态枚举模块的速度远快于有状态爆破模块，建议使用无状态枚举模块。
- api key 请在 config.yaml 中配置（首次运行会自动生成）。
- 大字典88万+，中字典3.6万+，小字典3800+，无状态爆破depth 1域名，**大字典**9分钟就可以枚举完成。
- 使用无状态和有状态枚举时，请关闭clash的tun模式，否则会误判为泛解析，导致无法进行后续的枚举。
- 被动api收集模块，有许多国外的API，使用Proxy会有更好的体验。


## 使用方法

EZ Subdomain 提供灵活的选项，可以根据您的需求定制枚举过程。

### 命令行选项

```plaintext
NAME:
   subdomain - A powerful subdomain scanner

USAGE:
   subdomain [global options]

DESCRIPTION:
   subdomain enumeration tool by ezreal team

选项：
   --target value, -t value         目标域名进行枚举（例如：example.com, nsfocus.com）
   --target-file value, --tf value  包含目标域名列表的文件
   --disable value                  禁用的模块：StatelessBlast, StateBlast, Passive, SearchEngine（默认值："StateBlast"）
   --dict-level value, --dl value   StatelessBlast：爆破模式的字典级别（1: 大, 2: 中, 3: 小）（默认值：2）
   --dict-path value, --dp value    StatelessBlast：自定义字典路径
   --depth value                    StatelessBlast：枚举目标域名上方的层级数（默认值：1）
   --forced                         StatelessBlast：强制枚举所有多层级域名（默认值：false）
   --proxy value                    Passive&Search：代理 URL
   --log-level value, --ll value    日志级别：debug, info, warning, error, disable（默认值："error"）
   --max-retry value, --mr value    StatelessBlast：最大重试次数（默认值：3）
   --timeout value                  StatelessBlast：每次 DNS 查询的超时时间（默认值：3）
   --bandwidth value, --bw value    StatelessBlast：带宽（单位：Mbps）（默认值：5）
   --dns value                      StatelessBlast：定义用于 DNS 解析的 DNS 服务器，例如 '114.114.114.114, 8.8.8.8'
   --max-api value, --ma value      Passive：最大 API 结果数（默认值：10000）
   --ua value                       Passive&Search：用户代理字符串（默认值："Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/126.0.0.0 Safari/537.36 Edg/126.0.0.0"）
   --output value, -o value         输出文件名（默认值："result.txt"）
   --config value                   配置文件路径（默认值："config.yaml"）
   --lic value                      ez证书 (默认值: "ez.lic")
   --help, -h                       显示帮助信息
```

### 使用示例

1. **基本枚举**：使用无状态爆破、被动 API 和搜索引擎模块枚举 `nsfocus.com`和`baidu.com` 的下一级域名(默认枚举深度为1，也就是枚举`x.nsfocus.com`)，并将结果保存到 `result.txt`。
   ```bash
   ez_subdomain -t nsfocus.com,baidu.com
   ```

2. **高级枚举**：枚举域名深度为2级（包括1级深度，也就是枚举先枚举`x.nsfocus.com`,再枚举`y.x.nsfocus.com`）;启用 debug 级别日志。
   ```bash
   ez_subdomain -t nsfocus.com --depth 2 -o nsfocus_depth2.txt --ll debug
   ```

3. **强制枚举**：在不使用智能枚举(--forced)的情况下枚举高深度(depth)子域名，确保每个低深度(depth)域名都进行扩展（注意：较慢）。
   ```bash
   ez_subdomain -t nsfocus.com --proxy http://127.0.0.1:7897 --depth 2 --forced -o nsfocus_depth2_forced.txt
   ```

4. **多目标枚举深度**： 多目标枚举(使用`,`分割)，枚举深度为1级（枚举深度会根据输入的目标进行自动化枚举，例如下面会枚举`x.nsfocus.com`以及`x.api.nsfocus.com`。）
    ```bash
    ez_subdomain -t nsfocus.com,api.nsfocus.com -o nsfocus_multiple.txt --depth 1
    ```

5. **批量枚举** 使用 `--tf` 参数，枚举文件中的所有域名(文件每行一个域名)。
   ```bash
   ez_subdomain --tf domain.txt -o domain_result.txt
   ```

6. **自定义子域名字典**：使用 `--dp` 参数指定自定义字典文件(文件每行一个词)。
   ```bash
   ez_subdomain -t nsfocus.com -o nsfocus_custom_dict.txt --dp custom_dict.txt
   ```

7. **仅无状态爆破**：禁用其他模块，使用大字典(-dict-level 1)枚举 `nsfocus.com`和`baidu.com` 的1级深度域名。
   ```bash
   ez_subdomain -t nsfocus.com,baidu.com --depth 1 -o nb_depth1_StatelessBlast.txt --dl 1 --disable StateBlast,Passive,SearchEngine
   ```

## 下个版本计划功能

- api增加。
- 增加更多高级枚举设置。
- 子域名黑名单。
- cdn判断优化。




# 开源许可合规扫描 · 平时作业

《开源创新思维》课程平时作业（第 2 章 开源合规与许可证）。用开源许可审计工具对一个真实开源项目的官方源码包做全量扫描，并分析工具的检出能力与盲区。

## 作业列表

| 作业 | 主题 | 材料 |
| --- | --- | --- |
| 作业 1 | 开源许可合规检测（Apache Creadur RAT） | [`作业1/`](作业1/) |

## 作业 1 摘要

- **扫描对象**：Apache Commons Lang `3.21.0` 官方源码包（`commons-lang3-3.21.0-src.tar.gz`，SHA-512 已与官方 `.sha512` 核对一致）
- **检测工具**：Apache Creadur RAT `0.18`
- **规模**：706 个文件，扫描耗时 13.7 秒
- **结果**：`Approved 692` / `Standards 694` / `Unapproved 2` / `Binaries 9` / `Notices 3` / `Archives 0`

两个未通过的条目均为非代码文件，不构成许可风险：

| 文件 | 定性 |
| --- | --- |
| `src/site/resources/release-notes/RELEASE-NOTES-3.9.txt` | 历史发行说明的纯文本粘贴，首部即正文，无许可头 |
| `src/test/resources/lang-708-input.txt` | 缺陷 LANG-708 的测试输入数据，本身就是"无头部的异常文本" |

## 目录结构

```
.
├── README.md
└── 作业1/
    ├── f250165-f25016515-徐悦-作业1.docx    # 作业正文（按学校模板排版）
    ├── f250165-f25016515-徐悦-作业1.pdf     # 同一份正文的 PDF 版
    └── 扫描报告原文-rat-report.txt          # RAT 输出的原始报告全文
```

## 复现方式

```bash
# 1. 下载源码包并校验
curl -O https://archive.apache.org/dist/commons/lang/source/commons-lang3-3.21.0-src.tar.gz
curl -O https://archive.apache.org/dist/commons/lang/source/commons-lang3-3.21.0-src.tar.gz.sha512
sha512sum commons-lang3-3.21.0-src.tar.gz        # 与 .sha512 内容比对
tar -xzf commons-lang3-3.21.0-src.tar.gz

# 2. 获取扫描工具
curl -O https://repo1.maven.org/maven2/org/apache/rat/apache-rat/0.18/apache-rat-0.18.jar

# 3. 全量扫描（需放开计数器上限，否则发现问题时直接抛异常退出、不写报告）
java -jar apache-rat-0.18.jar \
     --counter-max "UNAPPROVED:-1" --counter-max "UNKNOWN:-1" \
     --output-file rat-report.txt -- commons-lang3-3.21.0-src
```

环境：Windows 11 + JDK 17 + Apache Creadur RAT 0.18。

## 关于本仓库

仓库为私有，材料含学号与姓名，仅作课程作业归档与个人备份，不作公开分享。

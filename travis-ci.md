# Travis CI

## 是什么

Travis CI 是一个**托管式持续集成（CI）服务**——把代码仓库接进来，每次 push 或提交 PR 时，它在云端自动开一台机器，按仓库里的配置文件拉代码、装依赖、跑构建和测试，再把结果回报到 GitHub 的 PR 页面上。不用自己搭 Jenkins、不用维护构建机。

核心逻辑：**把"提交代码后要做的检查"从人工执行变成云端自动执行**，让"这次改动有没有搞坏东西"在合并之前就有答案。

## 怎么用

在仓库根目录放一个 `.travis.yml`，声明语言环境和要跑的命令：

```yaml
language: node_js
node_js:
  - "20"
script:
  - npm test
```

之后 push 代码即自动触发。配置极简是它当年流行的主因之一——Ruby / Python / Node 项目常常几行就能跑起来。

## 名字与历史

- **2011 年**创立，是**第一个为开源项目提供免费构建**的 CI 服务，且与 GitHub 深度绑定。此后近十年是开源界的**事实标准**（README 里的 build badge 徽章几乎成了标配，Rails 等项目 2011 年就迁了上去）。
- **2018 年 10 月** GitHub Actions 上线：免费、GitHub 原生集成，成为后来的替代者。
- **2019 年 1 月**被 Idera, Inc.（私募背景的软件集团）收购；次月大幅裁员，此后服务稳定性下降、构建镜像更新变慢，社区口碑从此转折。
- **2020 年 11 月**宣布终止开源项目的无限免费额度，公开仓库用户改用约 1 万 credit（≈1000 Linux 分钟）的试用额度；年底 `travis-ci.org` 转为只读并合并进 `travis-ci.com`，迁移期间大面积排队与混乱。
- **2021 年至今**：只提供付费的用量制套餐。2026 年仍在运营，但开源免费时代已经结束。

## 现状（2026）

| 套餐 | 参考价格 | 说明 |
|---|---|---|
| Open Source | 免费 | 开源仓库可申请 credit 额度，逐案审核 |
| Usage Based | 约 $15 / 用户 / 月起 | 含一定 build credits，超出另购 |
| Unlimited | 约 $78 / 月起 | 不限构建额度，并发上限更高 |
| Server（本地部署） | 约 $34 / 月起 | 私有云 / 本地部署，支持企业折扣 |

价格为第三方汇总，实际以官方定价页 / 计费文档为准。**新项目一般不再选它**：开源生态的 CI 份额已基本转移给 GitHub Actions。

## 与同类工具对比

| 工具 | 形态 | 配置文件 | 现状 |
|---|---|---|---|
| **Travis CI** | 商业 SaaS | `.travis.yml` | 仍运营但只剩付费档，开源份额流失 |
| **GitHub Actions** | GitHub 原生 | `.github/workflows/*.yml` | 当前主流，接替了 Travis 的开源地位 |
| **GitLab CI/CD** | GitLab 原生 | `.gitlab-ci.yml` | GitLab 用户默认选择 |
| **CircleCI** | 商业 SaaS | `.circleci/config.yml` | 仍在第一梯队 |
| **Jenkins** | 自建为主 | `Jenkinsfile` | 企业内网主导，可定制性最强、维护成本也最高 |

一句话区别：**Travis 把 CI 从"自建软件"变成了"开箱即用的 SaaS"**，而 GitHub Actions 把这件事变成了"代码托管平台的内置功能"——省掉了再接一个外部服务这一步。

## 一句话总结

Travis CI = 绑在 GitHub 上、用 `.travis.yml` 声明的托管 CI 服务；它开创了"开源项目免费自动跑测试"的模式并统治近十年，但被收购后质量下滑、2020 年终结开源免费额度，地位基本被 GitHub Actions 接手——**它的价值现在主要在于读懂存量项目的历史，以及理解 CI 的基本范式**。

## 参考

- [Travis CI 官方 FAQ](https://www.travis-ci.com/faq/)
- [Travis CI 计费文档](https://docs.travis-ci.com/user/billing-overview)
- [Travis CI 新定价与开源用户的反应（2020）](https://www.devclass.com/ci-cd/2020/11/25/not-a-complete-er-travis-ty-travis-ci-tries-calming-open-source-users-after-price-changes-and-perceived-quality-drop/1618094)
- [Migrating Your Open Source Builds Off Of Travis CI](https://earthly.dev/blog/migrating-from-travis)
- [A Large-Scale Longitudinal Study of Multi-CI Service Adoption（arXiv）](https://export.arxiv.org/pdf/2609.26181)

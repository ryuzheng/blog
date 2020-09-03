---
title: 阿里云PBS作业排队管理
date: 2020-06-27T16:59:18+08:00
published: true
slug: Aliyun_PBS
tags: ['PBS', '阿里云', '生物信息学']
cover_image: './images/aliyun-pbs.png'
canonical_url: false
description: "最近写到一个程序，想要先等某些任务执行完才继续执行后面的任务。虽然snakemake也可以qsub提交任务，但是snakemake还不是很熟练。而我的Python脚本，需要自定义计算的地方很多，用snakemake也不方便，于是就想找PBS原生的实现方法。"
---

找了很久PBS的文档，阿里云的PBS应该是OpenPBS，现在名字叫做[PBS Pro](https://www.pbspro.org/)，找了很久才找到个18.2版本的说明书，虽然服务器上的是18.1，但是版本相差不大，应该也通用。


[PBSUserGuide18.2.pdf](https://www.pbsworks.com/pdfs/PBSUserGuide18.2.pdf)


然后最近写到一个程序，想要先等某些任务执行完才继续执行后面的任务。虽然snakemake也可以qsub提交任务，但是snakemake还不是很熟练。而我的Python脚本，需要自定义计算的地方很多，用snakemake也不方便，于是就想找PBS原生的实现方法。


在网上搜索，找到 `-hold_jid` 和 `-W depend` 两种，一开始觉得 `-hold_jid` 这种比较简单明了，但是阿里云的 `qsub` 很多参数都不支持，其中 `-hold_jid` 就不支持。


## 用-W depend属性去修改
然后对于 `-W depend` 这个一开始有点迷糊，后来看过Userguide之后，才知道depend后面可以接多种参数，然后再用分号接job id；

```bash
# <type>:<arg list>[,<type>:<arg list> ...] 这种形式
# 其中<arg list>为<job ID>[:<job ID> ...] 这种形式的一串任务id
# type有如下几种，要执行的该任务我们称之为“任务A”，而arg list为depend的其他任务

after:<arg list> # arg list 开始执行 => 任务A执行
afterok:<arg list> # arg list 成功执行完 => 任务A执行
afternotok:<arg list> # arg list 执行完失败后 => 任务A执行
afterany:<arg list> # arg list 执行完后（不管是否成功完成） => 任务A执行
before:<arg list> # 任务 A 开始执行 => arg list执行（依赖on:count）
beforeok:<arg list> # 任务 A 成功执行完后 => arg list执行（依赖on:count）
beforenotok:<arg list> # 任务 A 执行完失败后 => arg list执行（依赖on:count）
beforeany:<arg list # 任务 A 执行完(不管是否成功) => arg list执行（依赖on:count）
on:count # 需要依赖的任务数，和以上的before一起搭配使用
```
下面为几个示例，


### 示例一
你有job1、job2、job3 三个任务，你想要job3在job1、job2完成后才开始执行
```bash
qsub job1
16394.jupiter
qsub job2
16395.jupiter
qsub -W depend=afterany:16394:16395 job3
16396.jupiter
```
### 示例二
你想要job2，必须在job1成功执行完后才开始执行
```bash
qsub job1
16397.jupiter
qsub -W depend=afterok:16397 job2
16396.jupiter
```
### 示例三
job1要在job2、job3前完成，要使用 `beforeany`，你必须同时使用 `on`
```bash
qsub -W depend=on:2 job1
16397.jupiter
qsub -W depend=beforeany:16397 job2
16398.jupiter
qsub -W depend=beforeany:16397 job3
16399.jupiter
```
## 用qalter去修改已经提交的任务
有时候，我们是多人使用一个集群，或者我们提交前没有考虑到，那么我们想修改已经提交的任务，就需要使用 `qalter` 命令。


比如，我们想让id为16397的任务插队，那么命令为 `qalter -W depend=afterok:16397 16396`，那么16396号任务就会在16397号任务成功完成后才能开始执行。


另外，如果我们已经像上面那样添加了afterok属性了，而我们又想修改为16396在16397开始执行后就可以开始执行，而不用等到执行成功之后，我们直接 `qalter -W depend=after:16397 16396` 是不能修改的。我们需要先 `qalter -W depend=afterok 16396` 清除afterok属性后，才可以重新修改。
出处：https://segmentfault.com/a/1190000010442764#articleHeader8

### job 中的 tags 和 runner，什么关系？

### stages 和 job 中的 stage，什么关系？
1. job 中的 stage，属于 stages 中的一个
2. 相同stage的job可以平行执行，下一个stage的job会在前一个stage的job成功后开始执行

### stages
stages用来定义可以被job调用的stages。stages的规范允许有灵活的多级pipelines。

stages中的元素顺序决定了对应job的执行顺序：
1. 相同stage的job可以平行执行。
2. 下一个stage的job会在前一个stage的job成功后开始执行。

接下仔细看看这个例子，它包含了3个stage：
```
stages:
    - build
    - test
    - deploy
```
1. 首先，所有build的jobs都是并行执行的。
2. 所有build的jobs执行成功后，test的jobs才会开始并行执行。
3. 所有test的jobs执行成功，deploy的jobs才会开始并行执行。
4. 所有的deploy的jobs执行成功，commit才会标记为success
5. 任何一个前置的jobs失败了，commit会标记为failed并且下一个stages的jobs都不会执行。

这有两个特殊的例子值得一提：
1. 如果.gitlab-ci.yml中没有定义stages，那么job's stages 会默认定义为 build，test 和 deploy。
2. 如果一个job没有指定stage，那么这个任务会分配到test stage。

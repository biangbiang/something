# 游戏道具出现概率的算法

目前开发的游戏里需要处理玩家点击宝箱后随机得到一些钱，得到钱的概率如下：

    金钱 概率
    500 1%
    300	5%
    100	24%
    50	70%

在这里我们需要先做一个能产生 1- 100之间的随机数，代码如下：

    /**
     * 产生一个随机数
     * @param min   - 最小数
     * @param max   - 最大数
     * @return
     *    最小数 和 最大数 - 1 范围的数
     *    例如：
     *      for (var i:int = 0; i < 300; i++) {
     *         trace(JFRandomUtil.random(1,3).toString());
     *      }
     *      结果：
     *         1，2，1，1，1，1，2 ....,1,1,2,2,2,2,2,1,1,1,2
     */
    function random(min:Number, max:Number):Number {
        return Math.floor(Math.random() * (max - min)) + min;
    }

随机随机函数搞定后，我们接下来就做一个概率计算函数，代码如下：

    /**
     * 根据概率表产生一个概率下标
     * @param arg_ProbabilityTable  - 概率表
     * @return
     *  概率表下标
     */
    function makeProbabilityValues(arg_ProbabilityTable:Array):int {
        var i:int;
        var randomValue:int = random(1,101);
        for (i = 0; i < arg_ProbabilityTable.length; i++) {
            if (randomValue  <= arg_ProbabilityTable[i]) {
                return i;
            }
            randomValue -= arg_ProbabilityTable[i];
        }
        return arg_ProbabilityTable.length;
    }

好了，接下来测试一下吧！

    var oneCount:int = 0;
    var twoCount:int = 0;
    var threeCount:int = 0;
    var fourCount:int = 0;
    for (var i:int = 0; i < 10000; i++) {
        var j:int = makeProbabilityValues([70,24,5,1]);
        if (j < 0 || j > 3) {
            Trace('error! j='+j.toString());
        }
        if (j == 0) {
            ++oneCount;
        }
        if (j == 1) {
            ++twoCount;
        }
        if (j == 2) {
            ++threeCount;
        }
        if (j == 3) {
            ++fourCount;
        }
    }
    Trace ('one = ' + oneCount.toString() + ' two:' + twoCount.toString() + ' three:' + threeCount.toString() + ' four:' + fourCount.toString());

结果：

    one = 7019 two:2357 three:522 four:102

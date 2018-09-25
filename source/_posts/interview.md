---
title: 面试
date: 2018-08-08 14:41:00
tags: [PHP, interview]
categories: [interview]
---
这边记录一些php的面试题(不保证正确性，准确性)，会不定期更新，如果有什么错误或者更好的方案，希望得到你的指导

1. 设计一个发红包，抢红包的系统

问题一： 金额是预先分配还是实时计算
答： 实时计算，预算需要占存储，并且会产生大量的数据库读写操作，实时效率很高，预算效率才低

```
function getRedPackageMoney($min = 1)
{
    $redPackageId = 10; // 红包ID
    $redis = new Redis();
    $redis->pconnect('127.0.0.1', 6379, 1);
    $remainSize = $redis->get('remainSize:' . $redPackageId);
    $remainMoney = $redis->get('remainMoney:' . $redPackageId);

    if (!$remainSize) {
        return json_encode(['status' => '400', 'message' => '红包已抢完']);
    }
    if ($remainSize == 1) {
        $redis->decr('remainSize:' . $redPackageId);
        $redis->decrby('remainMoney:' . $redPackageId, $remainMoney);
        return json_encode(['status' => 200, 'money' => $remainMoney]);
    }

    $safeTotal = ($remainMoney - $remainSize * $min) / $remainSize;
    $money = mt_rand($min, $safeTotal);
    $money = $money / 100;  // 主要ini配置项serialize_precision
    $remainMoney = $safeTotal - $money;

    $redis->decr('remainSize:' . $redPackageId);
    $redis->decrby('remainMoney:' . $redPackageId, $money);

    return json_encode(['status' => 200, 'money' => $money]);
}

$money = getRedPackageMoney();
print_r($money);
```

2. 有 1000 个一模一样的瓶子，其中有 999 瓶是普通的水，有一瓶是毒药。任何喝下毒药的生物都会在一星期之后死亡。现在，你只有 10 只小白鼠和一星期的时间，如何检验出哪个瓶子里有毒药？

答: 根据2^10=1024，所以10个老鼠可以确定1000个瓶子具体哪个瓶子有毒。具体实现跟3个老鼠确定8个瓶子原理一样。
```
000=0
001=1
010=2
011=3
100=4
101=5
110=6
111=7
```
一位表示一个老鼠，0-7表示8个瓶子。也就是分别将1、3、5、7号瓶子的药混起来给老鼠1吃，2、3、6、7号瓶子的药混起来给老鼠2吃，4、5、6、7号瓶子的药混起来给老鼠3吃，哪个老鼠死了，相应的位标为1。如老鼠1死了、老鼠2没死、老鼠3死了，那么就是101=5号瓶子有毒。同样道理10个老鼠可以确定1000个瓶子

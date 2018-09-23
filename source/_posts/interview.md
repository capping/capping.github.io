---
title: 面试
date: 2018-08-08 14:41:00
tags: [PHP, 面试]
categories: [PHP]
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

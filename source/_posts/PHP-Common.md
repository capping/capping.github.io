---
title: PHP 日常总结
date: 2018-10-11 11:10:46
tags: [PHP]
categories: [PHP]
---
PHP 日常总结，方便以后分类

1. 一个PHP实现的ID生成器

```
<?php

class Sequence
{
    const EPOCH = 1000000000000;

    const TIME_BITS  = 41;
    const NODE_BITS  = 10;
    const COUNT_BITS = 10;

    private $node = 0;

    private $ttl = 10;

    public function __construct($node)
    {
        $max = $this->max(self::NODE_BITS);

        if (is_int($node) === false || $node > $max || $node < 0) {
            throw new \InvalidArgumentException('node');
        }

        $this->node = $node;
    }

    public function generate($time = null)
    {
        if ($time === null) {
            $time = (int)(microtime(true) * 1000);
        }

        return ($this->time($time) << (self::NODE_BITS + self::COUNT_BITS)) |
               ($this->node << self::COUNT_BITS) |
               ($this->count($time));
    }

    public function restore($id)
    {
        $binary = decbin($id);

        $position = -(self::NODE_BITS + self::COUNT_BITS);

        return array(
            'time'  => bindec(substr($binary, 0, $position)) + self::EPOCH,
            'node'  => bindec(substr($binary, $position, - self::COUNT_BITS)),
            'count' => bindec(substr($binary, - self::COUNT_BITS)),
        );
    }

    public function setTTL($ttl)
    {
        $this->ttl = $ttl;
    }

    private function time($time)
    {
        $time -= self::EPOCH;

        $max = $this->max(self::TIME_BITS);

        if (is_int($time) === false || $time > $max || $time < 0) {
            throw new \InvalidArgumentException('time');
        }

        return $time;
    }

    private function count($time)
    {
        $key = "seq:count:" . ($time % ($this->ttl * 1000));

        while (!$count = apcu_inc($key)) {
            apcu_add($key, mt_rand(0, 9), $this->ttl);
        }

        $max = $this->max(self::COUNT_BITS);

        if ($count > $max) {
            throw new \UnexpectedValueException('count');
        }

        return $count;
    }

    private function max($bits)
    {
        return -1 ^ (-1 << $bits);
    }
}
```

2. PHP实现排列组合
```
<?php 
// 阶乘
function factorial($n) {
    return array_product(range(1, $n));
}

// 排列数
function A($n, $m) {
    return factorial($n) / factorial($n-$m);
}

// 组合数
function C($n, $m) {
    return A($n, $m) / factorial($m);
}

// 排列
function arrangement($a, $m) {
    $r = array();
    $n = count($a);
    if ($m <= 0 || $m > $n) {
      return $r;  
    }
    for ($i = 0; $i < $n; $i++) {
        $b = $a;
        $t = array_splice($b, $i, 1);
        if ($m == 1) {
            $r[] = $t;
        } else { 
            $c = arrangement($b, $m-1);
            foreach ($c as $v) { 
                $r[] = array_merge($t, $v);
            }
        }
    }
    return $r;
}

// 组合
function combination($a, $m) {
    $r = array();
    $n = count($a);
    if ($m <= 0 || $m > $n) {
        return $r;
    }
    for ($i = 0; $i < $n; $i++) {
        $t = array($a[$i]);
        if ($m == 1) {
            $r[] = $t;
        } else {
            $b = array_slice($a, $i+1);
            $c = combination($b, $m-1);
            foreach ($c as $v) {
                $r[] = array_merge($t, $v);
           }
        }
    }
    return $r;
}
// ====== 测试 ======
$a = array("A", "B", "C", "D");
$r = arrangement($a, 2);
print_r($r);
$r = A(4, 2);
echo $r."\n";
$r = combination($a, 2);
print_r($r);
echo $r."\n";
```

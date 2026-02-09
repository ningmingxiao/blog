
```
package main

import (
	"fmt"
	"iter"
)

// numberSeq 基于Go 1.23 iter包实现生成器
// 返回iter.Seq[int]（整数序列迭代器）
func numberSeq(n int) iter.Seq[int] {
	// 返回一个迭代器函数，该函数接收一个yield函数作为参数
	return func(yield func(int) bool) {
		i := 1
		for i <= n {
			// yield(i) 相当于Python的yield：返回当前值i
			// yield返回false时，终止迭代（外部break时触发）
			if !yield(i) {
				return
			}
			fmt.Printf("生成器继续执行，当前i=%d\n", i)
			i++
		}
	}
}

func main() {
	// 使用for-range迭代生成器（Go 1.23+ 原生支持）
	fmt.Println("迭代iter.Seq生成器：")
	for num := range numberSeq(5) {
		fmt.Printf("获取到值：%d\n", num)
		// 可选：模拟提前终止迭代（比如只取前3个值）
		if num == 3 {
			break
		}
	}
}
```


第一步：先搞懂 yield 是什么（不是关键字，是函数！）
在 Go 1.23 的iter.Seq迭代器中，yield 不是关键字（和 Python 的yield完全不同），而是一个传入生成器闭包的函数参数，它的类型是 func(值类型) bool（比如生成整数时是 func(int) bool）。
简单说：你写的生成器函数（比如numberSeq）会接收 Go 运行时传入的yield函数，你调用这个yield函数来实现 “返回值” 和 “感知外部迭代状态”。
第二步：拆解 yield(i) 的双重作用
yield(i) 执行时会做两件关键事，这是理解!yield(i)的核心：
1. 核心功能：向外部返回值（对应 Python 的yield i）
调用yield(i)时，会把i的值传递给外部的for-range循环，这是生成器 “产出值” 的核心逻辑，和 Python 里yield i的效果完全一致（比如外部循环会拿到i这个值）。
2. 关键返回值：告知 “外部是否还要继续迭代”
yield(i) 执行后会返回一个bool类型的值，这个值由外部迭代的状态决定：
✅ 返回true：外部还想继续迭代（比如for-range没结束、没触发break），生成器可以继续生成下一个值；
❌ 返回false：外部不想继续迭代了（比如外部执行了break、或者迭代被终止），生成器应该停止工作。
第三步：理解 !yield(i) 的逻辑
! 是 Go 中的逻辑非运算符（取反），!yield(i) 就是对yield(i)的返回值取反：
当yield(i)返回false（外部终止迭代）→ !yield(i)为true → 执行return，生成器函数直接退出；
当yield(i)返回true（外部继续迭代）→ !yield(i)为false → 跳过return，生成器继续生成下一个值。

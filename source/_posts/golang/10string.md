##### 获取字符串长度

    - 使用 (字节) bytes.Count() 统计
    - 使用 (字符串) strings.Count() 统计
    - 将字符串转换为 []rune 后调用 len 函数进行统计
    - 使用 utf8.RuneCountInString() 统计
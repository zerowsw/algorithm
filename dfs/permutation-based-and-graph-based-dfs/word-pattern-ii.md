# Word Pattern II

## Description

Given a `pattern` and a string `str`, find if `str` follows the same pattern.

Here **follow** means a full match, such that there is a [bijection](https://baike.baidu.com/item/%E5%8F%8C%E5%B0%84/942799?fr=aladdin) between a letter in `pattern` and a **non-empty**substring in `str`.\(i.e if `a` corresponds to `s`, then `b` cannot correspond to `s`. For example, given pattern = `"ab"`, str = `"ss"`, return `false`.\)

## Example

Given pattern = `"abab"`, str = `"redblueredblue"`, return `true`.  
Given pattern = `"aaaa"`, str = `"asdasdasdasd"`, return `true`.  
Given pattern = `"aabb"`, str = `"xyzabcxzyabc"`, return `false`.

## Solution

这道题与word pattern I 不同，需要尝试所有可能的映射关系，从而作出最后的判断。




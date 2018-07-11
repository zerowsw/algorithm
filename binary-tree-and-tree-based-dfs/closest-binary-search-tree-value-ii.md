# Closest Binary Search Tree Value II

## Description

Given a non-empty binary search tree and a target value, find `k` values in the BST that are closest to the target.

## Example

Given root = `{1}`, target = `0.000000`, k = `1`, return `[1]`.

## Challenge

Assume that the BST is balanced, could you solve it in less than O\(n\) runtime \(where n = total nodes\)?

## Solution

相对而言比较简单的做法，就是先inorder遍历，然后找最近的K个元素，时间复杂度为O\(n\)




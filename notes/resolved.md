Q: el-table-column 用 v-for 生成，改变数组后<template slot="header">中试图不能触发更新

A: 清空原数组，在 nextTick 中重新赋值
### 在vue2中使用 ::v-deep
```
::v-deep .el-col {
	margin-bottom: 20px;
}
```

’ >>> ‘和’ /deep/ '支持已弃用。

### 在vue3中::v-deep可以使用但是不推荐使用，官方推荐v-deep(.className)
```
::v-deep(.el-col) {
	margin-bottom: 20px;
}
```
```
// 缩写
:deep(.el-col) {
	margin-bottom: 20px;
}
```

全局样式设置 ::v-global(.ClassName) 缩写 :global(.ClassName)
插槽内的样式设置 ::v-slotted(.ClassName) 缩写 :slotted(.ClassName)

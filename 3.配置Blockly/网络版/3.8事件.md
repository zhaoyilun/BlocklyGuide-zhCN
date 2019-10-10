# 事件

工作区上的每个更改都会触发一个事件。这些事件完整地描述了每个变化的前后状态。

## 监听事件

工作区具有可用于侦听事件流的addChangeListener方法和removeChangeListener方法。一个例子是 实时生成代码。另一个例子是 最大块限制演示。通常情况下，这两个例子都不关心触发事件是什么。他们只是查看工作区的当前状态。

更复杂的事件监听器将查看触发事件。以下示例检测用户何时创建其第一条评论，发出警报，然后停止监听，以便不再触发其他警报。

```js
function onFirstComment(event) {
  if (event.type == Blockly.Events.CHANGE &&
      event.element == 'comment' &&
      !event.oldValue && event.newValue) {
    alert('Congratulations on creating your first comment!')
    workspace.removeChangeListener(onFirstComment);
  }
}
workspace.addChangeListener(onFirstComment);
```

模块具有另一种侦听事件流的方法。模块可以定义onchange 函数，只要块的工作空间发生更改，就会调用该函数。

## 事件类型

所有事件共享以下常见属性：

| Name | 类型 | 描述 |
|-|-|-|
| type | 串 | Blockly.Events.CREATE，Blockly.Events.DELETE，Blockly.Events.CHANGE，Blockly.Events.MOVE，Blockly.Events.UI之一 |
| workspaceId | 串 | 工作区的UUID。可以找到工作区Blockly.Workspace.getById(event.workspaceId) |
| blockId | 串 | 块的UUID。该块可以找到workspace.getBlockById(event.blockId) |
| group | 串 | 组的UUID。某些事件是不可分割的组的一部分，例如在堆栈中插入语句 |

## 块创建事件（Blockly.Events.BLOCK_CREATE）

块创建事件有两个附加属性：

| Name | 类型 | 描述 |
|-|-|-|
| xml | 宾语 | 定义新块和任何连接子块的XML树 |
| ids | 排列 | 一个数组，包含新块的UUID和任何连接的子块 |

## 块删除事件（Blockly.Events.BLOCK_DELETE）

块删除事件有两个附加属性：

| Name | 类型 | 描述 |
|-|-|-|
| oldXml |宾语 | 定义已删除块和任何已连接子块的XML树 |
| ids | 排列 | 包含已删除块的UUID和任何已连接子块的数组 |

## 块变更事件（Blockly.Events.BLOCK_DCHANGE）

块删除事件有两个附加属性：

| Name | 类型 | 描述 |
|-|-|-|
| element | 串 | 'field'，'comment'，'collapsed'，'disabled'，'inline'，'mutate'之一 |
| name | 串 | 当字段更改时字段的名称 |
| oldValue | 值 | 原有值 |
| newValue | 值 | 新值 |

## 块移动事件（Blockly.Events.BLOCK_MOVE）

块删除事件有两个附加属性：

| Name | 类型 | 描述 |
|-|-|-|
| oldParentId | 串 | 旧父块的UUID。如果它是顶级块，则未定义 |
| oldInputName | 串 | 旧父块的输入名称。如果它是顶级块或父级的下一个块，则为未定义 |
| oldCoordinate | 宾语 | X和Y坐标，如果它是顶级块。如果有父块，则未定义 |
| newParentId | 串 | 新父块的UUID。如果它是顶级块，则为未定义 |
| newInputName | 串 | 	新父母的输入名称。如果它是顶级块或父级的下一个块，则为未定义 |
| newCoordinate | 宾语 | X和Y坐标，如果它是顶级块。如果它有父项，则为未定义 |

## 变量创建事件（Blockly.Events.VAR_CREATE）

变量创建事件有两个附加属性：

| Name | 类型 | 描述 |
|-|-|-|
| varType | 串 | 变量的类型，如'int'或'string'。不需要是唯一的。这将默认为“”，这是一种特定类型 |
| varName | 串 | 变量的名称。这在变量和过程中是唯一的 |
| varId | 串 | 变量的唯一ID |


## 变量删除事件（Blockly.Events.VAR_DELETE）

变量删除事件有两个附加属性。

| Name | 类型 | 描述 |
|-|-|-|
varType | 串 | 变量的类型，如'int'或'string'。不需要是唯一的。这将默认为“”，这是一种特定类型 |
varName | 串 | 变量的名称。这在变量和过程中是唯一的 |
varId | 串 | 变量的唯一ID |

## 变量重命名事件(Blockly.Events.VAR_RENAME)
变量重命名事件有两个附加属性。

| Name | 类型 | 描述 |
|-|-|-|
oldName | 串 | 变量的当前名称。这在变量和过程中是唯一的 |
newName | 串 | 变量的新名称。这在变量和过程中是唯一的 |
varId | 串 | 变量的唯一ID |


## UI事件(lockly.Events.UI)
UI事件有三个附加属性。

| Name | 类型 | 描述 |
|-|-|-|
| element | 串 | 'selected'，'category'，'click'，'commentOpen'，'mutatorOpen'，'warningOpen'，'theme'之一 |
| oldValue | 值 | 原始价值 |
| newValue | 值 | 改变了价值 |

预计由UI事件表示的UI动作列表将随着时间的推移变得更加全面。例如滚动，缩放，拖动气泡等事件。
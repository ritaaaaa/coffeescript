# 生成器模式

## 问题

你需要准备一个复杂的、多部分的对象，但是你希望操作不止一次或有不同的配置。

## 方案

创建一个生成器封装对象的产生过程。

[Todo.txt](http://todotxt.com/) 格式提供了一个先进的但还是纯文本的方法来维护待办事项列表。手工输入每个项目有损耗且容易出错，然而 TodoTxtBuilder 类可以解决我们的麻烦：
```
class TodoTxtBuilder
    constructor: (defaultParameters={ }) ->
        @date = new Date(defaultParameters.date) or new Date
        @contexts = defaultParameters.contexts or [ ]
        @projects = defaultParameters.projects or [ ]
        @priority =  defaultParameters.priority or undefined
    newTodo: (description, parameters={ }) ->
        date = (parameters.date and new Date(parameters.date)) or @date
        contexts = @contexts.concat(parameters.contexts or [ ])
        projects = @projects.concat(parameters.projects or [ ])
        priorityLevel = parameters.priority or @priority
        createdAt = [date.getFullYear(), date.getMonth()+1, date.getDate()].join("-")
        contextNames = ("@#{context}" for context in contexts when context).join(" ")
        projectNames = ("+#{project}" for project in projects when project).join(" ")
        priority = if priorityLevel then "(#{priorityLevel})" else ""
        todoParts = [priority, createdAt, description, contextNames, projectNames]
        (part for part in todoParts when part.length > 0).join " "

builder = new TodoTxtBuilder(date: "10/13/2011")

builder.newTodo "Wash laundry"

# => '2011-10-13 Wash laundry'

workBuilder = new TodoTxtBuilder(date: "10/13/2011", contexts: ["work"])

workBuilder.newTodo "Show the new design pattern to Lucy", contexts: ["desk", "xpSession"]

# => '2011-10-13 Show the new design pattern to Lucy @work @desk @xpSession'

workBuilder.newTodo "Remind Sean about the failing unit tests", contexts: ["meeting"], projects: ["compilerRefactor"], priority: 'A'

# => '(A) 2011-10-13 Remind Sean about the failing unit tests @work @meeting +compilerRefactor'
```

## 讨论

TodoTxtBuilder 类负责所有文本的生成，让程序员关注每个工作项的独特元素。此外，命令行工具或 GUI 可以插入这个代码且之后仍然保持支持，提供轻松、更高版本的格式。

## 前期建设

并不是每次创建一个新的实例所需的对象都要从头开始，我们将负担转移到一个单独的对象，可以在对象创建过程中进行调整。
```
builder = new TodoTxtBuilder(date: "10/13/2011")

builder.newTodo "Order new netbook"

# => '2011-10-13 Order new netbook'

builder.projects.push "summerVacation"

builder.newTodo "Buy suntan lotion"

# => '2011-10-13 Buy suntan lotion +summerVacation'

builder.contexts.push "phone"

builder.newTodo "Order tickets"

# => '2011-10-13 Order tickets @phone +summerVacation'

delete builder.contexts[0]

builder.newTodo "Fill gas tank"

# => '2011-10-13 Fill gas tank +summerVacation'
```

## 练习

- 扩大 project-and context-tag 生成代码来过滤掉重复的条目。

- 一些 Todo.txt 用户喜欢在任务描述中插入项目和上下文的标签。添加代码来识别这些标签和过滤器的结束标记。




















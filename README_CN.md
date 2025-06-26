## 数据库

**作者：** Idreamtravel  
**版本：** 0.0.1
**类型：** 工具  

### 请注意！
**`sql_execute` 工具可以运行任何 SQL 查询；为了增强安全性，请始终使用只读数据库账户。**

### 描述

一个数据库工具，可以轻松地从现有数据库中查询数据。

您可以获取不同格式的数据，例如 `json`、`csv`、`yaml`、`xlsx`、`html`、`md` 等。还支持使用 URL 获取这些数据。

### 用法

#### 1. 输入用于授权的 databaseURI。目前支持 `mysql`、`postgresql`、`sqlite`、`sqlserver`、`oracle`，示例格式如下：
```shell
mysql+pymysql://root:123456@localhost:3306/test
postgresql+psycopg2://postgres:123456@localhost:5432/test
sqlite:///test.db
mssql+pymssql://<username>:<password>@<freetds_name>/?charset=utf8
oracle+oracledb://user:pass@hostname:port[/dbname][?service_name=<service>[&key=value&key=value...]]
```

> **注意：** 此插件总是在 Docker 中运行，因此 `localhost` 始终指 Docker 内部网络，请尝试使用 `host.docker.internal` 代替。

#### 2. 使用 `SQL 执行` (SQL Execute) 工具从数据库查询数据。
![sql](./_assets/sql_execute.png)
`输出格式` (OUTPUT FORMAT) 用于指定输出数据的格式。如果未指定，默认格式为 `json`，并将输出在工作流节点的 `json` 变量中。`md` 格式将输出在 `text` 变量中，其他格式将创建文件并输出在 `files` 变量中。

如果您输入了 `数据库 URI` (DB URI) 字段，它将覆盖授权时填写的 URI，因此这在您想在同一工作流中使用不同数据库时非常有用。

#### 3. 使用 `文本转 SQL` (Text to SQL) 工具可以将用户输入转换为有效的 SQL 查询。
![text](./_assets/text.png)

该工具将使用[此处](https://github.com/hjlarry/dify-plugin-database/blob/d6dd3695840e8eb5d673611784af148b1789da97/tools/text2sql.py#L9)的默认提示词（prompt）来生成 SQL 查询。如果您指定了 `表` (TABLES) 字段，它将仅将这些表的结构（schema）获取到 LLM 上下文中。

#### 4. 使用 `获取表模式` (Get Table Schema) 工具可以获取表的结构（schema）。

如果 `文本转 SQL` 工具无法生成有用的 SQL 查询，您可以使用此工具获取表的结构(在text字段中返回)，然后将该结构与您自己的提示词或其他信息结合，输入到 LLM 节点以生成有用的 SQL 查询。

#### 5. `CSV 查询` (CSV Query) 工具可以对 CSV 文件执行 SQL 查询。
![csv](./_assets/csv.png)
表名始终为 `csv`，列名是 CSV 文件的第一行。它支持输出 `json` 和 `md` 格式。

#### 6. 使用 `端点` (endpoint) 工具通过 URL 请求获取数据。

URL 请求格式示例：
```shell
curl -X POST 'https://daemon-plugin.dify.dev/o3wvwZfYFLU5iGopr5CxYmGaM5mWV7xf/sql' -H 'Content-Type: application/json' -d '{"query":"select * from test", "format": "md"}'
```

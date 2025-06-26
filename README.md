## database-tool

**Author:** Idreamtravel  
**Version:** 0.0.1
**Type:** tool   

### Be Carefull!
**The sql_execute tool can run any SQL query; for enhanced security, always use a read-only database account.**

### Description

A database tool make it easy to query data from existing databases.

You can get different format of data, like `json`, `csv`, `yaml`, `xlsx`, `html`, `md` etc. Also support use a url to get those data.

### Usage

#### 1. Input a databaseURI for Authorization. Now support `mysql`, `postgresql`, `sqlite`, `sqlserver`, `oracle`, example format:
```shell
mysql+pymysql://root:123456@localhost:3306/test
postgresql+psycopg2://postgres:123456@localhost:5432/test
sqlite:///test.db
mssql+pymssql://<username>:<password>@<freetds_name>/?charset=utf8
oracle+oracledb://user:pass@hostname:port[/dbname][?service_name=<service>[&key=value&key=value...]]
```

> **Note:**: this plugin always run in a docker, so the `localhost` always means docker internal network, try `host.docker.internal` instead.

#### 2. Use the `SQL Execute` tool to query data from the database.
![sql](./_assets/sql_execute.png)
The `OUTPUT FORMAT` is used to specify the format of the output data. If you don't specify it, the default format is `json` and will output in the `json` variable of workflow node. `md` will output in the `text` variable, other format will create file and output in the `files` variable.

If you input the `DB URI` field, it will overwrite the default authorization uri, so this will be useful if you want to use different databases in the same workflow.

#### 3. Use the `Text to SQL` tool can transform user input to a valid sql query.
![text](./_assets/text.png)

This tool will use the default prompt [here](https://github.com/hjlarry/dify-plugin-database/blob/d6dd3695840e8eb5d673611784af148b1789da97/tools/text2sql.py#L9) to generate a sql query. If you specify the `TABLES` field, it will only get those tables' schema into the LLM context.

#### 4. Use the `Get Table Schema` tool can get the schema of tables.

If the `Text to SQL` tool can't generate a helpful sql query, you can use this tool to get the schema of tables, then use the schema orginze with your own prompt or other information to a LLM node to generate a helpful sql query. 

#### 5. The `CSV Query` tool can execute a sql query from a csv file.
![csv](./_assets/csv.png)  
The table name is always `csv`,  the column name is the csv file's first line. It support output `json` and `md`.

#### 6. Use the `endpoint` tool to get the data from a url request.

example url request format:
```shell
curl -X POST 'https://daemon-plugin.dify.dev/o3wvwZfYFLU5iGopr5CxYmGaM5mWV7xf/sql' -H 'Content-Type: application/json' -d '{"query":"select * from test", "format": "md"}'
```

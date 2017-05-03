---
title: ASP.NET MVC Code first筆記---Code First for MySQL(Setting篇)
date: 2017-05-03 17:02:39
categories:
- ASP.NET MVC
tags:
- ASP.NET MVC
- Model
- Code First
---
一開始的設定都和[書上] [5]的一樣，但差別在於web.config
+ 請先下載好[MySQL for VisualStudio](https://dev.mysql.com/downloads/windows/visualstudio/)並安裝
+ 新建好專案之後，到 `工具->Nuget封裝管理員->管理方案的Nuget套件` 去下載MySQL.Data和MySQL.Data.Entity套件
---
<!-- more -->
### Web.config 設定

按照[書本上的流程] [5]弄好之後Web.config原本的設定：

```{zsh}
    <entityFramework>
        <defaultConnectionFactory type="System.Data.Entity.Infrastructure.LocalDbConnectionFactory, EntityFramework">
            <parameters>
                <parameter value="v13.0" />
            </parameters>
        </defaultConnectionFactory>
        <providers>
            <provider invariantName="MySql.Data.MySqlClient" 
					  type="MySql.Data.MySqlClient.MySqlProviderServices, MySql.Data.Entity.EF6, Version=6.9.9.0, Culture=neutral, PublicKeyToken=c5687fc88969c44d" />
            <provider invariantName="System.Data.SqlClient" 
					  type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
        </providers>
    </entityFramework>
```
將Web.config調整成下面這個樣子：
```{zsh}
    <entityFramework codeConfigurationType="MySql.Data.Entity.MySqlEFConfiguration, MySql.Data.Entity.EF6">
        <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework">
            <parameters>
                <parameter value="v13.0" />
            </parameters>
        </defaultConnectionFactory>
    <providers>
        <provider invariantName="MySql.Data.MySqlClient" 
				  type="MySql.Data.MySqlClient.MySqlProviderServices, MySql.Data.Entity.EF6, Version=6.9.9.0, Culture=neutral, PublicKeyToken=c5687fc88969c44d" />
        <provider invariantName="System.Data.SqlClient" 
                  type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
    </providers>
    </entityFramework>
```
connectionStrings那邊加入：
```{zsh}
    <connectionStrings>
        <add name="EFTestEntities" connectionString="server=localhost;user id=<你的帳號>;password=<你的密碼>;persistsecurityinfo=True;database=gounboxing_EFTest;" 
        providerName="MySql.Data.MySqlClient" />
    </connectionStrings>
```
---
### Migration 指令

`enable-migrations`: 開啟資料庫轉移
+ `enable-migrations –ContextTypeName BlogSample.Models.SampleContext`: 如果專案裡不只一個 DbContext 類別的話，可以原本的指令後面接 `<ProjectName>.Models.<SampleContext>`
	
`add-migration <移轉名稱>`: 新增執行資料庫移轉，標示每次移轉的版本編號或是具有意義的名稱

`update-database`: 執行更新資料庫移轉

+ `Update-Database -TargetMigration:<MigrationName or $InitialDatabase or version_number(0:initial)>`: rollback 回到前一次狀態或是回到初始狀態

---

### References
1. [ASP.NET MVC 學習資源整理 Part.4 - ASP.NET MVC 5 書籍 (Books)] [1]
2. [MySQL EF6 Support] [2]
3. [ASP.NET MVC 使用 Entity Framework Code First - 基礎入門] [3]
4. [entity-framework-start-over-undo-rollback-all-migrations] [4]
[1]: http://kevintsengtw.blogspot.tw/2015/03/aspnet-mvc-part4-aspnet-mvc-5-books.html (ASP.NET MVC 學習資源整理 Part.4 - ASP.NET MVC 5 書籍 (Books))
[2]: https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework60.html
[3]: http://kevintsengtw.blogspot.tw/2013/10/aspnet-mvc-entity-framework-code-first.html (ASP.NET MVC 使用 Entity Framework Code First - 基礎入門)
[4]: http://stackoverflow.com/questions/10282532/entity-framework-start-over-undo-rollback-all-migrations
[5]: https://www.tenlong.com.tw/products/9789863472643

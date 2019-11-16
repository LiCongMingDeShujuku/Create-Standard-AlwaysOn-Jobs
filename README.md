![CLEVER DATA GIT REPO](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/0-clever-data-github.png "李聪明的数据库")

# 创建标准的AlwaysOn作业
#### Create Standard AlwaysOn Jobs
**发布-日期: 2016年9月28日 (评论)**

## Contents

- [中文](#中文)
- [English](#English)
- [SQL Logic](#Logic)
- [Build Info](#Build-Info)
- [Author](#Author)
- [License](#License) 


## 中文
对于第一次学习或尝试使用AlwaysOn配置的人，创建一些标准的AlwaysOn作业用来实现管理目的是个不错的主意。你会发现有些时候你需要删除数据库，或者需要从HADR配置中删除所有的数据库，或者从可用性组中完全删除它们。
下面是一些创建这些作业的快速逻辑（logic）。你可以看到一些这方面典型的AlwaysOn作业。


## English
It’s always a good idea to create some Standard AlwaysOn jobs for management purposes whenever you’re first learning, or experimenting with the AlwaysOn configurations. You’ll find times when you need to remove databases, or all databases from the HADR configurations, or to completely remove them from Availability Groups.
Below you’ll find some quick logic to create these jobs. You can see some typical AlwaysOn Jobs for this purpose.

![#](images/create-standard-alwayson-jobs-a.png?raw=true "#")


这种情况下，我们着重看其中的两个。
ALWAYSON  - 从AG删除数据库
ALWAYSON  - 为数据库关闭HADR设置


In this case we’ll be focusing the 2 of them.
ALWAYSON – REMOVE DATABASES FROM AG
ALWAYSON – SET HADR OFF FOR DATABASES


![#](images/create-standard-alwayson-jobs-b.png?raw=true "#")


如果需要，你可以使用下面的逻辑（logic）并将它们放入你的快速作业中。
ALWAYSON  - 从AG删除数据库

You can take the logic below and throw them into a quick Job if needed.
ALWAYSON – REMOVE DATABASES FROM AG



---
## Logic
```SQL

use master;
 
set nocount on
declare @remove_from_ag         varchar(max)
declare @availability_group     varchar(255)
set     @remove_from_ag         = ''
set     @availability_group     = (select name from sys.availability_groups)
select distinct @remove_from_ag = @remove_from_ag +
'alter availability group [' + @availability_group + '] remove database [' + database_name + '];' + char(10)
from   sys.dm_hadr_database_replica_cluster_states
exec   (@remove_from_ag)

```

ALWAYSON  - 为数据库关闭HADR设置

ALWAYSON – SET HADR OFF FOR DATABASES

---
## Logic
```SQL

use master;
set nocount on
declare @set_hadr_off          varchar(max)
declare @availability_group    varchar(255)
set     @set_hadr_off          = ''
set     @availability_group    = (select name from sys.availability_groups)
select  distinct @set_hadr_off = @set_hadr_off +
'alter  database [' + database_name + '] set hadr off;' + char(10)
from    sys.dm_hadr_database_replica_cluster_states
exec    (@set_hadr_off)

```

[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Build-Info

| Build Quality | Build History |
|--|--|
|<table><tr><td>[![Build-Status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg?style=flat-square)](#)</td></tr><tr><td>[![Coverage](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?style=flat-square)](#)</td></tr><tr><td>[![Nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](#)</td></tr></table>|<table><tr><td>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](#)</td></tr></table>|

## Author

- **李聪明的数据库 Lee's Clever Data**
- **Mike的数据库宝典 Mikes Database Collection**
- **李聪明的数据库** "Lee Songming"

[![Gist](https://img.shields.io/badge/Gist-李聪明的数据库-<COLOR>.svg)](https://gist.github.com/congmingshuju)
[![Twitter](https://img.shields.io/badge/Twitter-mike的数据库宝典-<COLOR>.svg)](https://twitter.com/mikesdatawork?lang=en)
[![Wordpress](https://img.shields.io/badge/Wordpress-mike的数据库宝典-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)

---
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Lee Songming](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/1-clever-data-github.png "李聪明的数据库")


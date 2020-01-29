![MIKES DATA WORK GIT REPO](https://raw.githubusercontent.com/mikesdatawork/images/master/git_mikes_data_work_banner_01.png "Mikes Data Work")        

# Get List Of SQL Instances Installed With SQL
**Post Date: October 14, 2016**        



## Contents    
- [About Process](##About-Process)  
- [SQL Logic](#SQL-Logic)  
- [Build Info](#Build-Info)  
- [Author](#Author)  
- [License](#License)       

## About-Process

<p>Here's some quick SQL logic to show you all the instances that are installed on a server. This is simple enough, but still super helpful when creating more automatic processes around a multi-instance SQL Server.</p>      



## SQL-Logic
```SQL
use master;
 
set nocount on
 
declare @sql_instances table
 
(
 
[id] int identity(1,1)
 
, [rootkey] nvarchar(255)
 
, [sql_instances] nvarchar(255)
 
, [value] nvarchar(255)
 
)
 
insert into @sql_instances ([rootkey], [sql_instances], [value])
 
execute xp_regread
 
@rootkey = 'hkey_local_machine'
 
, @key = 'softwaremicrosoftmicrosoft sql server'
 
, @value_name = 'installedinstances'
 
select
 
'SQL_Instances_Installed' = upper([sql_instances])
 
from
 
@sql_instances
```

![Find SQL Instances]( https://mikesdatawork.files.wordpress.com/2016/10/image0011.png "Get List Of Instances With SQL")
 

Also… If you needed to check to see if there were numeric values in the SQL Instnance name; You could use this:



## SQL-Logic
```SQL
select
    right([sql_instances], 2)
,   case   
        when right([sql_instances], 2) not like '%[0-9]%' then 'This is NOT numeric'
        else right([sql_instances], 2)
    end as [sql_instances]
from
    @sql_instances
```


On occasion you may run into this error based on key values (missing or otherwise):
RegOpenKeyEx() returned error 2, 'The system cannot find the file specified.'
Msg 22001, Level 1, State 1
Since this error occurs it will not populate the temp table, and therefore will not return any instance names, but no worries as the xp_regread extended stored procedure is not the only approach you can make when reading the instance names from the registry. 
You can also try this little number called xp_instance_regenumvalues. With a slight modification you can produce the same results and get back some valuable information including the Instant path as this is evaluated from the 'key' within enumvalues.



## SQL-Logic
```SQL
declare @sql_instances  table
(
    [rootkey]   varchar(255)
,   [value]     varchar(255)
)
 
insert into @sql_instances 
exec master.dbo.xp_instance_regenumvalues 
    @rootkey    = N'HKEY_LOCAL_MACHINE'
,   @key        = N'SOFTWARE\\Microsoft\\Microsoft SQL Server\\Instance Names\\SQL';
 
select
    'Instance_Name' = upper([rootkey])
,   'Instance_Path' = upper([value])
from
    @sql_instances

```

[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Build-Info

| Build Quality | Build History |
|--|--|
|<table><tr><td>[![Build-Status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg?style=flat-square)](#)</td></tr><tr><td>[![Coverage](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?style=flat-square)](#)</td></tr><tr><td>[![Nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](#)</td></tr></table>|<table><tr><td>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](#)</td></tr></table>|

## Author

[![Gist](https://img.shields.io/badge/Gist-MikesDataWork-<COLOR>.svg)](https://gist.github.com/mikesdatawork)
[![Twitter](https://img.shields.io/badge/Twitter-MikesDataWork-<COLOR>.svg)](https://twitter.com/mikesdatawork)
[![Wordpress](https://img.shields.io/badge/Wordpress-MikesDataWork-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)

    
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Mikes Data Work](https://raw.githubusercontent.com/mikesdatawork/images/master/git_mikes_data_work_banner_02.png "Mikes Data Work")


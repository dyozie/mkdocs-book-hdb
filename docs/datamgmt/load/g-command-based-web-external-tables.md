---
title: Command-based Web External Tables
---

<!--
Licensed to the Apache Software Foundation (ASF) under one
or more contributor license agreements.  See the NOTICE file
distributed with this work for additional information
regarding copyright ownership.  The ASF licenses this file
to you under the Apache License, Version 2.0 (the
"License"); you may not use this file except in compliance
with the License.  You may obtain a copy of the License at

  http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing,
software distributed under the License is distributed on an
"AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
KIND, either express or implied.  See the License for the
specific language governing permissions and limitations
under the License.
-->

The output of a shell command or script defines command-based web table data. Specify the command in the `EXECUTE` clause of `CREATE EXTERNAL WEB                 TABLE`. The data is current as of the time the command runs. The `EXECUTE` clause runs the shell command or script on the specified master or virtual segments. The virtual segments run the command in parallel. Scripts must be executable by the gpadmin user and reside in the same location on the master or the hosts of virtual segments.

The command that you specify in the external table definition executes from the database and cannot access environment variables from `.bashrc` or `.profile`. Set environment variables in the `EXECUTE` clause. The following external web table, for example, runs a command on the HAWQ master host:

``` sql
CREATE EXTERNAL WEB TABLE output (output text)
EXECUTE 'PATH=/home/gpadmin/programs; export PATH; myprogram.sh'
    ON MASTER 
FORMAT 'TEXT';
```

The following command defines a web table that runs a script on five virtual segments.

``` sql
CREATE EXTERNAL WEB TABLE log_output (linenum int, message text) 
EXECUTE '/var/load_scripts/get_log_data.sh' ON 5 
FORMAT 'TEXT' (DELIMITER '|');
```

The virtual segments are selected by the resource manager at runtime.



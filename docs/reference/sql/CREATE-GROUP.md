---
title: CREATE GROUP
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

Defines a new database role.

## Synopsis<a id="topic1__section2"></a>

``` pre
CREATE GROUP <name> [ [WITH] <option> [ ... ] ]
```

where \<option\> can be:

``` pre
      SUPERUSER | NOSUPERUSER
    | CREATEDB | NOCREATEDB
    | CREATEROLE | NOCREATEROLE
    | CREATEUSER | NOCREATEUSER
    | INHERIT | NOINHERIT
    | LOGIN | NOLOGIN
    | [ ENCRYPTED | UNENCRYPTED ] PASSWORD '<password>'
    | VALID UNTIL '<timestamp>' 
    | IN ROLE <rolename> [, ...]
    | IN GROUP <rolename> [, ...]
    | ROLE <rolename> [, ...]
    | ADMIN <rolename> [, ...]
    | USER <rolename> [, ...]
    | SYSID <uid>
         
```

## Description<a id="topic1__section3"></a>

`CREATE GROUP` has been replaced by [CREATE ROLE](CREATE-ROLE.html), although it is still accepted for backwards compatibility.

## Compatibility<a id="topic1__section4"></a>

There is no `CREATE GROUP` statement in the SQL standard.

## See Also<a id="topic1__section5"></a>

[CREATE ROLE](CREATE-ROLE.html)

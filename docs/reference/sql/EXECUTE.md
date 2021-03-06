---
title: EXECUTE
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

Executes a prepared SQL statement.

## Synopsis<a id="topic1__section2"></a>

``` pre
EXECUTE <name> [ (<parameter> [, ...] ) ]
```

## Description<a id="topic1__section3"></a>

`EXECUTE` is used to execute a previously prepared statement. Since prepared statements only exist for the duration of a session, the prepared statement must have been created by a `PREPARE` statement executed earlier in the current session.

If the `PREPARE` statement that created the statement specified some parameters, a compatible set of parameters must be passed to the `EXECUTE` statement, or else an error is raised. Note that (unlike functions) prepared statements are not overloaded based on the type or number of their parameters; the name of a prepared statement must be unique within a database session.

For more information on the creation and usage of prepared statements, see `PREPARE`.

## Parameters<a id="topic1__section4"></a>

<dt>\<name\>   </dt>
<dd>The name of the prepared statement to execute.</dd>

<dt>\<parameter\>   </dt>
<dd>The actual value of a parameter to the prepared statement. This must be an expression yielding a value that is compatible with the data type of this parameter, as was determined when the prepared statement was created.</dd>

## Examples<a id="topic1__section5"></a>

Create a prepared statement for an `INSERT` statement, and then execute it:

``` pre
PREPARE fooplan (int, text, bool, numeric) AS INSERT INTO 
foo VALUES($1, $2, $3, $4);
EXECUTE fooplan(1, 'Hunter Valley', 't', 200.00);
```

## Compatibility<a id="topic1__section6"></a>

The SQL standard includes an `EXECUTE` statement, but it is only for use in embedded SQL. This version of the `EXECUTE` statement also uses a somewhat different syntax.

## See Also<a id="topic1__section7"></a>

[DEALLOCATE](DEALLOCATE.html), [PREPARE](PREPARE/index.html)

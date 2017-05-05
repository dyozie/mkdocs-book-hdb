---
title: ALTER OPERATOR CLASS
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

Changes the definition of an operator class.

## Synopsis<a id="synop"></a>

``` sql
ALTER OPERATOR CLASS <name> USING <index_method> RENAME TO <newname>

ALTER OPERATOR CLASS <name> USING <index_method> OWNER TO <newowner>
```

## Description<a id="desc"></a>

`ALTER OPERATOR CLASS` changes the definition of an operator class. 

You must own the operator class to use `ALTER OPERATOR CLASS`. To alter the owner, you must also be a direct or indirect member of the new owning role, and that role must have `CREATE` privilege on the operator class’s schema. (These restrictions enforce that altering the owner does not do anything you could not do by dropping and recreating the operator class. However, a superuser can alter ownership of any operator class anyway.)

## Parameters<a id="alteroperatorclass__section4"></a>

<dt> \<name\>   </dt>
<dd>The name (optionally schema-qualified) of an existing operator class.</dd>

<dt> \<index\_method\>   </dt>
<dd>The name of the index method this operator class is for.</dd>

<dt> \<newname\>   </dt>
<dd>The new name of the operator class.</dd>

<dt> \<newowner\>   </dt>
<dd>The new owner of the operator class</dd>

## Compatibility<a id="compat"></a>

There is no `ALTER OPERATOR` statement in the SQL standard.

## See Also<a id="see"></a>

[CREATE OPERATOR](CREATE-OPERATOR.html), [DROP OPERATOR CLASS](DROP-OPERATOR-CLASS/index.html)



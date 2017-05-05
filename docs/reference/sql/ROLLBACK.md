---
title: ROLLBACK
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

Aborts the current transaction.

## Synopsis<a id="topic1__section2"></a>

``` pre
ROLLBACK [WORK | TRANSACTION]
```

## Description<a id="topic1__section3"></a>

`ROLLBACK` rolls back the current transaction and causes all the updates made by the transaction to be discarded.

## Parameters<a id="topic1__section4"></a>

<dt>WORK  
TRANSACTION  </dt>
<dd>Optional key words. They have no effect.</dd>

## Notes<a id="topic1__section5"></a>

Use `COMMIT` to successfully end the current transaction.

Issuing `ROLLBACK` when not inside a transaction does no harm, but it will provoke a warning message.

## Examples<a id="topic1__section6"></a>

To discard all changes made in the current transaction:

``` sql
ROLLBACK;
```

## Compatibility<a id="topic1__section7"></a>

The SQL standard only specifies the two forms `ROLLBACK` and `ROLLBACK WORK`. Otherwise, this command is fully conforming.

## See Also<a id="topic1__section8"></a>

[BEGIN](BEGIN.html), [COMMIT](COMMIT.html), [SAVEPOINT](SAVEPOINT.html), [ROLLBACK TO SAVEPOINT](ROLLBACK-TO-SAVEPOINT/index.html)

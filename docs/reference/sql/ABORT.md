---
title: ABORT
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

## Synopsis<a id="synop"></a>

```pre
ABORT [ WORK | TRANSACTION ]
```

## Description<a id="abort__section3"></a>

`ABORT` rolls back the current transaction and causes all the updates made by the transaction to be discarded. This command is identical in behavior to the standard SQL command `ROLLBACK`, and is present only for historical reasons.

## Parameters<a id="abort__section4"></a>

<dt>WORK  
TRANSACTION  </dt>
<dd>Optional key words. They have no effect.</dd>

## Notes<a id="abort__section5"></a>

Use `COMMIT` to successfully terminate a transaction.

Issuing `ABORT` when not inside a transaction does no harm, but it will provoke a warning message.

## Compatibility<a id="compat"></a>

This command is a HAWQ extension present for historical reasons. ROLLBACK is the equivalent standard SQL command.

## See Also<a id="see"></a>

[BEGIN](BEGIN.html), [COMMIT](COMMIT.html), [ROLLBACK](ROLLBACK/index.html)



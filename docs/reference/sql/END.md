---
title: END
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

Commits the current transaction.

## Synopsis<a id="topic1__section2"></a>

``` pre
END [WORK | TRANSACTION]
```

## Description<a id="topic1__section3"></a>

`END` commits the current transaction. All changes made by the transaction become visible to others and are guaranteed to be durable if a crash occurs. This command is a HAWQ extension that is equivalent to [COMMIT](COMMIT.html).

## Parameters<a id="topic1__section4"></a>

<dt>WORK  
TRANSACTION  </dt>
<dd>Optional keywords. They have no effect.</dd>

## Examples<a id="topic1__section5"></a>

Commit the current transaction:

``` pre
END;
```

## Compatibility<a id="topic1__section6"></a>

`END` is a HAWQ extension that provides functionality equivalent to [COMMIT](COMMIT.html), which is specified in the SQL standard.

## See Also<a id="topic1__section7"></a>

[BEGIN](BEGIN.html), [ROLLBACK](ROLLBACK.html), [COMMIT](COMMIT.html)

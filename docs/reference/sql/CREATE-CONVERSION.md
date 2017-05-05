---
title: CREATE CONVERSION
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

Defines a new encoding conversion.

## Synopsis<a id="topic1__section2"></a>

``` pre
CREATE [DEFAULT] CONVERSION <name> FOR <source_encoding> TO 
     <dest_encoding> FROM <funcname>
```

## Description<a id="topic1__section3"></a>

`CREATE CONVERSION` defines a new conversion between character set encodings. Conversion names may be used in the convert function to specify a particular encoding conversion. Also, conversions that are marked `DEFAULT` can be used for automatic encoding conversion between client and server. For this purpose, two conversions, from encoding A to B and from encoding B to A, must be defined.

To create a conversion, you must have `EXECUTE` privilege on the function and `CREATE` privilege on the destination schema.


## Parameters<a id="topic1__section4"></a>

<dt>DEFAULT</dt>
<dd>Indicates that this conversion is the default for this particular source to destination encoding. There should be only one default encoding in a schema for the encoding pair.</dd>

<dt>\<name\>  </dt>
<dd>The name of the conversion. The conversion name may be schema-qualified. If it is not, the conversion is defined in the current schema. The conversion name must be unique within a schema.</dd>

<dt>\<source_encoding\>  </dt>
<dd>The source encoding name.</dd>

<dt>\<dest_encoding\>  </dt>
<dd>The destination encoding name.</dd>

<dt>\<funcname\>  </dt>
<dd>The function used to perform the conversion. The function name may be schema-qualified. If it is not, the function will be looked up in the path. The function must have the following signature:</dd>

``` pre
conv_proc(
    integer,  -- source encoding ID
    integer,  -- destination encoding ID
    cstring,  -- source string (null terminated C string)
    internal, -- destination (fill with a null terminated C string)
    integer   -- source string length
) RETURNS void;
```

## Notes<a id="topic1__section5"></a>

For this release of HAWQ, user-defined functions used in a user-defined cast must be defined as `IMMUTABLE`. Any compiled code (shared library files) for custom functions must be placed in the same location on every host in your HAWQ array (master and all segments). This location must also be in the `LD_LIBRARY_PATH` so that the server can locate the files.


## Examples<a id="topic1__section6"></a>

To create a conversion from encoding `UTF8` to `LATIN1` using `myfunc`:

``` pre
CREATE CONVERSION myconv FOR 'UTF8' TO 'LATIN1' FROM myfunc;
```

## Compatibility<a id="topic1__section7"></a>

There is no CREATE CONVERSION statement in the SQL standard.

## See Also<a id="topic1__section8"></a>

[DROP CONVERSION](DROP-CONVERSION.html), [CREATE FUNCTION](CREATE-FUNCTION/index.html)

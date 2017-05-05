---
title: Environment Variables
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

This topic contains a reference of the environment variables that you set for HAWQ.

Set these in your user’s startup shell profile (such as `~/.bashrc` or `~/.bash_profile`), or in `/etc/profile`, if you want to set them for all users.

## Required Environment Variables<a id="requiredenvironmentvariables"></a>

**Note:** `GPHOME`, `PATH` and `LD_LIBRARY_PATH` can be set by sourcing the `greenplum_path.sh` file from your HAWQ installation directory.

### GPHOME<a id="gphome"></a>

This is the installed location of your HAWQ software. For example:

``` pre
GPHOME=/usr/local/hawq  
export GPHOME
```

### PATH<a id="path"></a>

Your `PATH` environment variable should point to the location of the HAWQ bin directory. For example:

``` pre
PATH=$GPHOME/bin:$PATH
export PATH 
```

### LD\_LIBRARY\_PATH<a id="ld_library_path"></a>

The `LD_LIBRARY_PATH` environment variable should point to the location of the `HAWQ/PostgreSQL` library files. For example:

``` pre
LD_LIBRARY_PATH=$GPHOME/lib
export LD_LIBRARY_PATH
```

## Optional Environment Variables<a id="optionalenvironmentvariables"></a>

The following are HAWQ environment variables. You may want to add the connection-related environment variables to your profile, for convenience. That way, you do not have to type so many options on the command line for client connections. Note that these environment variables should be set on the HAWQ master host only.


### PGAPPNAME<a id="pgappname"></a>

This is the name of the application that is usually set by an application when it connects to the server. This name is displayed in the activity view and in log entries. The `PGAPPNAME` environmental variable behaves the same as the `application_name` connection parameter. The default value for `application_name` is `psql`. The name cannot be longer than 63 characters.

### PGDATABASE<a id="pgdatabase"></a>

The name of the default database to use when connecting.

### PGHOST<a id="pghost"></a>

The HAWQ master host name.

### PGHOSTADDR<a id="pghostaddr"></a>

The numeric IP address of the master host. This can be set instead of, or in addition to, `PGHOST`, to avoid DNS lookup overhead.

### PGPASSWORD<a id="pgpassword"></a>

The password used if the server demands password authentication. Use of this environment variable is not recommended, for security reasons (some operating systems allow non-root users to see process environment variables via ps). Instead, consider using the `~/.pgpass` file.

### PGPASSFILE<a id="pgpassfile"></a>

The name of the password file to use for lookups. If not set, it defaults to `~/.pgpass`.

See The Password File under [Configuring Client Authentication](../clientaccess/client_auth/index.html).

### PGOPTIONS<a id="pgoptions"></a>

Sets additional configuration parameters for the HAWQ master server.

### PGPORT<a id="pgport"></a>

The port number of the HAWQ server on the master host. The default port is 5432.

### PGUSER<a id="pguser"></a>

The HAWQ user name used to connect.

### PGDATESTYLE<a id="pgdatestyle"></a>

Sets the default style of date/time representation for a session. (Equivalent to `SET datestyle TO....`)

### PGTZ<a id="pgtz"></a>

Sets the default time zone for a session. (Equivalent to `SET timezone                   TO....`)

### PGCLIENTENCODING<a id="pgclientencoding"></a>

Sets the default client character set encoding for a session. (Equivalent to `SET client_encoding TO....`)

  



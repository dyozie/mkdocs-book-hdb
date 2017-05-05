---
title: Installing Procedural Languages and Package Extensions for HAWQ
---
This topic describes how to install procedural language and package extensions in HAWQ.

## Supported Procedural Languages <a id="supportpl"></a>

HAWQ includes support for the following procedural languages:

   - PL/pgSQL
   - PL/Java
   - PL/Perl  
   - PL/Python 
   - PL/R
   
The PL/pgSQL, PL/Java, PL/Perl, and PL/Python procedural languages are embedded within the HAWQ distribution (or within your HAWQ build if you choose to enable them as build options.) 

PL/pgSQL is automatically registered in all HAWQ databases.  To use the other procedural languages, you simply register the PL for each database in which you want to use the language.  See [Using PL/Python](../../hawq/plext/using_plpython.html#enableplpython), [Using PL/Perl](../../hawq/plext/using_plperl.html), and [Using PL/Java](../../hawq/plext/using_pljava/index.html) for language registration instructions.

The PL/R procedural language must be explicitly installed before use.  See [Installing PL/R](install_plr/index.html) for installation and language registration instructions for PL/R.

**Note**: If you enable a procedural language or extension in the <b>template1</b> database, any new database that you create in HAWQ will support the procedural language. Use this option only if you are absolutely certain you want to enable the procedural language or extension in all new databases.</p>

## Supported Extensions <a id="supportpl"></a>

### pgcrypto<a id="supportextpgcrypto"></a>
You can enable the `pgcrypto` package extension for use with HAWQ.  See [Using Cryptographic Functions](../../hawq/plext/using_pgcrypto/index.html) for instructions on how to enable this extension for use in the databases in your HAWQ cluster.
   

### MADLib <a id="installmadlib"></a>

The MADlib extension for HAWQ must be explicitly downloaded and installed before use. See [Installing MADlib](install_madlib/index.html) for MADlib installation and database registration instructions for HAWQ.

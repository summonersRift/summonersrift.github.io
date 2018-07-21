---
layout: post
title:  "MongoDB vs PostgreSQL"
date:   2018-07-21
---

<p class="intro">MongoDB is PostgreSQL is relational database like MySQL.</p>


<h3> SQL vs MongoDB Terminology and Concepts</h3>

<table border="1">
  <thead valign="bottom">
    <tr><th>SQL Terms/Concepts</th>
      <th>MongoDB Terms/Concepts</th>
    </tr>
  </thead>
  <tbody valign="top">
    <tr>
      <td>database</td>
      <td><a href="../glossary/#term-database"><em>database</em></a></td>
    </tr>
    <tr><td>table</td>
      <td><a href="../glossary/#term-collection"><em>collection</em></a></td>
    </tr>
    <tr><td>row</td>
      <td><a href="../glossary/#term-document"><em>document</em></a> or <a href="../glossary/#term-bson"><em>BSON</em></a> document</td>
    </tr>
    <tr>
      <td>column</td>
      <td><a href="../glossary/#term-field"><em>field</em></a></td>
    </tr>
    <tr>
      <td>index</td>
      <td><a href="../glossary/#term-index"><em>index</em></a></td>
    </tr>
    <tr>
      <td>table joins</td>
    <td>embedded documents and linking</td>
    </tr>
    <tr>
      <td><p>primary key</p>
       <p>Specify any unique column or column combination as primary
    key.</p>
      </td>
      <td><p><a href="../glossary/#term-primary-key"><em>primary key</em></a></p>
      <p>In MongoDB, the primary key is automatically set to the
      <a href="../glossary/#term-id"><em>_id</em></a> field.</p>
      </td>
    </tr>
    <tr>
      <td>aggregation (e.g. group by)</td>
      <td><p>aggregation framework</p>
    <p>See the <a href="../sql-aggregation-comparison/"><em>SQL to Aggregation Framework Mapping Chart</em></a>.</p>
    </td>
    </tr>
</tbody>
</table>

<h3>Examples</h3>
The following table presents the various SQL statements and the corresponding MongoDB statements. The examples in the table assume the following conditions:

The SQL examples assume a table named users.

The MongoDB examples assume a collection named users that contain documents of the following prototype:

{
  _id: ObjectID("509a8fb2f3f4948bd2f983a0"),
  user_id: "abc123",
  age: 55,
  status: 'A'
}


<h3>Create and Alter</h3>
<table border="1">
  <thead valign="bottom">
    <tr><th>SQL Schema Statements</th>
    <th>MongoDB Schema Statements</th>
    <th>Reference</th>
    </tr>
    </thead>
  <tbody valign="top">
    <tr><td><pre>CREATE TABLE users (
        id MEDIUMINT NOT NULL
            AUTO_INCREMENT,
        user_id Varchar(30),
        age Number,
        status char(1),
        PRIMARY KEY (id)
    )
    </pre>
    
    </td>
    <td><p>Implicitly created on first <a href="../method/db.collection.insert/#db.collection.insert" title="db.collection.insert"><tt>insert</tt></a> operation. The primary key <tt>_id</tt> is
    automatically added if <tt>_id</tt> field is not specified.</p>
    <pre>db.users.insert( {
        user_id: "abc123",
        age: 55,
        status: "A"
     } )
    </pre>
    
    <p>However, you can also explicitly create a collection:</p>
    <pre>db.createCollection("users")
    </pre>
    
    </td>
    <td>See
    <a href="../method/db.collection.insert/#db.collection.insert" title="db.collection.insert"><tt>insert()</tt></a> and
    <a href="../method/db.createCollection/#db.createCollection" title="db.createCollection"><tt>createCollection()</tt></a>
    for more information.</td>
    </tr>
    <tr><td><pre>ALTER TABLE users
    ADD join_date DATETIME
    </pre>
    
    </td>
    <td>Collections do not describe or enforce the structure of the
    constituent documents. See the <a href="http://www.mongodb.org/display/DOCS/Schema+Design" rel="nofollow">Schema Design</a> wiki page for more information.</td>
    <td>See <a href="../method/db.collection.update/#db.collection.update" title="db.collection.update"><tt>update()</tt></a> and
    <a href="../operators/#_S_set" title="$set"><tt>$set</tt></a> for more information on changing the structure
    of documents in a collection.</td>
    </tr>
    <tr><td><pre>ALTER TABLE users
    DROP COLUMN join_date
    </pre>
    
    </td>
    <td>Collections do not describe or enforce the structure of the
    constituent documents. See the <a href="http://www.mongodb.org/display/DOCS/Schema+Design" rel="nofollow">Schema Design</a> wiki page for more information.</td>
    <td>See <a href="../method/db.collection.update/#db.collection.update" title="db.collection.update"><tt>update()</tt></a> and
    <a href="../operators/#_S_set" title="$set"><tt>$set</tt></a> for more information on changing the structure
    of documents in a collection.</td>
    </tr>
    <tr><td><pre>CREATE INDEX idx_user_id_asc
    ON users(user_id)
    </pre>
    
    </td>
    <td><pre>db.users.ensureIndex( { user_id: 1 } )
    </pre>
    
    </td>
    <td>See <a href="../method/db.collection.ensureIndex/#db.collection.ensureIndex" title="db.collection.ensureIndex"><tt>ensureIndex()</tt></a>
    and <a href="../../core/indexes/"><em>indexes</em></a> for more information.</td>
    </tr>
    <tr><td><pre>CREATE INDEX
           idx_user_id_asc_age_desc
    ON users(user_id, age DESC) </pre>
    
    </td>
    <td><pre>db.users.ensureIndex( { user_id: 1, age: -1 } ) </pre>
    
    </td>
    </tr>
    <tr><td><pre>DROP TABLE users </pre>
    
    </td>
    <td><pre>db.users.drop() </pre>
    </td>
    </tr>
  </tbody>
</table>

<h3>Select</h3>
<table border="1">
  <thead valign="bottom">
    <tr><th>SQL SELECT Statements</th>
    <th>MongoDB find() Statements</th>
    <th>Reference</th>
    </tr>
  </thead>
  <tbody valign="top">
    <tr><td><pre>SELECT *
    FROM users
    </pre>
    
    </td>
    <td><pre>db.users.find()
    </pre>
    
    </td>
    <td>See <a href="../method/db.collection.find/#db.collection.find" title="db.collection.find"><tt>find()</tt></a>
    for more information.</td>
    </tr>
    <tr><td><pre>SELECT id, user_id, status
    FROM users
    </pre>
    
    </td>
    <td><pre>db.users.find(
        { },
        { user_id: 1, status: 1 }
    )
    </pre>
    
    </td>
    <td>See <a href="../method/db.collection.find/#db.collection.find" title="db.collection.find"><tt>find()</tt></a>
    for more information.</td>
    </tr>
    <tr><td><pre>SELECT user_id, status
    FROM users
    </pre>
    
    </td>
    <td><pre>db.users.find(
        { },
        { user_id: 1, status: 1, _id: 0 }
    )
    </pre>
    
    </td>
    <td>See <a href="../method/db.collection.find/#db.collection.find" title="db.collection.find"><tt>find()</tt></a>
    for more information.</td>
    </tr>
    <tr><td><pre>SELECT *
    FROM users
    WHERE status = "A"
    </pre>
    
    </td>
    <td><pre>db.users.find(
        { status: "A" }
    )
    </pre>
    
    </td>
    <td>See <a href="../method/db.collection.find/#db.collection.find" title="db.collection.find"><tt>find()</tt></a>
    for more information.</td>
    </tr>
    <tr><td><pre>SELECT user_id, status
    FROM users
    WHERE status = "A"
    </pre>
    
    </td>
    <td><pre>db.users.find(
        { status: "A" },
        { user_id: 1, status: 1, _id: 0 }
    )
    </pre>
    
    </td>
    <td>See <a href="../method/db.collection.find/#db.collection.find" title="db.collection.find"><tt>find()</tt></a>
    for more information.</td>
    </tr>
    <tr><td><pre>SELECT *
    FROM users
    WHERE status != "A"
    </pre>
    
    </td>
    <td><pre>db.users.find(
        { status: { $ne: "A" } }
    )
    </pre>
    
    </td>
    <td>See <a href="../method/db.collection.find/#db.collection.find" title="db.collection.find"><tt>find()</tt></a>
    and <a href="../operators/#_S_ne" title="$ne"><tt>$ne</tt></a> for more information.</td>
    </tr>
    <tr><td><pre>SELECT *
    FROM users
    WHERE status = "A"
    AND age = 50
    </pre>
    
    </td>
    <td><pre>db.users.find(
        { status: "A",
          age: 50 }
    )
    </pre>
    
    </td>
    <td>See <a href="../method/db.collection.find/#db.collection.find" title="db.collection.find"><tt>find()</tt></a>
    and <a href="../operators/#_S_and" title="$and"><tt>$and</tt></a> for more information.</td>
    </tr>
    <tr><td><pre>SELECT *
    FROM users
    WHERE status = "A"
    OR age = 50
    </pre>
    
    </td>
    <td><pre>db.users.find(
        { $or: [ { status: "A" } ,
                 { age: 50 } ] }
    )
    </pre>
    
    </td>
    <td>See <a href="../method/db.collection.find/#db.collection.find" title="db.collection.find"><tt>find()</tt></a>
    and <a href="../operators/#_S_or" title="$or"><tt>$or</tt></a> for more information.</td>
    </tr>
    <tr><td><pre>SELECT *
    FROM users
    WHERE age &gt; 25
    </pre>
    
    </td>
    <td><pre>db.users.find(
        { age: { $gt: 25 } }
    )
    </pre>
    
    </td>
    <td>See <a href="../method/db.collection.find/#db.collection.find" title="db.collection.find"><tt>find()</tt></a>
    and <a href="../operators/#_S_gt" title="$gt"><tt>$gt</tt></a> for more information.</td>
    </tr>
    <tr><td><pre>SELECT *
    FROM users
    WHERE age &lt; 25
    </pre>
    
    </td>
    <td><pre>db.users.find(
       { age: { $lt: 25 } }
    )
    </pre>
    
    </td>
    <td>See <a href="../method/db.collection.find/#db.collection.find" title="db.collection.find"><tt>find()</tt></a>
    and <a href="../operators/#_S_lt" title="$lt"><tt>$lt</tt></a> for more information.</td>
    </tr>
    <tr><td><pre>SELECT *
    FROM users
    WHERE age &gt; 25
    AND   age &lt;= 50
    </pre>
    
    </td>
    <td><pre>db.users.find(
       { age: { $gt: 25, $lte: 50 } }
    )
    </pre>
    
    </td>
    <td>See <a href="../method/db.collection.find/#db.collection.find" title="db.collection.find"><tt>find()</tt></a>,
    <a href="../operators/#_S_gt" title="$gt"><tt>$gt</tt></a>, and <a href="../operators/#_S_lte" title="$lte"><tt>$lte</tt></a> for
    more information.</td>
    </tr>
    <tr><td><pre>SELECT *
    FROM users
    WHERE user_id like "%bc%"
    </pre>
    
    </td>
    <td><pre>db.users.find(
       { user_id: /bc/ }
    )
    </pre>
    
    </td>
    <td>See <a href="../method/db.collection.find/#db.collection.find" title="db.collection.find"><tt>find()</tt></a>
    and <a href="../operators/#_S_regex" title="$regex"><tt>$regex</tt></a> for more information.</td>
    </tr>
    <tr><td><pre>SELECT *
    FROM users
    WHERE user_id like "bc%"
    </pre>
    
    </td>
    <td><pre>db.users.find(
       { user_id: /^bc/ }
    )
    </pre>
    
    </td>
    <td>See <a href="../method/db.collection.find/#db.collection.find" title="db.collection.find"><tt>find()</tt></a>
    and <a href="../operators/#_S_regex" title="$regex"><tt>$regex</tt></a> for more information.</td>
    </tr>
    <tr><td><pre>SELECT *
    FROM users
    WHERE status = "A"
    ORDER BY user_id ASC
    </pre>
    
    </td>
    <td><pre>db.users.find( { status: "A" } ).sort( { user_id: 1 } )
    </pre>
    
    </td>
    <td>See <a href="../method/db.collection.find/#db.collection.find" title="db.collection.find"><tt>find()</tt></a>
    and <a href="../method/cursor.sort/#cursor.sort" title="cursor.sort"><tt>sort()</tt></a>
    for more information.</td>
    </tr>
    <tr><td><pre>SELECT *
    FROM users
    WHERE status = "A"
    ORDER BY user_id DESC
    </pre>
    
    </td>
    <td><pre>db.users.find( { status: "A" } ).sort( { user_id: -1 } )
    </pre>
    
    </td>
    <td>See <a href="../method/db.collection.find/#db.collection.find" title="db.collection.find"><tt>find()</tt></a>
    and <a href="../method/cursor.sort/#cursor.sort" title="cursor.sort"><tt>sort()</tt></a>
    for more information.</td>
    </tr>
    <tr><td><pre>SELECT COUNT(*)
    FROM users
    </pre>
    
    </td>
    <td><pre>db.users.count()
    </pre>
    
    <p><em>or</em></p>
    <pre>db.users.find().count()
    </pre>
    
    </td>
    <td>See <a href="../method/db.collection.find/#db.collection.find" title="db.collection.find"><tt>find()</tt></a>
    and <a href="../method/cursor.count/#cursor.count" title="cursor.count"><tt>count()</tt></a> for
    more information.</td>
    </tr>
    <tr><td><pre>SELECT COUNT(user_id)
    FROM users
    </pre>
    
    </td>
    <td><pre>db.users.count( { user_id: { $exists: true } } )
    </pre>
    
    <p><em>or</em></p>
    <pre>db.users.find( { user_id: { $exists: true } } ).count()
    </pre>
    
    </td>
    <td>See <a href="../method/db.collection.find/#db.collection.find" title="db.collection.find"><tt>find()</tt></a>,
    <a href="../method/cursor.count/#cursor.count" title="cursor.count"><tt>count()</tt></a>, and
    <a href="../operators/#_S_exists" title="$exists"><tt>$exists</tt></a> for more information.</td>
    </tr>
    <tr><td><pre>SELECT COUNT(*)
    FROM users
    WHERE age &gt; 30
    </pre>
    
    </td>
    <td><pre>db.users.count( { age: { $gt: 30 } } )
    </pre>
    
    <p><em>or</em></p>
    <pre>db.users.find( { age: { $gt: 30 } } ).count()
    </pre>
    
    </td>
    <td>See <a href="../method/db.collection.find/#db.collection.find" title="db.collection.find"><tt>find()</tt></a>,
    <a href="../method/cursor.count/#cursor.count" title="cursor.count"><tt>count()</tt></a>, and
    <a href="../operators/#_S_gt" title="$gt"><tt>$gt</tt></a> for more information.</td>
    </tr>
    <tr><td><pre>SELECT DISTINCT(status)
    FROM users
    </pre>
    
    </td>
    <td><pre>db.users.distinct( "status" )
    </pre>
    
    </td>
    <td>See <a href="../method/db.collection.find/#db.collection.find" title="db.collection.find"><tt>find()</tt></a>
    and <a href="../method/db.collection.distinct/#db.collection.distinct" title="db.collection.distinct"><tt>distinct()</tt></a>
    for more information.</td>
    </tr>
    <tr><td><pre>SELECT *
    FROM users
    LIMIT 1
    </pre>
    
    </td>
    <td><pre>db.users.findOne()
    </pre>
    
    <p><em>or</em></p>
    <pre>db.users.find().limit(1)
    </pre>
    
    </td>
    <td>See <a href="../method/db.collection.find/#db.collection.find" title="db.collection.find"><tt>find()</tt></a>,
    <a href="../method/db.collection.findOne/#db.collection.findOne" title="db.collection.findOne"><tt>findOne()</tt></a>,
    and <a href="../method/cursor.limit/#cursor.limit" title="cursor.limit"><tt>limit()</tt></a>
    for more information.</td>
    </tr>
    <tr><td><pre>SELECT *
    FROM users
    LIMIT 5
    SKIP 10
    </pre>
    
    </td>
    <td><pre>db.users.find().limit(5).skip(10)
    </pre>
    
    </td>
    <td>See <a href="../method/db.collection.find/#db.collection.find" title="db.collection.find"><tt>find()</tt></a>,
    <a href="../method/cursor.limit/#cursor.limit" title="cursor.limit"><tt>limit()</tt></a>, and
    <a href="../method/cursor.skip/#cursor.skip" title="cursor.skip"><tt>skip()</tt></a> for
    more information.</td>
    </tr>
    <tr><td><pre>EXPLAIN SELECT *
    FROM users
    WHERE status = "A"
    </pre>
    
    </td>
    <td><pre>db.users.find( { status: "A" } ).explain()
    </pre>
    
    </td>
    <td>See <a href="../method/db.collection.find/#db.collection.find" title="db.collection.find"><tt>find()</tt></a>
    and <a href="../method/cursor.explain/#cursor.explain" title="cursor.explain"><tt>explain()</tt></a>
    for more information.</td>
    </tr>
  </tbody>
</table>


<h3>Update</h3>
<table border="1">
  <thead valign="bottom">
    <tr><th>SQL Update Statements</th>
        <th>MongoDB update() Statements</th>
     </tr>
  </thead>
  <tbody valign="top">
    <tr><td><pre>UPDATE users
    SET status = "C"
    WHERE age &gt; 25
    </pre>
    
    </td>
    <td><pre>db.users.update(
       { age: { $gt: 25 } },
       { $set: { status: "C" } },
       { multi: true }
    )
    </pre>
    
    </td>
    <td>See <a href="../method/db.collection.update/#db.collection.update" title="db.collection.update"><tt>update()</tt></a>,
    <a href="../operators/#_S_gt" title="$gt"><tt>$gt</tt></a>, and <a href="../operators/#_S_set" title="$set"><tt>$set</tt></a> for more
    information.</td>
    </tr>
    <tr><td><pre>UPDATE users
    SET age = age + 3
    WHERE status = "A"
    </pre>
    
    </td>
    <td><pre>db.users.update(
       { status: "A" } ,
       { $inc: { age: 3 } },
       { multi: true }
    )
    </pre>
    
    </td>
    <td>See <a href="../method/db.collection.update/#db.collection.update" title="db.collection.update"><tt>update()</tt></a>,
    <a href="../operators/#_S_inc" title="$inc"><tt>$inc</tt></a>, and <a href="../operators/#_S_set" title="$set"><tt>$set</tt></a> for more
    information.</td>
    </tr>
  </tbody>
</table>


<h3>Insert</h3>
<table border="1">
  <thead valign="bottom">
    <tr><th>SQL INSERT Statements</th>
    <th>MongoDB insert() Statements</th>
    <th>Reference</th>
    </tr>
  </thead>
  <tbody valign="top">
    <tr><td><pre>INSERT INTO users(user_id,
                      age,
                      status)
    VALUES ("bcd001",
            45,
            "A")
    </pre>
    
    </td>
    <td><pre>db.users.insert( {
           user_id: "bcd001",
           age: 45,
           status: "A"
    } )
    </pre>
    </td>
    </tr>
  </tbody>
</table>


<h3>Delete</h3>
<table border="1">
  <thead valign="bottom">
    <tr><th>SQL Delete Statements</th>
    <th>MongoDB remove() Statements</th>
    </tr>
  </thead>
    <tbody valign="top">
    <tr><td><pre>DELETE FROM users
    WHERE status = "D" </pre>
    </td>
    <td><pre>db.users.remove( { status: "D" } ) </pre>
    </td>
    </tr>

    <tr><td><pre>DELETE FROM users
    </pre>
    </td>
    <td><pre>db.users.remove( )
    </pre>
    </td>
    </tr>
    </tbody>
</table>



<p><br>
<b>Reference—</b><br>
1. <a href="https://gist.github.com/aponxi/4380516">https://gist.github.com/aponxi/4380516</a><br>



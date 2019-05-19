# Cassandra  
参考：https://docs.datastax.com/ja/cql-jajp/3.1/index.html  
  
### キースペース（データベース）を作成する  
参考：https://docs.datastax.com/ja/cql-jajp/3.1/cql/cql_reference/create_keyspace_r.html  
  
### カラムファミリ（テーブル）を作成する  
参考：https://docs.datastax.com/ja/cql-jajp/3.1/cql/cql_reference/create_table_r.html?hl=columnfamily  
  
Cassandraはプライマリ・キー定義内の最初のカラム名をパーティション・キーとして使用します。  
パーティションとは一つのノードです。  
パーティション・キーはパーティションを一意に決定します。  
残りのカラム、すなわちプライマリ・キー定義内でパーティション・キーでないカラムは、クラスター化カラムです。  
クラスター化とは、各パーティション内でクラスター化カラムの定義に基づきデータをソートするストレージ・エンジンのプロセスです。  
少量のデータを収集するためにパーティション全体を読み込む必要がある場合、クラスター化カラムの順番に基づいて格納されているときに行の取得が非常に効率的になります。  
```  
CREATE TABLE emp (  
empID int,  
deptID int,  
first_name varchar,  
last_name varchar,  
PRIMARY KEY (empID, deptID)  
);  
```  
この場合、empIDがパーティションキー、deptIDがクラスター化カラムです。  
  
複合パーティションキーとして定義したい場合は以下のように括弧をネストさせます。  
```  
CREATE TABLE Cats (  
block_id uuid,  
breed text,  
color text,  
short_hair boolean,  
PRIMARY KEY ((block_id, breed), color, short_hair)  
);  
```  
この場合、block_id, breedの組み合わせでパーティションが決まり、color, short_hairでクラスター化されます。  
  
### テーブルを検索する  
参考：https://docs.datastax.com/ja/cql-jajp/3.1/cql/cql_reference/select_r.html  
  

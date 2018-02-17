## Mapping
  
TypeはRDBMSでいうテーブルのことで  
その「テーブルのスキーマ」と同じ意味で、「TypeのMapping」という。  
  
Typeは三つの構造からなる  
```
1.ダイナミックマッピングに関する設定
  dynamic: {
    key:value
  }
  dynamic_template: {
    key:value
  }
2.メタフィールド（Typeのメタ情報を設定）
  _all: {
    key:value
  }
  _id: {
    key:value
  }
  ...
  他にもあるので https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-fields.html を参照
3.フィールド（Typeのフィールドを定義）
  properties: {
    "フィールド名":"フィールド型"
    フィールド型がオブジェクト型の例）
    "Type_field": { 
      "Object_field":"オブジェクトフィールドの型"
      "dynamic":"Type_fieldのダイナミックマッピングに関する設定"
      "マッピングパラメーター":"Type_fieldのメタ情報を設定"
    }
    ※マッピングパラメーターについては https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-params.html を参照
  }
```

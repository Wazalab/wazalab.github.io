---
layout: post
title: "[Rails] すべてのbooleanカラムの値を変更する方法"
date: 2021-11-22 06:34 +0000
---

## config.active_record.sqlite3.represent_boolean_as_integer = true

Rails6にアップグレードしたらSQliteのデフォルトboolean値の取扱い方が変わったみたい。
なにやら全ての値を変更せねばならず、大変だと思ったので自動化するスクリプトを書いた。

```
Rails.application.eager_load!
models = ApplicationRecord.descendants.collect { |type| type.name }
models.each{|m|
  col_hash = Object.const_get(m).columns_hash
  col_hash.each{|k,v|
    if col_hash[k].type == :boolean
      Object.const_get(m).where("#{k} = 't'").update_all( {k.to_sym => 1})
      Object.const_get(m).where("#{k} = 'f'").update_all( {k.to_sym => 0})
    end
  }
}

```




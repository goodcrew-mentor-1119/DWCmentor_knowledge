# ActsAsTaggableOnがMySQLでエラーになる場合の対処法

### _acts_as_taggable_on_migration.rb
**references型でbigintでないと怒られるのでtypeを追記**

```ruby
# frozen_string_literal: true

class ActsAsTaggableOnMigration < ActiveRecord::Migration[6.0]
  def self.up
    create_table ActsAsTaggableOn.tags_table do |t|
      t.string :name
      t.timestamps
    end

    create_table ActsAsTaggableOn.taggings_table do |t|
      # ----------
      # -- typeでbigintを指定する
      # ----------
      t.references :tag, type: :bigint foreign_key: { to_table: ActsAsTaggableOn.tags_table }

      # You should make sure that the column created is
      # long enough to store the required class names.
      t.references :taggable, polymorphic: true
      t.references :tagger, polymorphic: true

      # Limit is created to prevent MySQL error on index
      # length for MyISAM table type: http://bit.ly/vgW2Ql
      t.string :context, limit: 128

      t.datetime :created_at
    end

    add_index ActsAsTaggableOn.taggings_table, %i[taggable_id taggable_type context],
              name: 'taggings_taggable_context_idx'
  end

  def self.down
    drop_table ActsAsTaggableOn.taggings_table
    drop_table ActsAsTaggableOn.tags_table
  end
end
```

### _add_missing_unique_indices.rb
**indexを貼るでエラーになるので、コメントアウト**

```ruby
# frozen_string_literal: true

class AddMissingUniqueIndices < ActiveRecord::Migration[6.0]
  def self.up
    # add_index ActsAsTaggableOn.tags_table, :name, unique: true

    # remove_index ActsAsTaggableOn.taggings_table, :tag_id if index_exists?(ActsAsTaggableOn.taggings_table, :tag_id)
    # remove_index ActsAsTaggableOn.taggings_table, name: 'taggings_taggable_context_idx'
    # add_index ActsAsTaggableOn.taggings_table,
    #           %i[tag_id taggable_id taggable_type context tagger_id tagger_type],
    #           unique: true, name: 'taggings_idx'
  end

  def self.down
    # remove_index ActsAsTaggableOn.tags_table, :name

    # remove_index ActsAsTaggableOn.taggings_table, name: 'taggings_idx'

    # add_index ActsAsTaggableOn.taggings_table, :tag_id unless index_exists?(ActsAsTaggableOn.taggings_table, :tag_id)
    # add_index ActsAsTaggableOn.taggings_table, %i[taggable_id taggable_type context],
    #           name: 'taggings_taggable_context_idx'
  end
end
```

### _add_missing_taggable_index.rb
**indexを貼るでエラーになるので、コメントアウト**

```ruby
# frozen_string_literal: true

class AddMissingTaggableIndex < ActiveRecord::Migration[6.0]
  def self.up
    # add_index ActsAsTaggableOn.taggings_table, %i[taggable_id taggable_type context],
    #           name: 'taggings_taggable_context_idx'
  end

  def self.down
    # remove_index ActsAsTaggableOn.taggings_table, name: 'taggings_taggable_context_idx'
  end
end
```
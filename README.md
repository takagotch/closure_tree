### closure_tree
---



https://github.com/ClosureTree/closure_tree

```sh
rails g closure_tree:migration tag
rake db:migrate
rails g closure_tree:config

```

```ruby
class Tag < ActiveRecord::Base
  has_closure_tree
end
class AnotherTag < ActiveRecord::Base
  acts_as_tree
end

class AddParentIdToTag < ActiveRecord::Migration
  def change
    add_column :tags, :parent_id, :integer
  end
end

grandparent = Tag.create(name: 'Grandparent')
parent = grandparent.children.create(name: 'Parent')


```

```ruby
```

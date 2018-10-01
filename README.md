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

chid2 = Tag.new(name: 'Second Child')
parent.children << child2

child3 = Tag.new(name: 'Third Child')
parent.add_child child3

Tag.create(name: 'Fourth Child', parent: parent)

grandparent.self_and_descendants.collect(&:name)
child1.ancestry_path

child = Tag.find_or_create_by_path(%w[grandparent parent child])

child = Tag.find_or_create_by_path([
  {name: 'Grandparent', title: 'Sr.'},
  {name: 'Parent', title: 'Mrs.'},
  {name: 'Child', title: 'Jr.'}
])

child = Label.find_or_create_by_path([
  {type: 'DateLabel', name: '2014'},
  {type: 'DateLabel', name: 'August'},
  {type: 'DateLabel', name: '5'},
  {type: 'EventLabel', name: 'Visit the Getty Center'}
])

d = Tag.find_or_create_by_path %w[a b c d]
h = Tag.find_or_create_by_path %w[e f g h]
e = h.root
d.add_child(e)
h.ancestry_path

j = Tag.find 102
j.self_and_ancestor_ids
j.update parent_id: 96
j.self_and_ancestor_ids

b = Tag.find_or_create_by_path %w(a b)
a = b.parent
b2 = Tag.find_or_create_by_path %w(a b2)
d1 = b.find_or_create_by_path %w(c1 d1)
c1 = d.parent
d2 = b.find_or_create_by_path %w(c2 d2)
c2 = d2.parent

Tag.hash_tree
b.hash_tree
b.hash_tree(:limit_depth => 2)

comment.children.includes(:anthor)
comment.self_and_decendants.includes(:anthor)

# app/models/post.rb
has_closure_tree_root :root_comment
a_post.root_comment_including_tree(:anthor)

File.open("ex.dot", "w") { |f| f.write(Tag.root.to_dot_digraph) }

class Tag < ActiveRecord::Base
  has_closure_tree
end
class WhenTag < Tag ; end
class WhereTag < Tag ; end
class WhatTag < Tag ; end

class WhatTag < Tag
  def self.rebuild!
    Tag.rebuild!
  end
end

class Tag < ActiveRecord::Base
  has_closure_tree order: 'name'
end

t.integer :sort_order

class OrderedTag < ActiveRecord::Base
  has_closure_tree order: 'short_order', numeric_order: true
end

root = OrderedTag.create(name: 'root')
a = root.append_child(Label.new(name: 'a'))
b = OrderedTag.create(name: 'b')
c = OrderedTag.create(name: 'c')
a.append_sibling(b)
root.reload.children.pluck(:name)
a.prepend_sibling(b)
root.reload.children.pluck(:name)
a.append_sibling(c)
root.reload.chldren.pluck(:name)
a.append_sibling(c)
root.reload.children.pluck(:name)

has_closure_tree order: 'sort_order', numeric_order: true, dont_order_roots: true

class Tag
  has_closure_tree with_advisory_lock: false
end
```
```
en-US:
  closure_tree:
    loop_error: Your descendant cannot be your parent!
```

```ruby
describe "Tag with fixtures" do
  fixtures :tags
  before :each do
    Tag.rebuild!
  end
end

before do
  ENV['FLOCK_DIR'] = Dir.mktmpdir
end
after do
  FileUtils.remove_entry_secure ENV['FLOCK_DIR']
end

```
```sh
sudo apt-get install libpq-dev libsqlite3-dev libmysqlclient-dev
bundle install
```
```ruby
require 'spec_helper'
require 'closure_tree/test/matcher'
describe Category do
  it { should be_a closure_tree }
  it { is_expected.to be_a_closure_tree }
end
describe Label do
  it { should be_a_closure_tree.ordered}
  it { is_expected.to be_a_closure_tree.ordered }
end
describe TodoList::Item do
  it { should be_a_closure_tree.ordered(:priority_order) }
  it { is_expected.to be_a_closure_tree.orderd(:priority_order) }
end


```

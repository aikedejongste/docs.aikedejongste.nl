---
layout: default
title: Ruby
has_children: false
parent: Programming
---

# Ruby code snippets

For installation and dependencies see Rails page.

## Switch Ruby version

```bash
sudo ruby-switch —set ruby`cat .ruby-version | cut -c1-3`
```

## Loops

```ruby
User.all.each { |u| Connection.create(connected_user_id: 14, user_id: u.id, initiator: u.id, approved: true) rescue next }

update occupants set email='occupant_' || id::text || '@ietsveiligs.nl';

update contact_people set email='contact_' || id::text || '@ietsveiligs.nl';

def solution(n)
  f = 0
  (1..n).select { |i| (f += 1) if (n % i == 0)}
  return f
end
```

## Hashes and arrays

min/max key by value in hash:           key_with_min_value = h.min_by { |k, v| v }[0]
sum of items in array =                         reduce(:+)
update all items in array:              c.map {|e| e.abs }
count occurrences of 3 in array:         a.select { |v| v == 3 }.size
find first occurrence (index)            a.index { |v| v == 3 }
factor test is                                  24 % i == 0
strings met verschillend teken        a.split('').zip(b.split('')).select { |c1, c2| c1 != c2 }.count

Well, an array can be a stack or queue by limiting yourself to stack or queue methods (push, pop, shift, unshift).
Using push / pop gives LIFO(last in first out) behavior (stack),
while using push / shift gives FIFO behavior (queue).

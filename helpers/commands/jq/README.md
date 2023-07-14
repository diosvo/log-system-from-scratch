## [./jq](https://jqlang.github.io/jq/)

jq is a lightweight and flexible command-line JSON processor.

jq is like `sed` for JSON data - to slice and filter and map and transform structured data with the same ease that `sed`, `awk`, `grep` and friends let you play with text.

## 💡 How to use

The simplest jq program is the expression <code style="border-radius: 4px; padding: 2px 4px">.</code>, which takes the input and produces it unchanged as output.

```ruby
curl 'https://api.github.com/repos/jqlang/jq/commits?per_page=5' | jq '.'
```

### Transforming, sorting, and grouping

There's a lot of info we don't care about there, so we will:

- restrict it own to the most interesting fields
- filter null values with the `select` function
- groups the elements with the `group_by` function having the same `author_id` field into separate arrays.
- sorting an array with the specific field using `sort_by(path_expression)`
- get top 3 with the `reverse` and `limit` functions

  ```ruby
  curl 'https://api.github.com/repos/jqlang/jq/commits?per_page=100' \
  | jq '[.[]'\
  ' | {author_id: .author.id, name: .commit.author.name, message: .commit.message}'\
  ' | select(.name != null and .message != null)]'\
  ' | group_by(.author_id) | [.[] | {author_id: .[0].author_id, name: .[0].name, total_commits: . | length}]'\
  ' | sort_by(.total_commits)'\
  ' | reverse | [limit(3;.[])]'
  ```

Where:

The `|` operator in jq feeds the output of one filter (`.[]` returns each element of the array returned in the response, one at a time)

into

The input of another (`{...}` which builds an object out of those fields).

### Merging JSON documents

Use the command below to merge multiple JSON files together:

```ruby
jq <file_1.json> <file_2.json> ... <file_{n}.json>
```

If we could put those two documents into an array and merge values if they have the same key

```ruby
jq -s 'reduce .[] as $item ({}; reduce ($item | keys_unsorted[]) as $key (.; $item[$key] as $value | ($value | type) as $type |.[$key] = if ($type == "array") then (.[$key] + $value | unique) elif ($type == "object") then (.[$key] + $value) else $value end))' ./misc/user-1.json ./misc/user-2.json
```

**NOTE**:

- add: _adds all elements in that array to a single one_
- reduce: _combine all the elements of a data structure into a single value_

### 📚 References

- [documentation - jqlang](https://jqlang.github.io/jq/tutorial/)
- [jq tutorial - Szymon Stepniak](https://www.youtube.com/playlist?list=PLKaiHc24qCTSOGkkEpeIMupEmnInqHbbV)

## [./jq](https://jqlang.github.io/jq/)

jq is a lightweight and flexible command-line JSON processor.

jq is like `sed` for JSON data - to slice and filter and map and transform structured data with the same ease that `sed`, `awk`, `grep` and friends let you play with text.

### How to use

The simplest jq program is the expression <code style="border-radius: 4px; padding: 2px 4px">.</code>, which takes the input and produces it unchanged as output.

```ruby
curl 'https://api.github.com/repos/jqlang/jq/commits?per_page=5' | jq '.'
```

**Transforming, sorting, and grouping**:

There's a lot of info we don't care about there, so we will:

- restrict it down to the most interesting fields
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

### 📄 References

| Name                                                                                    | Source          |
| --------------------------------------------------------------------------------------- | --------------- |
| [documentation](https://jqlang.github.io/jq/tutorial/)                                  | jqlang          |
| [jq tutorial](https://www.youtube.com/playlist?list=PLKaiHc24qCTSOGkkEpeIMupEmnInqHbbV) | Szymon Stepniak |

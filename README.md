# Conditions - Expression Parser and Evaluator
The `conditions` package evaluates expressions to a `true` or `false` value.  Conceptually (and syntactically) similar to what you'd find in a `WHERE` clause in SQL.

It supports variable expansion, and nested / parenthesised expressions

## Operators
| Operator | Symbol | Syntax | Description |
| -------- | ------ | ------ | ----------- |
| AND | `AND` | `lhs AND rhs` | Will be `true` if both `lhs` and `rhs` are true |
| OR | `OR` | `lhs OR rhs` | Will be `true` if either `lhs` and `rhs` are true |
| EQ | `=` | `lhs = rhs` | Will be `true` if `lhs` is equal to `rhs` |
| NEQ | `!=` | `lhs != rhs` | Will be `true` if `lhs` is not equal to `rhs` |
| LT | `<` |  `lhs < rhs` | Will be `true` if `lhs` is less than`rhs` |
| LTE | `<=` | `lhs <= rhs` | Will be `true` if `lhs` is less than or equal to `rhs` |
| GT | `>` | `lhs > rhs` | Will be `true` if `lhs` is greater than to `rhs` |
| GTE | `>=` | `lhs >= rhs` | Will be `true` if `lhs` is greater than or equal to `rhs` |
| EREG | `=~` | `lhs =~ regex` | Will be `true` if `lhs` matches regular expression `regex` |
| NEREG | `!~` | `lhs !~ rhs` | Will be `true` if `lhs` does not match regular expression `regex` |
| IN | `IN` | `lhs IN [array, of, values]` | Will be `true` if `lhs` is in the array |
| NOTIN | `NOT IN` | `lhs NOT IN [array, of, values]` | Will be `true` if `lhs` is not in the array |
| CONTAINS | `CONTAINS` | `[array, of, values] CONTAINS rhs` | Will be `true` if `rhs` is in the array |
| CONTAINS | `NOT` | `[array, of, values] CONTAINS rhs` | Will be `true` if `rhs` is in the array |

## Examples
```
    package main
    
    import (
        "context"
        "fmt"
        "log"
        "strings"

        "github.com/jbirtley88/gremel/conditions"
    )

    func main() {
        // Variables can live in the context
        ctx := context.WithValue(
            context.Background(),
            "foo",
            123,
        )

        // Variables can also live in a map.
        // The map is checked first and ifthe variable is not found, then the context
        // is checked.
        vars := map[string]any{
            "bar": 999,
        }

	expression := `{foo} > 100 AND {bar} < 1000`
        p := conditions.NewParser(strings.NewReader(expression))
        expr, err := p.Parse()
        if err != nil {
            log.Fatal(err)
        }

        // Evaluate expression passing data for {vars}
        // viper.Set("config.undefined_is_zero", true)
        r, err := conditions.Evaluate(ctx, expr, vars)
        if err != nil {
            log.Fatal(err)
        }

        fmt.Printf("%s: %t\n", expression, r)
    }
```

# Licences
    Conditions uses the very permissive BSD-3-Clause licence.  There are zero restrictions on how you use it, other than you acknowledge using it (and please give it a star on github!)


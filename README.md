# fuzzy

Fuzzy finding for [Folio](https://github.com/samyyy2423/folio) programs: how far apart two words are, the
nearest of a list to a misspelt one ("did you mean...?"), and a list searched as a command palette or fzf
searches it, by letters in order, the best first, with the letters it matched. In Folio alone, so it works
everywhere Folio does.

```text
folio add fuzzy
```

```
use the package "fuzzy" as fuzzy

set commands to list of "build", "test", "tidy", "notebook"
set typed to "tset"
if not (typed is in commands)
    set near to fuzzy's closest(typed, commands)
    if near is not nothing
        say "There's no command '" typed "'. Did you mean '" near "'?"     # ... Did you mean 'test'?
    end
end

set files to list of "notes.txt", "main.folio", "README.md", "tools/net.folio"
say fuzzy's ranked("nm", files)                                 # ["tools/net.folio", "main.folio"]
say fuzzy's highlighted("nm", "tools/net.folio", "[", "]")       # tools/[n]et.[m]oth
```

## What it has

| | |
|---|---|
| `distance(a, b)` | letters changed, added, taken away, or swapped with the one next to them, to make a into b: `distance("kitten", "sitting")` is 3 |
| `similarity(a, b)` | from 0 (nothing alike) to 1 (the same) |
| `closest(word, choices)` | the nearest choice, not minding capitals, or `nothing` if none is near |
| `suggestions(word, choices)` | the near choices, nearest first |
| `ranked(query, choices)` | the choices that have the query's letters in order, best first |
| `score(query, choice)` | how well it matches (more is better), or `nothing` |
| `positions(query, choice)` | where the matched letters are, from 1 |
| `highlighted(query, choice, before, after)` | the choice with `before` and `after` around each run of matched letters: `"<b>"` and `"</b>"`, or the terminal package's codes |
| `matches(query, choice)` | whether it matches at all |

`closest` and `suggestions` take words at most a third of the word's length away (at least 1). `ranked`
doesn't mind capitals or the query's spaces, and scores as fzf does: letters next to each other, at the start
of a word (after a space, `_`, `-`, `/`, `.` or a capital in camelCase), and near the start score higher; ties
go to the shorter choice, then the first.

## Tests

```text
folio test fuzzy.test.folio
```

## License

Apache License 2.0, as Folio is: see [LICENSE](LICENSE).

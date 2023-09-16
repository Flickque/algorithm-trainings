# Regular Expressions in Brief

A regular expression, or regex, is a pattern that specifies a set of strings. They are a powerful tool for text processing, allowing for intricate text search and manipulation.

## 1. Basic Syntax:

- `.`: Matches any single character, except for line breaks (unless the `s` flag is used).
- `\d`: Matches any digit. Equivalent to `[0-9]`.
- `\D`: Matches any character that is not a digit.
- `\w`: Matches any word character (alphanumeric characters plus underscore). Equivalent to `[a-zA-Z0-9_]`.
- `\W`: Matches any non-word character.
- `\s`: Matches any whitespace character (spaces, tabs, line breaks).
- `\S`: Matches any non-whitespace character.
- `\b`: Asserts a position at a word boundary.
- `\B`: Asserts a position where there isn't a word boundary.

## 2. Quantifiers:

- `*`: Matches 0 or more of the preceding token.
- `+`: Matches 1 or more of the preceding token.
- `?`: Makes the preceding token optional.
- `{n}`: Matches exactly `n` of the preceding token.
- `{n,}`: Matches `n` or more of the preceding token.
- `{n,m}`: Matches between `n` and `m` (inclusive) of the preceding token.

## 3. Positional Anchors:

- `^`: Matches the start of a string.
- `$`: Matches the end of a string.
- `\A`: Matches the start of the input.
- `\Z`: Matches the end of the input, but before the final line terminator, if present.
- `\z`: Matches the absolute end of the input.

## 4. Groups, Ranges, and Lookaround:

- `|`: Acts as an OR. Matches the expression before or after the `|`.
- `(...)`: Groups multiple tokens together and creates a capture group. You can refer back to groups with `\1`, `\2`, etc.
- `(?:...)`: Creates a non-capturing group.
- `[...]`: Matches any one character inside the square brackets.
- `[^...]`: Matches any one character NOT inside the square brackets.
- `(?=...)`: Positive lookahead. Asserts that what directly follows the current position in the string matches the contained pattern.
- `(?!...)`: Negative lookahead. Asserts that what directly follows the current position in the string doesn't match the contained pattern.
- `(?<=...)`: Positive lookbehind. Asserts that what directly precedes the current position in the string matches the contained pattern.
- `(?<!...)`: Negative lookbehind. Asserts that what directly precedes the current position in the string doesn't match the contained pattern.

## 5. Escaping:

If you want to search for a character which is also a special regex symbol like `.` or `*`, you need to escape it using a backslash (`\`). For example, to match a literal dot, you would write `\.`, and to match a literal asterisk, you would write `\*`.

## Examples:

- To match an email address: `^[a-zA-Z0-9._-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,6}$`
- To match a date in the format `dd/mm/yyyy`: `^(0[1-9]|[12][0-9]|3[01])/(0[1-9]|1[0-2])/\d{4}$`
- To match any number (including decimals): `^-?\d*(\.\d+)?$`
- To match all html tags: `/<\/?[^>]+(>|$)/g`

## How Regular Expressions Work "Under the Hood", Core Operating Principle:

1. **Finite Automata**: Many implementations of regular expressions are based on finite automata. These are abstract machines consisting of states and transitions between these states. There are two main types of finite automata related to regex:
    - **NFA (Nondeterministic Finite Automaton)**: A non-deterministic automaton that can be in multiple states at once. This allows it to rapidly explore various matching avenues, but it can be less efficient in some scenarios.
    - **DFA (Deterministic Finite Automaton)**: A deterministic automaton is in one state at any given time. It can be faster than NFA for some tasks since it doesn't require backtracking and reprocessing the text.

2. **Compilation into Finite Automaton**: When you specify a regular expression, it's typically "compiled" into an internal representation (often in the form of an NFA or DFA). This automaton is then used to search for matches in the text.

3. **Backtracking**: Many regex engines, especially those using NFA, employ a technique called backtracking. This means if the engine finds a portion of the string that doesn't match the regex, it "rolls back" and tries a different path. This can lead to catastrophic backtracking if the regex is poorly designed.

## What Sets Regex Apart:

- **Specificity**: Regex is tailored for text operations. They have a rich syntax for various text-related tasks.
- **Performance**: While regex can be very fast for many tasks, they're not always the most efficient solution, especially with complex patterns.
- **Universality**: Regex is supported in nearly all modern programming languages and text-processing tools.



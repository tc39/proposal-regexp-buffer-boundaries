<!--#region:intro-->
# Regular Expression Buffer Boundaries for ECMAScript

<!--#endregion:intro-->

<!--#region:status-->
## Status

**Stage:** 1  
**Champion:** Ron Buckton ([@rbuckton](https://github.com/rbuckton))  

_For detailed status of this proposal see [TODO](#todo), below._  
<!--#endregion:status-->

<!--#region:authors-->
## Authors

* Ron Buckton ([@rbuckton](https://github.com/rbuckton))  
<!--#endregion:authors-->

<!--#region:motivations-->
# Motivations

> NOTE: See https://github.com/rbuckton/proposal-regexp-features for an overview of
> how this proposal fits into other possible future features for Regular Expressions.

Buffer Boundaries are a common feature across a wide array of regular expression engines that 
allow you to match the start or end of the entire input regardless of whether the `m` (multiline) flag
has been set. Buffer Boundaries also allow you to match the start/end of a line *and* the start/end of 
the input in a single RegExp using the `m` flag.

<!--#endregion:motivations-->

<!--#region:prior-art-->
# Prior Art 

* [Perl](https://rbuckton.github.io/regexp-features/engines/perl.html#feature-buffer-boundaries)  
* [PCRE](https://rbuckton.github.io/regexp-features/engines/pcre.html#feature-buffer-boundaries)  
* [Boost.Regex](https://rbuckton.github.io/regexp-features/engines/boost.regex.html#feature-buffer-boundaries)  
* [.NET](https://rbuckton.github.io/regexp-features/engines/dotnet.html#feature-buffer-boundaries)  
* [Oniguruma](https://rbuckton.github.io/regexp-features/engines/oniguruma.html#feature-buffer-boundaries)  
* [Hyperscan](https://rbuckton.github.io/regexp-features/engines/hyperscan.html#feature-buffer-boundaries)  
* [ICU](https://rbuckton.github.io/regexp-features/engines/icu.html#feature-buffer-boundaries)  
* [Glib/GRegex](https://rbuckton.github.io/regexp-features/engines/glib-gregex.html#feature-buffer-boundaries)  

See https://rbuckton.github.io/regexp-features/features/buffer-boundaries.html for additional information.

<!--#endregion:prior-art-->

<!--#region:syntax-->
# Syntax

Buffer boundaries are similar to the `^` and `$` anchors, except that they are not affected by the `m` (multiline) flag:

- `\A` &mdash; Matches the start of the input.
- `\z` &mdash; Matches the end of the input.
- `\Z` &mdash; A zero-width assertion consisting of an optional newline at the end of the buffer. Equivalent to `(?=\R?\z)`.

> NOTE: Requires the `u` or `v` flag, as `\A`, `\z`, and `\Z` are currently just escapes for `A`, `z` and `Z` without the `u` or `v` flag. 

> NOTE: Not supported inside of a character class.

For more information about the `v` flag, see https://github.com/tc39/proposal-regexp-set-notation.

For more information about the `\R` escape sequence, see https://github.com/tc39/proposal-regexp-r-escape.

<!--#endregion:syntax-->

<!--#region:semantics-->
<!-- # Semantics -->


<!--#endregion:semantics-->

<!--#region:examples-->
# Examples

```js
// without buffer boundaries
const pattern = String.raw`^foo$`;
const re1 = new RegExp(pattern, "u");
re1.test("foo"); // true
re1.test("foo\nbar"); // false

const re2 = new RegExp(pattern, "um");
re1.test("foo"); // true
re1.test("foo\nbar"); // true

// with buffer boundaries
const pattern = String.raw`\Afoo\z`;
const re1 = new RegExp(pattern, "u");
re1.test("foo"); // true
re1.test("foo\nbar"); // false

const re2 = new RegExp(pattern, "um");
re1.test("foo"); // true
re1.test("foo\nbar"); // false

// mixing buffer boundaries and anchors
const re = /\Afoo|^bar$|baz\z/um;
re.test("foo");         // true
re.test("foo\n");       // true
re.test("\nfoo");       // false

re.test("bar");         // true
re.test("bar\n");       // true
re.test("\nbar");       // true

re.test("baz");         // true
re.test("baz\n");       // false
re.test("\nbaz");       // true

// trailing buffer boundary
const re = /end\Z/u;
re.test("end");         // true
re.test("end\n");       // true (optional newline)
re.test("end\n\n");     // false
```

<!--#endregion:examples-->

<!--#region:api-->
<!-- # API -->

<!--#endregion:api-->

<!--#region:grammar-->
<!-- # Grammar

```grammarkdown
``` -->
<!--#endregion:grammar-->

<!--#region:references-->
<!-- # References

> TODO: Provide links to other specifications, etc.

* [Title](url)   -->
<!--#endregion:references-->

<!--#region:todo-->
# TODO

The following is a high-level list of tasks to progress through each stage of the [TC39 proposal process](https://tc39.github.io/process-document/):

### Stage 1 Entrance Criteria

* [x] Identified a "[champion][Champion]" who will advance the addition.  
* [x] [Prose][Prose] outlining the problem or need and the general shape of a solution.  
* [x] Illustrative [examples][Examples] of usage.  
* [ ] ~~High-level [API][API].~~  

### Stage 2 Entrance Criteria

* [x] [Initial specification text][Specification].  
* [ ] ~~[Transpiler support][Transpiler] (_Optional_).~~  

### Stage 3 Entrance Criteria

* [ ] [Complete specification text][Specification].  
* [ ] Designated reviewers have [signed off][Stage3ReviewerSignOff] on the current spec text.  
* [ ] The ECMAScript editor has [signed off][Stage3EditorSignOff] on the current spec text.  

### Stage 4 Entrance Criteria

* [ ] [Test262](https://github.com/tc39/test262) acceptance tests have been written for mainline usage scenarios and [merged][Test262PullRequest].  
* [ ] Two compatible implementations which pass the acceptance tests: [\[1\]][Implementation1], [\[2\]][Implementation2].  
* [ ] A [pull request][Ecma262PullRequest] has been sent to tc39/ecma262 with the integrated spec text.  
* [ ] The ECMAScript editor has signed off on the [pull request][Ecma262PullRequest].  
<!--#endregion:todo-->

<!-- The following links are used throughout the README: -->

[Process]: https://tc39.es/process-document/
[Proposals]: https://github.com/tc39/proposals/
[Grammarkdown]: http://github.com/rbuckton/grammarkdown#readme
[Champion]: #status
[Prose]: #motivations
[Examples]: #examples
[API]: #api
[Specification]: https://tc39.es/proposal-regexp-buffer-boundaries

[Transpiler]: #todo
[Stage3ReviewerSignOff]: #todo
[Stage3EditorSignOff]: #todo
[Test262PullRequest]: #todo
[Implementation1]: #todo
[Implementation2]: #todo
[Ecma262PullRequest]: #todo

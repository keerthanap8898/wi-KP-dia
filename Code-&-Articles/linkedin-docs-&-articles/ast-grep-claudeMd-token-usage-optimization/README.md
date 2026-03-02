
### My comment on [Aritra's LinkedIn Post](https://www.linkedin.com/feed/update/urn:li:activity:7434101650556100608):
> ([*i.e., Keerthana's extension to the Aritra's LinkedIn article.*](https://www.linkedin.com/feed/update/urn:li:activity:7434101650556100608))
#### **Ⓐ.** OG `claude.md` prompt [ '***`ast-grep`***' *only* ] -
> - *Whenever a search requires syntax-aware or structural matching, default to `ast-grep` as shown below. Set `--lang` to the language of the files being searched; & avoid falling back to text-only tools like `rg` or `grep` unless I explicitly request a plain-text search.*
>    ```bash
>    ast-grep --lang <language> -p '<pattern>'
>    ```
#### **Ⓑ.** NEW `claude.md` prompt [ '***`tree`***' *as well as* '***`ast-grep`***' ] -
> - *Optional optimization layer; before any non-trivial search/refactor, use `tree` as a pre-filter to map the repo & pick the narrowest relevant paths. Prefer the command below; add `--prune` if supported.*
>    ```bash
>     tree -d -L 3 -I '.git|target|node_modules|dist|build|out|vendor'
>    ```
> - *Then, whenever a search requires syntax-aware or structural matching, run `ast-grep` within those selected paths, & avoid falling back to text-only tools like `rg` or `grep` unless I explicitly request a plain-text search. If `tree` + `ast-grep` produce no relevant hits, explicitly ask me whether to use a plain-text search before running `rg`/`grep`.*
>    ```bash
>       ast-grep --lang <language> -p '<pattern>'
>    ```
---
### Some lightweight `tree` patterns
> 1.  Map directories first (cheap repo shape) -
>       ```bash 
>       tree -d -L 3 -I '.git|target|node_modules|dist|build|out|vendor' # (Add --prune if supported)
>       ```
> 2. Constrain by filetype in polyglot repos (repeat per language) -
>       ```bash
>       tree -L 4 -P '*.ts' -I '.git|target|node_modules|dist|build|out|vendor'
>       ```
> 3. Prefer full paths for copy-paste scoping -
>       ```bash
>       tree -fi -L 4 -P '*.ts' -I '.git|target|node_modules|dist|build|out|vendor'
>       ```

---

#### Original LinkedIn Post by Aritra:
> Post link: [linkedin.com/feed/update/urn:li:activity:7434101650556100608](https://www.linkedin.com/feed/update/urn:li:activity:7434101650556100608)
> 
> *"""*
> > *You're paying your AI agent to read documentation it's about to ignore.* 🚧
> > 
> > *By default, Claude uses `grep` - a text matching tool that globally searches for every occurrence of a string. For instance, search for `debounce()` in your codebase and it finds everything: the function definition, a comment explaining it, the JSDoc block, maybe a line in your README.*
> > 
> > *Now instruct your agent to refactor all usages of debounce(). It finds 4 results - 2 are in docstrings and comments. The agent has to load all 4, read them, realize 2 are documentation, and decide not to touch them.*
> > 
> > *Your instruction was to change the code. The extra files end up being tokens spent on noise the agent had to study just to ignore. The fix: one line in your CLAUDE.md.*
> > 
> > *Instruct Claude to use `ast-grep` instead. Unlike grep, ast-grep performs AST node-based matching, it understands code structure, not just strings. It won't match `debounce()` inside a comment because a comment isn't a function call node in the syntax tree.*
> > 
> > *Search for `debounce()` call expressions: you get exactly the code paths that need changing. Nothing else. Precision at search time = smaller context = fewer input tokens = less hallucination risk from irrelevant noise. What goes into your agent's reasoning matters as much as what comes out. Every input token should earn its cost.*
> >
> > ![screenie](https://github.com/keerthanap8898/wi-KP-dia/blob/main/Code-%26-Articles/linkedin-docs-%26-articles/ast-grep-claudeMd-token-usage-optimization/ast-grep-text-screenie.png)
>
> *"""*

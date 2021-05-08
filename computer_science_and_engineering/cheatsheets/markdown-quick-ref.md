## Welcome to GitHub Pages

You can use the [editor on GitHub](https://github.com/carryorange/carryorange.github.io/edit/main/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/carryorange/carryorange.github.io/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.

### Table
| Header 1 | Header 2|
| -------- | ------- |
| row 1    | row 1   |
| row 2    | row 2   |

### Graphviz

Requires the [Graphviz Markdown Preview extension](https://marketplace.visualstudio.com/items?itemName=geeklearningio.graphviz-markdown-preview)

```graphviz
digraph {
    rankdir=LR
    node [shape=record]
    root[label="Caching Patterns"]

    root -> subgraph read_cache {
        pattern_r1[label="read aside"]
        pattern_r2[label="read through"]
    } [tailport=e, headport=w]

    root -> subgraph write_cache {
        pattern_w1[label="write through"]
        pattern_w2[label="write behind cache"]
    } [tailport=e, headport=w]

}

```
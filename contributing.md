---
layout: page
title: Contributing
---

## Contributing

We welcome all contributions to this project [on GitHub](https://github.com/SonarSource/mxss-cheatsheet). You can either open an Issue to suggest improvements or directly open a Pull Request if you are confident with your suggestion.

Here are a few guidelines for Pull Requests:
- We expect an explanation of the added/changed data with a preference for publicly available references such as docs, blog posts, talks, etc.
- Make sure to use the markup syntax especially for presenting tags (using the `character). 

## Structure

The data presented in the main page is taken from `_data/data.yml` and contains the following structure:

```yaml
<topic>:
  - title: "a and [b](http://example.com) tags"
    search_terms:
      - a
      - b
    description: |
      * example of a tag
      * example of b tag
```

For any other change, refer to the relevant readme/html files.

## Building the website

We use Jekyll to generate the website and the search index. Install the required dependencies and then start Jekyll's local server:

```
$ bundle install
$ bundle exec jekyll serve
```

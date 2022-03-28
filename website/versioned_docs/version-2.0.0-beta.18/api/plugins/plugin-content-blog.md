---
sidebar_position: 2
id: plugin-content-blog
title: '📦 plugin-content-blog'
slug: '/api/plugins/@docusaurus/plugin-content-blog'
---

import APITable from '@site/src/components/APITable';

Provides the [Blog](blog.mdx) feature and is the default blog plugin for Docusaurus.

:::caution some features production only

The [feed feature](../../blog.mdx#feed) works by extracting the build output, and is **only active in production**.

:::

## Installation {#installation}

```bash npm2yarn
npm install --save @docusaurus/plugin-content-blog
```

:::tip

If you use the preset `@docusaurus/preset-classic`, you don't need to install this plugin as a dependency.

You can configure this plugin through the [preset options](#ex-config-preset).

:::

## Configuration {#configuration}

Accepted fields:

<APITable>

| Name | Type | Default | Description |
| --- | --- | --- | --- |
| `path` | `string` | `'blog'` | Path to the blog content directory on the file system, relative to site dir. |
| `editUrl` | <code>string \| EditUrlFunction</code> | `undefined` | Base URL to edit your site. The final URL is computed by `editUrl + relativePostPath`. Using a function allows more nuanced control for each file. Omitting this variable entirely will disable edit links. |
| `editLocalizedFiles` | `boolean` | `false` | The edit URL will target the localized file, instead of the original unlocalized file. Ignored when `editUrl` is a function. |
| `blogTitle` | `string` | `'Blog'` | Blog page title for better SEO. |
| `blogDescription` | `string` | `'Blog'` | Blog page meta description for better SEO. |
| `blogSidebarCount` | <code>number \| 'ALL'</code> | `5` | Number of blog post elements to show in the blog sidebar. `'ALL'` to show all blog posts; `0` to disable. |
| `blogSidebarTitle` | `string` | `'Recent posts'` | Title of the blog sidebar. |
| `routeBasePath` | `string` | `'blog'` | URL route for the blog section of your site. **DO NOT** include a trailing slash. Use `/` to put the blog at root path. |
| `tagsBasePath` | `string` | `'tags'` | URL route for the tags section of your blog. Will be appended to `routeBasePath`. **DO NOT** include a trailing slash. |
| `archiveBasePath` | <code>string \| null</code> | `'archive'` | URL route for the archive section of your blog. Will be appended to `routeBasePath`. **DO NOT** include a trailing slash. Use `null` to disable generation of archive. |
| `include` | `string[]` | `['**/*.{md,mdx}']` | Array of glob patterns matching Markdown files to be built, relative to the content path. |
| `exclude` | `string[]` | _See example configuration_ | Array of glob patterns matching Markdown files to be excluded. Serves as refinement based on the `include` option. |
| `postsPerPage` | <code>number \| 'ALL'</code> | `10` | Number of posts to show per page in the listing page. Use `'ALL'` to display all posts on one listing page. |
| `blogListComponent` | `string` | `'@theme/BlogListPage'` | Root component of the blog listing page. |
| `blogPostComponent` | `string` | `'@theme/BlogPostPage'` | Root component of each blog post page. |
| `blogTagsListComponent` | `string` | `'@theme/BlogTagsListPage'` | Root component of the tags list page. |
| `blogTagsPostsComponent` | `string` | `'@theme/BlogTagsPostsPage'` | Root component of the "posts containing tag" page. |
| `blogArchiveComponent` | `string` | `'@theme/BlogArchivePage'` | Root component of the blog archive page. |
| `remarkPlugins` | `any[]` | `[]` | Remark plugins passed to MDX. |
| `rehypePlugins` | `any[]` | `[]` | Rehype plugins passed to MDX. |
| `beforeDefaultRemarkPlugins` | `any[]` | `[]` | Custom Remark plugins passed to MDX before the default Docusaurus Remark plugins. |
| `beforeDefaultRehypePlugins` | `any[]` | `[]` | Custom Rehype plugins passed to MDX before the default Docusaurus Rehype plugins. |
| `truncateMarker` | `RegExp` | `/<!--\s*(truncate)\s*-->/` | Truncate marker marking where the summary ends. |
| `showReadingTime` | `boolean` | `true` | Show estimated reading time for the blog post. |
| `readingTime` | `ReadingTimeFunctionOption` | The default reading time | A callback to customize the reading time number displayed. |
| `authorsMapPath` | `string` | `'authors.yml'` | Path to the authors map file, relative to the blog content directory. |
| `feedOptions` | _See below_ | `{type: ['rss', 'atom']}` | Blog feed. |
| `feedOptions.type` | <code>FeedType \| FeedType[] \| 'all' \| null</code> | **Required** | Type of feed to be generated. Use `null` to disable generation. |
| `feedOptions.title` | `string` | `siteConfig.title` | Title of the feed. |
| `feedOptions.description` | `string` | <code>\`${siteConfig.title} Blog\`</code> | Description of the feed. |
| `feedOptions.copyright` | `string` | `undefined` | Copyright message. |
| `feedOptions.language` | `string` (See [documentation](http://www.w3.org/TR/REC-html40/struct/dirlang.html#langcodes) for possible values) | `undefined` | Language metadata of the feed. |
| `sortPosts` | <code>'descending' \| 'ascending' </code> | `'descending'` | Governs the direction of blog post sorting. |

</APITable>

```ts
type EditUrlFunction = (params: {
  blogDirPath: string;
  blogPath: string;
  permalink: string;
  locale: string;
}) => string | undefined;

type ReadingTimeOptions = {
  wordsPerMinute: number;
  wordBound: (char: string) => boolean;
};

type ReadingTimeFunction = (params: {
  content: string;
  frontMatter?: BlogPostFrontMatter & Record<string, unknown>;
  options?: ReadingTimeOptions;
}) => number;

type ReadingTimeFunctionOption = (params: {
  content: string;
  frontMatter: BlogPostFrontMatter & Record<string, unknown>;
  defaultReadingTime: ReadingTimeFunction;
}) => number | undefined;

type FeedType = 'rss' | 'atom' | 'json';
```

### Example configuration {#ex-config}

You can configure this plugin through preset options or plugin options.

:::tip

Most Docusaurus users configure this plugin through the preset options.

:::

```js config-tabs
// Preset Options: blog
// Plugin Options: @docusaurus/plugin-content-blog

const config = {
  path: 'blog',
  // Simple use-case: string editUrl
  // editUrl: 'https://github.com/facebook/docusaurus/edit/main/website/',
  // Advanced use-case: functional editUrl
  editUrl: ({locale, blogDirPath, blogPath, permalink}) =>
    `https://github.com/facebook/docusaurus/edit/main/website/${blogDirPath}/${blogPath}`,
  editLocalizedFiles: false,
  blogTitle: 'Blog title',
  blogDescription: 'Blog',
  blogSidebarCount: 5,
  blogSidebarTitle: 'All our posts',
  routeBasePath: 'blog',
  include: ['**/*.{md,mdx}'],
  exclude: [
    '**/_*.{js,jsx,ts,tsx,md,mdx}',
    '**/_*/**',
    '**/*.test.{js,jsx,ts,tsx}',
    '**/__tests__/**',
  ],
  postsPerPage: 10,
  blogListComponent: '@theme/BlogListPage',
  blogPostComponent: '@theme/BlogPostPage',
  blogTagsListComponent: '@theme/BlogTagsListPage',
  blogTagsPostsComponent: '@theme/BlogTagsPostsPage',
  remarkPlugins: [require('remark-math')],
  rehypePlugins: [],
  beforeDefaultRemarkPlugins: [],
  beforeDefaultRehypePlugins: [],
  truncateMarker: /<!--\s*(truncate)\s*-->/,
  showReadingTime: true,
  feedOptions: {
    type: '',
    title: '',
    description: '',
    copyright: '',
    language: undefined,
  },
};
```

## Markdown front matter {#markdown-front-matter}

Markdown documents can use the following Markdown front matter metadata fields, enclosed by a line `---` on either side.

Accepted fields:

<APITable>

| Name | Type | Default | Description |
| --- | --- | --- | --- |
| `authors` | `Authors` | `undefined` | List of blog post authors (or unique author). Read the [`authors` guide](../../blog.mdx#blog-post-authors) for more explanations. Prefer `authors` over the `author_*` front matter fields, even for single author blog posts. |
| `author` | `string` | `undefined` | ⚠️ Prefer using `authors`. The blog post author's name. |
| `author_url` | `string` | `undefined` | ⚠️ Prefer using `authors`. The URL that the author's name will be linked to. This could be a GitHub, Twitter, Facebook profile URL, etc. |
| `author_image_url` | `string` | `undefined` | ⚠️ Prefer using `authors`. The URL to the author's thumbnail image. |
| `author_title` | `string` | `undefined` | ⚠️ Prefer using `authors`. A description of the author. |
| `title` | `string` | Markdown title | The blog post title. |
| `date` | `string` | File name or file creation time | The blog post creation date. If not specified, this can be extracted from the file or folder name, e.g, `2021-04-15-blog-post.mdx`, `2021-04-15-blog-post/index.mdx`, `2021/04/15/blog-post.mdx`. Otherwise, it is the Markdown file creation time. |
| `tags` | `Tag[]` | `undefined` | A list of strings or objects of two string fields `label` and `permalink` to tag to your post. |
| `draft` | `boolean` | `false` | A boolean flag to indicate that the blog post is work-in-progress and therefore should not be published yet. However, draft blog posts will be displayed during development. |
| `hide_table_of_contents` | `boolean` | `false` | Whether to hide the table of contents to the right. |
| `toc_min_heading_level` | `number` | `2` | The minimum heading level shown in the table of contents. Must be between 2 and 6 and lower or equal to the max value. |
| `toc_max_heading_level` | `number` | `3` | The max heading level shown in the table of contents. Must be between 2 and 6. |
| `keywords` | `string[]` | `undefined` | Keywords meta tag, which will become the `<meta name="keywords" content="keyword1,keyword2,..."/>` in `<head>`, used by search engines. |
| `description` | `string` | The first line of Markdown content | The description of your document, which will become the `<meta name="description" content="..."/>` and `<meta property="og:description" content="..."/>` in `<head>`, used by search engines. |
| `image` | `string` | `undefined` | Cover or thumbnail image that will be used when displaying the link to your post. |
| `slug` | `string` | File path | Allows to customize the blog post url (`/<routeBasePath>/<slug>`). Support multiple patterns: `slug: my-blog-post`, `slug: /my/path/to/blog/post`, slug: `/`. |

</APITable>

```ts
type Tag = string | {label: string; permalink: string};

// An author key references an author from the global plugin authors.yml file
type AuthorKey = string;

type Author = {
  key?: AuthorKey;
  name: string;
  title?: string;
  url?: string;
  image_url?: string;
};

// The front matter authors field allows various possible shapes
type Authors = AuthorKey | Author | (AuthorKey | Author)[];
```

Example:

```md
---
title: Welcome Docusaurus v2
authors:
  - slorber
  - yangshun
  - name: Joel Marcey
    title: Co-creator of Docusaurus 1
    url: https://github.com/JoelMarcey
    image_url: https://github.com/JoelMarcey.png
tags: [hello, docusaurus-v2]
description: This is my first post on Docusaurus 2.
image: https://i.imgur.com/mErPwqL.png
hide_table_of_contents: false
---

A Markdown blog post
```

## i18n {#i18n}

Read the [i18n introduction](../../i18n/i18n-introduction.md) first.

### Translation files location {#translation-files-location}

- **Base path**: `website/i18n/[locale]/docusaurus-plugin-content-blog`
- **Multi-instance path**: `website/i18n/[locale]/docusaurus-plugin-content-blog-[pluginId]`
- **JSON files**: extracted with [`docusaurus write-translations`](../../cli.md#docusaurus-write-translations-sitedir)
- **Markdown files**: `website/i18n/[locale]/docusaurus-plugin-content-blog`

### Example file-system structure {#example-file-system-structure}

```bash
website/i18n/[locale]/docusaurus-plugin-content-blog
│
│ # translations for website/blog
├── authors.yml
├── first-blog-post.md
├── second-blog-post.md
│
│ # translations for the plugin options that will be rendered
└── options.json
```
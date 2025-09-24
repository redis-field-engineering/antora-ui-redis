
<p align="center">
  <a href="https://redis.io/">
    <img alt="Redis" src="https://redis.io/wp-content/uploads/2024/04/Logotype.svg" width="250" />
  </a>
</p>


This project generates and packages the static resources that Redis uses for document production.

This project is based on [Antora](https://antora.org).


## Development Quickstart

This section offers a basic tutorial to teach you how to preview it locally, and bundle it for use with Antora.
A more comprehensive tutorial can be found in the documentation at [docs.antora.org](https://docs.antora.org/).

### Prerequisites

To preview and bundle the Antora Redis UI, you need the following software on your computer:

* [git](https://git-scm.com/) (command: `git`)
* [Node.js](https://nodejs.org/) (commands: `node` and `npm`)
* [Gulp CLI](http://gulpjs.com/) (command: `gulp`)

### Preview the UI

The Redis Antora UI project is configured to preview offline.
The files in the `preview-src/` folder provide the sample content that allow you to see the UI in action.
In this folder, you'll primarily find pages written in AsciiDoc.
These pages provide a representative sample and kitchen sink of content from the real site.

To build the UI and preview it in a local web server, run the `preview` command:

```
npm install
gulp preview
```

You'll see a URL listed in the output of this command:

```
[12:00:00] Starting server...
[12:00:00] Server started http://localhost:5252
[12:00:00] Running server
```

Navigate to this URL to preview the site locally.

While this command is running, any changes you make to the source files will be instantly reflected in the browser.
This works by monitoring the project for changes, running the `preview:build` task if a change is detected, and sending the updates to the browser.

Press `Ctrl`+`C` to stop the preview server and end the continuous build.

### Package for Use with Antora

If you need to package the UI so you can use it to generate the documentation site locally, run the following command:

```
gulp bundle
```

If any errors are reported by lint, you'll need to fix them.

When the command completes successfully, the UI bundle will be available at `build/ui-bundle.zip`.
You can point Antora at this bundle using the `--ui-bundle-url` command-line option.

If you have the preview running, and you want to bundle without causing the preview to be clobbered, use:

```
gulp bundle:pack
```

The UI bundle will again be available at `build/ui-bundle.zip`.

## Configuration Options

The Redis UI bundle supports configuration through `site.keys` in your `antora-playbook.yml`:

### GitHub Project Link
```yaml
site:
  keys:
    github: https://github.com/your-org/your-project
```

### Edit Page Functionality

**To disable "Edit this Page" links (default behavior):**
```yaml
site:
  keys:
    github: https://github.com/your-org/your-project
    # No enable-edit key = edit functionality disabled
```

**To enable "Edit this Page" links:**
```yaml
site:
  keys:
    github: https://github.com/your-org/your-project
    enable-edit: true
```

When `enable-edit: true` is set, the UI will show edit links based on:
- `page.editUrl` (GitHub edit links) if available
- `page.fileUri` (local file links) for development

### Complete Configuration Example

```yaml
site:
  title: Your Redis Project Documentation
  url: https://your-org.github.io/your-project
  keys:
    github: https://github.com/your-org/your-project
    enable-edit: true  # Enable edit functionality

content:
  sources:
    - url: .
      branches: [HEAD, main]
      start_path: docs
      # edit_url is auto-generated for GitHub projects
      # or set to false to disable: edit_url: false

ui:
  bundle:
    url: path/to/antora-ui-redis/build/ui-bundle.zip
    snapshot: true
```

## Project Examples

### Redis Spark Connector (Edit Disabled)
```yaml
site:
  title: Redis Connector for Spark Documentation
  keys:
    github: https://github.com/redis-field-engineering/redis-spark-dist
    # No enable-edit = edit links disabled

content:
  sources:
    - url: .
      branches: [HEAD, main]
      start_path: docs
      edit_url: false  # Explicitly disable GitHub edit URLs
```

### Redis OM Spring (Edit Enabled)
```yaml
site:
  title: Redis OM Spring Documentation
  keys:
    github: https://github.com/redis/redis-om-spring
    enable-edit: true  # Enable edit functionality

content:
  sources:
    - url: .
      branches: [HEAD, main]
      start_path: docs
      # edit_url auto-generated for GitHub edits
```

## Extensions to the UI

### Theme Toggle

The Redis UI includes a dark/light theme toggle that:

* Automatically detects system preference on first visit
* Persists user preference in localStorage
* Supports smooth transitions between themes
* Uses Redis brand colors in both themes

### Redis Branding

This UI bundle includes:

* Redis brand fonts (Space Grotesk, Space Mono, JetBrains Mono)
* Redis brand colors and dark theme
* Redis logos and favicons
* Optimized navigation and layout for Redis documentation

### Edit Page Configuration

The UI bundle supports configurable "Edit this Page" functionality:

* **Default**: Edit links are disabled for security and consistency
* **Opt-in**: Projects can enable edit links via `site.keys.enable-edit: true`
* **Flexible**: Supports both GitHub edit URLs and local file paths
* **Generic**: Works across all Redis projects with appropriate configuration

## Authors

Development of Antora is led and sponsored by [OpenDevise Inc](https://opendevise.com/).

## Copyright and License

Copyright (C) 2017-present OpenDevise Inc. and the Antora Project.

Use of this software is granted under the terms of the [Mozilla Public License Version 2.0](https://www.mozilla.org/en-US/MPL/2.0/) (MPL-2.0).
See [LICENSE](LICENSE) to find the full license text.

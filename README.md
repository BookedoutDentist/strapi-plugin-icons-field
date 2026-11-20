
<div align="center" width="150px">
  <img style="height: auto;" src="https://res.cloudinary.com/djqnlydhz/image/upload/v1775143826/github/logo_mhoegm.png" alt="Logo - Strapi Plugin Icon Field" />
  <h1>Strapi Icons Custom Field </h1>
  <p>
    A customizable plugin for Strapi to integrate an icon field into your content types. This plugin allows users to easily select icons for different fields, improving the flexibility and user experience of your Strapi-based CMS.
  </p>
  <p>
   <a href="https://strapi.io">
      <img src="https://img.shields.io/badge/strapi-v5-blue" alt="Strapi supported version" />
    </a>
  </p>
</div>

> **BookedoutDentist fork** of [dupflo/strapi-plugin-icons-field](https://github.com/dupflo/strapi-plugin-icons-field).
> Adds [search synonyms](#search-synonyms-) via a `_synonyms.json` mapping file.
> `main` contains our changes; releases are tags like `v1.2.5-bod.1`. See [Development & release (BookedoutDentist)](#development--release-bookedoutdentist).

## Development & release (BookedoutDentist)

Strapi (`bod-ads-lp-za-strapi`) installs this plugin directly from a Git tag:

```json
"strapi-plugin-icons-field": "github:BookedoutDentist/strapi-plugin-icons-field#v1.2.5-bod.1"
```

**`dist/` is committed and is not built in Strapi.** With the tag, Strapi gets the ready-built code.
So whoever changes the source in `admin/src` or `server/src` must build it before committing.

1. Once: `npm ci`
2. Change the code
3. Build and check:
   ```bash
   npm run build
   npx tsc -p server/tsconfig.json --noEmit && npx tsc -p admin/tsconfig.json --noEmit
   ```
4. Bump the version (scheme `<upstream-version>-bod.<n>`) and commit the source **and** `dist/`:
   ```bash
   npm version 1.2.5-bod.2 --no-git-tag-version
   git add -A && git commit -m "feat: ..."
   ```
5. Tag and push:
   ```bash
   git tag v1.2.5-bod.2 && git push origin main --tags
   ```
6. In `bod-ads-lp-za-strapi`, update the tag in `package.json`, run `npm install` and commit `package.json` + `package-lock.json`.

Without a new tag and step 6, a change never reaches Strapi. Never move an existing tag; always create a new one.

**Testing without a release:** `npm pack` creates a `.tgz` that can be installed in Strapi with
`npm install --no-save /path/to/strapi-plugin-icons-field-<version>.tgz`. Then delete `node_modules/.strapi/vite`
and restart Strapi. A regular `npm install` restores the version from `package.json`.

**Pulling in upstream updates:**

```bash
git remote add upstream https://github.com/dupflo/strapi-plugin-icons-field.git   # once
git fetch upstream && git merge upstream/main
```

Then rebuild and release a new `-bod.<n>` tag as described above.

![Plugin Icon Field](https://res.cloudinary.com/djqnlydhz/image/upload/v1775143825/github/ct-plugin-icons-field_c4zuoi.webp)

## Features 🎨

- **Icon Picker**: Adds an icon field to your content types, enabling users to select from a variety of preloaded icons.
- **Search**: Filter icons by name and by synonyms directly in the picker modal.
- **Flexible output**: Choose between returning the full SVG code or just the icon name — ideal for SVG sprite workflows.
- **Customizable**: You can add any svg icons code directly from the `public` folder inside a custom subfolder (eg: `public/icons`), making this plugin adaptable to any design or theme.
- **Pass SVG code directly into your API**: No additional package needed on your frontend. You can directly rendered the plugin with a [custom component](#render-svg-icons-in-react) custom component on your frontend.
- **Dark mode**: The icon picker is fully compatible with Strapi's dark mode.
- **User-friendly**: Simple, intuitive field for content managers to pick and manage icons.

## Video Demo 🎬

![Demo](https://res.cloudinary.com/djqnlydhz/image/upload/v1775137409/github/strapi-plugin-icons-field_rjufkc.gif)

## Installation 🛠️

### 1. Install the package

```bash
npm install strapi-plugin-icons-field
```

> No additional configuration in `config/plugins.ts` is required. The plugin is automatically registered once the package is installed.

### 2. Add your icons to public folder

Create an `icons` folder (or any name you prefer) inside the `/public` directory and add your SVG icons there. You can also organize and group your icons into subfolders, for example: `/icons/marketing`.

<img src="https://res.cloudinary.com/djqnlydhz/image/upload/v1775143826/github/cf-plugin-public-folder_afnru2.webp" alt="Screenshot from public folder for Strapi icons field plugin" style="max-width: 400px; width: 100%; height: auto;" />

The system will directly scan the specified folder and its subfolders to make all SVG icons available for selection in the plugin.

### 3. Rebuild Strapi

After installing the plugin, rebuild your Strapi instance:

```bash
npm run build
npm run develop
```

or with yarn:

```bash
yarn build
yarn develop
```

## Add the Icon Field to your Collection Type 📋

Once the plugin is installed, you will be able to add an icon field to any collection type.

1. Go to **Content-Types Builder** in the Strapi Admin Panel.
2. Select the collection type where you want to add the icon field.
3. Choose **Icon** from the available field types.
4. Configure the field as needed (e.g., allowing custom subfolder icons selection or full list by default).
5. Save and deploy your collection type.


### Select a specific icons list ⚙️

If you don't want to display all your icons in the list, you can apply a custom selection by subfolder by entering their subfolder `name` attribute — one folder name per line.

You can enter **multiple folder names** to combine icons from different subfolders.

This allows your team to leverage icons that match your project or use cases.

![Plugin Basic Settings](https://res.cloudinary.com/djqnlydhz/image/upload/v1775143825/github/ct-plugin-basic-settings_ioabjt.webp)

### Advanced Settings ⚙️

![Advanced settings - API output format and show icon label](https://res.cloudinary.com/djqnlydhz/image/upload/v1775143826/github/cf-advanced-settings_ibuz5s.webp)

| Option | Type | Description |
| --- | --- | --- |
| **Required field** | Checkbox | Makes the field mandatory — the entry cannot be saved without selecting an icon |
| **Show icon label** | Checkbox | Displays the icon filename below each icon in the picker modal |
| **API output format** | Select | Chooses what value gets stored in the API (see below) |

#### Show icon label

When enabled, the icon's filename is displayed below each icon in the picker modal. Useful when icons are not self-explanatory by shape alone.

#### API output format

Choose what value gets stored in the API:

| Format | Stored value | Use case |
| --- | --- | --- |
| **SVG code (HTML)** *(default)* | Full SVG string | Render the icon directly without any extra dependency |
| **Icon name** | Icon filename without extension | Reference icons in an SVG sprite (`<use href="#icon-name" />`) |

A typical use case is when using an SVG sprite system on your frontend. The SVG code would be duplicated (once in the sprite, once via the API), but it allows the frontend to reference the icon in the sprite by its label:

```html
<svg><use href="#Ambassador" /></svg>
```

See this [sandbox example](https://codesandbox.io/p/sandbox/svg-workflow-xf7p8j?file=%2Fsrc%2Fcomponents%2FSvg.componet.tsx) for a practical implementation.

### Search synonyms 🔎

File names are often in one language (e.g. `night-splint`) while editors search in another (e.g. "Knirscherschiene").
Put a `_synonyms.json` next to your icons to make them findable by additional terms:

```json
{
  "_comment": "Keys starting with _ are ignored",
  "night-splint": {
    "de": ["Knirscherschiene", "Aufbissschiene", "Bruxismus"],
    "en": ["night guard", "bite splint", "teeth grinding"]
  },
  "x-ray": ["Röntgen", "DVT"]
}
```

A value can be a list of terms or an object grouping lists by language (any keys; all terms are searched).

- **`public/icons/_synonyms.json`** applies to every icon folder (useful when several sets share the same file names).
- **`public/icons/<folder>/_synonyms.json`** adds terms for that folder only; both lists are merged.
- The search is case-insensitive, ignores umlauts/accents (`rontgen` finds `Röntgen`) and treats dashes as spaces.
  With several words, every word must match the name or one of the synonyms.
- Hovering an icon in the picker shows its synonyms.
- A missing or invalid file never breaks the picker; it only logs a warning.

The file name can be changed in `config/plugins.js`:

```js
'icons-field': {
  enabled: true,
  config: { synonymsFile: '_synonyms.json' },
},
```

---

## Assign an icon to your document

Once the icon field is added to your collection type, you can select an icon from a predefined set or add your own. The icons will be displayed on the content manager interface and can be used for various purposes like UI customization, categorization, and more!

![Plugin Icon Field](https://res.cloudinary.com/djqnlydhz/image/upload/v1775143826/github/cf-plugin-modal_gketqs.webp)

## Usage in React.js ⚛️

You can easily render the SVG icons from this plugin in your React components using the `Icon` component. This component allows you to pass in the raw SVG code (as a string) and render it directly in your React app.

### Install the `html-react-parser` Dependency

First, ensure you have `html-react-parser` installed, as it's used to parse the SVG code.

```bash
npm install html-react-parser
```

or with yarn:

```bash
yarn add html-react-parser
```

### Render SVG Icons in React

Use the `Icon` component to render SVG icons. Here’s an example:

#### Example Code

```tsx
import React from 'react';
import Icon from '../components/Icon';  // Import your Icon component

export default function Page () {
  const iconSvgCode = `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="24" height="24"><path d="M12 2L2 7l3 11h14L22 7z"/></svg>`; // Example SVG icon code, you should replace with your Strapi API Call
  
  return (
    <div>
      <h1>Render SVG Icon</h1>
      {/* Render the SVG using the Icon component */}
      <Icon icon={iconSvgCode} className="my-icon" />
    </div>
  );
};
```

The homemade component

```tsx
import React, { HTMLAttributes }  from 'react';
import parse from 'html-react-parser';

interface RenderSvgProps extends HTMLAttributes<SVGElement> {
  icon: string;
}

export default function Icon({ icon, ...props }: RenderSvgProps) {
  const parsedElement = icon && parse(icon.trim());
  if (parsedElement && React.isValidElement(parsedElement)) {
    return React.cloneElement(parsedElement, props);
  }
  return null;
}
```

#### Explanation

- **`icon` Prop**: The `icon` prop takes the raw SVG code as a string. You can get this string either from your Strapi CMS or any other source.
- **`props`**: You can pass additional props like `className`, `style`, or `onClick` directly to customize the rendered SVG element.
- **`parse`**: The `parse` function from the `html-react-parser` library is used to convert the SVG code into React elements.

### Customize the SVG

You can also customize the SVG attributes (like `width`, `height`, or `fill`) by passing them as props to the `Icon` component.

#### Example with Custom Props

```tsx
<Icon 
  icon={iconSvgCode} 
  className="custom-icon" 
  width="48" 
  height="48" 
  fill="blue"
/>
```

In this example, the SVG will be rendered with a custom size (`48x48`) and a blue color fill.

---

## Contributing 🤝

We welcome contributions! If you'd like to contribute to this project, please fork the repository and create a pull request with your changes.

Before submitting, make sure to:

- Follow the existing code style and conventions.
- Write clear and concise commit messages.
- Ensure tests pass (if applicable).

## License 📜

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## Support 🆘

If you encounter any issues or have questions, feel free to create an issue on GitHub, and we will get back to you as soon as possible!

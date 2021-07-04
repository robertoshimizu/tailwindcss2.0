<h1 align="center">
	<a 
    href="https://tailwindcss.com/">
    <img
		width="400"
		alt="Tailwind logo"
		src="https://miro.medium.com/max/700/0*nGTqAcuGJb118YAO.png">
    </a>        
</h1>

# TailwindCSS 2.0

### The most basic and quick way to start using tailwind is via CDN

```html
<link href="https://unpkg.com/tailwindcss@^2/dist/tailwind.min.css" rel="stylesheet">
```

Just add it in your `index.html`:
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <link href="https://unpkg.com/tailwindcss@^2/dist/tailwind.min.css" rel="stylesheet">
</head>
<body>
    <h1 class="text-4xl font-bold text-center text-blue-500">Hello World</h1>
</body>
</html>
```

## Tailwind is a PostCSS Plugin

PostCSS is a tool for transforming CSS with JavaScript plugins. It provides features via its extensive plugin ecosystem to help improve your CSS writing experience. You can pick the plugins you need or even write a custom one for yourself. Tailwindcss and autoprefixer are PostCSS plugins, and they should be added to your PostCSS configuration. Check them in your `postcss.config.js` file.
This is a reason why postcss-cli and autoprefixer are installed together with tailwindcss. Check them in the `package.json` file.

Together with autoprefixer, the build will replace all css custom markers with tailwindcss generated code, which are all tailwindcss base styles, components and utility classes. Check the file  `public/build/taiwind.css` as a result of `build-tailwind` script in `package.json`. You should check for that in your Starter project. Now it is just link the stylesheet in the `index.html` and voilá! Below you find a complete setup workflow.

## Setup

- **Create a project folder** Move inside the project folder and initiate the project using npm or yarn.
```sh
cd <project folder>
yarn init
```
- **Install tailwindcss, postcss-cli and autoprefixer.** Basic components to start with.
```sh
yarn add tailwindcss postcss-cli autoprefixer
```
- **Create tailwind.config.js.** It creates an empty config file, that will be used to customize tailwind it later on.
```sh
npx tailwind init
```
- **Create postcss.config.js.** This file specifies plugins to be used in postcss. Create the file `postcss.config.js` at the root of the project and add:
```javascript
module.exports = {
    plugins:[
        require('tailwindcss'),
        require('autoprefixer'),
    ]
}
```
- **Create the tailwind.css file.** Create `tailwind.css` in the css folder.
```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```
- **Build the tailwind.css.** Generate a `tailwind.css` file processed by postcss.
```sh
yarn run postcss css/tailwind.css -o public/build/tailwind.css
```
You can follow above instructions watching [tailwind setup video](https://tailwindcss.com/course/setting-up-tailwind-and-postcss).


## Linking to html

In the public folder, create a new `index.html` file. Scaffold a html template. Link to the `/build/tailwind.css` and add a ``Hello World`` h1 tag, and style it using tailwind utility classes.
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <link rel="stylesheet" href="/build/tailwind.css">
</head>
<body>
    <h1 class="text-4xl font-bold text-center text-blue-500">Hello World! </h1>
</body>
</html>
```
Spin a *``Live Server``* and check it out!

## Integrating with Vuejs

Note that from branch-4 onwards, the project was converted to Vue, therefore the structure changed and the above instructions does not apply anymore.
To create a Vue project, use Vue CLI.
```sh
# install vue CLI
yarn global add @vue/cli
# create a Vue project in the current directory
vue create .
```
Make the necessary changes in Vue files and update paths in the scripts in package.json.

## Tools to streamline Tailwind code in VS Code

- [Sizzy](https://adamwathan.me/sizzy) for the browser preview on the right-hand size.
- [Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) to spin a webserver and render your html files live. Alternative and easier than ``Sizzy``.
- [Tailwind CSS Intellisense](https://marketplace.visualstudio.com/items?itemName=bradlc.vscode-tailwindcss) for intelligent auto-completion.

## Optimize for production

Use [Purgecss](https://purgecss.com/) to remove unused classes from production builds. In fact, you don't need to carry the full css file into production, it makes sense to carry only those classes that you actually are using for your project. That reduces significantly the size of the build files.
```sh
yarn add @fullhuman/postcss-purgecss
```
Modify the `postcss.config.js` to run purgecss only when your deploy it to production.
```javascript
module.exports = {
    plugins:[
        require('tailwindcss'),
        require('autoprefixer'),
        process.env.NODE_ENV === 'production' && require('@fullhuman/postcss-purgecss')({
            content:[
                './src/**/*.vue',
                './public/index.html',
            ],
            defaultExtractor: content => content.match(/[A-Za-z0-9-_:/]+/g) || []
        })
    ]
}
```
### References:
- [Designing with Tailwind CSS](https://tailwindcss.com/course): [GETTING UP AND RUNNING](https://tailwindcss.com/course/setting-up-tailwind-and-postcss)
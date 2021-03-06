# Install a new Frontity package

During the development of your project, you may want to install new Frontity packages, or even change the ones you are using \(for example if you want to use a different theme or a different analytics service\). In order to do this, you'll need to:

1. Install the Frontity package.
2. Add it to `frontity.settings.js`.

{% hint style="info" %}
Please note that this process is only necessary for Frontity packages. If you want to install a npm package you can use the normal `npm install some-package` procedure.
{% endhint %}

## 1. Install the package

At this point, you need to differentiate between the external packages \(which aren't meant to be modified\) and the local packages \(the ones you want to change at your will\), because its installation will be slightly different. If you want to understand better how external and local packages work you can check its [docs page](../learning-frontity/packages.md).

### External packages

These packages will be treated like any `npm` package and stored in `node_modules` folder, where all your dependencies are installed. In order to install a package as external you just have to run this command in the root of your project:

```text
npm install new-frontity-package
```

It will be automatically added to your `node_modules` folder and your `package.json` file.

### Local packages

The process to install a local package is pretty similar, but you'll have to make minor modifications.

1. You need to install the package as an external one by running `npm install new-frontity-package` 
2. It will be installed inside the `node_modules` folder of your project, so you'll need to look for the package and move it to the folder `packages` inside your Frontity project.
3. Next step would be to change your `package.json` . You'll have your new package inside the dependencies of your project, pointing to its latest version. As we are going to use it as a `local` package, we have to point it to its proper folder.

```javascript
{
  "name": "frontity-project",
  ...
  "dependencies": {
    ...
    "new-frontity-package": "./packages/new-frontity-package",
    //before it would be something like "new-frontity-package": "^1.0.0"
    ...
  }
}
```

4. Once changed, run `npm install` at the root of your project and it will work as `local` package.

{% hint style="info" %}
We are planning to release a new command, that will take care of all these steps.
{% endhint %}

## 2. Add it to \`frontity.settings.js\`

Once installed, the process it's the same for both external and local packages.

You have to include it in your `frontity.settings.js` , inside the `packages` array to  make it work. Remember to check if any settings are needed and include them as well \(You can check that at each specific [Frontity Package](../api-reference-1/) or [Frontity Theme](../frontity-themes/)\).

```javascript
const settings = {
  ...
  "packages": [
    ...
    //If the package has no settings
    "new-frontity-package",
    
    //If you want to add settings to your package
    {
      "name": "new-frontity-package",
      //Add your settings here
    },
    ...  
  ]
};

export default settings;
```

And that's all, your package is installed and working now.

{% hint style="info" %}
Note that if you are changing your theme or any other package, you may want to remove the old one in your `package.json` and in your `frontity.settings.js`
{% endhint %}


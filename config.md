# Foundry Config

Instantly get the ideal bundler configuration for your JavaScript packages. Given a list of packages from [npm](http://npmjs.com), both [Rollup](http://rollupjs.org) and [Webpack](https://webpack.js.org) configuration files are generated based on verified configurations. Foundry Config runs at [https://config.foundry.sh/](https://config.foundry.sh/)

## Usage

Start by searching for a npm package. Click on it to add it to your packages. Both Rollup and Webpack configuration files are automatically generated. Green packages are verified to be working on both Rollup and Webpack. For other packages, a best effort is made to generate a compatible configuration.

Once all of your packages have been added, click **Download** to clone a git repository with your configuration. The repository link expires after 1 day.

## Submitting New Configurations

If you find a package without a verified configuration, you can submit your configuration to improve Foundry Config. If you are familiar with the package and want to submit a minimal working config for Rollup and Webpack, upload them to a [GitHub Gist](https://gist.github.com/). Then, click on the package and submit both links.


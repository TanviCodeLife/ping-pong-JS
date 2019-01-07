
### Setting package.json file
1. Navigate to the root directory of project folder and type npm init -y in the command line. Creates a file called package.json in the project's root directory.
2. Install webpack package as a development dependency $ npm install webpack@4.0.1 --save-dev
3. Install a package to have access to the CLI (command line interface) for webpack.
$ npm install webpack-cli@2.0.9 --save-dev
4. We now have a new field: devDependencies. All of the dependencies we install with the --save-dev flag will be included in devDependencies.
5. You can also manually add a dependency to package.json by typing or copying and pasting it directly into the file and then running $ npm install
6. To remove dependencies, you should first remove the reference to the dependency you aren't using from package.json and then run the command $ npm prune.
7. To ensure the package will be installed for both development and production. $ npm install PACKAGE-NAME-HERE --save

Git practices
1. Commit specific files - $ git add ready-to-commit.js
2. $ git add file1 file2 file3
3. To verify you're committing the right code $ git diff ready-to-commit.js
4. Dont track API keys, npm libraries, node_modules/, .DS_Store using git($git commit command initiates tracking)
5. If already tracking a file that should not be tracked -
- first make sure to add that file to .gitignore.
- run $ git rm --cached [FILE-NAME], where [FILE-NAME] is the name of the file that shouldn't be tracked.

##webpack
webpack is a module bundler that bundles our code.
Webpack uses a dependency graph to recursively manage an application's assets.
Webpack uses entry point for our application called main.js where we include our interface logic. Webpack will recursively load and concatenate all the code from main.js as well as any required code from other files. This code would all be gathered into a single file with a name such as main.bundle.js. Webpack will load not just JavaScript files but also other assets such as CSS files and images as long as we have the right loaders and plugins.



##Configuring webpack.config.js

1. On the top level of the  project directory, lets create the webpack configuration file called webpack.config.js.
2. const path = require('path'); is added to configure the path where our output files will be saved.
2. We specify an entry point for webpack to enter recursively load and concatenate all the code.
3. We specify an output for webpack to save them in a file called bundle.js in a folder called dist.
4. All of our configuration is stored inside of module.export so that other files will be able to import this module.


##Setting up the project
1. All our JavaScript source code will go inside src while all of our bundled code will go inside dist (short for distribution). index.html should be inside our dist folder.
2. Add script tag for bundle.js in the index.html.
3. Webpack will bundle the src JS files into bundle.js
4. Add export to the function in the JS file and  makes it available for importing into other JS files.
5. Add import statement in the main.js entry point that consists of a few key things: what we are importing from functionName (pingPong) and where we are importing it from logic.js.
`import { functionName } from './logic.js';`

##Bundling code
1. $ npm run build in the root directory of the project. (Webpack will access the entry point at src/main.js. Then webpack will recursively add any dependencies (anything that needs to be imported from elsewhere). This project only has one dependency: ping-pong.js.)
2. Check the dist folder to find  bundle.js. The file includes all of the concatenated source code plus some code that webpack has added.
3. Make sure to add dist/ to the .gitignore to not track this directory in git.

##Bundling CSS
1. Add two new dependencies
$ npm install style-loader@0.20.2 css-loader@0.28.10 --save-dev.
2. In package.json file, two more dev dependencies style-loader and css-loader have been added
3. In webbapck.config.js, the test line tells webpack where to look for files that use the specified loaders. Note that the test line uses a regular expression to find files with a .css extension.
4. We need to import our css into our main.js file using `import './styles.css';`

##Processing html
1. In case of a bigger application, multiple HTML files might be present, all with multiple different entry points. If the entry point or output  configuration changes in webpack.config.js. In that case, the HTML file's script tags would need to be updated as well.
2. To be able only update the configuration file in case of changes to project, we'll use our first plugin: HtmlWebpackPlugin.
3. Move index.html file from the dist folder to the src folder.
4. Add dev dependency into package.json: $ npm install html-webpack-plugin@3.0.6 --save-dev and update webpack.config.js with plugins field.
5. Remove the <script> tag that links the bundled JS file from index.html. We no longer need it because webpack now will add it for us (using this inject: 'body' in webpack.config.js).
6. Now run $ npm run build. Webpack will add index.html to our dist folder.

##To run this project:
1. Git clone this project from Github
2. Cd into the project directory.
3. Run $ npm install to download necessary dependencies.
4. Run $ npm run build to create their own dist folder with a bundle.js file.

##Cleaning Up:
1. As more assets get added, dist folder will get cluttered. Webpack can take care of that.
2. $ npm install clean-webpack-plugin@0.1.18 --save-dev add the plugin to the webpack.config.js using Adding Plugin steps.
3. When we run $ npm run build, webpack will automatically clean out the contents of our dist folder before creating new bundle files.

##Adding Plugins:
1. First we require the new plugin and save it in a variable.
2. Then we add it to module.exports in the plugins section.
3. We create a new instance of the plugin

##Minifying Code:
1. To minify, or uglify, our code. First, install the UglifyJS Webpack Plugin.
2. Then require the plugin at the top of webpack.config.js
3. Next, add it to the list of plugins in the module.exports section of webpack.config.js.

##Webpack Development Server and Source Maps fro Debugging:
1. It helps to automatically rebundle and reloade the code when changes are made to the code.
2. With source maps, we can trace the error back to our code, making it easier to debug.
3. Add functionality for both the dev server and source maps using $ npm install webpack-dev-server@3.1.0 --save-dev
4. Update webpack.config.js with 'eval-source-map' as a dev tool and devServer.
5. For this to work, we need to update our configuration for UglifyJsPlugin with new UglifyJsPlugin({ sourceMap: true })
6. Then update the package.json with "start": "webpack-dev-server --open" in the scripts section to be able to run the development server at http://localhost:8080/.
7. Now run $ npm run start in the command line and this will start up the server.

##Dev and Production Modes
1. "scripts": {
    "build": "webpack --mode development",
    "start": "npm run build; webpack-dev-server --open --mode development"
  },

2. The command $ npm run start will first now build the code (create a bundle and a dist folder) before starting the develoment server.
3. --mode development sets which environment we are building our code for: development in this case.

##ESLint
1. ESlint (a popular JavaScript linter), and eslint-loader that allows us to use the linter with Webpack.
npm install eslint@4.18.2 --save-dev
npm install eslint-loader@2.0.0 --save-dev
2. In webpack.config.js, specify that ESLint should lint all JavaScript files except for those in our nodemodules_ directory that are external JS libraries.
3. Add the eslint-loader to the bottom of the array of rules
4. This is to make sure that our linter is running on our original files, and not the concatted and minified build file.
Note About Loaders: The loaders are applied bottom to top, so by placing it at the bottom, we ensure that it executes first before our other loaders.

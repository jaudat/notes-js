# My Test Driven JS Project Tooling Setup for VSCode:

(Inspired by Test-Driven React book by Trevor Burnham)

## Create New Project

- Open VSCode and go to it's terminal by using hotkey **Opt-\`** or by going to the Command Pallete using command **Cmd-Shift-P** and searching for
  `View: Toggle Integrated Terminal`.
- Go to the directory we want to create the new project in, and type **mkdir _project_name_**
- If we have `code` command installed in PATH we can then run **code -r _project_name_** to open that project in vscode. The -r flag is so that we open the project in the current window of vscode and not create a new one.
  - If `code` command is not in our PATH we can place it there by going to the Command Palette using **Cmd-Shift-P** and searching for
    `Shell Command: Install 'code' command in PATH`

## Node and Git

- Initial Git Repo with command **git init**
- Initialize node with command **npm init** or **npm init -y** if we want to accept all the default values
- Add a **.gitignore** file with the following:
  ```gitignore
  node_modules/
  .vscode/
  ```

## Jest

- Install Jest with command: **npm install --save-dev jest**
  - can run this npm binary using npx with command **npx jest**, Jest will by default run all files that end with the postfix `.test.js`.
- change script test section in _package.json_ to be `"test": "jest"`. Now we can run **npm run test** or since the test script is special in npm **npm test**.

## VSCode

- Following are the User Settings for VSCode. Please NOTE, if you have already set the once, these should be there, no need to set them again. To Go to User Settings open the Command Pallete with **Cmd-Shift-P** and Search for `Preferences: Open Settings (JSON)`and add these lines:
  ```js
  // User Settings
  {
    "workbench.settings.useSplitJSON": true,
    "editor.fontFamily": "Fira Code, Menlo, Monaco, 'Courier New', monospace",
    "editor.fontLigatures": true,
    "editor.fontSize": 13,
    "editor.renderIndentGuides": true,
    "git.enableSmartCommit": true,
    "editor.minimap.enabled": false,
    "breadcrumbs.enabled": false,
    "workbench.sideBar.location": "right",
    // "editor.tabSize": 2,
  }
  ```
- Then click on the Workspace tab of the Settings Page to go to WorkSpace Settings and add the following lines, these lines are project specific and override the User Settings as well as the System Defaults:
  ```js
  // appears in .vscode/settings.json in project directory
  {
    "files.exclude": {
      "node_modules": true,
      "package-lock.json": true,
    },
    "[javascript]": {
      "editor.tabSize": 2
    }
  }
  ```

## ESLint

- Install ESLint with command **npm install --save-dev eslint**

  - If we have test and source code mixed together i.e. not in seperate folders than create an **.eslintrc.js** file in the root directory containing the following lines:

    ```js
    /*
    * Example file structure
    * Palindromes/
    *   palindromes.js
    *   palindromes.test.js
    *   .eslintrc.js
    */

    // Code at Palindromes/.eslintrc.js
    module.exports = {
      plugins: ['jest],
      extends: ['eslint:recommended', 'plugin:jest/recommended'],
      parserOptions: {
        ecmaVersion: 6,
      },
      env: {
        node: true,
      },
    }
    ```

  - If the test code is in seperate directory, we can factor out the test part into a seperate **.eslintrc.js** file and place along with the tests:

    ```js
    /*
     * Example file structure
     * Palindromes/
     *   palindromes.js
     *   .eslintrc.js
     *   tests/
     *     palindromes.test.js
     *     .eslintrc.js
     */

    // Code at Palindromes/.eslintrc.js
    module.exports = {
      extends: ["eslint:recommended"],
      parserOptions: {
        ecmaVersion: 6
      },
      env: {
        node: true
      }
    };
    ```


    // Code at Palindromes/tests/.eslintrc.js
    module.exports = {
      plugins: ['jest'],
      extends: ['plugin:jest/recommended'],
    }
    ```

- We can now run eslint using the node package runner e.g. **npx eslint palindromes.js** or **npx eslint palindromes.test.js**. Every time ESLint runs against a JavaScript file, it looks for the closest configuration. Then it continues looking in all parent directories, merging all of the configurations it finds (with closer configurations given greater precedence). So when ESLint runs against tests/palindromes.test.js, it’ll apply not only the Jest plugin’s rules but also the "eslint: recommended" rules. The parserOptions and env will carry over as well. This inheritance pattern means we only have to make configuration changes to a single file for those rules to apply project wide.

- If we want to integrate ESLint with VSCode we can use the ESLint plugin by Dirk Baeumer. Now the lint will be running continiously while we type in VSCode. Some people may however find this annoying since it will undeerline red indicating an error when we start typing and haven't completed the sentence. If we would like the ESLint to be run only on Save by VSCode we could add the following lines to The User Setting of VSCode (by opening the Command Pallete with **Cmd-Shift-P** and searching for `Preferences: Open Settings (JSON)`)
  ```js
  {
    // User Settings
    ...
    "eslint.run": "onSave",
  }
  ```

## Prettier

- Since we’re using ESLint, we want the prettier-eslint command, which ensures that Prettier formats our code in a manner that’s consistent with our ESLint config. Therefore use command **npm install --save-dev prettier-eslint-cli**
- Running prettier command on console (e.g. **npx​​ ​​prettier-eslint​​ ​​tests/palindromes.test.js​**) will emit the formatted version to the console.
- Prettier has few configuration options, however the prettier-eslint bridge gives us access to finer controls. Add the following to `.eslintrc.js` file
  ```js
  // Code at Palindromes/.eslintrc.js
  module.exports = {
    extends: ["eslint:recommended"],
    parserOptions: {
      ecmaVersion: 6
    },
    env: {
      node: true
    },
    rules: {
      // single-quotes over double-quotes except in cases where double-quotes would avoid escaping
      // e.g. 'Your right' is preferable to "Your right", but "You're right" is preferable to 'You\'re right'
      quotes: ["error", "single", { avoidEscape: true }],
      // Many Devs prefer to have trailing commas at the end of their arrays in part because it keeps version control diffs simpler
      "comma-dangle": ["error", "always-multiline"]
    }
  };
  ```
- To integrate Prettier with VSCode we need to install the Prettier plugin by Esben Petersen and then for VSCode to detect the Prettier-ESLint bridge we must add the following line to the User Settings (by opening the Command Pallete with **Cmd-Shift-P** and searching for `Preferences: Open Settings (JSON)`):
  ```js
  // User Settings
  ...
  "prettier.eslintIntegration": true,
  ```
- We can now format js files from VSCode by opening up the Command Pallete using the hotkey **Cmd-Shift-P** and then searching for `Format Document`.
- If we want to format the document on Save we can open the Workspace settings of VSCode (by opening the Command Pallete with **Cmd-Shift-P** and searching for `Preferences: Open Settings (JSON)` and finally clicking on Workspace tab) and the add the following line to it:
  ```js
  // Workspace Settings
  ...
  "editor.formatOnSave": true,
  ```
  - There is also an `"editor.formatOnType"` flag too, but most developers find it too jarring.
  - We can also add project lint scripts to the `package.json` file of our node project by adding the following lines in the `script` section:
  ```js
  {
    // package.json
    ...
    "scripts": {
      "test": "jest",
      "lint": "eslint . && prettier-eslint --list-different **/*.js",
      "format": "prettier-eslint --write **/*.js",
    }
  }
  ```

## Wallaby.js

- Install Wallaby plugin for VSCode
- Create a Wallaby config file called `wallaby.js` at the root of the project. E.g.

  ```js
  /*
   * For cases where the test files are in a seperate folder, e.g.
   * Palindromes/
   *   palindromes.js
   *   .eslintrc.js
   *   wallaby.js
   *   tests/
   *     palindromes.test.js
   *     .eslintrc.js
   */

  //Code in wallaby.js file

  module.exports = function(wallaby) {
    return {
      testFramework: "jest",
      env: {
        type: "node"
      },
      tests: ["tests/**/*.test.js"],
      files: ["**/*.js", "!node_modules/**/*", "!**/*.test.js", "!**/.*"]
    };
  };

  // tests: ['tests/**/*.test.js'] --- tells Wallaby where to find the test files

  // files: ['**/*.js', '!node_modules/**/*', '!**/*.test.js', '!**/.*'] tells Wallaby where to find the source files.
  // The patterns preceded by ! are excluded, preventing Wallaby from trying to treat JS files in
  // node_modules/, or ending in '.test.js' or starting with '.' as  source files
  ```

- Run `Wallaby.js: Start` from the Command Pallete (**Cmd-Shift-P**) and select the configuration file when prompted

## React

-

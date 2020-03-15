# Better C on Vscode

## Plugins

### Necessary plugins

- `C/C++` from Microsoft
- `C Enhanced Theme` here.

### Recommend plugins

- `Bracket Pair Colorizer 2`

## Setting of `C/C++`

It works well with all default setting, to get a better experience, you can try below setting.

- `C_Cpp: Autocomplete` set to `Default`. We need IntelliSense auto completion engine but not the word-based completion engine provided by vscode.
- Set `C_Cpp: Default: Intelli Sense Mode` right. Set it wrongly will cause bad IntelliSense parser result. Choose it based on your code.
- `C_Cpp: Enhanced Colorization` set to `Enable`.
- `C_CPP: Error Squiggles` set to `EnabledIfIncludesResolve`
- `C_Cpp: Exclusion Policy`. If you need to exclude certain folds, set to `checkFolders` is enough and this is fast. If you need to exclude certain files, set to `checkFilesAndFolds`. Note that the exclude setting is inherited from `Files: Excludes`.
- `C_CPP: Intelli Sense Engine` set to `Default`, this is what we need. `Tag Parser` results is fuzzy and not context-aware. This means when you type an item of an structure, it will give you all the possible symbols it found in the whole project but not the symbols that defined with the structure. `Default` is best but it needs there's no includes error in our project, we will fix it in \*\*Project Setting` chapter.

## Project Setting

### Setting of `c_cpp_properties.json`

> Refer [Here](https://code.visualstudio.com/docs/cpp/c-cpp-properties-schema-reference) for a more detailed description.

Take below as example:

```json
{
  "env": {
    "myDefaultIncludePath": [
      "${workspaceFolder}",
      "${workspaceFolder}/include"
    ],
    "myCompilerPath": "/usr/local/bin/gcc-7"
  },
  "configurations": [
    {
      "name": "Mac",
      "intelliSenseMode": "clang-x64",
      "includePath": ["${myDefaultIncludePath}", "/another/path"],
      "macFrameworkPath": ["/System/Library/Frameworks"],
      "defines": ["FOO", "BAR=100"],
      "forcedInclude": ["${workspaceFolder}/include/config.h"],
      "compilerPath": "/usr/bin/clang",
      "cStandard": "c11",
      "cppStandard": "c++17",
      "compileCommands": "/path/to/compile_commands.json",
      "browse": {
        "path": ["${workspaceFolder}"],
        "limitSymbolsToIncludedHeaders": true,
        "databaseFilename": ""
      }
    }
  ],
  "version": 4
}
```

The most important is **setup the compile path** for your source code. You may not use vscode to compile your code, but vscode need to know the compiler path to resolve c library header files, like `stdio.h` or `stddef.h`. And you can test if all the setting is write by including `stdio.h` in one file. If there's something wrong, vscode will give your warning.

Use `/**` to indicated recursively search certain fold, like:

```
"includePath": ["${workspace}/foo/**]
```

Remove `/**` if you don't need a recursive search.

`**/` is another useful pattern.

### Setting of workspace `settings.json`

I use it to exclude folds or files. There are mainly two exclude variables we can set: `files.exclude` and `search.exclude`. `search.exclude` inherits `files.exclude`.

If you want to exclude all folders named `bar` in current workspace, set like below:

```json
{
  "files.exclude": {
    "**/bar": true
  }
}
```

## The End

I will update this file continuously.

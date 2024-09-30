# Clang Format

`clang-format` is a tool used to format C, C++, Java, and other codebases according to specified style guidelines. It is part of the LLVM project and is widely used in both open-source and industry projects to enforce consistent coding standards.

## Installation

Install `clang-format` using your preferred [package manager](https://en.wikipedia.org/wiki/List_of_software_package_management_systems):

```shell
# On Ubuntu (WSL2)
sudo apt update
sudo apt install clang-format

# On macOS, we can use Homebrew
brew update
brew install clang-format

# On other OS, google "<OS-name> install clang-format" to check for specific commands
# Once you install clang-format, verify it with the following command
clang-format --version
# This should display something like "clang-format version 18.1.8"
```

## IDE Support

### Visual Studio Code

Install the `clang-format` extension and enable it. Then add these two entries to the `.vscode/settings.json`:

```json
{
  "clang-format.executable": "/absolute/path/to/clang-format"
  "editor.formatOnSave": true,
}
```

The first entry specifies the path to the clang-format executable, and the second entry enables IDE to run the `clang-format`command on a file when saved. 

> [!NOTE]
>
> You can use the following command to get the absolute path to the `clang-format` executable:
>
> ```shell
> which clang-format
> ```

### JetBrains family (Intellij IDEA, PyCharm, etc.)

Go to `Settings > Editor > Code Style > C++` and select the `General` tab. Check the `ClangFormat` radio and click `OK` to close the settings window. 

If you want the IDE to reformat code on save, go to `Settings > Tools > Actions on Save` and check the `Reformat code`.

## Configuration File

Create a file named `.clang-format` in the root directory of your project and paste the following configuration. This file will ensure that `clang-format` uses the specified style settings whenever it is run in this directory or any of its subdirectories. You can further customize the code style by editing the configuration to fit your project's requirements. For detailed information on configuration options, refer to the [Clang Format Documentation](https://clang.llvm.org/docs/ClangFormatStyleOptions.html).

```ini
---
Language: Cpp
BasedOnStyle: LLVM
AccessModifierOffset: -3
AlignConsecutiveAssignments: false
AlignConsecutiveDeclarations: false
AlignOperands: true
AlignTrailingComments: false
AlwaysBreakTemplateDeclarations: Yes
BraceWrapping:
  AfterCaseLabel: false
  AfterClass: false
  AfterControlStatement: false
  AfterEnum: false
  AfterFunction: false
  AfterNamespace: false
  AfterStruct: false
  AfterUnion: false
  AfterExternBlock: false
  BeforeCatch: false
  BeforeElse: false
  BeforeLambdaBody: false
  BeforeWhile: true
  SplitEmptyFunction: true
  SplitEmptyRecord: true
  SplitEmptyNamespace: true
BreakBeforeBraces: Custom
BreakConstructorInitializers: AfterColon
IncludeCategories:
  - Regex: '^<[^.]*\.h>$'  # C system headers
    Priority: 1
  - Regex: '^<[^.]*>$'  # C++ system headers
    Priority: 2
  - Regex: '^<[^.]*\.h(pp)?>$'  # Other h/hpp files
    Priority: 3
  - Regex: '^".*"'  # Other files
    Priority: 4
IncludeIsMainRegex: '([-_](test|unittest))?$'
IndentCaseLabels: true
IndentWidth: 4
InsertNewlineAtEOF: true
MacroBlockBegin: ''
MacroBlockEnd: ''
MaxEmptyLinesToKeep: 2
NamespaceIndentation: None
PointerAlignment: Left
SpaceAfterTemplateKeyword: true
SpaceBeforeRangeBasedForLoopColon: true
SpacesInAngles: false
TabWidth: 4
SpacesBeforeTrailingComments: 2
Cpp11BracedListStyle: false
AlignAfterOpenBracket: AlwaysBreak
BinPackParameters: false
ColumnLimit: 100
```


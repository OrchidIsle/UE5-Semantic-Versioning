# UE5-Semantic-Versioning

This GitHub Action fetches Git tags and calculates the version and build number for Unreal Engine projects by reading the project version from an INI file. It's designed to be used with projects that follow Semantic Versioning in their development cycle.

## How it Works

The action performs the following steps:

1. Fetches all Git tags available in the repository.
2. Reads the specified INI file to extract the project's version.
3. Identifies the latest Git tag that matches a Semantic Versioning pattern.
4. Calculates the number of commits since the latest identified version tag.
5. Outputs the new version and build ID, considering the provided build prefix and the commit count.

## Requirements

- The repository must have an initial semantic tag of at least `0.0.0`. This is used as a baseline to calculate the number of commits for the build number.
- The INI file referenced should be from the project settings or must contain the project version in the correct format, typically found in `DefaultGame.ini`.
- The project settings' Semantic Version (SEM) should only be in the Major.Minor.Patch (e.g., `0.1.0`) format, with no build prefix or metadata. Build prefixes are intended to be added as part of your Continuous Integration (CI) process.
- The version in the project settings should be the next anticipated version you are actively working towards. If the version in the project settings is the same as the latest Git tag, the action will automatically increment the patch number by 1.

## Inputs

- `BUILD_PREFIX`: The prefix to apply to the build number (e.g., `dev`, `alpha`, `beta`, `rc`). This should align with your development stage.
- `CONFIG_DIR_PATH`: The full path to the `MyProject/Config` folder, including the path to the GitHub workspace. The folder should contain the DefaultGame.ini file.
- `ADD_BUILD_INFO`: Adds a `BuildInfo.ini` file that contains the Build ID to the `MyProject/Config` folder.

## Using the Action

To use this action in your workflow, add the following step:

```yaml
- name: Set Project Version and Build Number
  uses: OrchidIsle/UE5-Semantic-Versioning@latest
  with:
    BUILD_PREFIX: dev
    CONFIG_DIR_PATH: ${{ github.workspace }}/MyGameFolder/Config
    ADD_BUILD_INFO: true
```

## Outputs
Upon completion, this GitHub Action provides the following outputs, which are set as environment variables. These variables are accessible to subsequent steps in your workflow using the ${{ env.VAR_NAME }} syntax.

- BUILD_ID: This is the constructed identifier for the build. It encompasses the computed version number appended with the build prefix and the number of commits since the last version tag. For instance, 1.2.3-dev4 would indicate that there have been four commits since the version 1.2.3 was tagged, and the build is a development (dev) version.
- VERSION: Reflects the forthcoming release version in the Major.Minor.Patch format based on Semantic Versioning. This version is derived from the project settings and is potentially incremented (the patch number) if the current commit matches the latest version tag in the repository.

To utilize these outputs in subsequent steps of your workflow, reference them as shown in the following examples:

For the build ID:

```yaml
- name: Use Build ID
  run: echo "The build ID is ${{ env.BUILD_ID }}"
```

For the version:

```yaml
- name: Use Version
  run: echo "The next version is ${{ env.VERSION }}"
```

By integrating these environment variables into your CI/CD pipeline, you can dynamically manage build and release processes based on the versioning information provided by this action.

## Typical Usage
A typical use case is to set the project settings version to the version you plan to release next. For example, if you just released 1.2.0, set the project settings version to 1.3.0. During CI, if there have been commits since the 1.2.0 tag, the action would output something like 1.3.0-dev5, assuming there were 5 commits since the last tag.

## Additional Information
Ensure that the actions/checkout step is used before this action to clone the repository into the GitHub Actions runner.
The action is designed for use with Unreal Engine projects but can be adapted for other environments that use INI files for version management.
It's important to maintain consistent tagging and versioning within your project to ensure accurate build numbers.
The action will fail if the version is not found in the INI file, so it's crucial to update the version in the INI file as part of your development process.
For more information on Semantic Versioning, visit SemVer.org.

This README provides a clear guide on how to use the action, its requirements, and typical usage scenarios. It also provides some additional context and advice for maintaining consistent versioning within a project.

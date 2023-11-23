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
- `INI_FILE_PATH`: The full path to the INI file, including the path to the GitHub workspace. The INI file should contain the project version in the correct format.

## Using the Action

To use this action in your workflow, add the following step:

```yaml
- name: Set Project Version and Build Number
  uses: OrchidIsle/UE5-Semantic-Versioning@main
  with:
    BUILD_PREFIX: dev
    INI_FILE_PATH: ${{ github.workspace }}/MyGameFolder/Config/DefaultGame.ini

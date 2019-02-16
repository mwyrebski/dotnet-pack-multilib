dotnet-pack-multilib
====================

Project dependencies sample packing into single NuGet package.

This solution contains a project with two other dependencies:

* MultiLib.Project
  - MultiLib.Dependency1
  - MultiLib.Dependency2

Problem
-------

`dotnet pack` command can create NuGet packages but it always produces separate
NuGet packages for each project (see
[NuGet/Home#3891](https://github.com/NuGet/Home/issues/3891)). This works that
way also for dependent projects from the same solution.

Packing dependencies into single NuGet
--------------------------------------

External `nuget.exe` tool supports packing multiple assemblies into single
NuGet package. The simplest way of generating it can be:

    dotnet build
    nuget.exe pack MultiLib.Project\MultiLib.Project.csproj -IncludeReferencedProjects

Using NuSpec files
------------------

It is possible to define common manifest information using predefined [nuspec
file](https://docs.microsoft.com/en-us/nuget/reference/nuspec).

See local example: [MultiLib.Project.nuspec](./MultiLib.Project.nuspec)

Unfortunately, most of information will be hard-coded in this file but using
variables like `$version$` or `$configuration$` helps a little. Assemblies
which should be included in the NuGet package can be specified under `<files>`
tag.

Creating a package with nuspec file is simple:

    dotnet build -c Release -p:Version=1.5.6
    nuget pack MultiLib.Project.nuspec -Version 1.5.6 -Properties Configuration=Release

Providing parameters (like "Version") will overwrite values from project files.

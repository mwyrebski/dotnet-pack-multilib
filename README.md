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



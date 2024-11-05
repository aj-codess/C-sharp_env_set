https://cakebuild.net/docs/getting-started/setting-up-a-new-scripting-project

the above is a resource link to configure enviroment

create a dir - mkdir c#_cake_build
make a lconfig json file - dotnet new tool-manifest
install cake tool - dotnet tool install Cake.Tool --version 4.2.0

create a new dotnet console - dotnet new console - dotnet new console -o MyConsoleApp

create a new test - dotnet new xunit -o appname.tests

create a new solution  - dotnet new sln
and the above will create a solution on the dir

add app and test to the solution or sln
dotnet sln add ./app
dotnet sln add ./app.tests

in the home dir of the project, create build.cake, paste in build default command

pase initial build script into build.cake









/////////////////////////////////////////////////////////////////////////////////////////build.cake config based on the project
var target = Argument("target", "Test");
var configuration = Argument("configuration", "Release");

//////////////////////////////////////////////////////////////////////
// TASKS
//////////////////////////////////////////////////////////////////////

Task("Clean")
    .WithCriteria(c => HasArgument("rebuild"))
    .Does(() =>
{
    CleanDirectory($"./c#_cake_build/app/bin/{configuration}");
});

Task("Build")
    .IsDependentOn("Clean")
    .Does(() =>
{
    DotNetBuild("./c#_cake_build.sln", new DotNetBuildSettings
    {
        Configuration = configuration,
    });
});

Task("Test")
    .IsDependentOn("Build")
    .Does(() =>
{
    DotNetTest("./c#_cake_build.sln", new DotNetTestSettings
    {
        Configuration = configuration,
        NoBuild = true,
    });
});

//////////////////////////////////////////////////////////////////////
// EXECUTION
//////////////////////////////////////////////////////////////////////

RunTarget(target);

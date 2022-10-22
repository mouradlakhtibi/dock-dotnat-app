# Containerize .NET applications without writing Dockerfiles
This article introduces dotnet build-image, a tool that containerizes .NET applications automatically. You can use build-image to create Dockerfiles and containerized images. You will also discover how to use the tool in a GitHub workflow to create an image from a .NET application and push it to a repository.

## How to Install .NET Core (dotnet) on Ubuntu 22.04
Microsoft .NET Core is a free and open-source software framework designed with keeping Linux and macOS in mind. It is a cross-platform successor to .NET Framework available for Linux, macOS, and Windows systems. .NET Core 6 is an LTR release that will support for the next 3 years. It also supports hot reload and better git integration with Visual Studio 2022.

The Ubuntu 22.04 users can only install .NET Core 6.0. It doesn’t support .NET Core 3.1 or 2.0 since the distro only supports OpenSSL 3.

The developers should install the .NET Core SDK on their system and the staging or production server needs the .NET Core runtime only. This tutorial walks through installing the .NET core on Ubuntu 22.04 LTS Linux system. You can install .NET Core SDK or set up the runtime environment on your system.
### Step 1 – Enable Microsoft PPA
Open a terminal on your Ubuntu system and configure Microsoft PPA by running the following commands:
```
$ wget https://packages.microsoft.com/config/ubuntu/22.04/packages-microsoft-prod.deb
$ sudo dpkg -i packages-microsoft-prod.deb
```
Above commands will create a /etc/apt/sources.list.d/microsoft-prod.list file in your system with the required configuration.
### Step 2 – Installing .NET Core SDK on Ubuntu
.NET Core SDK is the Software development kit used for developing applications. If you are going to create an application or make changes to an existing application, you will require a .net core SDK package on your system.

To install .NET Core SDK on Ubuntu 22.04 LTS system, execute the following commands:
```
$ sudo apt install apt-transport-https 
$ sudo apt update 
$ sudo apt install dotnet-sdk-6.0
```
### Step 4 – Check .NET Core Version
```
$dotnet --version
```
now that we install the prerquirements , we will see how to build an image without using a dockerfile using dotnet-build-image tool

## Building an image or Dockerfile with dotnet build-image
To run a .NET application in a container, you need a Dockerfile. At first, learning about Dockerfiles and writing them can be fun. But after a while, it becomes repetitive. Moreover, the files need to be kept in sync with the application. And if you have several applications, you need to keep updating their Dockerfiles to use the same style.

The dotnet build-image is a global tool that writes the Dockerfile for you. The tool's features include:

It uses a multi-stage build: First, the application is published into one image, then the result is copied into a smaller runtime image.
The tool determines the .NET version an image should use from the .NET project (TargetFramework).
You can choose the image base's operating system, such as Ubuntu or Alpine.
It supports both Podman and Docker to build the image.
It caches NuGet packages across builds.
It uses the SDK version from global.json for publishing the application if the file is present.
 to start install the tool from NuGet:
```
$ dotnet tool install -g dotnet-build-image
```
Next, build an image from the default web template application:

```
$ dotnet new web -o dock-dotnet-app
$ cd dock-ditnet-app
$ dotnet build-image -t mywebapp
```
You will see the build in the terminal. Once the build is finished, run the image:
```
$ docker  run -d  -p 8080:8080 mywebapp
```
Open the web application in your browser at: http://localhost:8080


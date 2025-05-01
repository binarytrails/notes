# Ubuntu

## Install tools

    sudo apt update
    sudo apt install -y wget apt-transport-https gnupg

## Add Microsoft package repository

    wget https://packages.microsoft.com/config/ubuntu/$(lsb_release -rs)/packages-microsoft-prod.deb
    sudo dpkg -i packages-microsoft-prod.deb
    sudo apt update

## Install .NET SDK

    sudo apt install dotnet-sdk-8.0

# Build the project

    dotnet build
    dotnet publish -r win-x64 --self-contained true

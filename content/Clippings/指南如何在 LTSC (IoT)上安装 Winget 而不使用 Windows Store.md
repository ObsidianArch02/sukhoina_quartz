---
title: "[指南]如何在 LTSC (IoT)上安装 Winget 而不使用 Windows Store"
source: https://www.reddit.com/r/Windows10LTSC/comments/t7hg44/guide_how_to_install_winget_on_ltsc_iot_without/
author: 
published: 2022-03-06
created: 2025-01-09
description: 
tags:
  - clippings
---
There are clear and straightforward instructions on the [winget GitHub page](https://github.com/microsoft/winget-cli).  
在 winget 的 GitHub 页面上有明确且直接的说明。

Just download [VCLibs](https://docs.microsoft.com/troubleshoot/cpp/c-runtime-packages-desktop-bridge#how-to-install-and-update-desktop-framework-packages) and install with elevated PowerShell. Then download the App Installer (winget) from [here](https://github.com/microsoft/winget-cli/releases) or [store.rg-adguard](https://store.rg-adguard.net/) ([store version](https://www.microsoft.com/en-gb/p/app/9nblggh4nns1)) and install with elevated PowerShell.  
只需下载 VCLibs 并使用提升的 PowerShell 进行安装。然后从这里或 store.rg-adguard（商店版本）下载 App Installer（winget），并使用提升的 PowerShell 进行安装。

You can then start using winget-cli. You can also install packages by left-clicking on them because the App Installer is set automatically as the default app for the supported bundles (appx, etc.). All dependencies are downloaded and installed automatically. So when you, for example, want to install Calculator, you download the AppxBundle, open it with App Installer, and it will download and install Xaml dependency (and other dependencies if needed).  
您然后可以开始使用 winget-cli。您也可以通过左键单击它们来安装软件包，因为应用程序安装程序已自动设置为支持捆绑包（appx 等）的默认应用程序。所有依赖项都会自动下载和安装。所以当您，例如，想要安装计算器时，您会下载 AppxBundle，用应用程序安装程序打开它，然后它会下载并安装 Xaml 依赖项（以及如果需要其他依赖项）。

The only command you need for installing is `add-appxpackage` (specify the location with `-Path`) Example: `add-appxpackage -Path "C:\Users\%Username%\Downloads\Microsoft.DesktopAppInstaller_8wekyb3d8bbwe.msixbundle"`  
您需要安装的命令是 `add-appxpackage` （使用 `-Path` 指定位置）示例： `add-appxpackage -Path "C:\Users\%Username%\Downloads\Microsoft.DesktopAppInstaller_8wekyb3d8bbwe.msixbundle"`

For the pre-release version of winget, you can download Xaml (WinUI) from [GitHub](https://github.com/microsoft/microsoft-ui-xaml/tags) or [store.rg-adguard](https://store.rg-adguard.net/) ([store App Installer](https://www.microsoft.com/en-gb/p/app/9nblggh4nns1)'s or any other store app's dependencies). Here are direct links for the lazy: [x86](http://tlu.dl.delivery.mp.microsoft.com/filestreamingservice/files/9091d313-0dfb-4dea-925a-16df5f272433?P1=1648422364&P2=404&P3=2&P4=E5MSfHV1DJX9Nj8GeeC4b7LQtQdbQt17VQRXSvA%2bz4AncHWy2UkblnGHKX5y7oM%2bBUXB9eH%2fTYtO3KPitQKGaA%3d%3d) [x64](http://tlu.dl.delivery.mp.microsoft.com/filestreamingservice/files/d1a3bf2c-f6ed-4ee6-89eb-5fab766c9435?P1=1648422364&P2=404&P3=2&P4=ZSHzp99BduLk8goXUuB3mkd1OniApLVERfmLBjHualnjNhbxhJxAnO3DSmQLHBwqKpA27KSLygLbuP2l6xYK0Q%3d%3d)  
对于 winget 的预发布版本，您可以从 GitHub 或 store.rg-adguard（存储应用安装器或任何其他存储应用的依赖项）下载 Xaml（WinUI）。以下是懒人的直接链接：x86 x64
# SampleAppwithVMSS

An ASP.NET MVC 5 web application built on .NET Framework 4.8, structured for deployment and scaling on **Azure Virtual Machine Scale Sets (VMSS)** or Windows Server IIS environments.

---

## 📋 Table of Contents
- [Overview](#overview)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Getting Started](#getting-started)
  - [1. Clone Repository](#1-clone-repository)
  - [2. Restore Dependencies](#2-restore-dependencies)
  - [3. Build and Run](#3-build-and-run)
- [Configuration](#configuration)
- [Deployment to Azure VMSS](#deployment-to-azure-vmss)
- [Source Control](#source-control)

---

## 🔍 Overview

`SampleAppwithVMSS` provides a foundational web application designed to demonstrate hosting, load balancing, and autoscaling patterns in Azure using Virtual Machine Scale Sets. It leverages the classic ASP.NET MVC design pattern with server-side Razor views and client-side assets managed via bundling.

---

## 🛠 Tech Stack

- **Target Framework:** .NET Framework 4.8
- **Framework:** ASP.NET MVC 5.2.9
- **View Engine:** Razor (`.cshtml`)
- **Frontend / Styling:** Bootstrap 5.2.3, Modernizr 2.8.3
- **Scripting:** jQuery 3.7.0, jQuery Validation
- **Asset Optimization:** Microsoft.AspNet.Web.Optimization (WebGrease & Antlr)
- **Local Server:** IIS Express (SSL configured on port `44340`)

---

## 📁 Project Structure

```text
SampleAppwithVMSS/
│
├── .github/                       # GitHub workflow and configuration files
├── .gitignore                     # Git ignore rules for .NET and Visual Studio
├── SampleAppwithVMSS.sln          # Visual Studio Solution file
│
└── SampleAppwithVMSS/             # Main Web Application Project
    ├── App_Start/                 # App startup configuration
    │   ├── BundleConfig.cs        # Script and stylesheet bundle registration
    │   ├── FilterConfig.cs        # Global action filter configuration
    │   └── RouteConfig.cs         # URL routing definitions
    │
    ├── Content/                   # CSS stylesheets and Bootstrap styling
    ├── Controllers/               # MVC Controllers (e.g., HomeController)
    ├── Models/                    # Data transfer objects and business models
    ├── Properties/                # Assembly attributes and metadata
    ├── Scripts/                   # JavaScript libraries (jQuery, Bootstrap)
    ├── Views/                     # Razor views and shared layouts
    │   ├── Home/                  # Action-specific views (Index, About, Contact)
    │   └── Shared/                # Reusable layouts (_Layout.cshtml, Error.cshtml)
    │
    ├── Global.asax / .cs          # Application lifecycle events
    ├── packages.config            # NuGet package reference definitions
    ├── Web.config                 # Root application configuration
    ├── Web.Debug.config           # Debug configuration transformations
    └── Web.Release.config         # Release configuration transformations
```

---

## ⚙️ Prerequisites

Before you can build and run this project, make sure you have installed:

- [Visual Studio 2019 or 2022](https://visualstudio.microsoft.com/) with the **ASP.NET and web development** workload.
- [.NET Framework 4.8 Developer Pack](https://dotnet.microsoft.com/download/dotnet-framework/net48).
- [NuGet CLI](https://www.nuget.org/downloads) (optional, if building via command line).

---

## 🚀 Getting Started

### 1. Clone Repository

```bash
git clone <repository-url>
cd SampleAppwithVMSS
```

### 2. Restore Dependencies

Open the solution in Visual Studio and allow NuGet packages to restore automatically, or restore them via CLI:

```powershell
nuget restore SampleAppwithVMSS.sln
```

### 3. Build and Run

#### In Visual Studio:
1. Open `SampleAppwithVMSS.sln`.
2. Set `SampleAppwithVMSS` as the startup project.
3. Select `Debug` or `Release` configuration.
4. Press `F5` (with debugging) or `Ctrl + F5` (without debugging).
5. The application will launch in your browser at `https://localhost:44340/`.

#### Via Command Line (MSBuild):
```powershell
msbuild SampleAppwithVMSS.sln /p:Configuration=Release
```

---

## 🔧 Configuration

Application settings and compilation behavior are managed in:
- `Web.config`: Base settings for application routing, runtime assembly bindings, and compilation.
- `Web.Debug.config` & `Web.Release.config`: Environment-specific XML Document Transforms (XDT) applied during build/publish pipelines.

---

## ☁️ Deployment to Azure VMSS

When deploying to an **Azure Virtual Machine Scale Set (VMSS)** running Windows Server with IIS:

1. **Publish Web Package:**
   Generate a Web Deploy package or folder output from Visual Studio:
   ```powershell
   msbuild SampleAppwithVMSS\SampleAppwithVMSS.csproj /p:DeployOnBuild=true /p:PublishProfile=FolderProfile /p:Configuration=Release
   ```
2. **VMSS OS Configuration:**
   Ensure VM instances have the **Web-Server (IIS)** role and **NET-Framework-45-ASPNET** features enabled (via custom image, Azure VM Custom Script Extension, or Azure Automation State Configuration / DSC).
3. **Health Probes:**
   Configure an Azure Load Balancer or Application Gateway health probe pointing to the default HTTP route (e.g., `GET /` on port `80`) to manage autoscaling and unhealthy instance remediation.

---

## 🛡️ Source Control

A [.gitignore](.gitignore) is included at the root to exclude:
- Build outputs (`bin/`, `obj/`)
- Restorable NuGet packages (`packages/`)
- User-specific Visual Studio configurations (`.vs/`, `*.user`, `*.suo`)
- Local log and temporary files

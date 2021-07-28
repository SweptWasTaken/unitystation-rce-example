# unitystation-rce-example
A harmless proof of concept for how someone might take advantage of the lack of sandboxing in the multiplayer game Unitystation.

## Introduction

What is [sandboxing](https://qkey.com/blogs/news/what-is-application-sandboxing)?

[Unitystation](https://github.com/unitystation/unitystation) is an online role-playing game that allows users to host their own servers with custom content. The launcher downloads this custom content when connecting to a server but due to their lack of sandboxing and security distributing malicious code that could harm users simply trying to connect to a server is easy.

This is less a specific vulnerability and more a chronic issue with Unitystation's infrastructure. There is no quick fix to sandbox Unitystation, but due to the listing of servers on 

## Code in this example
```csharp
using System.Diagnostics;

namespace Unitystation
{
    class Program
    {
        static void Main(string[] args)
        {
            for (int i = 0; i < 100; i++)
            {
                Process.Start("C:\\Program Files (x86)\\Microsoft\\Edge\\Application\\msedge.exe", " https://en.wikipedia.org/wiki/Malware");
            }
        }
    }
}
```

## Editing 

This was not replicated on Unitystation's official server hub. The http request in [Line 78](https://github.com/unitystation/stationhub/blob/ee41ed64a98772e2d4a90639157896e363ca0bbb/UnitystationLauncher/Models/ServerManager.cs#L78) of their hubs [ServerManager.cs](https://github.com/unitystation/stationhub/blob/develop/UnitystationLauncher/Models/ServerManager.cs) was replaced with the json data from their [API](https://api.unitystation.org/serverlist) in order to get our custom server on the hub.

![Honeyview_2021-07-27_21-33-58](https://user-images.githubusercontent.com/49448379/127264256-6071b400-6cf0-4baa-b063-88991a4c7343.png)


# Unitystation RCE Example
A harmless proof of concept for to show how someone might take advantage of the lack of sandboxing in the multiplayer game Unitystation.

## About the vulnerability

[Unitystation](https://github.com/unitystation/unitystation) is an online role-playing game that allows users to host their own servers with custom content. The launcher downloads this custom content when connecting to a server and runs it. When a game is properly '[sandboxed](https://qkey.com/blogs/news/what-is-application-sandboxing)', the game checks the code being ran, and makes sure no malicious code is ran on the computer of the client. Unitystation however lacks sandboxing altogether. This means that a server host only has to change a single line in a config file to distribute malicious code to users simply trying to connect to a server is easy. Since Unitystation does not perform any checks at all, any code can be ran and any virus could be installed; cryptominers, ransomware, remote access tools, keyloggers, password-stealing software, etc. 

This issue is not fixed yet, and every time you join a Unitystation server you could be downloading malware. The developers have known about this issue for a very long time, and even after the proof of concept was shown they were not convinced the vulnerability needed to be fixed. Only after hours of discussion distributed over multiple days, a single developer was convinced this vulnerability needed to be patched, even though this patch would only consist of a few lines of code (less lines than the code of this proof of concept).

### Proof of concept

This video showcases how this vulnerability can be exploited. For the proof of concept we simply open 100 tabs of the Wikipedia article about malware, but any kind of virus could have been installed. If you would like to know how the proof of concept works, there is more information below the FAQ.

https://user-images.githubusercontent.com/39844191/129407823-124d78f8-c84e-429d-9745-dc6feda197dd.mp4


## FAQ

##### How long has this vulnerability existed?

As far as we can tell, this vulnerability has existed for about 1.5 years.

##### How long have Unitystation developers been aware of this vulnerability?

At least half a year, but likely more. It is important to note however, that this vulnerability is extremely obvious and should have been avoided right away.

##### How long has a proof of concept existed?

At the time of writing, for more than two weeks.

##### What are Unitystation developers doing about this?

After more than a year of this vulnerability existing, and multiple days of a proof of concept existing, a Unitystation contributor was finally convinced that this issue needs to be fixed. They wrote code to fix this issue, and this code was merged a few days later, but this version of the Stationhub has not yet been released. At the time of writing, the fix has been merged for 11 days, but the Unitystation developers have not yet released this build even though their players are at risk.

##### Was there really a server called "Malware Station" on the Unitystation hub?

No, Unitystation uses a whitelist to add servers to the hub. This does not mean that this vulnerability should be ignored, however. There are servers on the hub right now that are hosted by third parties, which means they should definitely *NOT* be trusted with arbitrary code execution on the client. If you are curious about how this server was displayed on the hub, read below.

## About the proof of concept

To make this proof of concept, the server was put on the hub locally by editing the source code. We added the server by adding the following code:

![Honeyview_2021-07-27_21-33-58](https://user-images.githubusercontent.com/49448379/127264256-6071b400-6cf0-4baa-b063-88991a4c7343.png)

This code overrides the JSON file normally received from the Unitystation website. The `WinDownload`, `OSXDownload` and `LinuxDownload` URLs are normally supplied by the server host. This means that the server host can supply any kind of malware. All they have to do is make sure the name of the executable ends with `station.exe`,  zip it, host it somewhere and add the URL to the server config. The code for the "malware" of the proof of concept is shown below, it simply opens up 100 Edge tabs of the Wikipedia page about malware, but any kind of virus could have been used instead.

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


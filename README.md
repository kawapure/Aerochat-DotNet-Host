# Aerochat .NET App Host

This repository contains the source code for the native .NET application host used by Aerochat since version 0.2.4. While it contains the entirety of the .NET source code, the only relevant part is the application host. I simply did not want to spend a significant amount of time to split the application host source code from the rest of the .NET source code (though I may do that at a further point in time).

## Why?

Aerochat targets operating systems which .NET itself no longer officially supports, such as Windows 7. There are some assumptions that the current .NET runtime and native host application (which is what this repo supplements) make which don't apply to Windows 7.

For example, `LoadLibraryEx` got some additional parameters, which are quite useful if you want to run more secure code, but will make the call completely fail on unupdated Windows Vista and 7. Fortunately, this update got backported to both operating systems.

## Building and deploying

I am not sure what environment you should use, but you should be on Windows. I am on Windows 10 build 19045.

1. Make sure to turn off your firewall if you're blocking all outgoing requests from unfamiliar applications. The `build.cmd` script tries to download things from NuGet and will fail if you don't let it connect to the internet.
2. Run `build -subset host -configuration Release`. You may get some NuGet error anyway, but this seems to not matter at all.
3. Your build artifacts should be in `artifacts/bin/win-<arch>-<config>/corehost`. For example, for me, it is in `artifacts/bin/win-x64-Release/corehost`. Grab the `apphost.exe` and drop it in the AppHostBin folder in the Aerochat source code.
# Holdcoin

This folder is for Holdcoin stuff which doesn't exist in the upstream Bitcoin Core project

Holdcoin Development
--------------------
### Windows
- [Setup vscode for C++ development](https://code.visualstudio.com/docs/cpp/config-msvc)
- [Install vcpkg (globally)](https://github.com/Microsoft/vcpkg)
- Run vscode task `dependencies`
- [Install Qt](./build_msvc/README.md#Qt)
- Configure `QtBaseDir` in [./build_msvc/common.qt.init.vcxproj](./build_msvc/common.qt.init.vcxproj)
- Run vscode task `autogen`
- Run vscode task `build`
- Copy binaries from `build_msvc\x64\Release` to a working directory.
- Run GUI with a local data dir and an rpc server `.\bitcoin-qt.exe -datadir=data -server`
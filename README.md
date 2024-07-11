# mym2408
## 目的
LuaをWindows環境（MSVC）でコンパイルできるようにする
## 方針
- 本家のLuaはsubmoduleで管理する
  - cloneするときは`--recursive`オプションをつける
- MSVC対応のCMakeプロジェクトを構成する。
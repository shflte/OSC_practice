[Spec](https://nycu-caslab.github.io/OSC2024/labs/lab1.html)

Project 開始變得有點龐大了，有點不知道怎麼切入。先從 Makefile 開始看懂好了，可以看懂檔案之間的 dependency。

## Makefile
這次 Makefile 撰寫的順序：
- Variable definition
	- Compiler & tools
	- Flags
	- Target binaries, directories, source files, generated object files
- Rules
	- Default target
	- QEMU target for testing
	- Clean
	- Rules to build the default target (in reverse order)
- .PHONY

這邊描述一些幫助理解這次的 Makefile 的東東。
常見的怪怪符號：
- `$<`: 第一個 dependency。
- `$^`: 所有 dependency。
- `$@`: 這條 rule 的 target。

常見的 assignment：
- `=`: Lazy assignment. 在被使用的時候才會展開。
- `:=`: Immediate assignment. Variable value 馬上就會被固定。
- `?=`: Conditional assignment. 只有在 variable 沒 value 或是為空時才會做 assignment。

Etc:
- `$(X:Y=Z)`: 把 `X` 中 match pattern `Y` 的都替換成 `Z`。`X` 可以是個 string, wildcard pattern 或是 variable；`Y` 要寫每個要替換的對象的 pattern。`Z` 是要替換成什麼。
- `$(wildcard Pattern)`: 把 match 右邊 `Pattern` 的所有東西用空格 concat 起來。
- `$(patsubst Y, Z, X)`: 類似 `$(X:Y=Z)`。
- `target: dependency | sth`: 這裡的 `sth` 是 order-only dependency，放在 `|` 右邊。Order-only dependency 放一些在 build 這個 target 時一定要存在但只需要存在即可的 dependency。這種 dependency 不會在被更新時 trigger target 的重新編譯。
- `.PHONY`: Phony 的意思是假的，在 Makefile 中的用途就是定義假的 targets。使用時機是某些 target 總是需要被處理。

# yuepack

A tool for packing yuescript projects into a single file.

Yuepack heavily depends on Yuescript. If you need a general solution for packing instead, use [luapack](https://github.com/le0developer/luapack).
Unlike luapack (which uses the regular `package` and `require` system of Lua), yuepack uses aliasing to import and export modules.

## Usage

Yuepack uses two "magic" macros that are inserted during the packing process.

- `$import "module", "name"`
- `$export "name", value`

The `$import` macro is used to import a module, and the `$export` macro is used to export a value.

Here is an example of a simple project:

```yue
-- main.yue
text = $import "module", "text"

print "Hello", text

-- module.yue
$export "text", "world"
```

You can find this example in the `example` directory.

To pack this project, run the following command:

```bash
yue -e yuepack.yue main.yue
```

This will generate a single Lua file with the following content:

```lua
-- yuepacked using 0.1.0
local _export_module_text
do
	do
		_export_module_text = "world"
	end
end
do
	local text = _export_module_text
	print("Hello", text)
end
```


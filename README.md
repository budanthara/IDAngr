# IDAngr

<p align="center">
<img src="http://andreafioraldi.altervista.org/idangr.png">
</p>

Use [angr](https://github.com/angr/angr) in the IDA Pro debugger generating a state from the current debug session.

> tested only with x86/x86_64 ELF binaries on linux at the moment

note: to install angr on Windows without compiling it look [here](https://github.com/andreafioraldi/angr-win64-wheels)

## GUI

The idangr_gui.py script must be loaded during the debug.

IDAngr adds a panel with a self explanatory interface.

You can set find/avoid addresses and symbolic memory directly from the context menu in the IDA View.

Explore other useful context menus in the panel with the rigth-click on items.

[![youtube_img](/images/youtube.png)](https://www.youtube.com/watch?v=orFYI9C1KqE)

## Plugin 

You can install indagr as a plugin (see [INSTALL.md](INSTALL.md)), to activate it press Ctrl+Alt+I.

## Api

IDAngr implements the [angrdbg](https://github.com/andreafioraldi/angrdbg) api in the IDA debugger.

Use `idangr.init()` to setup the library and access to the following api.

With `idangr.init(True, HOST, PORT)` you can connect and use a remote angrdbg server (start it on the remote machine using `python -m angrdbg`)

`idangr.is_initialized()` can be used in a script to check if init must be called or not.

#### StateShot

Return an angr state from the current debug session state.

#### StateManager

A wrapper around angr to simplify the symbolic values creation and to write the results back in the debugger when angr founds a valid path.

##### Methods
+ `instance.sim(key, size)`        create a symbolic value on a register or on a memory address (size is optional)
+ `instance[key]`                  get a register or a memory value
+ `instance.simulation_manager()`  create an angr simulation manager based on the state
+ `instance.to_dbg(found_state)`   transfer to the debugger state the evaluated value of the symbolic value created before with sim

note: memory values are the same that are returned by `state.mem[addr]`

## Example

```python
Python>sm = StateManager()
Python>sm.sim("edi")
Python>sm.sim("esi")
Python>m = sm.simulation_manager()
Python>m.explore(avoid=0x04005D5, find=0x00004005BC)
<SimulationManager with 1 found, 3 avoid>
Python>idc.GetRegValue("edi")
0
Python>idc.GetRegValue("esi")
5
Python>sm.to_dbg(m.found[0])
Python>idc.GetRegValue("edi")
2
Python>idc.GetRegValue("esi")
0
```

See [examples](https://github.com/andreafioraldi/IDAngr/tree/master/examples) folder.

## Other Debuggers

If you want to use angr in other debuggers looks at [angrdbg](https://github.com/andreafioraldi/angrdbg)

I'va also made an almost equal plugin for GDB: [angrgdb](https://github.com/andreafioraldi/angrgdb)

## TODO
+ add predefined constraints collection to gui
+ add support to angr data dependence graph integration in the ida view
+ add an iphyton shell to manually change the value in the gui
+ add a taint engine based on intel pin



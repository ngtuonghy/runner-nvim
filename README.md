# runner-nvim

A Neovim plugin to run code fast in terminals [**nvterm**](https://github.com/NvChad/nvterm)

<https://github.com/ngtuonghy/runner-nvim/assets/116539745/4b143ddd-9dc0-425c-821e-6547f3478541>

## Installation

- install the plugin with lazy.nvim as you would for any other:

```lua
 require("lazy").setup({

 {
  "ngtuonghy/runner-nvim",
  dependencies = {"nvchad/nvterm"},
  config = funtions()
  require("runner-nvim").setup{}
},

})
```

## Configuration

- The comment plugin needs to be initialised using:

  ```lua
  require("runner-nvim").setup{}
  ```

- However you can pass in some config options, the defaults are

```lua
require('runner-nvim').setup{
 terminals = "horizontal", -- "horizontal|vertical|float"
 clearprevious = false, -- clear output previous run
 autoremove = flase, -- auto clear $fileNameWithoutExt
 commands = {
  lua = {
   cmd = "lua run $filePath",
   Makefile = "Make" -- lua always priority run makefile
  },
  cpp = {
   cmd  = "cd $dir && g++ $fileName -o $fileNameWithoutExt && $dir$fileNameWithoutExt", --default
   debug = "cd $dir && g++ -g $fileName -o $fileNameWithoutExt", -- default
  },
 },
}
```

- Supported customized
  - $dir: The directory of the code file being run
  - $fileName: The base name of the code file being run
  - $fileNameWithoutExt: The base name of the code file being run without its extension
  - $realPath: absolute path to the current file

## Usage

- run debug before [dap](https://github.com/mfussenegger/nvim-dap/wiki/Debug-Adapter-installation)

```lua
local runner = require(runner-nvim)
dap.configurations.cpp = {
  {
    name = "Launch file shaha",
    type = "codelldb",
    request = "launch",
    program = function()
      local getDebug = runner.Getdebug()
      if getDebug == nil then
        print "The language is not setting debug mode"
        return nil
      else
        vim.fn.system(getDebug)
      end
      return vim.fn.fnamemodify(vim.fn.expand "%:p", ":r")
    end,
    cwd = "${workspaceFolder}",
    stopOnEntry = false,
    args = {},
  },
}
```

- command

```lua
Runnercode -- Run code
Runnerfast -- Run code visual select
Runnerdebug -- mode debug
Runnermakefile -- Toggle priority run Makefile
```

- Custom mappings

```lua
M.runner = {
  n = {
    ["<leader>rc"] = { "<cmd>Runnercode<cr>", "Run code" },
    ["<leader>rm"] = { "<cmd>Runnermakefile<cr>", "Toggle priority run Makefile" },
  },
  v = {
    ["<leader>rf"] = { "<cmd>Runnerfast<cr>", "Run code select" },
  },
}
```

## Thank you

- Thank's [vscode-code-runner](https://github.com/formulahendry/vscode-code-runner) the main inspiration of this plugin
  [](https://github.com/NvChad/nvterm)

- Thank's [nvterm](https://github.com/NvChad/nvterm) provide an API that implements this plugin

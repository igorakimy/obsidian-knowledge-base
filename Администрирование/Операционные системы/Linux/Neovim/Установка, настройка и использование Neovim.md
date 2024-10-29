### Установка терминала

В качестве терминала подойдет любой, например Alacritty, Kitty.

### Конфигурация

Создать следующий файл конфигурации:
```bash
nano ~/.config/alacritty/alacritty.toml
```
Прописать в файле конфигурации следующие настройки:
```bash
[env]
TERM = "xterm-256color"

[window]
padding.x = 20
padding.y = 20
decorations = "Buttonless"
opacity = 0.92
blur = true

[font]
normal.family = "JetBrainsMono Nerd Font"
size = 14.0
```

### Установка Neovim

Для установки необходимо выполнить следующую команду:
```bash
sudo apt install neovim
```

### Режимы

`Esc` - выйти из любого режима

##### Режим редактирования

`a` или `i` - перейти в режим редактирования

##### Режим команды

`:q` - закрыть текущую сессию neovim
`:w` - сохранить изменения в файле
`:c` - отменить текущие изменения, очистив буфер
`:edit <filename>` - открыть выбранный файл для редактирования
`:set nu` - включить отображение номеров строк
`:set relativenumber` - установить отображение номеров строк относительно текущей строки
`:buffers` - отобразить список открытых буферов
`:buffer 2` - открыть содержимое буфера под номером
`:registers` = посмотреть текущие регистры и их содержимое

##### Режим перемещения

`k` - вверх
`j` - вверх
`l` - вправо
`h` - влево
`w` - перемещение вперёд по словам
`b` - перемещение назад по словам
`e` - перемещение вперёд по окончаниям слов
`shift + w` - перемещение вперёд по словам с разделителем
`shift + b` - перемещение назад по словам с разделителем

Команды можно комбинировать, например, `:wq`.

##### Режим замены

`shift + r` - перейти в режим замены
`:s/<search-str>/<replace-str>` - искать строку и заменить её
`:s/<search-regexp>/<replace-str>/g` - искать все строки внутри файла и заменить их
`:%s/<search-regexp>/<replace-str>/g` - искать все подстроки внутри строки, а также буфера и заменить их
`:s/<search-regexp>/<replace-str>/gi` - искать все строки внутри файла и заменить их, игнорируя регистр символов


##### Режим поиска

`*` - на определённом слове производится поиск этого слова
`n` - перейти к следующему найденному слову
`shift + n` - перейти к предыдущему найденному слову
`:noh` - скрыть поисковое выделение
`/<search-regexp>` - поиск конкретной строки по регулярному выражению

##### Визуальный режим

`v` - перейти в визуальный режим
`shift + v` - перейти в построчный визуальный режим
`ctrl + shift + v` - перейти в блочный визуальный режим
`vw` - выделить слово справа, включая пробелы
`ve` - выделить только слово, без пробелов
`shift + u` - поменять регистр выделенных символов на противоположный
`:m -2` - переместить выделение на 2 строки вверх
`:m +5` - переместить выделение на 5 строк вниз
`shift + i` - перейти в режим вставки в блочном визуальном режиме

### Комбинации

###### Перемещение 
`10k` - перейти на 10 символов вверх
`0` - перейти к началу строки
`$` - перейти к концу строки
`gg` - перейти в начало файла
`shift + g` - перейти в конец файла
`10 + shift + g` - перейти к определённой строке
`shift + [` - перейти на блок вверх
`shift + ]` - перейти на блок вниз
`ctrl + u` - перейти на одну страницу вверх
`ctrl + d` - перейти на одну страницу вниз
`f + <char>` - переход по конкретному символу
`%` - переместиться к открывающему/закрывающему символу
`_` - перейти к началу строки с символами
`-` - перейти к предыдущей строке с символами
`+` - перейти к следующей строке с символами
`shift + u` - вернуться к предыдущему результату

###### Изменение
`cw` - удалить слово и сразу перейти к его изменению
`ci + "` - удалить всё внутри двойных скобок и перейти к редактированию
`p` - вставка значения cнизу из буфера обмена(регистра)
`shift + p` - вставка значения сверху из буфера обмена(регистра)


###### Удаление 
`dd` - удалить строку целиком
`dw` - удалить слово стоящее справа от курсора
`db` - удалить слово стоящее слева от курсора
`d2w` - удалить два слова справа
`d3j` - удалить 3 строки снизу
`diw` - удалить слово внутри, включая пробелы
`daw` - удалить слово снаружи, включая пробелы
`di + "` - удалить всё внутри двойных кавычек
`dit` - удалить всё внутри тега
`dt + :` - удалить всё до двоеточия

###### Копирование
`y` - скопировать (выделение)
`yw` - cкопировать слово справа
`yit` - скопировать всё внутри тега

### Макросы

`qq` - начать запись макроса
`@q` - прекратить запись макроса

### Конфигурация LUA

##### Базовая конфигурация

Скрипты на языке lua для конфигурации `neovim` должны храниться по следующему пути: `~/.config/nvim`.
Структура этой директории должна быть следующей:
```
init.lua
lua
- core
-- configs.lua
-- mappings.lua
- plugins
-- gitsigns.lua
```
Главным скриптом и точкой входа является файл `init.lua`.
Содержимое файла `init.lua`:
```lua
-- Базовая конфигурация

require("core.configs")
require("core.mappings")
require("core.lazy")
```
Содержимое файла `lua/core/configs.lua`:
```lua
-- Номера строк

vim.wo.number = true -- включить номера строк
vim.wo.relativenumber = true -- включить относительные номера строк

-- Мышь

vim.opt.mouse = "a"
vim.opt.mousefocus = true

-- Буфер обмена

vim.opt.clipboard = "unnamedplus"

-- Отступы

vim.opt.shiftwidth = 4 -- размер отступа в пробелах
vim.opt.tabstop = 4 -- размера таба в пробелах
vim.opt.softtabstop = 4 -- размера таба в пробелах

-- Другие

vim.opt.scrolloff = 8 -- размер строкового смещения при скроле
vim.opt.wrap = false -- отключить перенсимость строк
vim.opt.termguicolors = true

-- Заполняющие символы 

vim.opt.fillchars = {
	vert = "│",
	fold = " ",
	eob = " ",
	msgsep = "‾",
	foldopen = "▾",
	foldsep = "│",
	foldclose = "▸"
}
```
Содержимое файла `lua/core/mappings.lua`:
```lua
-- Leader

vim.g.mapleader = " "

-- Вставка

vim.keymap.set('i', 'jj', '<ESC>') -- быстрый выход изи режима вставки

-- Буферы

vim.keymap.set('n', '<leader>w', ':w<CR>')

-- Управление окнами

vim.keymap.set('n', '|', ':vsplit<CR>')
vim.keymap.set('n', '\\', ':split<CR>')

vim.keymap.set('n', '<c-k>', ':wincmd k<CR>')
vim.keymap.set('n', '<c-j>', ':wincmd j<CR>')
vim.keymap.set('n', '<c-l>', ':wincmd l<CR>')
vim.keymap.set('n', '<c-h>', ':wincmd h<CR>')

-- Дерево каталогов (neo-tree)

vim.keymap.set('n', '<leader>e', ':Neotree left toggle reveal<CR>')

-- Табы (bufferline)

vim.keymap.set('n', '<Tab>', ':BufferLineCycleNext<CR>')
vim.keymap.set('n', '<s-Tab>', ':BufferLineCyclePrev<CR>')
```

##### Установка плагинов

Необходимо создать файл `lua/core/lazy.lua` со следующим содержимым(используется lazy.nvim):
```lua
-- Bootstrap lazy.nvim
local lazypath = vim.fn.stdpath("data") .. "/lazy/lazy.nvim"
if not (vim.uv or vim.loop).fs_stat(lazypath) then
  local lazyrepo = "https://github.com/folke/lazy.nvim.git"
  local out = vim.fn.system({ "git", "clone", "--filter=blob:none", "--branch=stable", lazyrepo, lazypath })
  if vim.v.shell_error ~= 0 then
    vim.api.nvim_echo({
      { "Failed to clone lazy.nvim:\n", "ErrorMsg" },
      { out, "WarningMsg" },
      { "\nPress any key to exit..." },
    }, true, {})
    vim.fn.getchar()
    os.exit(1)
  end
end
vim.opt.rtp:prepend(lazypath)

-- Setup lazy.nvim
require("lazy").setup({
  spec = {
    -- import your plugins
    { import = "plugins" },
  },
  -- automatically check for plugin updates
  checker = { enabled = true },
})
```

Для создания плагина необходимо в папке `lua/plugins` создать соответствующий скрипт, с таким же именем как и у плагина. 

###### Тема
Например, чтобы установить тему, можно добавить создать файл `lua/plugins/github-theme.lua` со следующим содержимым:
```lua
return {
  'projekt0n/github-nvim-theme',
  name = 'github-theme',
  lazy = false,
  priority = 1000,
  config = function()
    require('github-theme').setup({
	    transparent = true
    })
    vim.cmd('colorscheme github_dark_high_contrast')
  end,
}
```

###### Файловый менеджер
Установка плагина файлового менеджера `lua/plugins/neo-tree.lua`:
```lua
return {
    "nvim-neo-tree/neo-tree.nvim",
    branch = "v3.x",
    dependencies = {
      "nvim-lua/plenary.nvim",
      "nvim-tree/nvim-web-devicons", 
	  "MunifTanjim/nui.nvim",
    }
}
```

###### Табы
Установка плагина для табов(открытых файлов) `lua/plugins/bufferline.lua`:
```lua
return {
	{
		'akinsho/bufferline.nvim', 
		version = "*", 
		dependencies = 'nvim-tree/nvim-web-devicons',
		config = function()
			require("bufferline").setup({})
		end
	}
}
```

###### Нижняя панель
Установка плагина для нижней панели `lua/plugins/lualine.lua`:
```lua
return {
	{
		'akinsho/bufferline.nvim', 
		version = "*", 
		dependencies = 'nvim-tree/nvim-web-devicons',
		config = function()
			require("bufferline").setup({
				options = {
					globalstatus = true
				}
			})
		end
	}
}
```

###### Панель поиска
Установка плагина для поисковой панели `lua/plugins/telescope.lua`:
```lua
return {
	'nvim-telescope/telescope.nvim', tag = '0.1.8',
    dependencies = { 'nvim-lua/plenary.nvim' },
	config = function()
		require('telescope').setup({})
		local builtin = require('telescope.builtin')
		vim.keymap.set('n', '<leader>ff', builtin.find_files, {})
		vim.keymap.set('n', '<leader>fw', builtin.live_grep, {})
		vim.keymap.set('n', '<leader>fb', builtin.buffers, {})
		vim.keymap.set('n', '<leader>fh', builtin.help_tags, {})
	end
}
```
Для корректной работы поиска по словам необходимо установить следующий пакет: 
```bash
sudo apt install ripgrep
```

###### Терминал
Установка плагина для терминала `lua/plugins/toggleterm.lua`:
```lua
return {
	{
		'akinsho/toggleterm.nvim', 
		version = "*", 
		config = true,
		config = function()
			require('toggleterm').setup({
				open_mapping = [[<c-`>]],
			})
			
			function _G.set_terminal_keymaps()
				local opts = {buffer = 0}
				vim.keymap.set('t', '<esc>', [[<C-\><C-n>]], opts)
				vim.keymap.set('t', 'jj', [[<C-\><C-n>]], opts)
				vim.keymap.set('t', '<C-h>', [[<Cmd>wincmd h<CR>]], opts)
				vim.keymap.set('t', '<C-j>', [[<Cmd>wincmd j<CR>]], opts)
				vim.keymap.set('t', '<C-k>', [[<Cmd>wincmd k<CR>]], opts)
				vim.keymap.set('t', '<C-l>', [[<Cmd>wincmd l<CR>]], opts)
				vim.keymap.set('t', '<C-w>', [[<C-\><C-n><C-w>]], opts)
			end

			-- if you only want these mappings for toggle term use term://*toggleterm#* instead
			vim.cmd('autocmd! TermOpen term://* lua set_terminal_keymaps()')
		end
	}
}
```

###### Автодополнение

Установить плагин `lua/plugins/cmp.lua`:
```lua
return {
	{'hrsh7th/cmp-nvim-lsp'},
	{'hrsh7th/cmp-buffer'},
	{'hrsh7th/cmp-path'},
	{'hrsh7th/cmp-cmdline'},
	{
		'hrsh7th/nvim-cmp',
		config = function()
			local cmp = require('cmp')
			cmp.setup({
				snippet = {
			      expand = function(args)
				      vim.fn["vsnip#anonymous"](args.body) 
			      end,
			    },
			    window = {},
			    mapping = cmp.mapping.preset.insert({
			        ['<C-b>'] = cmp.mapping.scroll_docs(-4),
			        ['<C-f>'] = cmp.mapping.scroll_docs(4),
			        ['<C-Space>'] = cmp.mapping.complete(),
			        ['<C-e>'] = cmp.mapping.abort(),
			        ['<CR>'] = cmp.mapping.confirm({ select = true }),
					['<Tab>'] = cmp.mapping(function(fallback)
						if cmp.visible() then
							cmp.select_next_item()
						else
							fallback()
						end 
					end, {"i", "s"}),
					['<S-Tab>'] = cmp.mapping(function(fallback)
						if cmp.visible() then
							cmp.select_next_item()
						else
							fallback()
						end 
					end, {"i", "s"})
			    }),
			    sources = cmp.config.sources({
			        { name = 'nvim_lsp' },
			        { name = 'vsnip' },
			    }, {
			        { name = 'buffer' },
			    })
			})
		end
	},
}
```
Установить плагин `lua/plugins/lsp.lua`:
```lua
return {
	{
		'neovim/nvim-lspconfig',
		config = function()
			local lspconfig = require('lspconfig')
			lspconfig.lua_ls.setup({})
			lspconfig.clangd.setup({})
			vim.api.nvim_create_autocmd('LspAttach', {
				group = vim.api.nvim_create_augroup('UserLspConfig', {}),
				callback = function(ev)
					vim.bo[ev.buf].omnifunc = 'v:lua.vim.lsp.omnifunc'

					local opts = { buffer = ev.buf }
					vim.keymap.set('n', 'gd', vim.lsp.buf.definition, opts)
					vim.keymap.set('n', 'K', vim.lsp.buf.hover, opts)
					vim.keymap.set('n', 'gi', vim.lsp.buf.implementation, opts)
					vim.keymap.set('n', '<C-k>', vim.lsp.buf.signature_help, opts)
					vim.keymap.set('n', '<Leader>D', vim.lsp.buf.type_definition, opts)
					vim.keymap.set('n', '<Leader>lr', vim.lsp.buf.rename, opts)
					vim.keymap.set({'n', 'v'}, '<Leader>la', vim.lsp.buf.code_action, opts)
					vim.keymap.set('n', '<Leader>lf', function() vim.lsp.buf.format {async = true} end, opts)
				end
			})
		end
	}
}
```
Установить плагин `lua/plugins/mason.lua`:
```lua
return {
	{
		'williamboman/mason.nvim',
		config = function()
			require('mason').setup()
		end
	},
	{
		'williamboman/mason-lspconfig.nvim',
		config = function()
			require('mason-lspconfig').setup({
				ensure_installed = {
					"lua_ls",
					"clangd",
				}
			})
		end
	}
}
```

###### Подсветка синтаксиса 
Установить плагин `lua/plugins/treesitter.lua`:
```lua
return {
	{
		'nvim-treesitter/nvim-treesitter',
		config = function()
			require('nvim-treesitter.configs').setup({
				ensure_installed = {
					"c",
					"lua",
					"query",
					"markdown",
					"markdown_inline",
					"javascript",
					"typescript",
					"go",
					"php",
					"python"
				},
				auto_install = true,
				highlight = {
					enable = true,
				}
			})
		end
	}
}
```

###### Переименование 
Установить плагин `lua/plugins/dressing.lua`:
```lua
return {
	{
		'stevearc/dressing.nvim',
		config = function()
			require('dressing').setup({})
		end
	}
}
```

###### Авто-форматирование
Установить плагин `lua/plugins/conform.lua`:
```lua
return {
	{
		'stevearc/conform.nvim',
		config = function()
			require('conform').setup({
				formatters_by_ft = {
					lua = {"stylua"},
					c = {"clang-format"},
					python = {"isort", "black"},
					javascript = {"prettier", stop_after_first = true},
				}
			})
			vim.api.nvim_create_autocmd("BufWritePre", {
				pattern = "*",
				callback = function(args)
					require("conform").format({bufnr = args.buf})
				end
			})
		end
	}
}
```

###### Линтер
Установить плагин `lua/plugins/lint.lua`:
```lua
return {
	{
		'mfussenegger/nvim-lint',
		config = function()
			require('lint').linters_by_ft = {
				c = {'clangtidy'}
			}
			vim.api.nvim_create_autocmd({"BufWritePost"}, {
				callback = function()
					require('lint').try_lint()
				end,
			})
		end
	}
}
```

###### Контроль версий(git)
Установить плагин `lua/plugins/fugitive.lua`:
```lua
return {
	{
		"tpope/vim-fugitive",
	},
	{
		"rbong/vim-flog",
		lazy = true,
		cmd = { "Flog", "Flogsplit", "Floggit" },
		dependencies = {
			"tpope/vim-fugitive",
		},
	},
}
```

###### Удобная навигация
Установить плагин `lua/plugins/leap.lua`:
```lua
return {
	"ggandor/leap.nvim",
	lazy = false,
	config = function()
		require("leap").add_default_mappings(true)
	end
}
```

###### Подсказки для биндов
Установить плагин `lua/plugins/which-key.lua`:
```lua
return {
	'folke/which-key.nvim',
	event = 'VeryLazy',
	opts = {},
	keys = {
		{
			"<leader>?",
			function()
				require('which-key').show({
					global = false,
				})
			end
			desc = "Buffer Local Keymaps (which-key)",
		}
	}
}
```
```Lua
local docker = require("docker") 

local function list_containers() 
  local containers = docker.list_containers() 
  local output = "" 
  for _, container in ipairs(containers) do 
  local name = container.Names[1]:sub(2) 
  local image = container.Image 
  local state = container.State 
  output = output .. string.format("%s (%s) [%s]\n", name, image, state) 
end 

local buf = vim.api.nvim_create_buf(false, true) 
  vim.api.nvim_buf_set_lines(buf, 0, -1, true, vim.split(output, "\n")) 
  local win = vim.api.nvim_open_win(buf, true, {}) 
end 

local function stop_container(name) 
   local success = docker.stop_container(name) 
   local output = "" 
   if success then 
     output = string.format("Stopped container: %s\n", name) 
   else 
     output = string.format("Failed to stop container: %s\n", name) 
end 

local buf = vim.api.nvim_create_buf(false, true) 
  vim.api.nvim_buf_set_lines(buf, 0, -1, true, {output}) 
  local win = vim.api.nvim_open_win(buf, true, {}) 
end 

local function start_container(name) 
  local success = docker.start_container(name) 
  local output = "" 
  if success then 
    output = string.format("Started container: %s\n", name) 
  else 
    output = string.format("Failed to start container: %s\n", name) 
end 

local buf = vim.api.nvim_create_buf(false, true) 
  vim.api.nvim_buf_set_lines(buf, 0, -1, true, {output}) 
  local win = vim.api.nvim_open_win(buf, true, {}) 
end
```


V2

```Lua
local docker = require("docker") 

local function list_containers() 
	local containers = docker.list_containers() 
	local output = "" 
	for _, container in ipairs(containers) do 
		local name = container.Names[1]:sub(2) 
		local image = container.Image 
		local state = container.State 
		output = output .. string.format("%s (%s) [%s]\n", name, image, state) end 
		
local buf = vim.api.nvim_create_buf(false, true) 
	vim.api.nvim_buf_set_lines(buf, 0, -1, true, vim.split(output, "\n")) 
	local opts = { relative = "editor", style = "minimal", statusline = "Docker Containers", } 
	local win = vim.api.nvim_open_win(buf, true, opts) 
end 

local function stop_container(name) 
	local success = docker.stop_container(name) 
	local output = "" 
	if success then 
		output = string.format("Stopped container: %s\n", name) 
	else 
		output = string.format("Failed to stop container: %s\n", name) 
end 

local buf = vim.api.nvim_create_buf(false, true) 
	vim.api.nvim_buf_set_lines(buf, 0, -1, true, {output}) 
	local opts = { relative = "editor", style = "minimal", statusline = "Docker Container Stopped", } 
	local win = vim.api.nvim_open_win(buf, true, opts) 
end 

local function start_container(name) 
	local success = docker.start_container(name) 
	local output = "" 
	if success then 
		output = string.format("Started container: %s\n", name) 
	else 
		output = string.format("Failed to start container: %s\n", name) 
end 

local buf = vim.api.nvim_create_buf(false, true) 
	vim.api.nvim_buf_set_lines(buf, 0, -1, true, {output}) 
	local opts = { relative = "editor", style = "minimal", statusline = "Docker Container Started", } 
	local win = vim.api.nvim_open_win(buf, true, opts) 
end

local function key_mappings() 
	vim.api.nvim_set_keymap("n", "<leader>dc", "<cmd>lua list_containers()<CR>", {noremap=true, silent=true}) 
	vim.api.nvim_set_keymap("n", "<leader>ds", "<cmd>lua stop_container(vim.fn.input('Stop container: '))<CR>", {noremap=true, silent=true})
```
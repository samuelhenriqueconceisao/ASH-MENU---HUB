-- ASH MENU Criado por Xaruto e Unknow

-- Serviços
local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local SoundService = game:GetService("SoundService")
local player = Players.LocalPlayer

-- Som de clique
local clickSound = Instance.new("Sound")
clickSound.SoundId = "rbxassetid://5419098671"
clickSound.Volume = 1
clickSound.Parent = SoundService

-- Função de clique
local function playClick()
	clickSound:Play()
end

-- Notificação
local function notify(title, text)
	game.StarterGui:SetCore("SendNotification", {
		Title = title;
		Text = text;
		Duration = 3;
	})
end

-- Interface Principal
local ScreenGui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
ScreenGui.Name = "ASH_MENU"

local MainFrame = Instance.new("Frame", ScreenGui)
MainFrame.Size = UDim2.new(0, 350, 0, 300)
MainFrame.Position = UDim2.new(0.5, -175, 0.5, -150)
MainFrame.BackgroundColor3 = Color3.fromRGB(230, 240, 255)
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Visible = true

-- Botão de abrir/fechar
local ToggleButton = Instance.new("TextButton", ScreenGui)
ToggleButton.Size = UDim2.new(0, 120, 0, 30)
ToggleButton.Position = UDim2.new(0, 20, 0, 20)
ToggleButton.Text = "🟦 Abrir/Fechar"
ToggleButton.BackgroundColor3 = Color3.fromRGB(0, 85, 255)
ToggleButton.TextColor3 = Color3.new(1, 1, 1)
ToggleButton.Font = Enum.Font.GothamBold
ToggleButton.TextSize = 14

ToggleButton.MouseButton1Click:Connect(function()
	playClick()
	MainFrame.Visible = not MainFrame.Visible
end)

-- Títulos das abas
local Tabs = {"Main", "Farms", "Misc"}
local TabButtons = {}
local CurrentTab = "Main"

local ButtonHolder = Instance.new("Frame", MainFrame)
ButtonHolder.Size = UDim2.new(1, 0, 0, 30)
ButtonHolder.Position = UDim2.new(0, 0, 0, 0)
ButtonHolder.BackgroundColor3 = Color3.fromRGB(0, 85, 255)

for i, tabName in ipairs(Tabs) do
	local btn = Instance.new("TextButton", ButtonHolder)
	btn.Size = UDim2.new(0, 100, 1, 0)
	btn.Position = UDim2.new(0, (i - 1) * 100, 0, 0)
	btn.Text = tabName
	btn.TextColor3 = Color3.new(1,1,1)
	btn.BackgroundColor3 = Color3.fromRGB(0, 85, 255)
	btn.Font = Enum.Font.GothamBold
	btn.TextSize = 14
	btn.MouseButton1Click:Connect(function()
		playClick()
		for _, tab in pairs(MainFrame:GetChildren()) do
			if tab:IsA("Frame") and tab.Name:find("Tab_") then
				tab.Visible = false
			end
		end
		MainFrame:FindFirstChild("Tab_"..tabName).Visible = true
	end)
	table.insert(TabButtons, btn)
end

-- Criar abas
local function createTab(name)
	local tab = Instance.new("Frame", MainFrame)
	tab.Name = "Tab_"..name
	tab.Size = UDim2.new(1, 0, 1, -30)
	tab.Position = UDim2.new(0, 0, 0, 30)
	tab.BackgroundTransparency = 1
	tab.Visible = (name == "Main")
	return tab
end

-- MAIN
local MainTab = createTab("Main")
local nickLabel = Instance.new("TextLabel", MainTab)
nickLabel.Size = UDim2.new(1, -20, 0, 30)
nickLabel.Position = UDim2.new(0, 10, 0, 10)
nickLabel.Text = "👤 Nick: " .. player.Name
nickLabel.BackgroundTransparency = 1
nickLabel.TextColor3 = Color3.new(0, 0, 0)
nickLabel.Font = Enum.Font.Gotham
nickLabel.TextSize = 16

local devLabel = Instance.new("TextLabel", MainTab)
devLabel.Size = UDim2.new(1, -20, 0, 30)
devLabel.Position = UDim2.new(0, 10, 0, 45)
devLabel.Text = "👨‍💻 Devs: Xaruto + Unknow"
devLabel.BackgroundTransparency = 1
devLabel.TextColor3 = Color3.new(0, 0, 0)
devLabel.Font = Enum.Font.Gotham
devLabel.TextSize = 16

-- Função para criar botões
local function createButton(parent, texto, callback)
	local btn = Instance.new("TextButton", parent)
	btn.Size = UDim2.new(1, -20, 0, 30)
	btn.Position = UDim2.new(0, 10, 0, #parent:GetChildren() * 35)
	btn.Text = texto
	btn.BackgroundColor3 = Color3.fromRGB(200, 220, 255)
	btn.TextColor3 = Color3.new(0, 0, 0)
	btn.Font = Enum.Font.Gotham
	btn.TextSize = 14
	btn.MouseButton1Click:Connect(function()
		playClick()
		callback()
	end)
end

-- FARMS
local FarmsTab = createTab("Farms")

createButton(FarmsTab, "🚗 Farm Uber", function()
	local carro = workspace.CarrosSpawnados:FindFirstChild("ok")
	local destino = workspace:FindFirstChild("LocalMarcado")
	if carro and destino and carro:FindFirstChild("DriveSeat") then
		if not carro.PrimaryPart then
			carro.PrimaryPart = carro:FindFirstChild("DriveSeat")
		end
		carro:SetPrimaryPartCFrame(destino.CFrame)
		notify("Farm Uber", "Transporte realizado!")
	else
		warn("Erro ao teleportar carro.")
	end
end)

createButton(FarmsTab, "🍕 Farm iFood", function()
	loadstring(game:HttpGet('https://raw.githubusercontent.com/Cr4ki3/Flexhub/refs/heads/main/Pizza'))()
end)

createButton(FarmsTab, "🌿 Farm Erva", function()
	loadstring(game:HttpGet("https://pastebin.com/raw/H3AhezX7"))()
end)

createButton(FarmsTab, "📍 Farm Rota", function()
	loadstring(game:HttpGet("https://pastebin.com/raw/mCYua8Vt"))()
end)

-- MISC
local MiscTab = createTab("Misc")

createButton(MiscTab, "🧍‍♂️ ESP Player", function()
	loadstring(game:HttpGet("https://pastebin.com/raw/Pvn65dXn"))() -- coloque o ESP aqui ou link
end)

createButton(MiscTab, "🚘 Renomear Carro", function()
	local character = player.Character or player.CharacterAdded:Wait()
	local hrp = character:WaitForChild("HumanoidRootPart")
	local carros = workspace.CarrosSpawnados:GetChildren()
	local carroPerto = nil
	local menorDist = math.huge
	for _, c in pairs(carros) do
		if c:IsA("Model") then
			local ref = c:FindFirstChild("DriveSeat") or c:FindFirstChildWhichIsA("BasePart")
			if ref then
				local dist = (ref.Position - hrp.Position).Magnitude
				if dist < menorDist then
					menorDist = dist
					carroPerto = c
				end
			end
		end
	end
	if carroPerto then
		carroPerto.Name = "ok"
		notify("Renomear", "Carro mais perto agora se chama 'ok'")
	else
		warn("Nenhum carro próximo.")
	end
end)

createButton(MiscTab, "💸 Puxar Dinheiro", function()
	-- seu código aqui
end)

createButton(MiscTab, "🔫 Puxar Arma", function()
	-- seu código aqui
end)

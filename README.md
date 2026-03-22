local player = game.Players.LocalPlayer

-- CRIAR GUI
local gui = Instance.new("ScreenGui")
gui.Name = "FakeHackESP"
gui.Parent = player:WaitForChild("PlayerGui")

-- BOTÃO ABRIR/FECHAR (AGORA NO CANTO SUPERIOR ESQUERDO)
local toggle = Instance.new("TextButton")
toggle.Size = UDim2.new(0,120,0,40)
toggle.Position = UDim2.new(0,10,0,10) -- AQUI FOI ALTERADO
toggle.Text = "ABRIR"
toggle.BackgroundColor3 = Color3.fromRGB(40,40,40)
toggle.TextColor3 = Color3.new(1,1,1)
toggle.Parent = gui

-- FRAME CENTRAL
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0,220,0,150)
frame.Position = UDim2.new(0.5,-110,0.5,-75)
frame.BackgroundColor3 = Color3.fromRGB(25,25,25)
frame.Visible = false
frame.Parent = gui

-- BOTÃO ESP
local espBtn = Instance.new("TextButton")
espBtn.Size = UDim2.new(0,180,0,50)
espBtn.Position = UDim2.new(0.5,-90,0.5,-25)
espBtn.Text = "ESP OFF"
espBtn.BackgroundColor3 = Color3.fromRGB(60,60,60)
espBtn.TextColor3 = Color3.new(1,1,1)
espBtn.Parent = frame

-- VARIÁVEIS
local espAtivo = false
local highlights = {}

-- ATIVAR ESP
local function ativarESP()
	for _, plr in pairs(game.Players:GetPlayers()) do
		if plr ~= player and plr.Character then
			local h = Instance.new("Highlight")
			h.FillColor = Color3.fromRGB(255,0,0)
			h.OutlineColor = Color3.fromRGB(255,255,255)
			h.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
			h.Parent = plr.Character
			
			table.insert(highlights, h)
		end
	end
end

-- DESATIVAR ESP
local function desativarESP()
	for _, h in pairs(highlights) do
		if h then
			h:Destroy()
		end
	end
	highlights = {}
end

-- BOTÃO ESP
espBtn.MouseButton1Click:Connect(function()
	espAtivo = not espAtivo
	
	if espAtivo then
		espBtn.Text = "ESP ON"
		ativarESP()
	else
		espBtn.Text = "ESP OFF"
		desativarESP()
	end
end)

-- ABRIR / FECHAR MENU
toggle.MouseButton1Click:Connect(function()
	frame.Visible = not frame.Visible
	
	if frame.Visible then
		toggle.Text = "FECHAR"
	else
		toggle.Text = "ABRIR"
	end
end)

-- NOVOS PLAYERS
game.Players.PlayerAdded:Connect(function(plr)
	plr.CharacterAdded:Connect(function(char)
		if espAtivo then
			local h = Instance.new("Highlight")
			h.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
			h.Parent = char
			table.insert(highlights, h)
		end
	end)
end)

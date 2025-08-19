-- ðŸ¤– Roblox Assistant Script - Windows Vista Style
-- Cole este script em ServerScriptService como LocalScript para funcionar
-- Criado para ajudar desenvolvedores Roblox com interface Windows Vista

local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService") 
local UserInputService = game:GetService("UserInputService")
local TextService = game:GetService("TextService")

local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- ðŸŽ¨ ConfiguraÃ§Ãµes de cores Windows Vista
local VISTA_CONFIG = {
    -- Cores principais
    PrimaryColor = Color3.fromRGB(54, 126, 231),        -- Azul Vista principal
    PrimaryDark = Color3.fromRGB(40, 100, 200),         -- Azul Vista escuro
    SecondaryColor = Color3.fromRGB(240, 245, 251),     -- Cinza claro Vista
    AccentColor = Color3.fromRGB(255, 193, 7),          -- Amarelo dourado
    
    -- Cores de fundo
    DesktopBg = Color3.fromRGB(135, 206, 235),          -- Fundo desktop Vista
    WindowBg = Color3.fromRGB(248, 250, 252),           -- Fundo janela
    SidebarBg = Color3.fromRGB(240, 245, 251),          -- Fundo sidebar
    ChatBg = Color3.fromRGB(255, 255, 255),             -- Fundo Ã¡rea chat
    
    -- Cores de borda e texto
    BorderColor = Color3.fromRGB(204, 204, 204),        -- Borda padrÃ£o
    TextColor = Color3.fromRGB(33, 33, 33),             -- Texto escuro
    TextLight = Color3.fromRGB(100, 100, 100),          -- Texto claro
    WhiteText = Color3.fromRGB(255, 255, 255)           -- Texto branco
}

-- ðŸ“š Templates de Scripts organizados por categoria
local SCRIPT_TEMPLATES = {
    {
        name = "Basic Script",
        category = "Basics",
        description = "Template bÃ¡sico para scripts Roblox",
        code = [[-- Basic Script Template
print("Hello, Roblox!")

-- Your code here]]
    },
    {
        name = "Part Creator", 
        category = "Building",
        description = "Creates a new part in workspace",
        code = [[-- Part Creator Script
local part = Instance.new("Part")
part.Name = "MyPart"
part.Size = Vector3.new(4, 1, 2)
part.Position = Vector3.new(0, 5, 0)
part.BrickColor = BrickColor.new("Bright blue")
part.Parent = workspace

print("Part created successfully!")]]
    },
    {
        name = "LocalScript Template",
        category = "Client", 
        description = "Template for LocalScripts (client-side)",
        code = [[-- LocalScript Template (Client-side)
local Players = game:GetService("Players")
local player = Players.LocalPlayer

-- Client-side code here
print("LocalScript running for:", player.Name)]]
    },
    {
        name = "Server Script Template",
        category = "Server",
        description = "Template for ServerScripts (server-side)", 
        code = [[-- ServerScript Template (Server-side)
local Players = game:GetService("Players")

-- Server-side code here
Players.PlayerAdded:Connect(function(player)
    print(player.Name .. " joined the game!")
end)

Players.PlayerRemoving:Connect(function(player)
    print(player.Name .. " left the game!")
end)]]
    },
    {
        name = "GUI Button Handler",
        category = "GUI",
        description = "Basic GUI button with click handler",
        code = [[-- GUI Button Handler
local Players = game:GetService("Players")
local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- Create ScreenGui
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "MyGui"
screenGui.Parent = playerGui

-- Create Button
local button = Instance.new("TextButton")
button.Name = "MyButton"
button.Size = UDim2.new(0, 200, 0, 50)
button.Position = UDim2.new(0.5, -100, 0.5, -25)
button.Text = "Click Me!"
button.TextScaled = true
button.BackgroundColor3 = Color3.fromRGB(100, 150, 200)
button.TextColor3 = Color3.white
button.Parent = screenGui

-- Button click handler
button.MouseButton1Click:Connect(function()
    print("Button clicked!")
    -- Add your button logic here
end)]]
    },
    {
        name = "Module Script Template",
        category = "Module",
        description = "Basic ModuleScript template",
        code = [[-- ModuleScript Template
local MyModule = {}

-- Module functions
function MyModule.myFunction(param1, param2)
    -- Add your function logic here
    return true
end

-- Initialize function (optional)
function MyModule.init()
    print("MyModule initialized!")
end

return MyModule]]
    }
}

-- ðŸ¤– Sistema de respostas da IA (igual Ã  aplicaÃ§Ã£o web)
local AI_RESPONSES = {
    greetings = {
        patterns = {"ola", "oi", "hello", "hi", "bom dia", "boa tarde", "boa noite"},
        responses = {
            "OlÃ¡! Sou o Assistente Roblox, especializado em Lua. Como posso ajudÃ¡-lo hoje?",
            "Oi! Estou aqui para ajudar com seus scripts em Roblox. O que vocÃª gostaria de criar?",
            "OlÃ¡, desenvolvedor! Pronto para criar alguns scripts incrÃ­veis em Lua?"
        }
    },
    scripting = {
        patterns = {"script", "criar", "fazer", "code", "codigo", "create", "make", "gerar"},
        responses = {
            "Claro! Posso ajudÃ¡-lo a criar scripts para Roblox. Que tipo de script vocÃª precisa?\n\nAlgumas opÃ§Ãµes:\n- Scripts de servidor (ServerScript)\n- Scripts locais (LocalScript)\n- Scripts de mÃ³dulo (ModuleScript)\n- Scripts de interface (GUI)\n- Scripts de construÃ§Ã£o\n\nPor favor, me diga mais detalhes sobre o que vocÃª quer fazer!",
            "Vou ajudÃ¡-lo a criar um script! Preciso de mais informaÃ§Ãµes:\n\n1. Que tipo de script? (ServerScript, LocalScript, etc.)\n2. O que ele deve fazer?\n3. HÃ¡ alguma funcionalidade especÃ­fica que vocÃª precisa?\n\nCompartilhe os detalhes e eu criarei o cÃ³digo para vocÃª!"
        }
    },
    parts = {
        patterns = {"part", "parte", "create part", "criar parte", "block", "bloco"},
        responses = {
            "Vou criar um script para vocÃª fazer uma parte! Aqui estÃ¡ um exemplo bÃ¡sico:\n\n```lua\n-- Script para criar uma parte\nlocal part = Instance.new(\"Part\")\npart.Name = \"MinhaParte\"\npart.Size = Vector3.new(4, 1, 2)\npart.Position = Vector3.new(0, 5, 0)\npart.BrickColor = BrickColor.new(\"Bright blue\")\npart.Material = Enum.Material.Plastic\npart.Parent = workspace\n\nprint(\"Parte criada com sucesso!\")\n```\n\nEste script cria uma parte azul no workspace. VocÃª pode modificar as propriedades como tamanho, posiÃ§Ã£o, cor e material conforme necessÃ¡rio!"
        }
    },
    gui = {
        patterns = {"gui", "interface", "button", "botao", "screen", "tela"},
        responses = {
            "Aqui estÃ¡ um exemplo de como criar uma interface simples com botÃ£o:\n\n```lua\n-- LocalScript para criar GUI\nlocal Players = game:GetService(\"Players\")\nlocal player = Players.LocalPlayer\nlocal playerGui = player:WaitForChild(\"PlayerGui\")\n\n-- Criar ScreenGui\nlocal screenGui = Instance.new(\"ScreenGui\")\nscreenGui.Name = \"MinhaInterface\"\nscreenGui.Parent = playerGui\n\n-- Criar botÃ£o\nlocal button = Instance.new(\"TextButton\")\nbutton.Size = UDim2.new(0, 200, 0, 50)\nbutton.Position = UDim2.new(0.5, -100, 0.5, -25)\nbutton.Text = \"Clique Aqui!\"\nbutton.TextScaled = true\nbutton.BackgroundColor3 = Color3.fromRGB(100, 150, 200)\nbutton.TextColor3 = Color3.white\nbutton.Parent = screenGui\n\n-- FunÃ§Ã£o do botÃ£o\nbutton.MouseButton1Click:Connect(function()\n    print(\"BotÃ£o clicado!\")\n    -- Adicione sua lÃ³gica aqui\nend)\n```\n\nEste Ã© um LocalScript que cria uma interface com um botÃ£o clicÃ¡vel!"
        }
    },
    player = {
        patterns = {"player", "jogador", "join", "entrar", "leave", "sair"},
        responses = {
            "Aqui estÃ¡ um script para detectar quando jogadores entram e saem:\n\n```lua\n-- ServerScript para eventos de jogador\nlocal Players = game:GetService(\"Players\")\n\n-- Quando um jogador entra\nPlayers.PlayerAdded:Connect(function(player)\n    print(player.Name .. \" entrou no jogo!\")\n    \n    -- Dar boas-vindas no chat\n    local message = \"Bem-vindo, \" .. player.Name .. \"!\"\n    -- VocÃª pode criar uma GUI de boas-vindas aqui\nend)\n\n-- Quando um jogador sai\nPlayers.PlayerRemoving:Connect(function(player)\n    print(player.Name .. \" saiu do jogo!\")\n    -- Salvar dados do jogador ou fazer limpeza\nend)\n```\n\nEste Ã© um ServerScript que monitora a entrada e saÃ­da de jogadores!"
        }
    },
    help = {
        patterns = {"help", "ajuda", "como", "how", "what", "o que"},
        responses = {
            "Posso ajudÃ¡-lo com vÃ¡rias coisas relacionadas a Roblox e Lua:\n\n**Scripts BÃ¡sicos:**\n- Criar partes e objetos\n- Manipular propriedades\n- Eventos e conexÃµes\n\n**Interface (GUI):**\n- BotÃµes e menus\n- Frames e labels\n- Eventos de clique\n\n**Sistema de Jogador:**\n- Detectar entrada/saÃ­da\n- Dados do jogador\n- Teleporte e spawn\n\n**Scripts AvanÃ§ados:**\n- ModuleScripts\n- RemoteEvents/Functions\n- DataStores\n\nO que vocÃª gostaria de aprender ou criar?",
            "Estou aqui para ajudar com programaÃ§Ã£o em Lua para Roblox! Posso:\n\nâœ¨ **Criar scripts personalizados**\nðŸ”§ **Explicar conceitos de Lua**\nðŸŽ® **Ajudar com mecÃ¢nicas de jogo**\nðŸ–¥ï¸ **Desenvolver interfaces**\nðŸ“š **Fornecer templates e exemplos**\n\nApenas me diga o que vocÃª precisa e eu criarei o cÃ³digo para vocÃª!"
        }
    }
}

-- FunÃ§Ã£o para gerar resposta da IA
local function generateAIResponse(userMessage)
    local message = string.lower(userMessage)
    
    for category, data in pairs(AI_RESPONSES) do
        for _, pattern in pairs(data.patterns) do
            if string.find(message, pattern) then
                local responses = data.responses
                return responses[math.random(1, #responses)]
            end
        end
    end
    
    -- Resposta padrÃ£o
    local defaultResponses = {
        "Interessante! Pode me dar mais detalhes sobre o que vocÃª estÃ¡ tentando fazer?",
        "Posso ajudÃ¡-lo com isso! Me conte mais sobre o que vocÃª precisa.",
        "Que legal! Compartilhe mais informaÃ§Ãµes e eu criarei o cÃ³digo para vocÃª!"
    }
    return defaultResponses[math.random(1, #defaultResponses)]
end

-- VariÃ¡veis globais para gerenciar o estado
local conversations = {}
local currentConversation = nil
local messagesArea = nil
local scriptTypes = {
    {value = "", label = "Geral"},
    {value = "server", label = "ServerScript"},
    {value = "client", label = "LocalScript"},
    {value = "module", label = "ModuleScript"},
    {value = "building", label = "Building"},
    {value = "gui", label = "Interface (GUI"}
}
local selectedScriptType = ""

-- ðŸŽ¨ Criar a interface principal (igual Ã  aplicaÃ§Ã£o web)
local function createMainInterface()
    -- ðŸ“± ScreenGui principal
    local screenGui = Instance.new("ScreenGui")
    screenGui.Name = "RobloxAssistantVista"
    screenGui.ResetOnSpawn = false
    screenGui.IgnoreGuiInset = false
    screenGui.Parent = playerGui
    
    -- ðŸŒŒ Fundo desktop Vista
    local desktopBg = Instance.new("Frame")
    desktopBg.Name = "DesktopBackground"
    desktopBg.Size = UDim2.new(1, 0, 1, 0)
    desktopBg.Position = UDim2.new(0, 0, 0, 0)
    desktopBg.BackgroundColor3 = VISTA_CONFIG.DesktopBg
    desktopBg.BorderSizePixel = 0
    desktopBg.ZIndex = 1
    desktopBg.Parent = screenGui
    
    -- Gradiente do fundo
    local bgGradient = Instance.new("UIGradient")
    bgGradient.Color = ColorSequence.new{
        ColorSequenceKeypoint.new(0, Color3.fromRGB(195, 230, 255)),  -- Azul claro
        ColorSequenceKeypoint.new(1, VISTA_CONFIG.PrimaryColor)       -- Azul Vista
    }
    bgGradient.Rotation = 135
    bgGradient.Parent = desktopBg
    
    -- ðŸ’¼ Janela principal (design Vista)
    local mainWindow = Instance.new("Frame")
    mainWindow.Name = "MainWindow"
    mainWindow.Size = UDim2.new(0, 1000, 0, 700)  -- Maior como na aplicaÃ§Ã£o web
    mainWindow.Position = UDim2.new(0.5, -500, 0.5, -350)
    mainWindow.BackgroundColor3 = VISTA_CONFIG.WindowBg
    mainWindow.BorderColor3 = Color3.fromRGB(255, 255, 255)
    mainWindow.BorderSizePixel = 2
    mainWindow.Active = true
    mainWindow.Draggable = true
    mainWindow.ZIndex = 2
    mainWindow.Parent = screenGui
    
    -- Cantos arredondados Vista
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 12)
    corner.Parent = mainWindow
    
    -- ðŸŒ† Efeito de sombra Vista aprimorada
    local shadow = Instance.new("Frame")
    shadow.Name = "Shadow"
    shadow.Size = UDim2.new(1, 8, 1, 8)
    shadow.Position = UDim2.new(0, 4, 0, 4)
    shadow.BackgroundColor3 = Color3.fromRGB(31, 38, 135)
    shadow.BackgroundTransparency = 0.6
    shadow.BorderSizePixel = 0
    shadow.ZIndex = mainWindow.ZIndex - 1
    shadow.Parent = screenGui
    
    -- Cantos arredondados para sombra
    local shadowCorner = Instance.new("UICorner")
    shadowCorner.CornerRadius = UDim.new(0, 12)
    shadowCorner.Parent = shadow
    
    -- ðŸ“Ž Barra de tÃ­tulo Vista (estilo aplicaÃ§Ã£o web)
    local titleBar = Instance.new("Frame")
    titleBar.Name = "TitleBar"
    titleBar.Size = UDim2.new(1, 0, 0, 44)
    titleBar.Position = UDim2.new(0, 0, 0, 0)
    titleBar.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
    titleBar.BorderSizePixel = 0
    titleBar.ZIndex = 3
    titleBar.Parent = mainWindow
    
    -- Gradiente sutil da barra de tÃ­tulo
    local titleGradient = Instance.new("UIGradient")
    titleGradient.Color = ColorSequence.new{
        ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 255, 255)),
        ColorSequenceKeypoint.new(1, Color3.fromRGB(230, 235, 245))
    }
    titleGradient.Rotation = 180
    titleGradient.Parent = titleBar
    
    -- Cantos superiores arredondados
    local titleCorner = Instance.new("UICorner")
    titleCorner.CornerRadius = UDim.new(0, 12)
    titleCorner.Parent = titleBar
    
    -- ðŸš€ Ãcone e tÃ­tulo da janela
    local titleIcon = Instance.new("ImageLabel")
    titleIcon.Name = "TitleIcon"
    titleIcon.Size = UDim2.new(0, 24, 0, 24)
    titleIcon.Position = UDim2.new(0, 10, 0, 10)
    titleIcon.BackgroundTransparency = 1
    titleIcon.Image = "rbxasset://textures/ui/GuiImagePlaceholder.png"  -- Ãcone placeholder
    titleIcon.ImageColor3 = VISTA_CONFIG.PrimaryColor
    titleIcon.ZIndex = 4
    titleIcon.Parent = titleBar
    
    local titleLabel = Instance.new("TextLabel")
    titleLabel.Name = "TitleLabel"
    titleLabel.Size = UDim2.new(1, -120, 1, 0)
    titleLabel.Position = UDim2.new(0, 40, 0, 0)
    titleLabel.BackgroundTransparency = 1
    titleLabel.Text = "Roblox Lua Assistant"
    titleLabel.TextColor3 = VISTA_CONFIG.TextColor
    titleLabel.TextSize = 14
    titleLabel.Font = Enum.Font.SourceSansBold
    titleLabel.TextXAlignment = Enum.TextXAlignment.Left
    titleLabel.ZIndex = 4
    titleLabel.Parent = titleBar
    
    -- ðŸŸ£ BotÃµes da janela (estilo Vista)
    local buttonContainer = Instance.new("Frame")
    buttonContainer.Name = "ButtonContainer"
    buttonContainer.Size = UDim2.new(0, 90, 1, 0)
    buttonContainer.Position = UDim2.new(1, -90, 0, 0)
    buttonContainer.BackgroundTransparency = 1
    buttonContainer.ZIndex = 4
    buttonContainer.Parent = titleBar
    
    -- BotÃ£o minimizar
    local minimizeButton = Instance.new("TextButton")
    minimizeButton.Name = "MinimizeButton"
    minimizeButton.Size = UDim2.new(0, 24, 0, 24)
    minimizeButton.Position = UDim2.new(0, 5, 0, 10)
    minimizeButton.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
    minimizeButton.Text = "âˆ’"
    minimizeButton.TextColor3 = VISTA_CONFIG.TextColor
    minimizeButton.TextSize = 12
    minimizeButton.Font = Enum.Font.SourceSansBold
    minimizeButton.BorderSizePixel = 1
    minimizeButton.BorderColor3 = VISTA_CONFIG.BorderColor
    minimizeButton.ZIndex = 5
    minimizeButton.Parent = buttonContainer
    
    local minCorner = Instance.new("UICorner")
    minCorner.CornerRadius = UDim.new(0, 4)
    minCorner.Parent = minimizeButton
    
    -- BotÃ£o maximizar
    local maximizeButton = Instance.new("TextButton")
    maximizeButton.Name = "MaximizeButton"
    maximizeButton.Size = UDim2.new(0, 24, 0, 24)
    maximizeButton.Position = UDim2.new(0, 32, 0, 10)
    maximizeButton.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
    maximizeButton.Text = "â–¡"
    maximizeButton.TextColor3 = VISTA_CONFIG.TextColor
    maximizeButton.TextSize = 12
    maximizeButton.Font = Enum.Font.SourceSansBold
    maximizeButton.BorderSizePixel = 1
    maximizeButton.BorderColor3 = VISTA_CONFIG.BorderColor
    maximizeButton.ZIndex = 5
    maximizeButton.Parent = buttonContainer
    
    local maxCorner = Instance.new("UICorner")
    maxCorner.CornerRadius = UDim.new(0, 4)
    maxCorner.Parent = maximizeButton
    
    -- BotÃ£o fechar
    local closeButton = Instance.new("TextButton")
    closeButton.Name = "CloseButton"
    closeButton.Size = UDim2.new(0, 24, 0, 24)
    closeButton.Position = UDim2.new(0, 59, 0, 10)
    closeButton.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
    closeButton.Text = "âœ•"
    closeButton.TextColor3 = VISTA_CONFIG.TextColor
    closeButton.TextSize = 12
    closeButton.Font = Enum.Font.SourceSansBold
    closeButton.BorderSizePixel = 1
    closeButton.BorderColor3 = VISTA_CONFIG.BorderColor
    closeButton.ZIndex = 5
    closeButton.Parent = buttonContainer
    
    local closeCorner = Instance.new("UICorner")
    closeCorner.CornerRadius = UDim.new(0, 4)
    closeCorner.Parent = closeButton
    
    -- Efeitos hover para os botÃµes
    for _, button in pairs({minimizeButton, maximizeButton, closeButton}) do
        button.MouseEnter:Connect(function()
            if button == closeButton then
                button.BackgroundColor3 = Color3.fromRGB(220, 50, 50)
                button.TextColor3 = Color3.white
            else
                button.BackgroundColor3 = Color3.fromRGB(240, 245, 255)
            end
        end)
        
        button.MouseLeave:Connect(function()
            button.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
            button.TextColor3 = VISTA_CONFIG.TextColor
        end)
    end
    
    -- ðŸ“‹ Ãrea de conteÃºdo (layout da aplicaÃ§Ã£o web)
    local contentArea = Instance.new("Frame")
    contentArea.Name = "ContentArea"
    contentArea.Size = UDim2.new(1, 0, 1, -44)
    contentArea.Position = UDim2.new(0, 0, 0, 44)
    contentArea.BackgroundTransparency = 1
    contentArea.ZIndex = 3
    contentArea.Parent = mainWindow
    
    -- ðŸ“‹ Painel lateral (igual Ã  aplicaÃ§Ã£o web)
    local sidePanel = Instance.new("Frame")
    sidePanel.Name = "SidePanel"
    sidePanel.Size = UDim2.new(0, 256, 1, 0)  -- 64 * 4 = 256 (w-64 no Tailwind)
    sidePanel.Position = UDim2.new(0, 0, 0, 0)
    sidePanel.BackgroundColor3 = VISTA_CONFIG.SidebarBg
    sidePanel.BorderSizePixel = 0
    sidePanel.ZIndex = 3
    sidePanel.Parent = contentArea
    
    -- Borda direita do sidebar
    local sidebarBorder = Instance.new("Frame")
    sidebarBorder.Name = "SidebarBorder"
    sidebarBorder.Size = UDim2.new(0, 2, 1, 0)
    sidebarBorder.Position = UDim2.new(1, -2, 0, 0)
    sidebarBorder.BackgroundColor3 = VISTA_CONFIG.BorderColor
    sidebarBorder.BorderSizePixel = 0
    sidebarBorder.ZIndex = 4
    sidebarBorder.Parent = sidePanel
    
    -- ðŸ†• BotÃ£o "Nova Conversa" (igual Ã  aplicaÃ§Ã£o web)
    local newConversationBtn = Instance.new("TextButton")
    newConversationBtn.Name = "NewConversationButton"
    newConversationBtn.Size = UDim2.new(1, -8, 0, 40)
    newConversationBtn.Position = UDim2.new(0, 4, 0, 4)
    newConversationBtn.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
    newConversationBtn.Text = "+  Nova Conversa"
    newConversationBtn.TextColor3 = VISTA_CONFIG.TextColor
    newConversationBtn.TextSize = 14
    newConversationBtn.Font = Enum.Font.SourceSansBold
    newConversationBtn.BorderSizePixel = 1
    newConversationBtn.BorderColor3 = VISTA_CONFIG.BorderColor
    newConversationBtn.ZIndex = 4
    newConversationBtn.Pk

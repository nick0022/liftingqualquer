local WindUI = loadstring(game:HttpGet("https://github.com/Footagesus/WindUI/releases/latest/download/main.lua"))()

-- Create window
local Window = WindUI:CreateWindow({
  Title = "Simulador de Levantamento Gigante",
  Icon = "door-open",
  Author = "lk",
  Folder = "MoonHub",
  Size = UDim2.fromOffset(580, 460),
  Transparent = true,
  Theme = "Dark",
  Resizable = true,
  SideBarWidth = 200,
  Background = "", -- rbxassetid only
  BackgroundImageTransparency = 0.42,
  HideSearchBar = true,
  ScrollBarEnabled = false,
  User = {
      Enabled = true,
      Anonymous = false,
      Callback = function()
          print("clicked")
      end,
  },
  KeySystem = { -- <- ↓ remove this all, if you dont neet the key system
      Key = { "1234", "5678" },
      Note = "Example Key System. 1234",
      Thumbnail = {
          Image = "rbxassetid://",
          Title = "Thumbnail",
      },
      URL = "https://github.com/Footagesus/WindUI",
      SaveKey = true,
  },
})

-- Create tabs
local Tabs = {
    Status = Window:Tab({Title = "Status", Icon = "package"}),
    Farm = Window:Tab({Title = "Farm", Icon = "package"}),
}

Window:SelectTab(1)


local StatusSection = Tabs.Farm:Section({
  Title = "Status do Player",
  Icon = "bird",
  Opened = true,
 })

local Players = game:GetService("Players")
local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui") -- Essencial para buscar os tempos

-- Criação do Parágrafo de Status
-- Usamos textos iniciais que serão substituídos na primeira atualização.
local StatusParagraph = Tabs.Status:Paragraph({
    Title = "Estatísticas de " .. player.DisplayName,
    Desc = "Carregando estatísticas...",
    Color = "Blue", -- Cor personalizada
    Thumbnail = "rbxassetid://16723221995", -- Ícone de "gráfico/stats"
    ThumbnailSize = 80,
    Locked = false,
    Buttons = {} 
})

-- Função para ATUALIZAR as informações do parágrafo
local function updateStatus()
    -- 1. Buscar os valores do 'leaderstats'
    local leaderstats = player:FindFirstChild("leaderstats")
    
    -- Usamos 'or "N/A"' para o caso do valor ainda não existir
    local strength = leaderstats and leaderstats:FindFirstChild("Strength") and leaderstats.Strength.Value or "N/A"
    local coins = leaderstats and leaderstats:FindFirstChild("Coins") and leaderstats.Coins.Value or "N/A"
    local stage = leaderstats and leaderstats:FindFirstChild("Stage") and leaderstats.Stage.Value or "N/A"
    local kills = leaderstats and leaderstats:FindFirstChild("Kills") and leaderstats.Kills.Value or "N/A"

    -- 2. Buscar os valores de TEMPO da Interface Gráfica (PlayerGui)
    -- Este caminho é longo, então buscamos de forma segura passo a passo
    local timeWeight = "Buscando..."
    local timeStage = "Buscando..."
    local timeRebirth = "Buscando..."

    local mainGui = playerGui:FindFirstChild("Main")
    if mainGui then
        local boostsFrame = mainGui:FindFirstChild("Boosts")
        if boostsFrame then
            -- Tempo para o Próximo Peso
            local untilNextWeight = boostsFrame:FindFirstChild("UntilNextWeight")
            if untilNextWeight and untilNextWeight:FindFirstChild("Frame") and untilNextWeight.Frame:FindFirstChild("Button") and untilNextWeight.Frame.Button:FindFirstChild("Text") and untilNextWeight.Frame.Button.Text:FindFirstChild("Title") then
                timeWeight = untilNextWeight.Frame.Button.Text.Title.Text
            end
            
            -- Tempo para o Próximo Estágio
            local untilNextStage = boostsFrame:FindFirstChild("UntilNextStage")
            if untilNextStage and untilNextStage:FindFirstChild("Frame") and untilNextStage.Frame:FindFirstChild("Button") and untilNextStage.Frame.Button:FindFirstChild("Text") and untilNextStage.Frame.Button.Text:FindFirstChild("Title") then
                timeStage = untilNextStage.Frame.Button.Text.Title.Text
            end

            -- Tempo para o Próximo Renascimento
            local untilRebirth = boostsFrame:FindFirstChild("UntilRebirth")
            if untilRebirth and untilRebirth:FindFirstChild("Frame") and untilRebirth.Frame:FindFirstChild("Button") and untilRebirth.Frame.Button:FindFirstChild("Text") and untilRebirth.Frame.Button.Text:FindFirstChild("Title") then
                timeRebirth = untilRebirth.Frame.Button.Text.Title.Text
            end
        end
    end
    
    -- 3. Formatar todas as informações para exibição
    -- \n\n cria uma linha em branco para separar os blocos de status.
    local statusDescription = string.format(
        "💪 Força: %s\n" ..
        "🪙 Moedas: %s\n" ..
        "🔪 Abates: %s\n" ..
        "🗺️ Estágio: %s\n\n" .. -- Linha de separação
        "⏱️ Próximo Peso: %s\n" ..
        "⏱️ Próximo Estágio: %s\n" ..
        "🔄 Próximo Renascimento: %s",
        tostring(strength), 
        tostring(coins), 
        tostring(kills), 
        tostring(stage),
        timeWeight,
        timeStage,
        timeRebirth
    )

    -- 4. Atualizar o parágrafo na UI
    StatusParagraph:SetDesc(statusDescription)
end

-- Loop de Atualização
-- Roda a cada 1 segundo para manter as informações atualizadas sem sobrecarregar.
task.spawn(function()
    while task.wait(1) do
        -- 'pcall' executa a função de forma segura, prevenindo erros de parar o script.
        pcall(updateStatus)
    end
end)


local PrincipalSection = Tabs.Farm:Section({
   Title = "Funções Principais",
   Icon = "bird",
   Opened = true,
  })
  
  -- Variável para controlar se a função de auto levantar está ativa
  local autoLevantarAtivo = false
  
  -- Inicia um loop em uma nova thread para não travar o jogo
  task.spawn(function()
      while true do
          -- A cada 0.1 segundos, verifica se a função está ativa
          if autoLevantarAtivo then
              local args = {
                  "24"
              }
              game:GetService("ReplicatedStorage"):WaitForChild("2663824b812c4e1e80abcb37b7ea983c"):FireServer(unpack(args))
          end
          task.wait(0.1) -- Usar task.wait é a prática moderna no lugar de wait()
      end
  end)


    -- Variável para controlar se a função de auto levantar está ativa
    local autoComprarAtivo = false
  
    -- Inicia um loop em uma nova thread para não travar o jogo
    task.spawn(function()
        while true do
            -- A cada 0.1 segundos, verifica se a função está ativa
            if autoComprarAtivo then
              local args = {
                "BUYALLWEIGHTS"
              }
              game:GetService("ReplicatedStorage"):WaitForChild("1eb183c1aa464630895584d6306f8a3c"):InvokeServer(unpack(args))              
            end
            task.wait(1) -- Usar task.wait é a prática moderna no lugar de wait()
        end
    end)
  

    -- Variável para controlar se a função de auto levantar está ativa
    local autoEquipBestAtivo = false
  
    -- Inicia um loop em uma nova thread para não travar o jogo
    task.spawn(function()
        while true do
            -- A cada 0.1 segundos, verifica se a função está ativa
            if autoEquipBestAtivo then
              local args = {
                "EQUIPBEST"
              }
              game:GetService("ReplicatedStorage"):WaitForChild("7c467027f4c5402bb8b0472abe80d31c"):FireServer(unpack(args))                        
            end
            task.wait(1) -- Usar task.wait é a prática moderna no lugar de wait()
        end
    end)


        -- Variável para controlar se a função de auto levantar está ativa
        local autoRollCrateAtivo = false
  
        -- Inicia um loop em uma nova thread para não travar o jogo
        task.spawn(function()
            while true do
                -- A cada 0.1 segundos, verifica se a função está ativa
                if autoRollCrateAtivo then
                  local args = {
                    "ROLL",
                    "WoodenCrate"
                  }
                  game:GetService("ReplicatedStorage"):WaitForChild("PetsEvent"):FireServer(unpack(args))                                        
                end
                task.wait(0.5) -- Usar task.wait é a prática moderna no lugar de wait()
            end
        end)


  local Levantar = Tabs.Farm:Toggle({
   Title = "auto levantar",
   Desc = "funciona sem precisar estar com o peso selecionado",
   Icon = "bird",
   Type = "Checkbox",
   Default = false,
   Callback = function(state)
      -- A função do toggle agora é apenas mudar o estado da nossa variável de controle
      autoLevantarAtivo = state
  end
  })

  local Comprar = Tabs.Farm:Toggle({
    Title = "Auto Comprar",
    Desc = "Compra todos os pesos disponiveis automaticamente",
    Icon = "bird",
    Type = "Checkbox",
    Default = false,
    Callback = function(state) 

      autoComprarAtivo = state
    end
})

local EquipBest = Tabs.Farm:Toggle({
  Title = "Auto Equipar melhor pet",
  Desc = "equipa seus melhores pets automaticamente",
  Icon = "bird",
  Type = "Checkbox",
  Default = false,
  Callback = function(state) 

    autoEquipBestAtivo = state
  end
})

local RollCrate = Tabs.Farm:Toggle({
  Title = "Auto Girar caixa",
  Desc = "gira caixa automaticamente",
  Icon = "bird",
  Type = "Checkbox",
  Default = false,
  Callback = function(state) 

    autoRollCrateAtivo = state
  end
})

local Players = game:GetService("Players")
local player = Players.LocalPlayer

local SellTp = Tabs.Farm:Button({
    Title = "Vender e voltar",
    Desc = "Vende e retorna automaticamente",
    Locked = false,
    Callback = function()
        -- Verifica se o personagem do jogador existe
        if not player.Character or not player.Character:FindFirstChild("HumanoidRootPart") then
            print("Personagem não encontrado.")
            return
        end

        local character = player.Character
        local humanoidRootPart = character.HumanoidRootPart

        -- 1. Salvar a posição original do jogador
        local originalCFrame = humanoidRootPart.CFrame
        print("Posição original salva:", originalCFrame.Position)

        -- 2. Levar o jogador para a nova posição
        -- Em Roblox, é melhor manipular o CFrame para teleporte
        local newPosition = Vector3.new(-520, 13, -25)
        humanoidRootPart.CFrame = CFrame.new(newPosition)
        print("Teleportado para a nova posição.")

        -- 3. Aguardar 5 segundos
        task.wait(1) -- 'task.wait()' é a forma moderna e mais precisa de 'wait()'

        -- Verifica novamente se o personagem ainda existe antes de voltar
        if not player.Character or not player.Character:FindFirstChild("HumanoidRootPart") then
            print("Personagem não encontrado para retornar.")
            return
        end
        
        -- Garante que estamos movendo a parte correta, caso o personagem tenha sido recarregado
        humanoidRootPart = player.Character.HumanoidRootPart

        -- 4. Voltar para a posição original
        humanoidRootPart.CFrame = originalCFrame
        print("Retornou à posição original.")
    end
})

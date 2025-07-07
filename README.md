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
    Farm = Window:Tab({Title = "Farm", Icon = "package"}),
}

Window:SelectTab(1)


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

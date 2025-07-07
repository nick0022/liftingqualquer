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

--// Variáveis de Controle \\--
local toggleAtivado = false -- Controla se o toggle está ligado ou desligado.
local autoClickerThread = nil -- Armazena a nossa "thread" para que possamos pará-la depois.

--// Configuração - JÁ CONFIGURADO PARA VOCÊ \\--
-- Localiza o RemoteEvent específico que você encontrou.
local remoteDoClique = game:GetService("ReplicatedStorage"):WaitForChild("2663824b812c4e1e80abcb37b7ea983c")

-- Argumento que será enviado ao servidor a cada clique.
local argumentoDoClique = "24" --<< MUDANÇA AQUI: Adicionado o argumento que você forneceu.

--// Interface do Usuário (Seu código) \\--
-- Supondo que 'Tab' já foi definido pela sua UI Library.
local Toggle = Tabs.Farm:Toggle({
    Title = "Auto-Clique",
    Desc = "Ative para começar a clicar automaticamente.",
    Icon = "rbxassetid://6034823491", -- Ícone de um dedo clicando
    Type = "Checkbox",
    Default = false,
    Callback = function(state)
        toggleAtivado = state -- Atualiza nossa variável de controle com o estado do botão (true/false)
        
        if toggleAtivado then
            -- Se o botão foi LIGADO, criamos uma nova thread para o loop de cliques.
            print("Auto-Clique ATIVADO.")
            
            -- Usamos task.spawn para que o loop rode em segundo plano sem travar o jogo.
            autoClickerThread = task.spawn(function()
                -- O loop continuará enquanto o toggle estiver ativado.
                while toggleAtivado do
                    -- Dispara o evento para o servidor, simulando o clique com o argumento correto.
                    -- O 'pcall' garante que, se o remote der erro, o script não quebre.
                    pcall(function()
                        remoteDoClique:FireServer(argumentoDoClique) --<< MUDANÇA AQUI: Agora envia o argumento "24".
                    end)
                    
                    -- Espera um curto período antes do próximo clique.
                    -- IMPORTANTE: Não use um valor muito baixo (como 0) para não ser desconectado do jogo.
                    -- 0.2 segundos é um valor seguro e rápido. Ajuste conforme necessário.
                    task.wait(0.2) 
                end
            end)
        else
            -- Se o botão foi DESLIGADO, paramos a thread.
            print("Auto-Clique DESATIVADO.")
            if autoClickerThread then
                task.cancel(autoClickerThread) -- Cancela a thread do loop.
                autoClickerThread = nil -- Limpa a variável.
            end
        end
    end
})

print("Script de Auto-Clique (versão específica) carregado com sucesso!")

local ExtraSection = Tabs.Farm:Section({
  Title = "Funções Extras",
  Icon = "bird",
  Opened = true,
})

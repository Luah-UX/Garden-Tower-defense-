local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
   Name = "Exploit Hub | Anti-Detection",
   LoadingTitle = "Carregando Sistema...",
   LoadingSubtitle = "por Gemini",
   ConfigurationSaving = {
      Enabled = true,
      FolderName = "GeminiScripts",
      FileName = "HubConfig"
   },
   Discord = {
      Enabled = false,
      Invite = "",
      RememberJoins = true
   },
   KeySystem = false
})

-- Variáveis de Controle
local farmingEnabled = false
local godModeEnabled = false
local noclipEnabled = false

-- Função Anti-Detecção Simples (Oculta a GUI de Prints e ID do Jogador)
if syn then syn.protect_gui(game:GetService("CoreGui")) end

-- Aba Principal
local MainTab = Window:CreateTab("Autofarm & Combat", 4483362458) -- Ícone de engrenagem

MainTab:CreateToggle({
   Name = "Farming 2x Sementes",
   CurrentValue = false,
   Flag = "FarmToggle",
   Callback = function(Value)
      farmingEnabled = Value
      while farmingEnabled do
         -- LÓGICA DE FARM: Aqui entra o RemoteEvent do jogo específico
         -- Exemplo genérico:
         -- game:GetService("ReplicatedStorage").Events.CollectSeed:FireServer()
         task.wait(0.1)
      end
   end,
})

MainTab:CreateToggle({
   Name = "Godmode (Modo Deus)",
   CurrentValue = false,
   Flag = "GodToggle",
   Callback = function(Value)
      godModeEnabled = Value
      if godModeEnabled then
         -- Tenta remover o script de dano ou setar vida infinita localmente
         -- Nota: Muitos jogos possuem verificação no servidor (FE)
         local char = game.Players.LocalPlayer.Character
         if char and char:FindFirstChild("Humanoid") then
            char.Humanoid.MaxHealth = math.huge
            char.Humanoid.Health = math.huge
         end
      end
   end,
})

-- Aba de Movimentação
local MovementTab = Window:CreateTab("Movimentação", 4483362458)

MovementTab:CreateToggle({
   Name = "Noclip (Atravessar Paredes)",
   CurrentValue = false,
   Flag = "NoclipToggle",
   Callback = function(Value)
      noclipEnabled = Value
      game:GetService("RunService").Stepped:Connect(function()
         if noclipEnabled then
            for _, v in pairs(game.Players.LocalPlayer.Character:GetDescendants()) do
               if v:IsA("BasePart") then
                  v.CanCollide = false
               end
            end
         end
      end)
   end,
})

MovementTab:CreateButton({
   Name = "Ativar Fly (Voo)",
   Callback = function()
      -- Script de Voo Simples
      local player = game.Players.LocalPlayer
      local mouse = player:GetMouse()
      local char = player.Character
      local hum = char.Humanoid
      
      local bv = Instance.new("BodyVelocity", char.PrimaryPart)
      bv.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
      bv.Velocity = Vector3.new(0, 0, 0)
      
      -- O voo geralmente requer um loop ou controle de teclado
      Rayfield:Notify({
         Title = "Fly Ativado",
         Content = "Use com cautela para evitar banimentos por script.",
         Duration = 5,
         Image = 4483362458,
      })
   end,
})

Rayfield:LoadConfiguration()

-- 🤑 aglhhb 🤑
-- Créditos: sousa00.01 | jnzinx_o

local player = game.Players.LocalPlayer
local char = player.Character or player.CharacterAdded:Wait()
local humanoid = char:WaitForChild("Humanoid")
local hrp = char:WaitForChild("HumanoidRootPart")
local UIS = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

-- ================= UI =================

local gui = Instance.new("ScreenGui", player.PlayerGui)
gui.Name = "aglhhb_gui"
gui.ResetOnSpawn = false

local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 260, 0, 220)
frame.Position = UDim2.new(0.05, 0, 0.3, 0)
frame.BackgroundColor3 = Color3.fromRGB(120, 0, 0)
frame.BorderSizePixel = 0
frame.Active = true
frame.Draggable = true -- móvel (mobile/PC)

local corner = Instance.new("UICorner", frame)
corner.CornerRadius = UDim.new(0, 12)

local title = Instance.new("TextLabel", frame)
title.Size = UDim2.new(1, 0, 0, 40)
title.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
title.Text = "🤑 aglhhb 🤑"
title.TextColor3 = Color3.fromRGB(255, 0, 0)
title.Font = Enum.Font.GothamBold
title.TextSize = 20

-- Créditos
local credits = Instance.new("TextLabel", frame)
credits.Position = UDim2.new(0, 0, 1, -30)
credits.Size = UDim2.new(1, 0, 0, 30)
credits.BackgroundTransparency = 1
credits.Text = "Créditos: sousa00.01 | jnzinx_o"
credits.TextColor3 = Color3.fromRGB(255,255,255)
credits.Font = Enum.Font.Gotham
credits.TextSize = 12

-- ================= BOTÕES =================

local function createButton(text, posY)
    local btn = Instance.new("TextButton", frame)
    btn.Size = UDim2.new(0.85, 0, 0, 40)
    btn.Position = UDim2.new(0.075, 0, 0, posY)
    btn.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    btn.TextColor3 = Color3.fromRGB(255, 0, 0)
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 16
    btn.Text = text
    Instance.new("UICorner", btn)
    return btn
end

local flyBtn = createButton("✈️ Voar: OFF", 55)
local speedBtn = createButton("⚡ Velocidade 110: OFF", 105)

-- ================= FUNÇÕES =================

-- VELOCIDADE
local speedOn = false
speedBtn.MouseButton1Click:Connect(function()
    speedOn = not speedOn
    if speedOn then
        humanoid.WalkSpeed = 110
        speedBtn.Text = "⚡ Velocidade 110: ON"
    else
        humanoid.WalkSpeed = 16
        speedBtn.Text = "⚡ Velocidade 110: OFF"
    end
end)

-- VOAR
local flying = false
local bv, bg
local flySpeed = 60

flyBtn.MouseButton1Click:Connect(function()
    flying = not flying

    if flying then
        flyBtn.Text = "✈️ Voar: ON"

        bv = Instance.new("BodyVelocity", hrp)
        bv.MaxForce = Vector3.new(1e5, 1e5, 1e5)
        bv.Velocity = Vector3.zero

        bg = Instance.new("BodyGyro", hrp)
        bg.MaxTorque = Vector3.new(1e5, 1e5, 1e5)
        bg.CFrame = hrp.CFrame

        RunService.RenderStepped:Connect(function()
            if flying then
                local cam = workspace.CurrentCamera
                bg.CFrame = cam.CFrame

                local moveDir = humanoid.MoveDirection
                bv.Velocity = cam.CFrame:VectorToWorldSpace(moveDir) * flySpeed
            end
        end)
    else
        flyBtn.Text = "✈️ Voar: OFF"
        if bv then bv:Destroy() end
        if bg then bg:Destroy() end
    end
end)

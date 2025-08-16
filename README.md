-- GUI erstellen
local ScreenGui = Instance.new("ScreenGui", game.CoreGui)

local Frame = Instance.new("Frame", ScreenGui)
Frame.Size = UDim2.new(0, 200, 0, 100)
Frame.Position = UDim2.new(0.5, -100, 0.5, -50)
Frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)

local Button = Instance.new("TextButton", Frame)
Button.Size = UDim2.new(1, 0, 1, 0)
Button.Text = "Fly ON/OFF"
Button.BackgroundColor3 = Color3.fromRGB(60, 60, 60)

-- Fly Script
local flying = false
local player = game.Players.LocalPlayer
local mouse = player:GetMouse()

local function toggleFly()
    flying = not flying
    local char = player.Character or player.CharacterAdded:Wait()
    local hrp = char:WaitForChild("HumanoidRootPart")
    local bv, bg

    if flying then
        bv = Instance.new("BodyVelocity")
        bv.MaxForce = Vector3.new(4000,4000,4000)
        bv.Velocity = Vector3.zero
        bv.Parent = hrp

        bg = Instance.new("BodyGyro")
        bg.MaxTorque = Vector3.new(4000,4000,4000)
        bg.CFrame = hrp.CFrame
        bg.Parent = hrp

        -- Bewegung
        game:GetService("RunService").RenderStepped:Connect(function()
            if flying and bv and bg then
                local camCF = workspace.CurrentCamera.CFrame
                local move = Vector3.new()

                if game.UserInputService:IsKeyDown(Enum.KeyCode.W) then
                    move = move + camCF.LookVector
                end
                if game.UserInputService:IsKeyDown(Enum.KeyCode.S) then
                    move = move - camCF.LookVector
                end
                if game.UserInputService:IsKeyDown(Enum.KeyCode.A) then
                    move = move - camCF.RightVector
                end
                if game.UserInputService:IsKeyDown(Enum.KeyCode.D) then
                    move = move + camCF.RightVector
                end
                if game.UserInputService:IsKeyDown(Enum.KeyCode.Space) then
                    move = move + Vector3.new(0,1,0)
                end
                if game.UserInputService:IsKeyDown(Enum.KeyCode.LeftShift) then
                    move = move - Vector3.new(0,1,0)
                end

                bv.Velocity = move * 50 -- Geschwindigkeit
                bg.CFrame = camCF
            end
        end)
    else
        if hrp:FindFirstChildOfClass("BodyVelocity") then
            hrp:FindFirstChildOfClass("BodyVelocity"):Destroy()
        end
        if hrp:FindFirstChildOfClass("BodyGyro") then
            hrp:FindFirstChildOfClass("BodyGyro"):Destroy()
        end
    end
end

-- Button Klick
Button.MouseButton1Click:Connect(toggleFly)

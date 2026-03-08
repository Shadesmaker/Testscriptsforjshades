-- [[ MULTI-TOOL GUI WITH TOGGLE ]] --
local Players = game:GetService("Players")
local Lighting = game:GetService("Lighting")
local TeleportService = game:GetService("TeleportService")
local player = Players.LocalPlayer

-- 1. Create the Main ScreenGui
local ScreenGui = Instance.new("ScreenGui", player.PlayerGui)
ScreenGui.Name = "MyCustomExecutorGui"
ScreenGui.ResetOnSpawn = false -- Keeps GUI active after you die

-- 2. Create the "White Square" Toggle Button
local ToggleBtn = Instance.new("TextButton", ScreenGui)
ToggleBtn.Size = UDim2.new(0, 30, 0, 30) -- Small square
ToggleBtn.Position = UDim2.new(0, 10, 0, 10) -- Top left corner
ToggleBtn.BackgroundColor3 = Color3.new(1, 1, 1) -- Pure white
ToggleBtn.Text = "" -- No text, just a square
ToggleBtn.BorderSizePixel = 0

-- 3. Create the Main Menu Frame
local MainFrame = Instance.new("Frame", ScreenGui)
MainFrame.Size = UDim2.new(0, 220, 0, 320)
MainFrame.Position = UDim2.new(0.5, -110, 0.5, -160)
MainFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
MainFrame.BorderSizePixel = 2
MainFrame.Visible = true -- Starts visible
MainFrame.Active = true
MainFrame.Draggable = true -- You can move it around

-- Toggle Logic (Destroy/Rebuild view)
ToggleBtn.MouseButton1Click:Connect(function()
    MainFrame.Visible = not MainFrame.Visible
end)

-- Helper function for buttons
local function createButton(text, pos, callback)
    local btn = Instance.new("TextButton", MainFrame)
    btn.Size = UDim2.new(0, 200, 0, 35)
    btn.Position = pos
    btn.Text = text
    btn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    btn.TextColor3 = Color3.new(1, 1, 1)
    btn.MouseButton1Click:Connect(callback)
    return btn
end

-- --- FEATURES --- --

-- 1. SKYBOX (Avatar Style)
createButton("Avatar Skybox", UDim2.new(0, 10, 0, 10), function()
    local content = Players:GetUserThumbnailAsync(player.UserId, Enum.ThumbnailType.HeadShot, Enum.ThumbnailSize.Size420x420)
    local sky = Instance.new("Sky", Lighting)
    sky.SkyboxBk, sky.SkyboxDn, sky.SkyboxFt, sky.SkyboxLf, sky.SkyboxRt, sky.SkyboxUp = content, content, content, content, content, content
end)

-- 2. SERVER HOP (Safe Escape)
createButton("Server Hop", UDim2.new(0, 10, 0, 50), function()
    local servers = game:GetService("HttpService"):JSONDecode(game:HttpGet("https://games.roblox.com" .. game.PlaceId .. "/servers/Public?sortOrder=Desc&limit=100")).data
    for _, s in pairs(servers) do
        if s.playing < s.maxPlayers and s.id ~= game.JobId then
            TeleportService:TeleportToPlaceInstance(game.PlaceId, s.id)
            break
        end
    end
end)

-- 3. INFINITE JUMP
local InfJump = true
game:GetService("UserInputService").JumpRequest:Connect(function()
    if InfJump then player.Character:FindFirstChildOfClass('Humanoid'):ChangeState("Jumping") end
end)
createButton("Toggle Inf Jump", UDim2.new(0, 10, 0, 90), function()
    InfJump = not InfJump
end)

-- 4. EDITABLE OVERHEAD TITLE
local titleBox = Instance.new("TextBox", MainFrame)
titleBox.Size = UDim2.new(0, 200, 0, 35)
titleBox.Position = UDim2.new(0, 10, 0, 135)
titleBox.PlaceholderText = "Type Title & Enter"
titleBox.FocusLost:Connect(function()
    local head = player.Character.Head
    if head:FindFirstChild("MyTitle") then head.MyTitle:Destroy() end
    local bg = Instance.new("BillboardGui", head)
    bg.Name = "MyTitle"
    bg.Size = UDim2.new(0, 200, 0, 50)
    bg.StudsOffset = Vector3.new(0, 3, 0)
    local lbl = Instance.new("TextLabel", bg)
    lbl.Size = UDim2.new(1, 0, 1, 0)
    lbl.Text = titleBox.Text
    lbl.BackgroundTransparency = 1
    lbl.TextColor3 = Color3.new(1, 1, 1)
end)

-- 5. CLIENT MESSAGE (5 Seconds)
local msgBox = Instance.new("TextBox", MainFrame)
msgBox.Size = UDim2.new(0, 200, 0, 35)
msgBox.Position = UDim2.new(0, 10, 0, 180)
msgBox.PlaceholderText = "Your Message Here"
msgBox.FocusLost:Connect(function()
    local mGui = Instance.new("ScreenGui", player.PlayerGui)
    local msg = Instance.new("TextLabel", mGui)
    msg.Size = UDim2.new(1, 0, 0, 100)
    msg.Position = UDim2.new(0, 0, 0.4, 0)
    msg.Text = msgBox.Text
    msg.TextScaled = true
    msg.BackgroundColor3 = Color3.new(0,0,0)
    msg.TextColor3 = Color3.new(1,1,1)
    task.wait(5)
    mGui:Destroy()
end)

-- 6. DESTROY SCRIPT COMPLETELY (Quick Exit)
createButton("DESTROY ALL", UDim2.new(0, 10, 0, 275), function()
    ScreenGui:Destroy()
end)

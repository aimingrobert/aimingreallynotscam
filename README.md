local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
   Name = "24k flur",
   Icon = 0, -- Icon in Topbar. Can use Lucide Icons (string) or Roblox Image (number). 0 to use no icon (default).
   LoadingTitle = "24k flur",
   LoadingSubtitle = "Balls",
   Theme = "Default", -- Check https://docs.sirius.menu/rayfield/configuration/themes

   DisableRayfieldPrompts = false,
   DisableBuildWarnings = false, -- Prevents Rayfield from warning when the script has a version mismatch with the interface

   ConfigurationSaving = {
      Enabled = true,
      FolderName = nil, -- Create a custom folder for your hub/game
      FileName = "24kFlur"
   },

   Discord = {
      Enabled = true, -- Prompt the user to join your Discord server if their executor supports it
      Invite = "https://discord.gg/ZtE7X6pGDa", -- The Discord invite code, do not include discord.gg/. E.g. discord.gg/ ABCD would be ABCD
      RememberJoins = true -- Set this to false to make them join the discord every time they load it up
   },

   KeySystem = false, -- Set this to true to use our key system
   KeySettings = {
      Title = "24kFlur Key system",
      Subtitle = "Link in Discord Server",
      Note = "go into my yt channel ghostcrosssyk and bio there is the discord and buy there", -- Use this to tell the user how to get a key
      FileName = "1x0error", -- It is recommended to use something unique as other scripts using Rayfield may overwrite your key file
      SaveKey = true, -- The user's key will be saved, but if you change the key, they will be unable to use your script
      GrabKeyFromSite = true, -- If this is true, set Key below to the RAW site you would like Rayfield to get the key from
      Key = {"https://pastebin.com/raw/36HbadVF"} -- List of keys that will be accepted by the system, can be RAW file links (pastebin, github etc) or simple strings ("hello","key22")
   }
})

local MainTab = Window:CreateTab("Home", nil) -- Title, Image
local MainSection = MainTab:CreateSection("Main")

local Button = MainTab:CreateButton({
   Name = "Aimlock(Z)",
   Callback = function()
   local UserInputService = game:GetService("UserInputService")
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera
local LocalPlayer = Players.LocalPlayer

local lockedTarget = nil
local isLocked = false

local function getClosestPlayer()
    local closestPlayer = nil
    local shortestDistance = math.huge

    for _, player in pairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("Head") then
            local head = player.Character.Head
            local headPosition, onScreen = Camera:WorldToViewportPoint(head.Position)
            if onScreen then
                local distance = (Vector2.new(headPosition.X, headPosition.Y) - UserInputService:GetMouseLocation()).magnitude
                if distance < shortestDistance then
                    shortestDistance = distance
                    closestPlayer = playerа
                end
            end
        end
    end

    return closestPlayer
end

local function toggleLock()
    if isLocked then
        -- Unlock camera
        isLocked = false
        lockedTarget = nil
    else
        -- Lock onto the closest player's head
        local target = getClosestPlayer()
        if target and target.Character and target.Character:FindFirstChild("Head") then
            lockedTarget = target
            isLocked = true
        end
    end
end

RunService.RenderStepped:Connect(function()
    if isLocked and lockedTarget and lockedTarget.Character and lockedTarget.Character:FindFirstChild("Head") then
        Camera.CFrame = CFrame.new(Camera.CFrame.Position, lockedTarget.Character.Head.Position)
    elseif isLocked then
        -- If target is gone (died or left), reset lock
        isLocked = false
        lockedTarget = nil
    end
end)

UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if not gameProcessed and input.KeyCode == Enum.KeyCode.Z then
        toggleLock()
    end
end)

   end,
})

local Slider = MainTab:CreateSlider({
   Name = "Walkspeed slider only in other games",
   Range = {0, 300},
   Increment = 1,
   Suffix = "Speed",
   CurrentValue = 16,
   Flag = "example", -- A flag is the identifier for the configuration file, make sure every element has a different flag if you're using configuration saving to ensure no overlaps
   Callback = function(Value)
        game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = (Value)
   end,
})

local Button = MainTab:CreateButton({
   Name = "Fly(X)",
   Callback = function()
local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local Workspace = game:GetService("Workspace")

local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")
local rootPart = character:WaitForChild("HumanoidRootPart")

local flying = false
local moveDirection = Vector3.new(0, 0, 0)
local speed = 250  -- Changed speed from 100 to 250

-- Function to start flying
local function startFlying()
    if flying then return end
    flying = true
    
    -- Use AssemblyLinearVelocity instead of BodyVelocity to avoid anti-cheat detection
    rootPart.AssemblyLinearVelocity = Vector3.new(0, 0, 0)
end

-- Function to stop flying
local function stopFlying()
    if not flying then return end
    flying = false
    rootPart.AssemblyLinearVelocity = Vector3.new(0, 0, 0)
end

-- Function to update movement
local function updateMovement()
    if flying then
        local camera = Workspace.CurrentCamera
        local camLook = camera.CFrame.LookVector
        local rightVector = camera.CFrame.RightVector
        
        local move = Vector3.new(0, 0, 0)
        if UserInputService:IsKeyDown(Enum.KeyCode.W) then
            move = move + Vector3.new(camLook.X, 0, camLook.Z)
        end
        if UserInputService:IsKeyDown(Enum.KeyCode.S) then
            move = move - Vector3.new(camLook.X, 0, camLook.Z)
        end
        if UserInputService:IsKeyDown(Enum.KeyCode.A) then
            move = move - Vector3.new(rightVector.X, 0, rightVector.Z)
        end
        if UserInputService:IsKeyDown(Enum.KeyCode.D) then
            move = move + Vector3.new(rightVector.X, 0, rightVector.Z)
        end
        if UserInputService:IsKeyDown(Enum.KeyCode.Space) then
            move = move + Vector3.new(0, 1, 0)
        end
        if UserInputService:IsKeyDown(Enum.KeyCode.LeftShift) then
            move = move - Vector3.new(0, 1, 0)
        end
        
        if move.Magnitude > 0 then
            rootPart.AssemblyLinearVelocity = move.Unit * speed
        else
            rootPart.AssemblyLinearVelocity = Vector3.new(0, 0, 0)
        end
    end
end

-- Detect key presses for movement
UserInputService.InputBegan:Connect(function(input, processed)
    if processed then return end
    if input.KeyCode == Enum.KeyCode.X then
        if flying then
            stopFlying()
        else
            startFlying()
        end
    end
end)

-- Update movement continuously
RunService.RenderStepped:Connect(updateMovement)

   end,
})

local Button = MainTab:CreateButton({
   Name = "Canghing animation to goofy",
   Callback = function()
   local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

-- Replace these animation IDs with cool ones from the Roblox catalog
local animationIds = {
    Idle = "616006778",        -- Cool idle animation
    Walk = "616013216",        -- Smooth walking animation
    Run = "616013216",         -- Fast running animation
    Jump = "616008936",        -- Stylish jump animation
    Fall = "616005863",        -- Epic falling animation
    Climb = "616003713",       -- Parkour climb animation
    Swim = "616012453"         -- Cool swimming animation
}

local function applyAnimations(character)
    if not character then return end

    local animateScript = character:FindFirstChild("Animate")
    if not animateScript then return end

    -- Update animations
    animateScript.idle.Animation1.AnimationId = "rbxassetid://" .. animationIds.Idle
    animateScript.walk.WalkAnim.AnimationId = "rbxassetid://" .. animationIds.Walk
    animateScript.run.RunAnim.AnimationId = "rbxassetid://" .. animationIds.Run
    animateScript.jump.JumpAnim.AnimationId = "rbxassetid://" .. animationIds.Jump
    animateScript.fall.FallAnim.AnimationId = "rbxassetid://" .. animationIds.Fall
    animateScript.climb.ClimbAnim.AnimationId = "rbxassetid://" .. animationIds.Climb
    animateScript.swim.Swim.AnimationId = "rbxassetid://" .. animationIds.Swim
end

-- Apply animations when character spawns
LocalPlayer.CharacterAppearanceLoaded:Connect(function(character)
    applyAnimations(character)
end)

-- Apply animations immediately if already spawned
if LocalPlayer.Character then
    applyAnimations(LocalPlayer.Character)
end
   end,
})

local Button = MainTab:CreateButton({
   Name = "j3rk off(r6)",
   Callback = function()
   loadstring(game:HttpGet("https://pastefy.app/wa3v2Vgm/raw"))()
   end,
})

local Button = MainTab:CreateButton({
   Name = "Rejoin",
   Callback = function()
   local TeleportService = game:GetService("TeleportService")
local Players = game:GetService("Players")
local player = Players.LocalPlayer

TeleportService:Teleport(game.PlaceId, player) -- Rejoins the same server

   end,
})

local Button = MainTab:CreateButton({
   Name = "Tp ctrl plus left click",
   Callback = function()
   local UserInputService = game:GetService("UserInputService")
local Players = game:GetService("Players")
local player = Players.LocalPlayer
local mouse = player:GetMouse()

local ctrlHeld = false -- Track if CTRL is held

-- Detect when CTRL is pressed
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if input.KeyCode == Enum.KeyCode.LeftControl then
        ctrlHeld = true
    end
end)

-- Detect when CTRL is released
UserInputService.InputEnded:Connect(function(input, gameProcessed)
    if input.KeyCode == Enum.KeyCode.LeftControl then
        ctrlHeld = false
    end
end)

-- Detect left-click and teleport if CTRL is held
mouse.Button1Down:Connect(function()
    if ctrlHeld and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
        local targetPosition = mouse.Hit.Position -- Get clicked position
        player.Character.HumanoidRootPart.CFrame = CFrame.new(targetPosition + Vector3.new(0, 3, 0)) -- Teleport slightly above ground
    end
end)

   end,
})

local Button = MainTab:CreateButton({
   Name = "Wall bang",
   Callback = function()
   -- Create ScreenGui
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

-- Create Toggle Button
local ToggleButton = Instance.new("TextButton")
ToggleButton.Parent = ScreenGui
ToggleButton.Size = UDim2.new(0, 100, 0, 50)
ToggleButton.Position = UDim2.new(0.1, 0, 0.1, 0)
ToggleButton.Text = "OFF"
ToggleButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)

-- Variable to track toggle state
local deleteMode = false

-- Toggle button function
ToggleButton.MouseButton1Click:Connect(function()
    deleteMode = not deleteMode
    if deleteMode then
        ToggleButton.Text = "ON"
        ToggleButton.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
    else
        ToggleButton.Text = "OFF"
        ToggleButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
    end
end)

-- Detect clicks on the map
local Player = game.Players.LocalPlayer
local Mouse = Player:GetMouse()

Mouse.Button1Down:Connect(function()
    if deleteMode then
        local target = Mouse.Target
        if target and target:IsA("BasePart") then
            target:Destroy()
        end
    end
end)
   end,
})

local Button = MainTab:CreateButton({
   Name = "strafe(t)Have to be locking with lock",
   Callback = function()
local UserInputService = game:GetService("UserInputService")
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local Mouse = Players.LocalPlayer:GetMouse()

local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

local strafing = false
local targetTorso = nil
local baseRadius = 10     -- Distance from target
local baseSpeed = 500     -- Faster speed of strafing (500 for high speed)
local chaosFactor = 5     -- Unpredictable movement factor

-- Function to find the player's torso under the mouse (HumanoidRootPart)
local function findPlayerTorsoUnderMouse()
    local target = Mouse.Target
    if target and target.Parent then
        local targetCharacter = target.Parent
        local targetPlayer = Players:GetPlayerFromCharacter(targetCharacter)
        if targetPlayer and targetPlayer ~= player then
            return targetCharacter:FindFirstChild("HumanoidRootPart") -- Lock onto their torso
        end
    end
    return nil
end

-- Crazy strafe around the target's torso
local function startStrafing()
    if strafing then
        strafing = false
        return
    end

    targetTorso = findPlayerTorsoUnderMouse()
    if not targetTorso then
        warn("No player selected under the mouse!")
        return
    end

    strafing = true
    local angle = 0

    while strafing and targetTorso.Parent do
        local deltaTime = RunService.RenderStepped:Wait()
        angle = angle + (baseSpeed + math.random(-chaosFactor, chaosFactor)) * deltaTime

        -- Unpredictable movement radius
        local dynamicRadius = baseRadius + math.random(-chaosFactor, chaosFactor)

        -- Calculate new position around the target's torso
        local x = targetTorso.Position.X + math.cos(angle) * dynamicRadius
        local z = targetTorso.Position.Z + math.sin(angle) * dynamicRadius
        local y = targetTorso.Position.Y + math.sin(angle * 2) * chaosFactor -- Erratic height changes

        -- Lock onto the target's torso
        humanoidRootPart.CFrame = CFrame.new(Vector3.new(x, y, z), targetTorso.Position)
    end
end

-- Keybind to toggle crazy strafing
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if not gameProcessed and input.KeyCode == Enum.KeyCode.T then
        startStrafing()
    end
end)

   end,
})

local Button = MainTab:CreateButton({
   Name = "Noclip(v)",
   Callback = function()
   local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local Players = game:GetService("Players")

local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()

local noclip = false

-- Function to enable/disable noclip
local function toggleNoclip()
    noclip = not noclip
    if noclip then
        print("Noclip enabled")
    else
        print("Noclip disabled")
    end
end

-- Runs every frame to disable collisions if noclip is on
RunService.Stepped:Connect(function()
    if noclip and character then
        for _, part in pairs(character:GetChildren()) do
            if part:IsA("BasePart") then
                part.CanCollide = false -- Disable collisions
            end
        end
    end
end)

-- Keybind to toggle noclip (now using V)
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if not gameProcessed and input.KeyCode == Enum.KeyCode.V then
        toggleNoclip()
    end
end)

   end,
})

local Button = MainTab:CreateButton({
   Name = "Spin",
   Callback = function()
   local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:FindFirstChild("HumanoidRootPart")
local spinning = _G.spinning or false -- Store state globally

if humanoidRootPart then
    if spinning then
        _G.spinning = false
    else
        _G.spinning = true
        while _G.spinning do
            humanoidRootPart.CFrame = humanoidRootPart.CFrame * CFrame.Angles(0, math.rad(30), 0)
            task.wait(0.1)
        end
    end
end

   end,
})

local Button = MainTab:CreateButton({
   Name = "Esp",
   Callback = function()
   local players = game:GetService("Players")
local runService = game:GetService("RunService")
local localPlayer = players.LocalPlayer
local espEnabled = _G.espEnabled or false -- Store state globally

if espEnabled then
    _G.espEnabled = false
    for _, v in pairs(workspace:GetChildren()) do
        if v:IsA("Model") and v:FindFirstChild("ESP") then
            v:FindFirstChild("ESP"):Destroy()
        end
    end
else
    _G.espEnabled = true
    local function createESP(player)
        if player ~= localPlayer then
            local character = player.Character or player.CharacterAdded:Wait()
            if character and character:FindFirstChild("HumanoidRootPart") then
                local billboard = Instance.new("BillboardGui")
                billboard.Name = "ESP"
                billboard.Adornee = character:FindFirstChild("HumanoidRootPart")
                billboard.Size = UDim2.new(5, 0, 2, 0)
                billboard.StudsOffset = Vector3.new(0, 3, 0)
                billboard.AlwaysOnTop = true
                
                local nameLabel = Instance.new("TextLabel", billboard)
                nameLabel.Size = UDim2.new(1, 0, 0.5, 0)
                nameLabel.Text = player.Name
                nameLabel.BackgroundTransparency = 1
                nameLabel.TextColor3 = Color3.new(1, 1, 1)
                nameLabel.TextStrokeTransparency = 0.5
                
                local healthLabel = Instance.new("TextLabel", billboard)
                healthLabel.Size = UDim2.new(1, 0, 0.5, 0)
                healthLabel.Position = UDim2.new(0, 0, 0.5, 0)
                healthLabel.BackgroundTransparency = 1
                healthLabel.TextColor3 = Color3.new(0, 1, 0)
                healthLabel.TextStrokeTransparency = 0.5
                
                local function updateHealth()
                    while _G.espEnabled and character:FindFirstChild("Humanoid") do
                        healthLabel.Text = "Health: " .. math.floor(character.Humanoid.Health)
                        task.wait(0.1)
                    end
                end
                
                billboard.Parent = character
                task.spawn(updateHealth)
            end
        end
    end
    
    for _, player in pairs(players:GetPlayers()) do
        if player ~= localPlayer then
            createESP(player)
        end
    end
    
    players.PlayerAdded:Connect(createESP)
end

   end,
})

local Button = MainTab:CreateButton({
   Name = "Cool commands",
   Callback = function()
   local players = game:GetService("Players")
local runService = game:GetService("RunService")
local teleportService = game:GetService("TeleportService")
local localPlayer = players.LocalPlayer
local camera = workspace.CurrentCamera
local viewing = false

local function getPlayerByPartialName(partialName)
    partialName = partialName:lower()
    for _, player in pairs(players:GetPlayers()) do
        if player.Name:lower():sub(1, #partialName) == partialName or player.DisplayName:lower():sub(1, #partialName) == partialName then
            return player
        end
    end
    return nil
end

local function onChatted(message)
    local args = message:split(" ")
    local command = args[1]:lower()
    local targetName = table.concat(args, " ", 2)
    local targetPlayer = getPlayerByPartialName(targetName)
    
    if command == ".focus" and targetPlayer and targetPlayer.Character then
        local root = targetPlayer.Character:FindFirstChild("HumanoidRootPart")
        if root then
            camera.CameraSubject = root
        end
    elseif command == ".tp" and targetPlayer and targetPlayer.Character then
        local root = targetPlayer.Character:FindFirstChild("HumanoidRootPart")
        local localRoot = localPlayer.Character and localPlayer.Character:FindFirstChild("HumanoidRootPart")
        if root and localRoot then
            localRoot.CFrame = root.CFrame + Vector3.new(0, 3, 0) -- Teleport above the player
        end
    elseif command == ".view" then
        if viewing then
            camera.CameraSubject = localPlayer.Character:FindFirstChild("Humanoid")
            viewing = false
        elseif targetPlayer and targetPlayer.Character then
            local humanoid = targetPlayer.Character:FindFirstChild("Humanoid")
            if humanoid then
                camera.CameraSubject = humanoid
                viewing = true
            end
        end
    elseif command == ".rj" or command == ".rejoin" then
        teleportService:Teleport(game.PlaceId, localPlayer)
    end
end

localPlayer.Chatted:Connect(onChatted)

   end,
})

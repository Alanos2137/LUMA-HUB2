--WHAT YOU WANNA HERE





















local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
   Name = "LUNA HUB",
   Icon = 140407192788352, -- Icon in Topbar. Can use Lucide Icons (string) or Roblox Image (number). 0 to use no icon (default).
   LoadingTitle = "LOADING...",
   LoadingSubtitle = "by Progamershacke1230",
   Theme = "Amethyst", 

   DisableRayfieldPrompts = false,
   DisableBuildWarnings = false, -- Prevents Rayfield from warning when the script has a version mismatch with the interface

   ConfigurationSaving = {
      Enabled = true,
      FolderName = nil, -- Create a custom folder for your hub/game
      FileName = "LUNA"
   },

   Discord = {
      Enabled = false, -- Prompt the user to join your Discord server if their executor supports it
      Invite = "noinvitelink", -- The Discord invite code, do not include discord.gg/. E.g. discord.gg/ ABCD would be ABCD
      RememberJoins = true -- Set this to false to make them join the discord every time they load it up
   },

   KeySystem = true, 
   KeySettings = {
      Title = "THANK NOTE",
      Subtitle = "THANK FOR USE ME GUI ",
      Note = "JOIN DC FOR KEY https://discord.gg/SeVgGbKh", -- Use this to tell the user how to get a key
      FileName = "https://discord.gg/2223nPTg", -- It is recommended to use something unique as other scripts using Rayfield may overwrite your key file
      SaveKey = True, 
      GrabKeyFromSite = false, -- If this is true, set Key below to the RAW site you would like Rayfield to get the key from
      Key = {"2025"} 
   }
})

Rayfield:Notify({
   Title = "SCRIPT",
   Content = "MADE BY PROGAMERSHACKE1230",
   Duration = 6.5,
   Image = "heart",
})

local MAINTab = Window:CreateTab("CREDITS", "credit-card")
local MAINSection = MAINTab:CreateSection("MAIN")

local Label = MAINTab:CreateLabel("KEY RESET EVERY 3DAYS", "scan-eye")
local Label = MAINTab:CreateLabel("KEY ON https://discord.gg/2223nPTg", "scan-eye")

local MAINTab = Window:CreateTab("GUIS FOR GAMES", "user")
local MAINSection = MAINTab:CreateSection("BLOOD DEBT")

local Button = MAINTab:CreateButton({
   Name = "GUI BLOOD DEBT",
   Callback = function()
loadstring(game:HttpGet("https://raw.githubusercontent.com/ratlabsofficial/scripts/refs/heads/main/blooddebt"))()
   end,
})


local MAINSection = MAINTab:CreateSection("DEAD RAILS")

local Button = MAINTab:CreateButton({
   Name = "GUI DEAD RAILS",
   Callback = function()
loadstring(game:HttpGet("https://raw.githubusercontent.com/gumanba/Scripts/main/DeadRails"))()
   end,
})

local MAINSection = MAINTab:CreateSection("BROOKHAVEN")

local Button = MAINTab:CreateButton({
   Name = "MANGOHUB",
   Callback = function()
loadstring(game:HttpGet("https://raw.githubusercontent.com/rogelioajax/lua/main/MangoHub", true))()
   end,
})

local MAINSection = MAINTab:CreateSection("AIM BOT")

local Button = MAINTab:CreateButton({
   Name = "AIM BOT",
   Callback = function()
local teamCheck = false
local fov = 90
local smoothing = 1
local predictionFactor = 0  -- Adjust this factor to improve prediction accuracy
local highlightEnabled = false  -- Variable to enable or disable target highlighting. Change to False if using an ESP script.
local lockPart = "Head"  -- Choose what part it locks onto. Ex. HumanoidRootPart or Head
 
local Toggle = false  -- Enable or disable toggle mode
local ToggleKey = Enum.KeyCode.E  -- Choose the key for toggling aimbot lock
 
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local StarterGui = game:GetService("StarterGui")
local Players = game:GetService("Players")
 
StarterGui:SetCore("SendNotification", {
    Title = "Universal Aimbot";
    Text = "made by bran-bon";
    Duration = 5;
})
 
local FOVring = Drawing.new("Circle")
FOVring.Visible = true
FOVring.Thickness = 1
FOVring.Radius = fov
FOVring.Transparency = 0.8
FOVring.Color = Color3.fromRGB(255, 128, 128)
FOVring.Position = workspace.CurrentCamera.ViewportSize / 2
 
local currentTarget = nil
local aimbotEnabled = true
local toggleState = false  -- Variable to keep track of toggle state
local debounce = false  -- Debounce variable
 
local function getClosest(cframe)
    local ray = Ray.new(cframe.Position, cframe.LookVector).Unit
    local target = nil
    local mag = math.huge
    local screenCenter = workspace.CurrentCamera.ViewportSize / 2
 
    for i, v in pairs(Players:GetPlayers()) do
        if v.Character and v.Character:FindFirstChild(lockPart) and v.Character:FindFirstChild("Humanoid") and v.Character:FindFirstChild("HumanoidRootPart") and v ~= Players.LocalPlayer and (v.Team ~= Players.LocalPlayer.Team or (not teamCheck)) then
            local screenPoint, onScreen = workspace.CurrentCamera:WorldToViewportPoint(v.Character[lockPart].Position)
            local distanceFromCenter = (Vector2.new(screenPoint.X, screenPoint.Y) - screenCenter).Magnitude
 
            if onScreen and distanceFromCenter <= fov then
                local magBuf = (v.Character[lockPart].Position - ray:ClosestPoint(v.Character[lockPart].Position)).Magnitude
 
                if magBuf < mag then
                    mag = magBuf
                    target = v
                end
            end
        end
    end
 
    return target
end
 
local function updateFOVRing()
    FOVring.Position = workspace.CurrentCamera.ViewportSize / 2
end
 
local function highlightTarget(target)
    if highlightEnabled and target and target.Character then
        local highlight = Instance.new("Highlight")
        highlight.Adornee = target.Character
        highlight.FillColor = Color3.fromRGB(255, 128, 128)
        highlight.OutlineColor = Color3.fromRGB(255, 0, 0)
        highlight.Parent = target.Character
    end
end
 
local function removeHighlight(target)
    if highlightEnabled and target and target.Character and target.Character:FindFirstChildOfClass("Highlight") then
        target.Character:FindFirstChildOfClass("Highlight"):Destroy()
    end
end
 
local function predictPosition(target)
    if target and target.Character and target.Character:FindFirstChild("HumanoidRootPart") then
        local velocity = target.Character.HumanoidRootPart.Velocity
        local position = target.Character[lockPart].Position
        local predictedPosition = position + (velocity * predictionFactor)
        return predictedPosition
    end
    return nil
end
 
local function handleToggle()
    if debounce then return end
    debounce = true
    toggleState = not toggleState
    wait(0.3)  -- Debounce time to prevent multiple toggles
    debounce = false
end
 
loop = RunService.RenderStepped:Connect(function()
    if aimbotEnabled then
        updateFOVRing()
 
        local localPlayer = Players.LocalPlayer.Character
        local cam = workspace.CurrentCamera
        local screenCenter = workspace.CurrentCamera.ViewportSize / 2
 
        if Toggle then
            if UserInputService:IsKeyDown(ToggleKey) then
                handleToggle()
            end
        else
            toggleState = UserInputService:IsMouseButtonPressed(Enum.UserInputType.MouseButton2)
        end
 
        if toggleState then
            if not currentTarget then
                currentTarget = getClosest(cam.CFrame)
                highlightTarget(currentTarget)  -- Highlight the new target if enabled
            end
 
            if currentTarget and currentTarget.Character and currentTarget.Character:FindFirstChild(lockPart) then
                local predictedPosition = predictPosition(currentTarget)
                if predictedPosition then
                    workspace.CurrentCamera.CFrame = workspace.CurrentCamera.CFrame:Lerp(CFrame.new(cam.CFrame.Position, predictedPosition), smoothing)
                end
                FOVring.Color = Color3.fromRGB(0, 255, 0)  -- Change FOV ring color to green when locked onto a target
            else
                FOVring.Color = Color3.fromRGB(255, 128, 128)  -- Revert FOV ring color to original when not locked onto a target
            end
        else
            if currentTarget and highlightEnabled then
                removeHighlight(currentTarget)  -- Remove highlight from the old target
            end
            currentTarget = nil
            FOVring.Color = Color3.fromRGB(255, 128, 128)  -- Revert FOV ring color to original when not locked onto a target
        end
    end
end)
 
})


local MAINTab = Window:CreateTab("GUI", "earth")
local MAINSection = MAINTab:CreateSection("MAIN")



local Button = MAINTab:CreateButton({
   Name = "SUS HUB ",
   Callback = function()
loadstring(game:HttpGet("https://raw.githubusercontent.com/ShutUpJamesTheLoser/freaky/refs/heads/main/fe", true))()
   end,
})

local Button = MAINTab:CreateButton({
   Name = "FIGHTER FE ",
   Callback = function()
loadstring(game:HttpGet("https://pastefy.app/wxVAgZpT/raw"))()
   end,
})

local Button = MAINTab:CreateButton({
   Name = "Fe Superman ",
   Callback = function()
loadstring(game:HttpGet("https://pastebin.com/raw/5yLUa0Dd"))(); 
   end,
})

local Button = MAINTab:CreateButton({
   Name = "jerk off r6 and r16 ",
   Callback = function()
loadstring(game:HttpGet("https://pastefy.app/wa3v2Vgm/raw"))() 
loadstring(game:HttpGet("https://pastefy.app/YZoglOyJ/raw"))() 
   end,
})

local Button = MAINTab:CreateButton({
   Name = "INFINITE  YIELD",
   Callback = function()
 loadstring(game:HttpGet('https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source'))()
   end,
})

local Button = MAINTab:CreateButton({
   Name = "Vereus script ",
   Callback = function()
loadstring(game:HttpGet('https://pastebin.com/raw/wC64LrAJ',true))()
   end,
})

local Button = MAINTab:CreateButton({
   Name = "Fe script ",
   Callback = function()
local ScreenGui = Instance.new("ScreenGui")
local Frame = Instance.new("Frame")
local Credits = Instance.new("TextLabel")
local insaneall = Instance.new("TextButton")
local daball = Instance.new("TextButton")
local dabothers = Instance.new("TextButton")
local insaneothers = Instance.new("TextButton")

-- Properties

ScreenGui.Parent = game.CoreGui

Frame.Parent = ScreenGui
Frame.Active = true
Frame.BackgroundColor3 = Color3.new(0, 0, 0)
Frame.BackgroundTransparency = 0.5
Frame.Draggable = true
Frame.Position = UDim2.new(0, 341, 0, 41)
Frame.Size = UDim2.new(0, 358, 0, 423)

Credits.Name = "Credits"
Credits.Parent = Frame
Credits.BackgroundColor3 = Color3.new(0, 0, 0)
Credits.BackgroundTransparency = 0.5
Credits.Position = UDim2.new(0, 79, 0, 0)
Credits.Size = UDim2.new(0, 200, 0, 50)
Credits.Font = Enum.Font.SciFi
Credits.FontSize = Enum.FontSize.Size24
Credits.Text = "By Cozmo V3rm"
Credits.TextColor3 = Color3.new(0, 1, 1)
Credits.TextSize = 24

insaneall.Name = "insaneall"
insaneall.Parent = Frame
insaneall.BackgroundColor3 = Color3.new(0, 0, 0)
insaneall.BackgroundTransparency = 0.5
insaneall.Position = UDim2.new(0, 0, 0, 106)
insaneall.Size = UDim2.new(0, 155, 0, 50)
insaneall.Font = Enum.Font.SciFi
insaneall.FontSize = Enum.FontSize.Size18
insaneall.Text = "FE Insane all"
insaneall.TextColor3 = Color3.new(0, 1, 1)
insaneall.TextSize = 18

daball.Name = "daball"
daball.Parent = Frame
daball.BackgroundColor3 = Color3.new(0, 0, 0)
daball.BackgroundTransparency = 0.5
daball.Position = UDim2.new(0, 0, 0, 228)
daball.Size = UDim2.new(0, 155, 0, 50)
daball.Font = Enum.Font.SciFi
daball.FontSize = Enum.FontSize.Size18
daball.Text = "FE Dab all"
daball.TextColor3 = Color3.new(0, 1, 1)
daball.TextSize = 18

dabothers.Name = "dabothers"
dabothers.Parent = Frame
dabothers.BackgroundColor3 = Color3.new(0, 0, 0)
dabothers.BackgroundTransparency = 0.5
dabothers.Position = UDim2.new(0, 203, 0, 230)
dabothers.Size = UDim2.new(0, 155, 0, 50)
dabothers.Font = Enum.Font.SciFi
dabothers.FontSize = Enum.FontSize.Size18
dabothers.Text = "FE Dab others"
dabothers.TextColor3 = Color3.new(0, 1, 1)
dabothers.TextSize = 18

insaneothers.Name = "insaneothers"
insaneothers.Parent = Frame
insaneothers.BackgroundColor3 = Color3.new(0, 0, 0)
insaneothers.BackgroundTransparency = 0.5
insaneothers.Position = UDim2.new(0, 203, 0, 105)
insaneothers.Size = UDim2.new(0, 155, 0, 50)
insaneothers.Font = Enum.Font.SciFi
insaneothers.FontSize = Enum.FontSize.Size18
insaneothers.Text = "FE Insane others"
insaneothers.TextColor3 = Color3.new(0, 1, 1)
insaneothers.TextSize = 18

insaneall.MouseButton1Click:connect(function()
for i,v in pairs(game.Players:GetPlayers()) do
local AnimationId = "33796059"
local Anim = Instance.new("Animation")
Anim.AnimationId = "rbxassetid://"..AnimationId
local k = v.Character.Humanoid:LoadAnimation(Anim)
k:Play()
k:AdjustSpeed(90)
end
end)
insaneothers.MouseButton1Click:connect(function()
for i,v in pairs(game.Players:GetPlayers()) do
if v.Name~=game.Players.LocalPlayer.Name then
local AnimationId = "33796059"
local Anim = Instance.new("Animation")
Anim.AnimationId = "rbxassetid://"..AnimationId
local k = v.Character.Humanoid:LoadAnimation(Anim)
k:Play()
k:AdjustSpeed(90)
end
end
end)
dabothers.MouseButton1Click:connect(function()
for i,v in pairs(game.Players:GetPlayers()) do
if v.Name~=game.Players.LocalPlayer.Name then
local AnimationId = "248263260"
local Anim = Instance.new("Animation")
Anim.AnimationId = "rbxassetid://"..AnimationId
local k = v.Character.Humanoid:LoadAnimation(Anim)
k:Play()
k:AdjustSpeed(1)
end
end
end)
daball.MouseButton1Click:connect(function()
for i,v in pairs(game.Players:GetPlayers()) do
local AnimationId = "248263260"
local Anim = Instance.new("Animation")
Anim.AnimationId = "rbxassetid://"..AnimationId
local k = v.Character.Humanoid:LoadAnimation(Anim)
k:Play()
k:AdjustSpeed(1)
end
end)
   end,
})

local Button = MAINTab:CreateButton({
   Name = "FE Grandmaster ",
   Callback = function()
-- Methods
local Methods = loadstring(game:HttpGet("https://raw.githubusercontent.com/coolsk8rboy/The-John-Cena-Factory/main/JohnCenasMain.lua"))()
-- Setup
Methods:SetIdleAnimation(7142933331, .1)
Methods:SetWalkAnimation(7142732585, .1)
Methods:SetWalkSpeed(13)
Methods:EnableSprinting(7142895580, .1, 25)
-- Fling
Methods:BodyPartFlingOnTouch("Right Arm")
Methods:BodyPartFlingOnTouch("Left Arm")
Methods:BodyPartFlingOnTouch("Right Leg")
Methods:BodyPartFlingOnTouch("Left Leg")
-- Key Connections
Methods:OnKeyPress("q", function()
    Methods:ApplyVelocityFoward(-5)
    Methods:ApplyVelocityUpward(80)
    Methods:PlayAnimation(7142738887, .1, 5)
end)
Methods:OnKeyPress("e", function()
    Methods:ApplyVelocityFoward(5)
    Methods:ApplyVelocityUpward(80)
    Methods:PlayAnimation(6936454190, .1, 5)
end)
-- Attacks
Methods:NewAttack("Attack1", "z", 7142975235, .0, 0.1)
Methods:NewAttack("Attack2", "x", 7142973417, .0, 0.1)
Methods:NewAttack("Attack3", "c", 7142740591, .0, 0.1) --3
Methods:NewAttack("Attack4", "v", 7142741890, .0, 0.1) --3
Methods:NewAttack("Attack5", "b", 6936458635, .0, 0.1) --3
Methods:NewAttack("Attack6", "t", 4837749916,  .0, 0.1) --2
-- Finishing up
Methods:SetScriptCreator("CROAXER")
Methods:SystemMessage([[Controls:
z - Combo 1
x - Combo 2
c - Combo 3
v - Combo 4
b - Combo 5
e - Front Flip
q - Back Flip
t - Taunt
Left Shift - Sprint
]])
Methods:PlaySound(1842802203, true)
Methods:RunScript()
   end,
})

local Button = MAINTab:CreateButton({
   Name = "FE FLIP",
   Callback = function()
wait(3)

--[[ Info ]]--

local ver = "2.00"
local scriptname = "feFlip"


--[[ Keybinds ]]--

local FrontflipKey = Enum.KeyCode.Z
local BackflipKey = Enum.KeyCode.X
local AirjumpKey = Enum.KeyCode.C


--[[ Dependencies ]]--

local ca = game:GetService("ContextActionService")
local zeezy = game:GetService("Players").LocalPlayer
local h = 0.0174533
local antigrav


--[[ Functions ]]--

function zeezyFrontflip(act,inp,obj)
	if inp == Enum.UserInputState.Begin then
		zeezy.Character.Humanoid:ChangeState("Jumping")
		wait()
		zeezy.Character.Humanoid.Sit = true
		for i = 1,360 do 
			delay(i/720,function()
			zeezy.Character.Humanoid.Sit = true
				zeezy.Character.HumanoidRootPart.CFrame = zeezy.Character.HumanoidRootPart.CFrame * CFrame.Angles(-h,0,0)
			end)
		end
		wait(0.55)
		zeezy.Character.Humanoid.Sit = false
	end
end

function zeezyBackflip(act,inp,obj)
	if inp == Enum.UserInputState.Begin then
		zeezy.Character.Humanoid:ChangeState("Jumping")
		wait()
		zeezy.Character.Humanoid.Sit = true
		for i = 1,360 do
			delay(i/720,function()
			zeezy.Character.Humanoid.Sit = true
				zeezy.Character.HumanoidRootPart.CFrame = zeezy.Character.HumanoidRootPart.CFrame * CFrame.Angles(h,0,0)
			end)
		end
		wait(0.55)
		zeezy.Character.Humanoid.Sit = false
	end
end

function zeezyAirjump(act,inp,obj)
	if inp == Enum.UserInputState.Begin then
		zeezy.Character:FindFirstChildOfClass'Humanoid':ChangeState("Seated")
		wait()
		zeezy.Character:FindFirstChildOfClass'Humanoid':ChangeState("Jumping")	
	end
end


--[[ Binds ]]--

ca:BindAction("zeezyFrontflip",zeezyFrontflip,false,FrontflipKey)
ca:BindAction("zeezyBackflip",zeezyBackflip,false,BackflipKey)
ca:BindAction("zeezyAirjump",zeezyAirjump,false,AirjumpKey)

--[[ Load Message ]]--

print(scriptname .. " " .. ver .. " loaded successfully")
print("")

local notifSound = Instance.new("Sound",workspace)
notifSound.PlaybackSpeed = 1.5
notifSound.Volume = 0.15
notifSound.SoundId = "rbxassetid://170765130"
notifSound.PlayOnRemove = true
notifSound:Destroy()
game.StarterGui:SetCore("SendNotification", {Title = "feFlip", Text = "feFlip loaded successfully!", Icon = "rbxassetid://505845268", Duration = 5, Button1 = "Okay"})
   end,
})

local Button = MAINTab:CreateButton({
   Name = "FE Ball roll",
   Callback = function()
-- I DONT OWN THE SCRIPT
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera

local SPEED_MULTIPLIER = 30
local JUMP_POWER = 60
local JUMP_GAP = 0.3

local character = game.Players.LocalPlayer.Character

for i,v in ipairs(character:GetDescendants()) do
   if v:IsA("BasePart") then
       v.CanCollide = false
   end
end

local ball = character.HumanoidRootPart
ball.Shape = Enum.PartType.Ball
ball.Size = Vector3.new(5,5,5)
local humanoid = character:WaitForChild("Humanoid")
local params = RaycastParams.new()
params.FilterType = Enum.RaycastFilterType.Blacklist
params.FilterDescendantsInstances = {character}

local tc = RunService.RenderStepped:Connect(function(delta)
   ball.CanCollide = true
   humanoid.PlatformStand = true
if UserInputService:GetFocusedTextBox() then return end
if UserInputService:IsKeyDown("W") then
ball.RotVelocity -= Camera.CFrame.RightVector * delta * SPEED_MULTIPLIER
end
if UserInputService:IsKeyDown("A") then
ball.RotVelocity -= Camera.CFrame.LookVector * delta * SPEED_MULTIPLIER
end
if UserInputService:IsKeyDown("S") then
ball.RotVelocity += Camera.CFrame.RightVector * delta * SPEED_MULTIPLIER
end
if UserInputService:IsKeyDown("D") then
ball.RotVelocity += Camera.CFrame.LookVector * delta * SPEED_MULTIPLIER
end
--ball.RotVelocity = ball.RotVelocity - Vector3.new(0,ball.RotVelocity.Y/50,0)
end)

UserInputService.JumpRequest:Connect(function()
local result = workspace:Raycast(
ball.Position,
Vector3.new(
0,
-((ball.Size.Y/2)+JUMP_GAP),
0
),
params
)
if result then
ball.Velocity = ball.Velocity + Vector3.new(0,JUMP_POWER,0)
end
end)

Camera.CameraSubject = ball
humanoid.Died:Connect(function() tc:Disconnect() end)
   end,
})


local MAINTab = Window:CreateTab("MISC", "hammer")
local MAINSection = MAINTab:CreateSection("MAIN")

local Button = MAINTab:CreateButton({
   Name = "infinite yield",
   Callback = function()
loadstring(game:HttpGet(('https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source'),true))()
   end,
})

local MAINSelection = MAINTab:CreateSection("Functions")

local Slider = MAINTab:CreateSlider({
   Name = "WalkSpeed",
   Range = {0, 500},
   Increment = 1,
   Suffix = "Speed",
   CurrentValue = 16,
   Flag = "Slider1", 
   Callback = function(Value)
        game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = (Value)
   end,
})

local Slider = MAINTab:CreateSlider({
   Name = "JumpPower",
   Range = {0, 500},
   Increment = 1,
   Suffix = "Jump",
   CurrentValue = 16,
   Flag = "Slider1", 
   Callback = function(Value)
        game.Players.LocalPlayer.Character.Humanoid.JumpPower = (Value)
   end,
})
